# Kinesis connector

[Kinesis](https://aws.amazon.com/kinesis/) is Amazon’s fully managed cloud-based service for real-time processing of large, distributed data streams.

This connector allows the use of Kinesis streams as tables in Trino, such that each data-blob/message in a Kinesis stream is presented as a row in Trino. A flexible table mapping approach lets us treat fields of the messages as columns in the table.

Under the hood, a Kinesis [shard iterator](https://docs.aws.amazon.com/kinesis/latest/APIReference/API\_GetShardIterator.html) is used to retrieve the records, along with a series of [GetRecords](https://docs.aws.amazon.com/kinesis/latest/APIReference/API\_GetRecords.html) calls. The shard iterator starts by default 24 hours before the current time, and works its way forward. To be able to query a stream, table mappings are needed. These table definitions can be stored on Amazon S3 (preferred), or stored in a local directory on each Trino node.

This connector is a **read-only** connector. It can only fetch data from Kinesis streams, but cannot create streams or push data into existing streams.

To configure the Kinesis connector, create a catalog properties file `etc/catalog/example.properties` with the following contents, replacing the properties as appropriate:

```
connector.name=kinesis
kinesis.access-key=XXXXXX
kinesis.secret-key=XXXXXX
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Configuration properties[#](https://trino.io/docs/current/connector/kinesis.html#configuration-properties) <a href="#configuration-properties" id="configuration-properties"></a>

The following configuration properties are available:

| Property name                                | Description                                                           |
| -------------------------------------------- | --------------------------------------------------------------------- |
| `kinesis.access-key`                         | Access key to AWS account or blank to use default provider chain      |
| `kinesis.secret-key`                         | Secret key to AWS account or blank to use default provider chain      |
| `kinesis.aws-region`                         | AWS region to be used to read kinesis stream from                     |
| `kinesis.default-schema`                     | Default schema name for tables                                        |
| `kinesis.table-description-location`         | Directory containing table description files                          |
| `kinesis.table-description-refresh-interval` | How often to get the table description from S3                        |
| `kinesis.hide-internal-columns`              | Controls whether internal columns are part of the table schema or not |
| `kinesis.batch-size`                         | Maximum number of records to return in one batch                      |
| `kinesis.fetch-attempts`                     | Read attempts made when no records returned and not caught up         |
| `kinesis.max-batches`                        | Maximum batches to read from Kinesis in one single query              |
| `kinesis.sleep-time`                         | Time for thread to sleep waiting to make next attempt to fetch batch  |
| `kinesis.iterator-from-timestamp`            | Begin iterating from a given timestamp instead of the trim horizon    |
| `kinesis.iterator-offset-seconds`            | Number of seconds before current time to start iterating              |

#### `kinesis.access-key`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-access-key) <a href="#kinesis-access-key" id="kinesis-access-key"></a>

Defines the access key ID for AWS root account or IAM roles, which is used to sign programmatic requests to AWS Kinesis.

This property is optional; if not defined, the connector tries to follow `Default-Credential-Provider-Chain` provided by AWS in the following order:

* Environment Variable: Load credentials from environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.
* Java System Variable: Load from java system as `aws.accessKeyId` and `aws.secretKey`.
* Profile Credentials File: Load from file typically located at `~/.aws/credentials`.
* Instance profile credentials: These credentials can be used on EC2 instances, and are delivered through the Amazon EC2 metadata service.

#### `kinesis.secret-key`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-secret-key) <a href="#kinesis-secret-key" id="kinesis-secret-key"></a>

Defines the secret key for AWS root account or IAM roles, which together with Access Key ID, is used to sign programmatic requests to AWS Kinesis.

This property is optional; if not defined, connector will try to follow `Default-Credential-Provider-Chain` same as above.

#### `kinesis.aws-region`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-aws-region) <a href="#kinesis-aws-region" id="kinesis-aws-region"></a>

Defines AWS Kinesis regional endpoint. Selecting appropriate region may reduce latency in fetching data.

This field is optional; The default region is `us-east-1` referring to end point ‘kinesis.us-east-1.amazonaws.com’.

See [Kinesis Data Streams regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#ak\_region) for a current list of available regions.

#### `kinesis.default-schema`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-default-schema) <a href="#kinesis-default-schema" id="kinesis-default-schema"></a>

Defines the schema which contains all tables that were defined without a qualifying schema name.

This property is optional; the default is `default`.

#### `kinesis.table-description-location`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-table-description-location) <a href="#kinesis-table-description-location" id="kinesis-table-description-location"></a>

References an S3 URL or a folder within Trino deployment that holds one or more JSON files ending with `.json`, which contain table description files. The S3 bucket and folder will be checked every 10 minutes for updates and changed files.

This property is optional; the default is `etc/kinesis`.

#### `kinesis.table-description-refresh-interval`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-table-description-refresh-interval) <a href="#kinesis-table-description-refresh-interval" id="kinesis-table-description-refresh-interval"></a>

This property controls how often the table description is refreshed from S3.

This property is optional; the default is `10m`.

#### `kinesis.batch-size`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-batch-size) <a href="#kinesis-batch-size" id="kinesis-batch-size"></a>

Defines the maximum number of records to return in one request to Kinesis Streams. Maximum limit is `10000` records.

This field is optional; the default value is `10000`.

#### `kinesis.max-batches`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-max-batches) <a href="#kinesis-max-batches" id="kinesis-max-batches"></a>

The maximum number of batches to read in a single query. The default value is `1000`.

#### `kinesis.fetch-attempts`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-fetch-attempts) <a href="#kinesis-fetch-attempts" id="kinesis-fetch-attempts"></a>

Defines the number of attempts made to read a batch from Kinesis Streams, when no records are returned and the _millis behind latest_ parameter shows we are not yet caught up. When records are returned no additional attempts are necessary. `GetRecords` has been observed to return no records even though the shard is not empty. That is why multiple attempts need to be made.

This field is optional; the default value is `2`.

#### `kinesis.sleep-time`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-sleep-time) <a href="#kinesis-sleep-time" id="kinesis-sleep-time"></a>

Defines the duration for which a thread needs to sleep between `kinesis.fetch-attempts` made to fetch data.

This field is optional; the default value is `1000ms`.

#### `kinesis.iterator-from-timestamp`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-iterator-from-timestamp) <a href="#kinesis-iterator-from-timestamp" id="kinesis-iterator-from-timestamp"></a>

Use an initial shard iterator type of `AT_TIMESTAMP` starting `kinesis.iterator-offset-seconds` before the current time. When this is false, an iterator type of `TRIM_HORIZON` is used, meaning it starts from the oldest record in the stream.

The default is true.

#### `kinesis.iterator-offset-seconds`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-iterator-offset-seconds) <a href="#kinesis-iterator-offset-seconds" id="kinesis-iterator-offset-seconds"></a>

When `kinesis.iterator-from-timestamp` is true, the shard iterator starts at `kinesis.iterator-offset-seconds` before the current time.

The default is `86400` seconds (24 hours).

#### `kinesis.hide-internal-columns`[#](https://trino.io/docs/current/connector/kinesis.html#kinesis-hide-internal-columns) <a href="#kinesis-hide-internal-columns" id="kinesis-hide-internal-columns"></a>

In addition to the data columns defined in a table description file, the connector maintains a number of additional columns for each table. If these columns are hidden, they can still be used in queries, but they do not show up in `DESCRIBE <table-name>` or `SELECT *`.

This property is optional; the default is true.

### Internal columns[#](https://trino.io/docs/current/connector/kinesis.html#internal-columns) <a href="#internal-columns" id="internal-columns"></a>

For each defined table, the connector maintains the following columns:

| Column name          | Type        | Description                                                                                                                                                                         |
| -------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `_shard_id`          | `VARCHAR`   | ID of the Kinesis stream shard which contains this row.                                                                                                                             |
| `_shard_sequence_id` | `VARCHAR`   | Sequence id within the Kinesis shard for this row.                                                                                                                                  |
| `_segment_start`     | `BIGINT`    | Lowest offset in the segment (inclusive) which contains this row. This offset is partition specific.                                                                                |
| `_segment_end`       | `BIGINT`    | Highest offset in the segment (exclusive) which contains this row. The offset is partition specific. This is the same value as `_segment_start` of the next segment (if it exists). |
| `_segment_count`     | `BIGINT`    | Running count for the current row within the segment. For an uncompacted topic, `_segment_start + _segment_count` is equal to `_partition_offset`.                                  |
| `_message_valid`     | `BOOLEAN`   | True if the decoder could decode the message successfully for this row. When false, data columns mapped from the message should be treated as invalid.                              |
| `_message`           | `VARCHAR`   | Message bytes as an UTF-8 encoded string. This is only useful for a text topic.                                                                                                     |
| `_message_length`    | `BIGINT`    | Number of bytes in the message.                                                                                                                                                     |
| `_message_timestamp` | `TIMESTAMP` | Approximate arrival time of the message (milliseconds granularity).                                                                                                                 |
| `_key`               | `VARCHAR`   | Key bytes as an UTF-8 encoded string. This is only useful for textual keys.                                                                                                         |
| `_partition_key`     | `VARCHAR`   | Partition Key bytes as a UTF-8 encoded string.                                                                                                                                      |

For tables without a table definition file, the `_message_valid` column is always `true`.

### Table definition[#](https://trino.io/docs/current/connector/kinesis.html#table-definition) <a href="#table-definition" id="table-definition"></a>

A table definition file consists of a JSON definition for a table, which corresponds to one stream in Kinesis. The name of the file can be arbitrary but must end in `.json`. The structure of the table definition is as follows:

```
{
      "tableName": ...,
      "schemaName": ...,
      "streamName": ...,
      "message": {
          "dataFormat": ...,
          "fields": [
              ...
         ]
      }
  }
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| Field        | Required | Type        | Description                                                                   |
| ------------ | -------- | ----------- | ----------------------------------------------------------------------------- |
| `tableName`  | required | string      | Trino table name defined by this file.                                        |
| `schemaName` | optional | string      | Schema which contains the table. If omitted, the default schema name is used. |
| `streamName` | required | string      | Name of the Kinesis Stream that is mapped                                     |
| `message`    | optional | JSON object | Field definitions for data columns mapped to the message itself.              |

Every message in a Kinesis stream can be decoded using the definition provided in the message object. The JSON object message in the table definition contains two fields:

| Field        | Required | Type       | Description                                                                                 |
| ------------ | -------- | ---------- | ------------------------------------------------------------------------------------------- |
| `dataFormat` | required | string     | Selects the decoder for this group of fields.                                               |
| `fields`     | required | JSON array | A list of field definitions. Each field definition creates a new column in the Trino table. |

Each field definition is a JSON object. At a minimum, a name, type, and mapping must be provided. The overall structure looks like this:

```
{
    "name": ...,
    "type": ...,
    "dataFormat": ...,
    "mapping": ...,
    "formatHint": ...,
    "hidden": ...,
    "comment": ...
}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

| Field        | Required | Type    | Description                                                                                                          |
| ------------ | -------- | ------- | -------------------------------------------------------------------------------------------------------------------- |
| `name`       | required | string  | Name of the column in the Trino table.                                                                               |
| `type`       | required | string  | Trino type of the column.                                                                                            |
| `dataFormat` | optional | string  | Selects the column decoder for this field. Defaults to the default decoder for this row data format and column type. |
| `mapping`    | optional | string  | Mapping information for the column. This is decoder specific – see below.                                            |
| `formatHint` | optional | string  | Sets a column specific format hint to the column decoder.                                                            |
| `hidden`     | optional | boolean | Hides the column from `DESCRIBE <table name>` and `SELECT *`. Defaults to `false`.                                   |
| `comment`    | optional | string  | Adds a column comment which is shown with `DESCRIBE <table name>`.                                                   |

The name field is exposed to Trino as the column name, while the mapping field is the portion of the message that gets mapped to that column. For JSON object messages, this refers to the field name of an object, and can be a path that drills into the object structure of the message. Additionally, you can map a field of the JSON object to a string column type, and if it is a more complex type (JSON array or JSON object) then the JSON itself becomes the field value.

There is no limit on field descriptions for either key or message.

### Type mapping[#](https://trino.io/docs/current/connector/kinesis.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

Because Trino and Kinesis each support types that the other does not, this connector [maps some types](https://trino.io/docs/current/language/types.html#type-mapping-overview) when reading data. Type mapping depends on the RAW, CSV, JSON, and AVRO file formats.

#### Row decoding[#](https://trino.io/docs/current/connector/kinesis.html#row-decoding) <a href="#row-decoding" id="row-decoding"></a>

A decoder is used to map data to table columns.

The connector contains the following decoders:

* `raw`: Message is not interpreted; ranges of raw message bytes are mapped to table columns.
* `csv`: Message is interpreted as comma separated message, and fields are mapped to table columns.
* `json`: Message is parsed as JSON, and JSON fields are mapped to table columns.
* `avro`: Message is parsed based on an Avro schema, and Avro fields are mapped to table columns.

Note

If no table definition file exists for a table, the `dummy` decoder is used, which does not expose any columns.

**Raw decoder**[**#**](https://trino.io/docs/current/connector/kinesis.html#raw-decoder)

The raw decoder supports reading of raw byte-based values from message or key, and converting it into Trino columns.

For fields, the following attributes are supported:

* `dataFormat` - Selects the width of the data type converted.
* `type` - Trino data type. See the following table for a list of supported data types.
* `mapping` - `<start>[:<end>]` - Start and end position of bytes to convert (optional).

The `dataFormat` attribute selects the number of bytes converted. If absent, `BYTE` is assumed. All values are signed.

Supported values are:

* `BYTE` - one byte
* `SHORT` - two bytes (big-endian)
* `INT` - four bytes (big-endian)
* `LONG` - eight bytes (big-endian)
* `FLOAT` - four bytes (IEEE 754 format)
* `DOUBLE` - eight bytes (IEEE 754 format)

The `type` attribute defines the Trino data type on which the value is mapped.

Depending on the Trino type assigned to a column, different values of dataFormat can be used:

| Trino data type          | Allowed `dataFormat` values    |
| ------------------------ | ------------------------------ |
| `BIGINT`                 | `BYTE`, `SHORT`, `INT`, `LONG` |
| `INTEGER`                | `BYTE`, `SHORT`, `INT`         |
| `SMALLINT`               | `BYTE`, `SHORT`                |
| `DOUBLE`                 | `DOUBLE`, `FLOAT`              |
| `BOOLEAN`                | `BYTE`, `SHORT`, `INT`, `LONG` |
| `VARCHAR` / `VARCHAR(x)` | `BYTE`                         |

No other types are supported.

The `mapping` attribute specifies the range of the bytes in a key or message used for decoding. It can be one or two numbers separated by a colon (`<start>[:<end>]`).

If only a start position is given:

* For fixed width types, the column uses the appropriate number of bytes for the specified `dataFormat` (see above).
* When the `VARCHAR` value is decoded, all bytes from the start position to the end of the message is used.

If start and end position are given:

* For fixed width types, the size must be equal to the number of bytes used by specified `dataFormat`.
* For the `VARCHAR` data type all bytes between start (inclusive) and end (exclusive) are used.

If no `mapping` attribute is specified, it is equivalent to setting the start position to 0 and leaving the end position undefined.

The decoding scheme of numeric data types (`BIGINT`, `INTEGER`, `SMALLINT`, `TINYINT`, `DOUBLE`) is straightforward. A sequence of bytes is read from input message and decoded according to either:

* big-endian encoding (for integer types)
* IEEE 754 format for (for `DOUBLE`).

The length of a decoded byte sequence is implied by the `dataFormat`.

For the `VARCHAR` data type, a sequence of bytes is interpreted according to UTF-8 encoding.

**CSV decoder**[**#**](https://trino.io/docs/current/connector/kinesis.html#csv-decoder)

The CSV decoder converts the bytes representing a message or key into a string using UTF-8 encoding, and interprets the result as a link of comma-separated values.

For fields, the `type` and `mapping` attributes must be defined:

* `type` - Trino data type. See the following table for a list of supported data types.
* `mapping` - The index of the field in the CSV record.

The `dataFormat` and `formatHint` attributes are not supported and must be omitted.

| Trino data type                            | Decoding rules                                                                     |
| ------------------------------------------ | ---------------------------------------------------------------------------------- |
| `BIGINT`, `INTEGER`, `SMALLINT`, `TINYINT` | Decoded using Java `Long.parseLong()`                                              |
| `DOUBLE`                                   | Decoded using Java `Double.parseDouble()`                                          |
| `BOOLEAN`                                  | “true” character sequence maps to `true`. Other character sequences map to `false` |
| `VARCHAR` / `VARCHAR(x)`                   | Used as is                                                                         |

No other types are supported.

**JSON decoder**[**#**](https://trino.io/docs/current/connector/kinesis.html#json-decoder)

The JSON decoder converts the bytes representing a message or key into Javascript Object Notaion (JSON) according to [**RFC 4627**](https://datatracker.ietf.org/doc/html/rfc4627.html). The message or key must convert into a JSON object, not an array or simple type.

For fields, the following attributes are supported:

* `type` - Trino data type of column.
* `dataFormat` - Field decoder to be used for column.
* `mapping` - Slash-separated list of field names to select a field from the JSON object.
* `formatHint` - Only for `custom-date-time`.

The JSON decoder supports multiple field decoders with `_default` being used for standard table columns and a number of decoders for date and time-based types.

The following table lists Trino data types, which can be used in `type` and matching field decoders, and specified via `dataFormat` attribute:

| Trino data type                                                                          | Allowed `dataFormat` values                                                                 |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `BIGINT`, `INTEGER`, `SMALLINT`, `TINYINT`, `DOUBLE`, `BOOLEAN`, `VARCHAR`, `VARCHAR(x)` | Default field decoder (omitted `dataFormat` attribute)                                      |
| `DATE`                                                                                   | `custom-date-time`, `iso8601`                                                               |
| `TIME`                                                                                   | `custom-date-time`, `iso8601`, `milliseconds-since-epoch`, `seconds-since-epoch`            |
| `TIME WITH TIME ZONE`                                                                    | `custom-date-time`, `iso8601`                                                               |
| `TIMESTAMP`                                                                              | `custom-date-time`, `iso8601`, `rfc2822`, `milliseconds-since-epoch`, `seconds-since-epoch` |
| `TIMESTAMP WITH TIME ZONE`                                                               | `custom-date-time`, `iso8601`, `rfc2822`, `milliseconds-since-epoch`, `seconds-since-epoch` |

No other types are supported.

**DEFAULT FIELD DECODER**[**#**](https://trino.io/docs/current/connector/kinesis.html#default-field-decoder)

This is the standard field decoder. It supports all the Trino physical data types. A field value is transformed under JSON conversion rules into boolean, long, double, or string values. This decoder should be used for columns that are not date or time based.

**DATE AND TIME DECODERS**[**#**](https://trino.io/docs/current/connector/kinesis.html#date-and-time-decoders)

To convert values from JSON objects to Trino `DATE`, `TIME`, `TIME WITH TIME ZONE`, `TIMESTAMP` or `TIMESTAMP WITH TIME ZONE` columns, select special decoders using the `dataFormat` attribute of a field definition.

* `iso8601` - Text based, parses a text field as an ISO 8601 timestamp.
* `rfc2822` - Text based, parses a text field as an [**RFC 2822**](https://datatracker.ietf.org/doc/html/rfc2822.html) timestamp.
* `custom-date-time` - Text based, parses a text field according to Joda format pattern specified via `formatHint` attribute. The format pattern should conform to [https://www.joda.org/joda-time/apidocs/org/joda/time/format/DateTimeFormat.html](https://www.joda.org/joda-time/apidocs/org/joda/time/format/DateTimeFormat.html).
* `milliseconds-since-epoch` - Number-based, interprets a text or number as number of milliseconds since the epoch.
* `seconds-since-epoch` - Number-based, interprets a text or number as number of milliseconds since the epoch.

For `TIMESTAMP WITH TIME ZONE` and `TIME WITH TIME ZONE` data types, if timezone information is present in decoded value, it is used as a Trino value. Otherwise, the result time zone is set to `UTC`.

**Avro decoder**[**#**](https://trino.io/docs/current/connector/kinesis.html#avro-decoder)

The Avro decoder converts the bytes representing a message or key in Avro format based on a schema. The message must have the Avro schema embedded. Trino does not support schemaless Avro decoding.

The `dataSchema` must be defined for any key or message using `Avro` decoder. `Avro` decoder should point to the location of a valid Avro schema file of the message which must be decoded. This location can be a remote web server (e.g.: `dataSchema: 'http://example.org/schema/avro_data.avsc'`) or local file system(e.g.: `dataSchema: '/usr/local/schema/avro_data.avsc'`). The decoder fails if this location is not accessible from the Trino cluster.

The following attributes are supported:

* `name` - Name of the column in the Trino table.
* `type` - Trino data type of column.
* `mapping` - A slash-separated list of field names to select a field from the Avro schema. If the field specified in `mapping` does not exist in the original Avro schema, a read operation returns `NULL`.

The following table lists the supported Trino types that can be used in `type` for the equivalent Avro field types:

| Trino data type          | Allowed Avro data type |
| ------------------------ | ---------------------- |
| `BIGINT`                 | `INT`, `LONG`          |
| `DOUBLE`                 | `DOUBLE`, `FLOAT`      |
| `BOOLEAN`                | `BOOLEAN`              |
| `VARCHAR` / `VARCHAR(x)` | `STRING`               |
| `VARBINARY`              | `FIXED`, `BYTES`       |
| `ARRAY`                  | `ARRAY`                |
| `MAP`                    | `MAP`                  |

No other types are supported.

**AVRO SCHEMA EVOLUTION**[**#**](https://trino.io/docs/current/connector/kinesis.html#avro-schema-evolution)

The Avro decoder supports schema evolution with backward compatibility. With backward compatibility, a newer schema can be used to read Avro data created with an older schema. Any change in the Avro schema must also be reflected in Trino’s topic definition file. Newly added or renamed fields must have a default value in the Avro schema file.

The schema evolution behavior is as follows:

* Column added in new schema: Data created with an older schema produces a _default_ value when the table is using the new schema.
* Column removed in new schema: Data created with an older schema no longer outputs the data from the column that was removed.
* Column is renamed in the new schema: This is equivalent to removing the column and adding a new one, and data created with an older schema produces a _default_ value when the table is using the new schema.
* Changing type of column in the new schema: If the type coercion is supported by Avro, then the conversion happens. An error is thrown for incompatible types.

### SQL support[#](https://trino.io/docs/current/connector/kinesis.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides globally available and read operation statements to access data and metadata from Kinesis streams.

\
