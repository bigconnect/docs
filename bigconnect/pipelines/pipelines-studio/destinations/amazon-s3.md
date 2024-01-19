# Amazon S3

The Amazon S3 destination writes data to Amazon S3.&#x20;

To write data to an Amazon Kinesis Firehose delivery system, use the Kinesis Firehose destination. To write data to Amazon Kinesis Streams, use the Kinesis Producer destination.

With the Amazon S3 destination, you configure the region, bucket, and common prefix to define where to write objects. You can use a partition prefix to specify the S3 partition to write to. You can also configure a prefix and suffix for the object name, and a time basis and data time zone for the stage.

You configure the authentication method that the destination uses to connect to Amazon S3.

The Amazon S3 destination can write data asynchronously to improve performance when writing to multiple prefixes. You can configure advanced properties to tune performance.

You can configure the destination to use Amazon Web Services server-side encryption to protect the data written to Amazon S3. You can also use a proxy user and compress data with gzip when writing to Amazon S3.

The Amazon S3 destination creates an object for each batch of data written to Amazon S3.

The destination can generate events for an event stream.&#x20;

### Authentication Method <a href="#ariaid-title2" id="ariaid-title2"></a>

You can configure the Amazon S3 destination to authenticate with Amazon Web Services (AWS) using an instance profile or AWS access keys. When accessing a public bucket, you can connect anonymously using no authentication.

For more information about the authentication methods and details on how to configure each method, see Security in Amazon Stages.

### Bucket <a href="#ariaid-title3" id="ariaid-title3"></a>

When you configure the bucket where records should be written, you can specify an exact bucket name or you can use an expression that evaluates to a bucket name.

Note: The bucket name must be DNS compliant. For more information about bucket naming conventions, see the [Amazon S3 documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html).

For example, to write to buckets based on data in the Type field, you can use the following expression to define the bucket: `${record:value('/Type)}`.

With this expression, the destination writes records to buckets based on the data in the Type field. If an expression evaluates to a bucket that does not exist, the destination handles the record based on the error handling configured in the stage.

If you use datetime variables in the expression, be sure to configure the time basis for the stage.

### Partition Prefix <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use a partition prefix to organize objects by partitions. You can use the partition prefix to write to existing partitions or to create new partitions as needed. When a partition specified in the partition prefix does not exist, the destination creates the partition.

You can specify an exact partition name for the partition prefix, or you can use an expression that evaluates to a partition name.

For example, to write to partitions based on data in the Country field, you can use the following expression as the partition prefix: `${record:value('/Country')}`.

With this expression, the destination writes records to partitions based on the country data in the record, and creates partitions for countries that do not already have a partition.

If you use datetime variables in the expression, be sure to configure the time basis for the stage. You might also need to configure the Data Time Zone property.

### Time Basis and Data Time Zone for Time-Based Buckets and Partition Prefixes <a href="#ariaid-title5" id="ariaid-title5"></a>

The time basis and the data time zone comprises the time used by the Amazon S3 destination to write records to a time-based bucket or partition prefix. When the configured bucket or partition prefix does not include time-based functions, you can ignore the time basis property.

A bucket or partition prefix has a time component when it includes datetime variables, such as `${YYYY()}` or `${DD()}`, or when it includes an expression that evaluates to a datetime value, such as `${record:valueOrDefault("/Timestamp")}.`

For details about datetime variables, see Datetime Variables.

You can use the following times as the time basis:Processing TimeWhen you use processing time as the time basis, the destination performs writes based on the processing time and the configured bucket and partition prefix. The processing time is the time associated with the Data Pipelines running the pipeline, by default. You can specify a different time zone by configuring the Data Time Zone property. To use the processing time as the time basis, use the following expression:

```
${time:now()}
```

This is the default time basis.Record TimeWhen you use the time associated with a record as the time basis, you specify a date field in the record. The destination writes data based on the datetimes associated with the records, adjusting for the value specified for the Data Time Zone property.To use a time associated with the record, use an expression that calls a field and resolves to a datetime value, such as `${record:value("/Timestamp")}`.For example, say you define the Partition Prefix property using the following datetime variables:

```
logs-${YYYY()}-${MM()}-${DD()}
```

If you use the time of processing as the time basis, the destination writes records to partitions based on when it processes each record. If you use the time associated with the data, such as a transaction timestamp, then the destination writes records to the partitions based on that timestamp. If a partition does not exist, the destination creates the needed partition.

Or, say you define the Bucket property as follows:

```
${YYYY()}-${MM()}
```

If you use the time of processing as the time basis, the destination writes records to buckets based on when it processes each record. If you use the time associated with the data, such as a transaction timestamp, then the destination writes records to the buckets based on that timestamp. If a bucket does not exist, the destination handles the record based on the error record handling configured for the stage.

### Object Names <a href="#ariaid-title6" id="ariaid-title6"></a>

The Amazon S3 destination creates an object, or file, for each batch of data written. Objects generally use the following naming convention:

```
<prefix>-<UTC timestamp>-<counter>
```

For example: `sdc-1462405014177-1`.

