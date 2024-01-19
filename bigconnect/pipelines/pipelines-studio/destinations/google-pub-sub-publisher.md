# Google Pub/Sub Publisher

The Google Pub/Sub Publisher destination publishes messages to a Google Pub/Sub topic. You can use other destinations to write to Google BigQuery, Google Bigtable, and Google Cloud Storage.&#x20;

When you configure the destination, you define the Google Pub/Sub topic ID to write messages to. You also define the project ID and credentials to use when connecting to Google Pub/Sub.

By default, the Google Pub/Sub Publisher destination writes messages in batches. Using advanced properties, you can configure the conditions that trigger writing a new batch or disable batch processing to write messages individually. You can also configure the action the destination takes when it reads messages faster than it can write messages.

A Google Pub/Sub message contains a payload and optional user-defined attributes that describe the payload content. When records include record header attributes, the Google Pub/Sub Publisher destination includes the record header attributes in message attributes. The destination does not include internal record header attributes in message attributes.&#x20;

### Credentials <a href="#ariaid-title2" id="ariaid-title2"></a>

When the Google Pub/Sub Publisher destination publishes messages to a Google Pub/Sub topic, the destination must pass credentials to Google Pub/Sub.

You can provide credentials using one the following options:

* Google Cloud default credentials
* Credentials in a file
* Credentials in a stage property

### Data Formats <a href="#ariaid-title3" id="ariaid-title3"></a>

Google Pub/Sub Publisher writes data to Google Pub/Sub based on the data format that you select. You can use the following data formats:

**Avro**

The stage writes records based on the Avro schema. You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. The Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in the Confluent Schema Registry by the schema ID or subject.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the stage to register the Avro schema with the Confluent Schema Registry. You can also optionally include the schema definition in the message. Omitting the schema definition can improve performance, but requires the appropriate schema management to avoid losing track of the schema associated with the data.

You can include the Avro schema in the output.

You can also compress data with an Avro-supported compression codec. When using Avro compression, avoid configuring any other compression properties in the stage.

**Binary**

The stage writes binary data to a single field in the record.

**Delimited**

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

**JSON**

The destination writes records as JSON data. You can use one of the following formats:

* Array - Each file includes a single array. In the array, each element is a JSON representation of each record.
* Multiple objects - Each file includes multiple JSON objects. Each object is a JSON representation of a record.

**Protobuf**

Writes one record in a message. Uses the user-defined message type and the definition of the message type in the descriptor file to generate the message.

**Record**

The destination writes records in the Record data format.

**Text**

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.

When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.

When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

**XML**

The destination creates a valid XML document for each record. The destination requires the record to have a single root field that contains the rest of the record data.&#x20;

The destination can include indentation to produce human-readable documents. It can also validate that the generated XML conforms to the specified schema definition. Records with invalid schemas are handled based on the error handling configured for the destination.
