# Redis

The Redis destination writes data to Redis.&#x20;

When you configure the destination, you specify the mode that the destination uses to process records. You also specify properties to connect to Redis, including the URI of the Redis server.

In batch mode, the Redis destination can use CRUD operations defined in the sdc.operation.type record header attribute to process data. When CRUD operations are not specified in a record, the destination treats it like an Upsert record.&#x20;

### Mode <a href="#ariaid-title2" id="ariaid-title2"></a>

The Redis destination writes data to Redis using one of the following modes:

**Batch mode**

In batch mode, the destination writes data to Redis key-value pairs. You configure each key-value pair by selecting the incoming fields to use as the key and the value. You also select the data type of the Redis value. You can configure the destination to write to multiple key-value pairs.For example, you are processing records that contain a String city field and a Map latitude\_longitude field. Let’s assume that one record contains the following data:

```
city: {String} "San Francisco"
latitude_longitude: {Map}
    latitude: {String} "37.7749"
    longitude: {String} "-122.4194"
```

You select the city field as the Redis key, and the latitude\_longitude field as the Redis value. Data Pipelines can convert the Map data type to the Redis Hash data type, so you select Hash for the data type of the Redis value, as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/RedisDestinationBatchMode.png)

When you run the pipeline, the Redis destination writes the key "San Francisco" to Redis with a Hash value containing the latitude and longitude.

**Publish mode**

In publish mode, the destination publishes data as messages to a Redis channel. You specify the channel to use and the data format of the messages. The publish mode pushes each record as one message to the specified Redis channel.

#### Data Types for Batch Mode <a href="#ariaid-title3" id="ariaid-title3"></a>

When you configure the destination for batch mode, you select the incoming fields to use as the Redis key and value. You also select the data type of the Redis value. If needed, the Redis destination converts the Data Pipelines data type of the incoming value field to the selected Redis data type.

When appropriate, use a Field Type Converter processor earlier in the pipeline to convert data types.

The following table lists the Data Pipelines data types that can be converted to Redis data types:

| Data Pipelines Data Type | Redis Data Type |
| ------------------------ | --------------- |
| String                   | String          |
| List                     | List or Set     |
| Map                      | Hash            |

Note: The remaining Data Pipelines and Redis data types are not supported.

#### Data Formats for Publish Mode <a href="#ariaid-title4" id="ariaid-title4"></a>

When you configure the destination for publish mode, the destination publishes messages to Redis based on the data format that you select. You can use the following data formats:

Avro

The stage writes records based on the Avro schema. You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. The Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in the Confluent Schema Registry by the schema ID or subject.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the stage to register the Avro schema with the Confluent Schema Registry. You can also optionally include the schema definition in the message. Omitting the schema definition can improve performance, but requires the appropriate schema management to avoid losing track of the schema associated with the data.

You can include the Avro schema in the output.You can also compress data with an Avro-supported compression codec. When using Avro compression, avoid configuring any other compression properties in the stage.

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

Record

The destination writes records in the Record data format.

Text

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.

You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

### Define the CRUD Operation <a href="#ariaid-title5" id="ariaid-title5"></a>

When using the Redis destination in batch mode, you can use CRUD operations to write to Redis. To use CRUD operations, define the CRUD operation record header attribute for each record earlier in the pipeline. Records without the attribute defined are treated as Upsert: new records are written and existing records are updated.

To use CRUD operations to write records, set the following CRUD operation record header attribute:sdc.operation.typeWhen defined, the Redis destination uses the CRUD operation in the sdc.operation.type record header attribute when writing to Redis. The destination supports the following values for the sdc.operation.type attribute:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE
* 4 for UPSERT

If your pipeline includes a CRUD-enabled origin that processes changed data, the destination simply reads the operation type from the `sdc.operation.type` header attribute that the origin generates. If your pipeline uses a non-CDC origin, you can use the Expression Evaluator or a scripting processor to define the record header attribute.&#x20;

