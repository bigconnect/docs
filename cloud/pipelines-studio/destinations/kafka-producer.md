# Kafka Producer

The Kafka Producer destination writes data to a Kafka cluster. The destination can also send responses to a microservice origin when used in a microservice pipeline.&#x20;

When you configure a Kafka Producer, you define connection information, the partition strategy, and data format to use. You can also configure Kafka Producer to determine the topic to write to at runtime.

The Kafka Producer passes data to partitions in the Kafka topic based on the partition strategy that you choose. You can optionally write a batch of records to the Kafka cluster as a single message. When you want the destination to send responses to a microservice origin within a microservice pipeline, you specify the type of response to send.

You can add additional Kafka configuration properties as needed. You can configure the destination to use Kafka security features. You can also configure the destination to pass message key values stored in the record to Kafka as Kafka message keys.

You can configure the Kafka Producer to work with the Confluent Schema Registry. The Confluent Schema Registry is a distributed storage layer for Avro schemas which uses Kafka as its underlying storage mechanism.

### Broker List <a href="#ariaid-title2" id="ariaid-title2"></a>

The Kafka Producer connects to Kafka based on the topic and associated brokers that you specify. To ensure a connection in case a specified broker goes down, list as many brokers as possible.

### Runtime Topic Resolution <a href="#ariaid-title3" id="ariaid-title3"></a>

Kafka Producer can write a record to the topic based on an expression. When Kafka Producer evaluates a record, it calculates the expression based on record values and writes the record to the resulting topic.

When performing runtime topic resolution, Kafka Producer can write to any topic by default. You can create a white list of topics to limit the number of topics Kafka Producer attempts to use. When you create a white list, any record that resolves to an unlisted topic is sent to the stage for error handling. Use a white list when record data might resolve to invalid topic names.

### Partition Strategy <a href="#ariaid-title4" id="ariaid-title4"></a>

The partition strategy determines how to write data to Kafka partitions. You can use a partition strategy to balance the work load or to write data semantically.

The Kafka Producer provides the following partition strategies:

**Round-Robin**

Writes each record to a different partition using a cyclical order. Use for load balancing.

**Random**

Writes each record to a different partition using a random order. Use for load balancing.

**Expression**

Writes each record to a partition based on the results of the partition expression. Use to perform semantic partitioning.When you configure the partition expression, define the expression to evaluate to the partition where you want each record written. The expression must return a numeric value.For example, the following expression writes records to two partitions based on the value in the Age field:

```
${record:value('/Age') < 21 ? 0 : 1}
```

The following example writes to three partitions based on the value of the Age field:

```
${record:value('/a') < 21 ? 0 : record:value('/a') < 55 ? 1 : 2}
```

**Default**

Writes each record using the default partition strategy that Kafka provides.When you use the default partition strategy, you configure a partition expression that returns the partition key from the records, such as `${record:value('/partitionkey')}`. The expression must return a string value.

The Kafka Producer writes each record to a partition based on a hash of the partition key.

### Send Microservice Responses <a href="#ariaid-title5" id="ariaid-title5"></a>

The Kafka Producer destination can send responses to a microservice origin when you use the destination in a microservice pipeline.

When sending responses to a microservice origin, the destination can send one of the following:

* All successfully written records.
* Responses from the destination system - For information about the possible responses, see the documentation for the destination system.

### Additional Kafka Properties <a href="#ariaid-title6" id="ariaid-title6"></a>

You can add custom Kafka configuration properties to the Kafka Producer destination.

When you add a Kafka configuration property, enter the exact property name and the value. The stage does not validate the property names or values.

Several properties are defined by default, you can edit or remove the properties as necessary.

Note: Because the stage uses several configuration properties, it ignores user-defined values for the following properties:

* key.serializer.class
* metadata.broker.list
* partitioner.class
* producer.type
* serializer.class

### Kafka Security <a href="#ariaid-title7" id="ariaid-title7"></a>

You can configure the Kafka Producer destination to connect securely to Kafka through SSL/TLS, SASL, or both. For more information about the methods and details on how to configure each method, see Security in Kafka Stages.

### Data Formats <a href="#ariaid-title8" id="ariaid-title8"></a>

The Kafka Producer destination writes data to Kafka based on the data format that you select.

The Kafka Producer destination processes data formats as follows:

Avro

The destination writes records based on the Avro schema.You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. The Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in the Confluent Schema Registry by the schema ID or subject.

    You must specify the method that the Kafka Producer uses to serialize the messages in the Avro format. To embed the Avro schema ID in each message that the destination writes, set the key and value serializers to Confluent on the Kafka tab.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the destination to register the Avro schema with the Confluent Schema Registry. You can also optionally include the schema definition in the message.

You can include the Avro schema in the output. You can also compress data with an Avro-supported compression codec.

Binary

The stage writes binary data to a single field in the record.

Delimited

The destination writes records as delimited data. When you use this data format, the root field must be list or list-map.You can use the following delimited format types:

* Default CSV - File that includes comma-separated values. Ignores empty lines in the file.
* RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.
* MS Excel CSV - Microsoft Excel comma-separated file.
* MySQL CSV - MySQL comma-separated file.
* Tab-Separated Values - File that includes tab-separated values.
* PostgreSQL CSV - PostgreSQL comma-separated file.
* PostgreSQL Text - PostgreSQL text file.
* Custom - File that uses user-defined delimiter, escape, and quote characters.
* Multi Character Delimited - File that uses multiple user-defined characters to delimit fields and lines, and single user-defined escape and quote characters.

JSON

The destination writes records as JSON data. You can use one of the following formats:

* Array - Each file includes a single array. In the array, each element is a JSON representation of each record.
* Multiple objects - Each file includes multiple JSON objects. Each object is a JSON representation of a record.

Protobuf

Writes one record in a message. Uses the user-defined message type and the definition of the message type in the descriptor file to generate the message.

RecordThe destination writes records in the Record data format.

Text

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.

When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

XML

The destination creates a valid XML document for each record. The destination requires the record to have a single root field that contains the rest of the record data.&#x20;

The destination can include indentation to produce human-readable documents. It can also validate that the generated XML conforms to the specified schema definition. Records with invalid schemas are handled based on the error handling configured for the destination.
