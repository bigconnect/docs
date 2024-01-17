# Google Cloud Storage

The Google Cloud Storage destination writes data to objects in Google Cloud Storage. You can use other destinations to write to Google BigQuery, Google Bigtable, and Google Pub/Sub.&#x20;

The destination creates an object for each batch of data written to Google Cloud Storage.

With the Google Cloud Storage destination, you configure the bucket and common prefix to define where to write objects. You can use a partition prefix to specify the partition to write to. You can configure a prefix for the object name, and a time basis and data time zone for the stage. When using any data format except whole file, you can also configure a suffix for the object name and compress data with gzip before writing to Google Cloud Storage.

You also define the project ID and credentials to use when connecting to Google Cloud Storage.

The destination can generate events for an event stream.&#x20;

### Credentials <a href="#ariaid-title2" id="ariaid-title2"></a>

Before writing to Google Cloud Storage, the Google Cloud Storage destination must pass credentials to Google Cloud Storage.

You can provide credentials using one the following options:

* Google Cloud default credentials
* Credentials in a file
* Credentials in a stage property

### Partition Prefix <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use a partition prefix to organize objects by partitions. You can use the partition prefix to write to existing partitions or to create new partitions as needed. When a partition specified in the partition prefix does not exist, the destination creates the partition.

You can specify an exact partition name for the partition prefix, or you can use an expression that evaluates to a partition name.

For example, to write to partitions based on data in the Country field, you can use the following expression as the partition prefix: `${record:value('/Country')}`.

With this expression, the destination writes records to partitions based on the country data in the record, and creates partitions for countries that do not already have a partition.

If you use datetime variables in the expression, be sure to configure the time basis for the stage.

### Time Basis, Data Time Zone, and Time-Based Partition Prefixes <a href="#ariaid-title4" id="ariaid-title4"></a>

The time basis and the data time zone comprises the time used by the Google Cloud Storage destination to write records to a time-based partition prefix. When the configured partition prefix does not include time-based functions, you can ignore the time basis property.

A partition prefix has a time component when it includes datetime variables, such as `${YYYY()}` or `${DD()}`, or when it includes an expression that evaluates to a datetime value, such as `${record:value("/Timestamp")}.`

For details about datetime variables, see Datetime Variables.

You can use the following times as the time basis:

**Processing Time**

When you use processing time as the time basis, the destination performs writes based on the processing time and the configured partition prefix. The processing time is the time associated with the Data Pipelines instance running the pipeline, by default. You can specify a different time zone by configuring the Data Time Zone property. To use the processing time as the time basis, use the following expression:

```
${time:now()}
```

This is the default time basis.

**Record Time**

When you use the time associated with a record as the time basis, you specify a date field in the record. The destination writes data based on the datetimes associated with the records, adjusting for the value specified for the Data Time Zone property.

To use a time associated with the record, use an expression that calls a field and resolves to a datetime value, such as `${record:value("/Timestamp")}`.For example, say you define the Partition Prefix property using the following datetime variables:

```
logs-${YYYY()}-${MM()}-${DD()}
```

If you use the time of processing as the time basis, the destination writes records to partitions based on when it processes each record. If you use the time associated with the data, such as a transaction timestamp, then the destination writes records to the partitions based on that timestamp. If a partition does not exist, the destination creates the needed partition.

### Object Names <a href="#ariaid-title5" id="ariaid-title5"></a>

The Google Cloud Storage destination creates an object, or file, for each batch of data written. Objects generally use the following naming convention:

```
<prefix>-<UUID>
```

You configure the object name prefix. For example: `sdc-c9a2db16-b5d0-44cb-b3f5-d0781cced760`.

You can optionally configure an object name suffix for all data formats except whole file. When you configure a suffix, it is added to the object name after a period, as follows:

```
<prefix>-<UUID>.<optional suffix>
```

For example: `sdc-c9a2db16-b5d0-44cb-b3f5-d0781cced760.txt`.

#### Whole File Names <a href="#ariaid-title6" id="ariaid-title6"></a>

When you use the whole file data format, the object name prefix is optional. Whole files are named based on the File Name Expression whole file property. If you configure an object name prefix, whole files are named as follows:

```
<prefix>-<results of the file name expression>
```

### Event Generation <a href="#ariaid-title7" id="ariaid-title7"></a>

The Google Cloud Storage destination can generate events that you can use in an event stream. When you enable event generation, Google Cloud Storage generates event records each time the destination completes writing to an object or completes streaming a whole file.

Google Cloud Storage events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title8" id="ariaid-title8"></a>

Event records generated by the Google Cloud Storage destination have the following event-related record header attributes. Record header attributes are stored as String values:

| Record Header Attribute       | Description                                                                                                                                                                                                                                            |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>GCS Object Written - Generated when the destination completes writing to an object.</li><li>wholeFileProcessed - Generated when the destination completes streaming a whole file.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                           |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                      |

The Google Cloud Storage destination can generate the following types of event records:Object writtenThe destination generates an object written event record when it completes writing to an object.Object written event records have the `sdc.event.type` record header attribute set to `GCS Object Written` and include the following fields:

| Field       | Description                              |
| ----------- | ---------------------------------------- |
| bucket      | Bucket where the object is located.      |
| objectKey   | Object key name that was written.        |
| recordCount | Number of records written to the object. |

Whole file processedThe destination generates an event record when it completes streaming a whole file. Whole file event records have the `sdc.event.type` record header attribute set to `wholeFileProcessed` and include the following fields:

| Field             | Description                                                                                                                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| sourceFileInfo    | <p>A map of attributes about the original whole file that was processed.</p><p>The attribute names depend on the information provided by the origin system.</p>                                                                            |
| targetFileInfo    | <p>A map of attributes about the whole file written to the destination system. The attributes include:</p><ul><li>bucket - The bucket where the whole file is written.</li><li>objectKey - The object key name that was written.</li></ul> |
| checksum          | <p>Checksum generated for the written file.</p><p>Included only when you configure the destination to include checksums in the event record.</p>                                                                                           |
| checksumAlgorithm | <p>Algorithm used to generate the checksum.</p><p>Included only when you configure the destination to include checksums in the event record.</p>                                                                                           |

### Data Formats <a href="#ariaid-title9" id="ariaid-title9"></a>

The Google Cloud Storage destination writes data to Google Cloud Storage based on the data format that you select. You can use the following data formats:

**Avro**

The destination writes records based on the Avro schema. You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in Confluent Schema Registry by the schema ID or subject.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the destination to register the Avro schema with Confluent Schema Registry.

The destination includes the schema definition in each file.You can compress data with an Avro-supported compression codec. When using Avro compression, avoid using other compression properties in the destination.

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

Writes a batch of messages in each file.Uses the user-defined message type and the definition of the message type in the descriptor file to generate the messages in the file.

Record

The destination writes records in the Record data format.

**Text**

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.

When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

**Whole File**

Streams whole files to the destination system. The destination writes the data to the file and location defined in the stage. If a file of the same name already exists, you can configure the destination to overwrite the existing file or send the current file to error.

Written files use the default permissions defined in the destination system.You can configure the destination to generate a checksum for the written file and pass checksum information to the destination system in an event record.
