# Kafka Multitopic Consumer

The Kafka Multitopic Consumer origin reads data from multiple topics in an Apache Kafka cluster. The origin can use multiple threads to enable parallel processing of data. When preferred, you can use the Kafka Consumer to read from a single topic using a single thread.

When you configure a Kafka Multitopic Consumer, you configure the consumer group name and the brokers to use. You also specify the topics to process and the number of threads to use. In Kafka, make sure that the partition assignment strategy is configured appropriately.

You can configure the origin to produce a single record when a message includes multiple objects. You can add additional Kafka configuration properties as needed. You can configure the origin to use Kafka security features. You can also configure the origin to capture Kafka message keys and store them in the record.

When processing Avro data, you can configure the Kafka Multitopic Consumer to work with the Confluent Schema Registry. The Confluent Schema Registry is a distributed storage layer for Avro schemas which uses Kafka as its underlying storage mechanism.

Kafka Multitopic Consumer includes record header attributes that enable you to use information about the record in pipeline processing.

### Offset Management <a href="#ariaid-title2" id="ariaid-title2"></a>

The first time that a Kafka Multitopic Consumer origin identified by a consumer group receives messages from a topic, an offset entry is created for that consumer group and topic. The offset entry is created in Kafka.

The Kafka Multitopic Consumer origin begins receiving messages in the topic based on whether or not a stored offset entry exists:No stored offsetWhen the consumer group and topic combination does not have a previously stored offset, the Kafka Multitopic Consumer origin uses the Auto Offset Reset property to determine the first message to read. You can set the origin to read messages in the topic starting from the earliest message, latest message, or a particular timestamp. The default setting is the earliest message, which results in the origin reading all existing messages in the topic.Previously stored offsetWhen the consumer group and topic combination has a previously stored offset, the Kafka Multitopic Consumer origin receives messages starting with the next unprocessed message after the stored offset. For example, when you stop and restart the pipeline, processing resumes from the last committed offset.

### Multithreaded Processing <a href="#ariaid-title3" id="ariaid-title3"></a>

The Kafka Multitopic Consumer origin performs parallel processing and enables the creation of a multithreaded pipeline. The Kafka Multitopic Consumer origin uses multiple concurrent threads based on the Number of Threads property and the partition assignment strategy defined in the Kafka cluster.

