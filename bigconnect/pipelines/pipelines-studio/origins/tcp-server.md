# TCP Server

The TCP Server origin listens at the specified port numbers, establishes TCP sessions with clients that initiate TCP connections, and then processes the incoming data. The origin can process data from tables with simple numeric primary keys. The origin cannot process data from tables with compound or non-numeric primary keys.&#x20;

The origin can operate in different modes. The modes determine the messages that it can process. It can process NetFlow messages or syslog messages. It can also process supported Data Pipelines data formats passed as data separated by specified characters, passed as character-based data with length prefixes, or passed in Flume events as Avro messages.

The TCP Server can process data from multiple clients simultaneously, creating separate batches for each client, and sending acknowledgements to the originating client after parsing each record or committing each batch. You can configure the origin to use multiple threads to improve performance when processing of large volumes of data. And on 64-bit Linux systems, you can enable native Epoll transports to further improve performance.

When you configure the TCP Server origin, you specify the ports to use and the TCP mode that indicates the type of data the origin will receive. Then you configure mode-related properties, such as the characters that separate records.

You can optionally configure the acknowledgements that you want to send and the amount of time that the origin waits to receive data before closing the connection. You can also configure SSL/TLS properties, including default transport protocols and cipher suites.

### Multithreaded Processing <a href="#ariaid-title2" id="ariaid-title2"></a>

The TCP Server origin performs parallel processing and enables the creation of a multithreaded pipeline.

When you enable multithreaded processing, the TCP Server origin uses multiple concurrent threads based on the Number of Receiver Threads property. When you start the pipeline, the origin creates the number of threads specified in the property.

As clients initiate TCP connections, the origin establishes TCP sessions and waits for data. Upon filling a batch, the origin passes the batch to an available pipeline runner.

A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin. Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

For example, say you enable multithreaded processing and set the Number of Receiver Threads property to 5. When you start the pipeline, the origin creates five threads, and Data Pipelines creates a matching number of pipeline runners. Upon receiving data, the origin passes a batch to each of the pipeline runners for processing.

Each pipeline runner performs the processing associated with the rest of the pipeline. After a batch is written to pipeline destinations, the pipeline runner becomes available for another batch of data. Each batch is processed and written as quickly as possible, independent from other batches processed by other pipeline runners, so batches may be written differently from the read-order.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

### Closing Connections for Invalid Data <a href="#ariaid-title3" id="ariaid-title3"></a>

When the TCP Server origin receives invalid data, it closes the connection to the TCP client that sent the data. It also passes the data to the pipeline for error handling.

For example, when you configure the origin, you specify the maximum record size. When a TCP client sends a message that translates to larger than the maximum record size, the origin disconnects from the client and passes the message to the pipeline for error handling.

Similarly, say the TCP Server origin is configured to process XML data. If the origin receives an invalid XML document, it disconnects from the sending client and passes the data to the pipeline for error handling.

### Sending Acknowledgements <a href="#ariaid-title4" id="ariaid-title4"></a>

You can configure the TCP Server origin to send acknowlegements, a.k.a. acks., to the originating client. The acknowledgement message can be a simple text message, such as "Ack". Or, you can use the expression language to include additional information in the message.

The origin can send two types of acknowledgements:record processed acknowledgementWhen you configure a record processed acknowledgement, the origin sends acks after it receives and processes each record. It sends the ack after parsing a record from the incoming data.batch completed acknowledgementWhen you configure a batch completed acknowledgement, the origin sends acks after the pipeline completes processing the batch. It sends the ack after the batch is committed to all destinations.

#### Using Expressions in Messages <a href="#ariaid-title5" id="ariaid-title5"></a>

You can use the Data Pipelines expression language to create custom acknowledgement messages. You might use expressions to include information about Data Pipelines, the pipeline, record, or batch in the message.

For example, if you have multiple Data Pipelines processing data from the same client, you might use the following record processed message to include the Data Pipelines host name and the pipeline title in the message, along with a record identifier:

```
${record:value('/id')} was processed by Data Pipelines: ${sdc:hostname()}, 
pipeline: ${pipeline:title()}.
```

You can set the time zone to use for datetime values returned by expressions. By default, the origin uses UTC.

Note: In a batch completion message, record functions return information from the last record in the batch.You can use the batchSize variable to return the number of records included in the batch. The batchSize variable can be used only with the TCP Server origin and must be typed into the message. The batchSize variable does not appear in the expression completion list.For example, the following message includes the number of records in the batch, the transaction ID of the last record in the batch, and the pipeline that performed the processing:

```
Pipeline: ${pipeline:title()} committed a batch whose last record was 
${record:value('/transactionID')} and included ${batchSize} messages.
```

### TCP Modes <a href="#ariaid-title6" id="ariaid-title6"></a>

The TCP Server origin processes data differently depending on the mode that you select. The origin provides the following modes:

**NetFlow messages**

The TCP Server origin can process NetFlow 5 and NetFlow 9 messages. When processing NetFlow messages, the stage generates different records based on the NetFlow version. When processing NetFlow 9, the records are generated based on the NetFlow 9 configuration properties.&#x20;

To process NetFlow messages, set the TCP Mode property to NetFlow. Then, for NetFlow 9 data, configure the properties on the NetFlow 9 tab. NetFlow 5 data does not require additional configuration.syslog messages