You configure the object name prefix.

The UTC timestamp is the time when the object is created, to the millisecond. The counter is used when multiple objects are created in the same millisecond.

You can optionally configure an object name suffix for all data formats except whole file. When you configure a suffix, it is added to the object name after a period, as follows:

```
<prefix>-<UTC timestamp>-<counter>.<optional suffix>
```

For example: `sdc-1462405014177-1.txt`.

#### Whole File Names <a href="#ariaid-title7" id="ariaid-title7"></a>

When you use the whole file data format, the object name prefix is optional. Whole files are named based on the File Name Expression whole file property. If you configure an object name prefix, whole files are named as follows:

```
<prefix>-<results of the file name expression>
```

### Event Generation <a href="#ariaid-title8" id="ariaid-title8"></a>

The Amazon S3 destination can generate events that you can use in an event stream. When you enable event generation, the Amazon S3 destination generates event records each time after writing to an object or streaming a whole file.

Amazon S3 events can be used in any logical way. For example:

* With the Amazon S3 executor to add metadata to closed objects or whole files after receiving an event.
* With the Spark executor to run a Spark application after receiving an event.
* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title9" id="ariaid-title9"></a>

Event records generated by Amazon S3 destination have the following event-related record header attributes. Record header attributes are stored as String values:

| Record Header Attribute       | Description                                                                                                                                                                                                                                           |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>S3 Object Written - Generated when the destination completes writing to an object.</li><li>wholeFileProcessed - Generated when the destination completes streaming a whole file.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                          |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                     |

The Amazon S3 destination can generate the following types of event records:Object writtenThe destination generates an object written event record when it completes writing to an object.Object written event records have the `sdc.event.type` record header attribute set to `S3 Object Written` and include the following fields:

| Field       | Description                              |
| ----------- | ---------------------------------------- |
| bucket      | Bucket where the object is located.      |
| objectKey   | Object key name that was written.        |
| recordCount | Number of records written to the object. |

The destination generates an event record when it completes streaming a whole file. Whole file event records have the `sdc.event.type` record header attribute set to `wholeFileProcessed` and include the following fields:

| Field             | Description                                                                                                                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| sourceFileInfo    | <p>A map of attributes about the original whole file that was processed.</p><p>The attribute names depend on the information provided by the origin system.</p>                                                                            |
| targetFileInfo    | <p>A map of attributes about the whole file written to the destination system. The attributes include:</p><ul><li>bucket - The bucket where the whole file is written.</li><li>objectKey - The object key name that was written.</li></ul> |
| checksum          | <p>Checksum generated for the written file.</p><p>Included only when you configure the destination to include checksums in the event record.</p>                                                                                           |
| checksumAlgorithm | <p>Algorithm used to generate the checksum.</p><p>Included only when you configure the destination to include checksums in the event record.</p>                                                                                           |

### Server-Side Encryption <a href="#ariaid-title10" id="ariaid-title10"></a>

You can configure the destination to use Amazon Web Services server-side encryption (SSE) to protect data written to Amazon S3. When configured for server-side encryption, the destination passes required server-side encryption configuration values to Amazon S3. Amazon S3 uses the values to encrypt the data as it is written to Amazon S3.

When you enable server-side encryption for the destination, you select one of the following ways that Amazon S3 manages the encryption keys:

Amazon S3-Managed Encryption Keys (SSE-S3)

When you use server-side encryption with Amazon S3-managed keys, Amazon S3 manages the encryption keys for you.

AWS KMS-Managed Encryption Keys (SSE-KMS)

When you use server-side encryption with AWS Key Management Service (KMS), you specify the Amazon resource name (ARN) of the AWS KMS master encryption key that you want to use. You can also specify key-value pairs to use for the encryption context.

Customer-Provided Encryption Keys (SSE-C)

When you use server-side encryption with customer-provided keys, you specify the following information:

* Base64 encoded 256-bit encryption key
* Base64 encoded 128-bit MD5 digest of the encryption key using RFC 1321

For more information about using server-side encryption to protect data in Amazon S3, see the Amazon S3 documentation.

### Data Formats <a href="#ariaid-title11" id="ariaid-title11"></a>

The Amazon S3 destination writes data to Amazon S3 based on the data format that you select.

The Amazon S3 destination processes data formats as follows:

Avro

The destination writes records based on the Avro schema. You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in Confluent Schema Registry by the schema ID or subject.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the destination to register the Avro schema with Confluent Schema Registry.

The destination includes the schema definition in each file.You can compress data with an Avro-supported compression codec. When using Avro compression, avoid using other compression properties in the destination.

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

Writes a batch of messages in each file.Uses the user-defined message type and the definition of the message type in the descriptor file to generate the messages in the file.

Record

The destination writes records in the Record data format.

Text

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

Whole File

Streams whole files to the destination system. The destination writes the data to the file and location defined in the stage. If a file of the same name already exists, you can configure the destination to overwrite the existing file or send the current file to error.Written files use the default permissions defined in the destination system.You can configure the destination to generate a checksum for the written file and pass checksum information to the destination system in an event record.
