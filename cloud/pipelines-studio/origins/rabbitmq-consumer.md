# RabbitMQ Consumer

RabbitMQ Consumer reads AMQP messages from a single RabbitMQ queue.&#x20;

When you configure the origin, you specify the information needed to connect to RabbitMQ client. You also define a queue and the bindings to use. You can use multiple bindings to read messages from one or more exchanges. You can also configure SSL/TLS properties, including default transport protocols and cipher suites.

You can configure the origin to generate a single record for each RabbitMQ message. By default, if a message includes multiple objects, the origin generates a record for each object.

You can add custom configuration options and define connection credentials as needed.

### Queue <a href="#ariaid-title2" id="ariaid-title2"></a>

RabbitMQ Consumer reads messages from a specified queue.

When you configure the origin, you define the queue name and other properties. If a queue of the specified name does not exist, RabbitMQ creates the queue based on the properties that you defined.

If the queue does exist but the properties are incorrect, error messages display when trying to connect.

### Record Header Attributes <a href="#ariaid-title3" id="ariaid-title3"></a>

The RabbitMQ Consumer origin includes information from RabbitMQ basic properties in the record header attributes of the generated record. When the origin processes Avro data, it includes the Avro schema in an avroSchema record header attribute.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes.

The origin includes information in the following record header attributes when available:

* avroSchema - When processing Avro data, provides the Avro schema.
* appId - Provides information from the RabbitMQ app-id property.
* contentType - Provides information from the RabbitMQ content-type property.
* contentEncoding - Provides information from the RabbitMQ content-encoding property.
* correlationId - Provides information from the RabbitMQ correlation-id property.
* deliveryMode - Provides information from the RabbitMQ delivery-mode property.
* deliveryTag - Provides information from the RabbitMQ delivery-tag property.
* exchange - Provides information from the RabbitMQ exchange-name property.
* expiration - Provides information from the RabbitMQ expiration property.
* messageId - Provides information from the RabbitMQ message-id property.
* messageType - Provides information from the RabbitMQ type property.
* priority - Provides information from the RabbitMQ priority property.
* redelivered - Provides information from the RabbitMQ redelivered property.
* replyTo - Provides information from the RabbitMQ reply-to property.
* routingKey - Provides information from the RabbitMQ routing-key property.
* timestamp - Provides information from the RabbitMQ timestamp property.
* userId - Provides information from the RabbitMQ user-id property.

### Data Formats <a href="#ariaid-title4" id="ariaid-title4"></a>

The RabbitMQ Consumer origin processes data differently based on the data format. The origin can process the following types of data:

Avro

Generates a record for every message. Includes a `precision` and `scale` field attribute for each Decimal field.The stage includes the Avro schema in an `avroSchema` record header attribute. You can use one of the following methods to specify the location of the Avro schema definition:

* Message/Data Includes Schema - Use the schema in the message.
* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the stage to look up the schema in Confluent Schema Registry by the schema ID embedded in the message or by the schema ID or subject specified in the stage configuration.

Using a schema in the stage configuration or retrieving a schema from Confluent Schema Registry overrides any schema that might be included in the message and can improve performance.

**Binary**

Generates a record with a single byte array field at the root of the record.When the data exceeds the user-defined maximum data size, the origin cannot process the data. Because the record is not created, the origin cannot pass the record to the pipeline to be written as an error record. Instead, the origin generates a stage error.

**Delimited**

Generates a record for each delimited line.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record.You can also configure the stage to replace a string constant with null values and to ignore control characters.

**JSON**

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin processes the object based on the error handling configured for the stage.

**Log**

Generates a record for every log line.When a line exceeds the user-defined maximum line length, the origin truncates longer lines.You can include the processed log line as a field in the record. If the log line is truncated, and you request the log line in the record, the origin includes the truncated line.You can define the log format or type to be read.

**Protobuf**

Generates a record for every protobuf message. By default, the origin assumes messages contain multiple protobuf messages.Protobuf messages must match the specified message type and be described in the descriptor file.When the data for a record exceeds 1 MB, the origin cannot continue processing data in the message. The origin handles the message based on the stage error handling property and continues reading the next message.

**Record**

Generates a record for every record. Use to process records generated by a pipeline using the Record data format.For error records, the origin provides the original record as read from the origin in the original pipeline, as well as error information that you can use to correct the record.When processing error records, the origin expects the error file names and contents as generated by the original pipeline.

**Text**

Generates a record for each line of text or for each section of text based on a custom delimiter.When a line or section exceeds the maximum line length defined for the origin, the origin truncates it. The origin adds a boolean field named Truncated to indicate if the line was truncated.

**XML**

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

Note: Field attributes and record header attributes are written to destination systems automatically only when you use the SDC RPC data format in destinations.&#x20;

When a record exceeds the user-defined maximum record length, the origin skips the record and continues processing with the next record. It sends the skipped record to the pipeline for error handling.Use the XML data format to process valid XML documents.&#x20;

**Tip**: If you want to process invalid XML documents, you can try using the text data format with custom delimiters.&#x20;