The TCP Server origin processes syslog messages in accordance with [RFC 6587](https://tools.ietf.org/html/rfc6587), except the origin does not support method changes.The TCP Server origin can process the following types of syslog messages:

* [RFC 5424](https://tools.ietf.org/html/rfc5424)
* [RFC 3164](https://tools.ietf.org/html/rfc3164)
* Non-standard common messages, such as RFC 3339 dates with no version digit

To process syslog messages, set the TCP Mode property to "syslog" and configure the transfer framing mode.

**Important**: All TCP clients must use the same transfer framing mode to transmit data.Use one of the following transfer framing modes:

* Octet counting - The frame indicates the length of the syslog message and includes the entire contents of the message.
*   Non-transparent framing - The frame includes the syslog message and a user-defined trailing separator characters. The origin uses the separator characters to create records.Use the following Java Unicode syntax to specify a separator character:

    ```
    \u<Unicode character code>
    ```

    To define multiple characters, simply list them as a group as appropriate, such as:

    ```
    \u<Unicode character code>\u<Unicode character code>\u<Unicode character code>
    ```

    For example, the default separator character is line feed, whose Unicode character code is 000A. To specify this as the separator character, enter `\u000A.`

**Separated records**

The TCP Server origin can process the supported Data Pipelines data formats when the data is separated by the specified record separator characters.

To process supported data formats, set the TCP Mode property to Separated Records and specify the record separator characters. Then, specify the data format of the data and configure any related properties.

Important: All TCP clients must use the same record separator characters.

**Character data with length prefix**

The TCP Server origin can process the supported Data Pipelines data formats when passed as character-based data with a length prefix.

To process supported data formats, set the TCP Mode property to Character Data with Length Prefix and specify the character set of the data. Then, specify the data format of the data and configure any related properties.

A length prefix consists of the digits that indicate the length of the data, and a space character. The data to be converted to a record should immediately follow the space character.

Note: The length prefix must be in a single-byte encoding, such as UTF-8. The data can be in any valid character set, which you specify in the origin.

For example, say a TCP client sends the following UTF-8 data:

```
11 hello world
```

The length prefix is "11 ", which indicates that the data to be converted is 11 bytes long. The origin then converts the following 11 bytes, "hello world" to a record.

You can use this TCP mode to capture raw syslog messages that are framed with octet counting into a string field.

**Flume Avro IPC Server**

The TCP Server origin can process the supported Data Pipelines data formats when passed in Flume events as Avro messages from a [Flume Avro sink](http://flume.apache.org/FlumeUserGuide.html#avro-sink). Use the TCP Server origin instead of the HTTP Server origin to more efficiently process Flume events.

To process supported data formats, set the TCP Mode property to Flume Avro IPC Server and configure the additional Bind Address property. Then, specify the data format of the data and configure any related properties.

### Data Formats <a href="#ariaid-title7" id="ariaid-title7"></a>

In Separated Record or Character Data with Length Prefix TCP mode, the TCP Server origin processes data differently based on the data format.

The origin can also process the following types of data when separated by the appropriate record separator:

Avro

Generates a record for every message. Includes a `precision` and `scale` field attribute for each Decimal field.The stage includes the Avro schema in an `avroSchema` record header attribute. You can use one of the following methods to specify the location of the Avro schema definition:

* Message/Data Includes Schema - Use the schema in the message.
* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the stage to look up the schema in Confluent Schema Registry by the schema ID embedded in the message or by the schema ID or subject specified in the stage configuration.

Using a schema in the stage configuration or retrieving a schema from Confluent Schema Registry overrides any schema that might be included in the message and can improve performance.

Binary

Generates a record with a single byte array field at the root of the record.When the data exceeds the user-defined maximum data size, the origin cannot process the data. Because the record is not created, the origin cannot pass the record to the pipeline to be written as an error record. Instead, the origin generates a stage error.

Delimited

Generates a record for each delimited line.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record.You can also configure the stage to replace a string constant with null values and to ignore control characters.

JSON

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin processes the object based on the error handling configured for the stage.

Log

Generates a record for every log line.When a line exceeds the user-defined maximum line length, the origin truncates longer lines.You can include the processed log line as a field in the record. If the log line is truncated, and you request the log line in the record, the origin includes the truncated line.You can define the log format or type to be read.

Protobuf

Generates a record for every protobuf message. By default, the origin assumes messages contain multiple protobuf messages.Protobuf messages must match the specified message type and be described in the descriptor file.When the data for a record exceeds 1 MB, the origin cannot continue processing data in the message. The origin handles the message based on the stage error handling property and continues reading the next message.

Record

Generates a record for every record. Use to process records generated by a Data Pipelines pipeline using the Record data format.For error records, the origin provides the original record as read from the origin in the original pipeline, as well as error information that you can use to correct the record.When processing error records, the origin expects the error file names and contents as generated by the original pipeline.

Text

Generates a record for each line of text or for each section of text based on a custom delimiter.When a line or section exceeds the maximum line length defined for the origin, the origin truncates it. The origin adds a boolean field named Truncated to indicate if the line was truncated.

XML

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

Note: Field attributes and record header attributes are written to destination systems automatically only when you use the SDC RPC data format in destinations.&#x20;

When a record exceeds the user-defined maximum record length, the origin skips the record and continues processing with the next record. It sends the skipped record to the pipeline for error handling.

Use the XML data format to process valid XML documents.&#x20;

Tip: If you want to process invalid XML documents, you can try using the text data format with custom delimiters.&#x20;