Important: In Kafka, make sure that the partition assignment strategy is set to the strategy you want to use. For example, in Kafka 0.10.1, you can set partition.assignment.strategy property to range or roundrobin. For more information about the strategies, see [the Kafka documentation](https://kafka.apache.org/documentation/).

When performing multithreaded processing, the Kafka Multitopic Consumer origin checks the list of topics to process and creates the specified number of threads. Each thread connects to Kafka and creates a batch of data from a partition assigned by the broker based on the Kafka partition assignment strategy. Then, it passes the batch to an available pipeline runner.

A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin. Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

For example, say you set the Number of Threads property to 5. When you start the pipeline, the origin creates five threads, and Data Pipelines creates a matching number of pipeline runners. The threads are assigned to different partitions based on the Kafka partition assignment strategy. Upon receiving data, the origin passes a batch to each of the pipeline runners for processing.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

For more information about multithreaded pipelines, see Multithreaded Pipeline Overview. For more information about the Kafka partition assignment strategies, see the [Kafka documentation](https://kafka.apache.org/documentation/).

### Additional Kafka Properties <a href="#ariaid-title4" id="ariaid-title4"></a>

You can add custom Kafka configuration properties to the Kafka Multitopic Consumer.

When you add the Kafka configuration property, enter the exact property name and the value. The Kafka Multitopic Consumer does not validate the property names or values.

Note: The Kafka Multitopic Consumer origin uses the following Kafka configuration properties. The origin ignores user-defined values for these properties:

* auto.commit.interval.ms
* bootstrap.servers
* enable.auto.commit
* group.id
* max.poll.records

### Record Header Attributes <a href="#ariaid-title5" id="ariaid-title5"></a>

The Kafka Multitopic Consumer origin creates record header attributes that include information about the originating file for the record. When the origin processes Avro data, it includes the Avro schema in an avroSchema record header attribute.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes. For more information about working with record header attributes, see Working with Header Attributes.

The Kafka Multitopic Consumer origin creates the following record header attributes:

* avroSchema - When processing Avro data, provides the Avro schema.
* offset - The offset where the record originated.
* partition - The partition where the record originated.
* topic - The topic where the record originated.

### Kafka Security <a href="#ariaid-title6" id="ariaid-title6"></a>

You can configure the Kafka Multitopic Consumer origin to connect securely to Kafka through SSL/TLS, SASL, or both. For more information about the methods and details on how to configure each method, see Security in Kafka Stages.

### Data Formats <a href="#ariaid-title7" id="ariaid-title7"></a>

The Kafka Multitopic Consumer origin processes data differently based on the data format. Kafka Multitopic Consumer can process the following types of data:

Avro

Generates a record for every message. Includes a `precision` and `scale` field attribute for each Decimal field.The origin includes the Avro schema in an `avroSchema` record header attribute. You can use one of the following methods to specify the location of the Avro schema definition:

* Message/Data Includes Schema - Use the schema in the message.
* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the origin to look up the schema in Confluent Schema Registry by the schema ID embedded in the message or by the schema ID or subject specified in the stage configuration.

    You must specify the method that the origin uses to deserialize the message. If the Avro schema ID is embedded in each message, set the key and value deserializers to Confluent on the Kafka tab.

Using a schema in the stage configuration or retrieving a schema from Confluent Schema Registry overrides any schema that might be included in the message and can improve performance.

Binary

Generates a record with a single byte array field at the root of the record.When the data exceeds the user-defined maximum data size, the origin cannot process the data. Because the record is not created, the origin cannot pass the record to the pipeline to be written as an error record. Instead, the origin generates a stage error.DatagramGenerates a record for every message. The origin can process [collectd](https://collectd.org/) messages, NetFlow 5 and NetFlow 9 messages, and the following types of syslog messages:

* [RFC 5424](https://tools.ietf.org/html/rfc5424)
* [RFC 3164](https://tools.ietf.org/html/rfc3164)
* Non-standard common messages, such as RFC 3339 dates with no version digit

When processing NetFlow messages, the stage generates different records based on the NetFlow version. When processing NetFlow 9, the records are generated based on the NetFlow 9 configuration properties. For more information, see NetFlow Data Processing.

Delimited

Generates a record for each delimited line.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record.You can also configure the stage to replace a string constant with null values and to ignore control characters.For more information about reading delimited data, see Reading Delimited Data.

JSON

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin processes the object based on the error handling configured for the stage.

Log

Generates a record for every log line.When a line exceeds the user-defined maximum line length, the origin truncates longer lines.You can include the processed log line as a field in the record. If the log line is truncated, and you request the log line in the record, the origin includes the truncated line.You can define the log format or type to be read.

Protobuf

Generates a record for every protobuf message. By default, the origin assumes messages contain multiple protobuf messages.Protobuf messages must match the specified message type and be described in the descriptor file.When the data for a record exceeds 1 MB, the origin cannot continue processing data in the message. The origin handles the message based on the stage error handling property and continues reading the next message.For information about generating the descriptor file, see Protobuf Data Format Prerequisites.

Record

Generates a record for every record. Use to process records generated by a pipeline using the Record data format.For error records, the origin provides the original record as read from the origin in the original pipeline, as well as error information that you can use to correct the record.When processing error records, the origin expects the error file names and contents as generated by the original pipeline.

Text

Generates a record for each line of text or for each section of text based on a custom delimiter.When a line or section exceeds the maximum line length defined for the origin, the origin truncates it. The origin adds a boolean field named Truncated to indicate if the line was truncated.For more information about processing text with a custom delimiter, see Text Data Format with Custom Delimiters.

XML

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

**Note**: Field attributes and record header attributes are written to destination systems automatically only when you use the Pipelines RPC data format in destinations. For more information about working with field attributes and record header attributes, and how to include them in records, see Field Attributes and Record Header Attributes.

When a record exceeds the user-defined maximum record length, the origin skips the record and continues processing with the next record. It sends the skipped record to the pipeline for error handling.Use the XML data format to process valid XML documents. For more information about XML processing, see Reading and Processing XML Data.Tip: If you want to process invalid XML documents, you can try using the text data format with custom delimiters. For more information, see Processing XML Data with Custom Delimiters.
