# Redis connector

The Redis connector allows querying of live data stored in [Redis](https://redis.io/). This can be used to join data between different systems like Redis and Hive.

Each Redis key/value pair is presented as a single row in Trino. Rows can be broken down into cells by using table definition files.

Currently, only Redis key of string and zset types are supported, only Redis value of string and hash types are supported.

### Requirements <a href="#requirements" id="requirements"></a>

Requirements for using the connector in a catalog to connect to a Redis data source are:

* Redis 2.8.0 or higher (Redis Cluster is not supported)
* Network access, by default on port 6379, from the Trino coordinator and workers to Redis.

### Configuration <a href="#configuration" id="configuration"></a>

To configure the Redis connector, create a catalog properties file `etc/catalog/example.properties` with the following content, replacing the properties as appropriate:

```
connector.name=redis
redis.table-names=schema1.table1,schema1.table2
redis.nodes=host:port
```

#### Multiple Redis servers <a href="#multiple-redis-servers" id="multiple-redis-servers"></a>

You can have as many catalogs as you need. If you have additional Redis servers, simply add another properties file to `etc/catalog` with a different name, making sure it ends in `.properties`.

### Configuration properties <a href="#configuration-properties" id="configuration-properties"></a>

The following configuration properties are available:

| Property name                       | Description                                                                                     |
| ----------------------------------- | ----------------------------------------------------------------------------------------------- |
| `redis.table-names`                 | List of all tables provided by the catalog                                                      |
| `redis.default-schema`              | Default schema name for tables                                                                  |
| `redis.nodes`                       | Location of the Redis server                                                                    |
| `redis.scan-count`                  | Redis parameter for scanning of the keys                                                        |
| `redis.max-keys-per-fetch`          | Get values associated with the specified number of keys in the redis command such as MGET(key…) |
| `redis.key-prefix-schema-table`     | Redis keys have schema-name:table-name prefix                                                   |
| `redis.key-delimiter`               | Delimiter separating schema\_name and table\_name if redis.key-prefix-schema-table is used      |
| `redis.table-description-dir`       | Directory containing table description files                                                    |
| `redis.table-description-cache-ttl` | The cache time for table description files                                                      |
| `redis.hide-internal-columns`       | Controls whether internal columns are part of the table schema or not                           |
| `redis.database-index`              | Redis database index                                                                            |
| `redis.user`                        | Redis server username                                                                           |
| `redis.password`                    | Redis server password                                                                           |

#### `redis.table-names`[#](https://trino.io/docs/current/connector/redis.html#redis-table-names) <a href="#redis-table-names" id="redis-table-names"></a>

Comma-separated list of all tables provided by this catalog. A table name can be unqualified (simple name) and is placed into the default schema (see below), or qualified with a schema name (`<schema-name>.<table-name>`).

For each table defined, a table description file (see below) may exist. If no table description file exists, the table only contains internal columns (see below).

This property is optional; the connector relies on the table description files specified in the `redis.table-description-dir` property.

#### `redis.default-schema` <a href="#redis-default-schema" id="redis-default-schema"></a>

Defines the schema which will contain all tables that were defined without a qualifying schema name.

This property is optional; the default is `default`.

#### `redis.nodes` <a href="#redis-nodes" id="redis-nodes"></a>

The `hostname:port` pair for the Redis server.

This property is required; there is no default.

Redis Cluster is not supported.

#### `redis.scan-count` <a href="#redis-scan-count" id="redis-scan-count"></a>

The internal COUNT parameter for the Redis SCAN command when connector is using SCAN to find keys for the data. This parameter can be used to tune performance of the Redis connector.

This property is optional; the default is `100`.

#### `redis.max-keys-per-fetch`[#](https://trino.io/docs/current/connector/redis.html#redis-max-keys-per-fetch) <a href="#redis-max-keys-per-fetch" id="redis-max-keys-per-fetch"></a>

The internal number of keys for the Redis MGET command and Pipeline HGETALL command when connector is using these commands to find values of keys. This parameter can be used to tune performance of the Redis connector.

This property is optional; the default is `100`.

#### `redis.key-prefix-schema-table` <a href="#redis-key-prefix-schema-table" id="redis-key-prefix-schema-table"></a>

If true, only keys prefixed with the `schema-name:table-name` are scanned for a table, and all other keys are filtered out. If false, all keys are scanned.

This property is optional; the default is `false`.

#### `redis.key-delimiter` <a href="#redis-key-delimiter" id="redis-key-delimiter"></a>

The character used for separating `schema-name` and `table-name` when `redis.key-prefix-schema-table` is `true`

This property is optional; the default is `:`.

#### `redis.table-description-dir` <a href="#redis-table-description-dir" id="redis-table-description-dir"></a>

References a folder within Trino deployment that holds one or more JSON files, which must end with `.json` and contain table description files.

Note that the table description files will only be used by the Trino coordinator node.

This property is optional; the default is `etc/redis`.

#### `redis.table-description-cache-ttl` <a href="#redis-table-description-cache-ttl" id="redis-table-description-cache-ttl"></a>

The Redis connector dynamically loads the table description files after waiting for the time specified by this property. Therefore, there is no need to update the `redis.table-names` property and restart the Trino service when adding, updating, or deleting a file end with `.json` to `redis.table-description-dir` folder.

This property is optional; the default is `5m`.

#### `redis.hide-internal-columns` <a href="#redis-hide-internal-columns" id="redis-hide-internal-columns"></a>

In addition to the data columns defined in a table description file, the connector maintains a number of additional columns for each table. If these columns are hidden, they can still be used in queries, but they do not show up in `DESCRIBE <table-name>` or `SELECT *`.

This property is optional; the default is `true`.

#### `redis.database-index` <a href="#redis-database-index" id="redis-database-index"></a>

The Redis database to query.

This property is optional; the default is `0`.

#### `redis.user` <a href="#redis-user" id="redis-user"></a>

The username for Redis server.

This property is optional; the default is `null`.

#### `redis.password` <a href="#redis-password" id="redis-password"></a>

The password for password-protected Redis server.

This property is optional; the default is `null`.

### Internal columns <a href="#internal-columns" id="internal-columns"></a>

For each defined table, the connector maintains the following columns:

| Column name      | Type    | Description                                                                                                                                |
| ---------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `_key`           | VARCHAR | Redis key.                                                                                                                                 |
| `_value`         | VARCHAR | Redis value corresponding to the key.                                                                                                      |
| `_key_length`    | BIGINT  | Number of bytes in the key.                                                                                                                |
| `_value_length`  | BIGINT  | Number of bytes in the value.                                                                                                              |
| `_key_corrupt`   | BOOLEAN | True if the decoder could not decode the key for this row. When true, data columns mapped from the key should be treated as invalid.       |
| `_value_corrupt` | BOOLEAN | True if the decoder could not decode the message for this row. When true, data columns mapped from the value should be treated as invalid. |

For tables without a table definition file, the `_key_corrupt` and `_value_corrupt` columns are `false`.

### Table definition files <a href="#table-definition-files" id="table-definition-files"></a>

With the Redis connector it is possible to further reduce Redis key/value pairs into granular cells, provided the key/value string follows a particular format. This process defines new columns that can be further queried from Trino.

A table definition file consists of a JSON definition for a table. The name of the file can be arbitrary, but must end in `.json`.

```
{
    "tableName": ...,
    "schemaName": ...,
    "key": {
        "dataFormat": ...,
        "fields": [
            ...
        ]
    },
    "value": {
        "dataFormat": ...,
        "fields": [
            ...
       ]
    }
}
```

| Field        | Required | Type        | Description                                                                       |
| ------------ | -------- | ----------- | --------------------------------------------------------------------------------- |
| `tableName`  | required | string      | Trino table name defined by this file.                                            |
| `schemaName` | optional | string      | Schema which will contain the table. If omitted, the default schema name is used. |
| `key`        | optional | JSON object | Field definitions for data columns mapped to the value key.                       |
| `value`      | optional | JSON object | Field definitions for data columns mapped to the value itself.                    |

Please refer to the Kafka connector page for the description of the `dataFormat` as well as various available decoders.

In addition to the above Kafka types, the Redis connector supports `hash` type for the `value` field which represent data stored in the Redis hash.

```
{
    "tableName": ...,
    "schemaName": ...,
    "value": {
        "dataFormat": "hash",
        "fields": [
            ...
       ]
    }
}
```



### Type mapping <a href="#type-mapping" id="type-mapping"></a>

Because Trino and Redis each support types that the other does not, this connector [maps some types](https://trino.io/docs/current/language/types.html#type-mapping-overview) when reading data. Type mapping depends on the RAW, CSV, JSON, and AVRO file formats.

#### Row decoding[#](https://trino.io/docs/current/connector/redis.html#row-decoding) <a href="#row-decoding" id="row-decoding"></a>

A decoder is used to map data to table columns.

The connector contains the following decoders:

* `raw`: Message is not interpreted; ranges of raw message bytes are mapped to table columns.
* `csv`: Message is interpreted as comma separated message, and fields are mapped to table columns.
* `json`: Message is parsed as JSON, and JSON fields are mapped to table columns.
* `avro`: Message is parsed based on an Avro schema, and Avro fields are mapped to table columns.

Note

If no table definition file exists for a table, the `dummy` decoder is used, which does not expose any columns.

**Raw decoder**

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

**CSV decoder**

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

**JSON decoder**

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

**DEFAULT FIELD DECODER**

This is the standard field decoder. It supports all the Trino physical data types. A field value is transformed under JSON conversion rules into boolean, long, double, or string values. This decoder should be used for columns that are not date or time based.

**DATE AND TIME DECODERS**

To convert values from JSON objects to Trino `DATE`, `TIME`, `TIME WITH TIME ZONE`, `TIMESTAMP` or `TIMESTAMP WITH TIME ZONE` columns, select special decoders using the `dataFormat` attribute of a field definition.

* `iso8601` - Text based, parses a text field as an ISO 8601 timestamp.
* `rfc2822` - Text based, parses a text field as an [**RFC 2822**](https://datatracker.ietf.org/doc/html/rfc2822.html) timestamp.
* `custom-date-time` - Text based, parses a text field according to Joda format pattern specified via `formatHint` attribute. The format pattern should conform to https://www.joda.org/joda-time/apidocs/org/joda/time/format/DateTimeFormat.html.
* `milliseconds-since-epoch` - Number-based, interprets a text or number as number of milliseconds since the epoch.
* `seconds-since-epoch` - Number-based, interprets a text or number as number of milliseconds since the epoch.

For `TIMESTAMP WITH TIME ZONE` and `TIME WITH TIME ZONE` data types, if timezone information is present in decoded value, it is used as a Trino value. Otherwise, the result time zone is set to `UTC`.

**Avro decoder**

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

**AVRO SCHEMA EVOLUTION**

The Avro decoder supports schema evolution with backward compatibility. With backward compatibility, a newer schema can be used to read Avro data created with an older schema. Any change in the Avro schema must also be reflected in Trino’s topic definition file. Newly added or renamed fields must have a default value in the Avro schema file.

The schema evolution behavior is as follows:

* Column added in new schema: Data created with an older schema produces a _default_ value when the table is using the new schema.
* Column removed in new schema: Data created with an older schema no longer outputs the data from the column that was removed.
* Column is renamed in the new schema: This is equivalent to removing the column and adding a new one, and data created with an older schema produces a _default_ value when the table is using the new schema.
* Changing type of column in the new schema: If the type coercion is supported by Avro, then the conversion happens. An error is thrown for incompatible types.

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides globally available and read operation statements to access data and metadata in Redis.

### Performance <a href="#performance" id="performance"></a>

The connector includes a number of performance improvements, detailed in the following sections.

#### Pushdown <a href="#pushdown" id="pushdown"></a>

Note

The connector performs pushdown where performance may be improved, but in order to preserve correctness an operation may not be pushed down. When pushdown of an operation may result in better performance but risks correctness, the connector prioritizes correctness.

**Predicate pushdown support**

The connector supports pushdown of keys of `string` type only, the `zset` type is not supported. Key pushdown is not supported when multiple key fields are defined in the table definition file.

The connector supports pushdown of equality predicates, such as `IN` or `=`. Inequality predicates, such as `!=`, and range predicates, such as `>`, `<`, or `BETWEEN` are not pushed down.

In the following example, the predicate of the first query is not pushed down since `>` is a range predicate. The other queries are pushed down:

```
-- Not pushed down
SELECT * FROM nation WHERE redis_key > 'CANADA';
-- Pushed down
SELECT * FROM nation WHERE redis_key = 'CANADA';
SELECT * FROM nation WHERE redis_key IN ('CANADA', 'POLAND');
```
