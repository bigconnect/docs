# Phoenix connector

The Phoenix connector allows querying data stored in [Apache HBase](https://hbase.apache.org/) using [Apache Phoenix](https://phoenix.apache.org/).

### Requirements[#](https://trino.io/docs/current/connector/phoenix.html#requirements) <a href="#requirements" id="requirements"></a>

To query HBase data through Phoenix, you need:

* Network access from the Trino coordinator and workers to the ZooKeeper servers. The default port is 2181.
* A compatible version of Phoenix: all 5.x versions starting from 5.1.0 are supported.

### Configuration[#](https://trino.io/docs/current/connector/phoenix.html#configuration) <a href="#configuration" id="configuration"></a>

To configure the Phoenix connector, create a catalog properties file `etc/catalog/example.properties` with the following contents, replacing `host1,host2,host3` with a comma-separated list of the ZooKeeper nodes used for discovery of the HBase cluster:

```
connector.name=phoenix5
phoenix.connection-url=jdbc:phoenix:host1,host2,host3:2181:/hbase
phoenix.config.resources=/path/to/hbase-site.xml
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The optional paths to Hadoop resource files, such as `hbase-site.xml` are used to load custom Phoenix client connection properties.

The following Phoenix-specific configuration properties are available:

| Property name                 | Required | Description                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `phoenix.connection-url`      | Yes      | `jdbc:phoenix[:zk_quorum][:zk_port][:zk_hbase_path]`. The `zk_quorum` is a comma separated list of ZooKeeper servers. The `zk_port` is the ZooKeeper port. The `zk_hbase_path` is the HBase root znode path, that is configurable using `hbase-site.xml`. By default the location is `/hbase`                                                                                                      |
| `phoenix.config.resources`    | No       | Comma-separated list of configuration files (e.g. `hbase-site.xml`) to use for connection properties. These files must exist on the machines running Trino.                                                                                                                                                                                                                                        |
| `phoenix.max-scans-per-split` | No       | Maximum number of HBase scans that will be performed in a single split. Default is 20. Lower values will lead to more splits in Trino. Can also be set via session propery `max_scans_per_split`. For details see: [https://phoenix.apache.org/update\_statistics.html](https://phoenix.apache.org/update\_statistics.html). (This setting has no effect when guideposts are disabled in Phoenix.) |

#### General configuration properties[#](https://trino.io/docs/current/connector/phoenix.html#general-configuration-properties) <a href="#general-configuration-properties" id="general-configuration-properties"></a>

The following table describes general catalog configuration properties for the connector:

| Property name                                               | Description                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `case-insensitive-name-matching`                            | Support case insensitive schema and table names. Defaults to `false`.                                                                                                                                                                                                                                                                                                     |
| `case-insensitive-name-matching.cache-ttl`                  | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which case insensitive schema and table names are cached. Defaults to `1m`.                                                                                                                                                                                                        |
| `case-insensitive-name-matching.config-file`                | Path to a name mapping configuration file in JSON format that allows Trino to disambiguate between schemas and tables with similar names in different cases. Defaults to `null`.                                                                                                                                                                                          |
| `case-insensitive-name-matching.config-file.refresh-period` | Frequency with which Trino checks the name matching configuration file for changes. The [duration value](https://trino.io/docs/current/admin/properties.html#prop-type-duration) defaults to `0s` (refresh disabled).                                                                                                                                                     |
| `metadata.cache-ttl`                                        | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which metadata, including table and column statistics, is cached. Defaults to `0s` (caching disabled).                                                                                                                                                                             |
| `metadata.cache-missing`                                    | Cache the fact that metadata, including table and column statistics, is not available. Defaults to `false`.                                                                                                                                                                                                                                                               |
| `metadata.schemas.cache-ttl`                                | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which schema metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                    |
| `metadata.tables.cache-ttl`                                 | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which table metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                     |
| `metadata.statistics.cache-ttl`                             | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which tables statistics are cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                 |
| `metadata.cache-maximum-size`                               | Maximum number of objects stored in the metadata cache. Defaults to `10000`.                                                                                                                                                                                                                                                                                              |
| `write.batch-size`                                          | Maximum number of statements in a batched execution. Do not change this setting from the default. Non-default values may negatively impact performance. Defaults to `1000`.                                                                                                                                                                                               |
| `dynamic-filtering.enabled`                                 | Push down dynamic filters into JDBC queries. Defaults to `true`.                                                                                                                                                                                                                                                                                                          |
| `dynamic-filtering.wait-timeout`                            | Maximum [duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which Trino waits for dynamic filters to be collected from the build side of joins before starting a JDBC query. Using a large timeout can potentially result in more detailed dynamic filters. However, it can also increase latency for some queries. Defaults to `20s`. |

#### Appending query metadata[#](https://trino.io/docs/current/connector/phoenix.html#appending-query-metadata) <a href="#appending-query-metadata" id="appending-query-metadata"></a>

The optional parameter `query.comment-format` allows you to configure a SQL comment that is sent to the datasource with each query. The format of this comment can contain any characters and the following metadata:

* `$QUERY_ID`: The identifier of the query.
* `$USER`: The name of the user who submits the query to Trino.
* `$SOURCE`: The identifier of the client tool used to submit the query, for example `trino-cli`.
* `$TRACE_TOKEN`: The trace token configured with the client tool.

The comment can provide more context about the query. This additional information is available in the logs of the datasource. To include environment variables from the Trino cluster with the comment , use the `${ENV:VARIABLE-NAME}` syntax.

The following example sets a simple comment that identifies each query sent by Trino:

```
query.comment-format=Query sent by Trino.
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

With this configuration, a query such as `SELECT * FROM example_table;` is sent to the datasource with the comment appended:

```
SELECT * FROM example_table; /*Query sent by Trino.*/
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following example improves on the preceding example by using metadata:

```
query.comment-format=Query $QUERY_ID sent by user $USER from Trino.
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If `Jane` sent the query with the query identifier `20230622_180528_00000_bkizg`, the following comment string is sent to the datasource:

```
SELECT * FROM example_table; /*Query 20230622_180528_00000_bkizg sent by user Jane from Trino.*/
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note

Certain JDBC driver settings and logging configurations might cause the comment to be removed.

#### Domain compaction threshold[#](https://trino.io/docs/current/connector/phoenix.html#domain-compaction-threshold) <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improve performance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large [dynamic filters](https://trino.io/docs/current/admin/dynamic-filtering.html). The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` [catalog session property](https://trino.io/docs/current/sql/set-session.html#session-properties-definition) can be used to adjust the default value of `5000` for this threshold.

#### Procedures[#](https://trino.io/docs/current/connector/phoenix.html#procedures) <a href="#procedures" id="procedures"></a>

*   `system.flush_metadata_cache()`

    Flush JDBC metadata caches. For example, the following system call flushes the metadata caches for all schemas in the `example` catalog

    ```
    USE example.example_schema;
    CALL system.flush_metadata_cache();
    ```

    ![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Case insensitive matching[#](https://trino.io/docs/current/connector/phoenix.html#case-insensitive-matching) <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

When `case-insensitive-name-matching` is set to `true`, Trino is able to query non-lowercase schemas and tables by maintaining a mapping of the lowercase name to the actual name in the remote system. However, if two schemas and/or tables have names that differ only in case (such as “customers” and “Customers”) then Trino fails to query them due to ambiguity.

In these cases, use the `case-insensitive-name-matching.config-file` catalog configuration property to specify a configuration file that maps these remote schemas/tables to their respective Trino schemas/tables:

```
{
  "schemas": [
    {
      "remoteSchema": "CaseSensitiveName",
      "mapping": "case_insensitive_1"
    },
    {
      "remoteSchema": "cASEsENSITIVEnAME",
      "mapping": "case_insensitive_2"
    }],
  "tables": [
    {
      "remoteSchema": "CaseSensitiveName",
      "remoteTable": "tablex",
      "mapping": "table_1"
    },
    {
      "remoteSchema": "CaseSensitiveName",
      "remoteTable": "TABLEX",
      "mapping": "table_2"
    }]
}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Queries against one of the tables or schemes defined in the `mapping` attributes are run against the corresponding remote entity. For example, a query against tables in the `case_insensitive_1` schema is forwarded to the CaseSensitiveName schema and a query against `case_insensitive_2` is forwarded to the `cASEsENSITIVEnAME` schema.

At the table mapping level, a query on `case_insensitive_1.table_1` as configured above is forwarded to `CaseSensitiveName.tablex`, and a query on `case_insensitive_1.table_2` is forwarded to `CaseSensitiveName.TABLEX`.

By default, when a change is made to the mapping configuration file, Trino must be restarted to load the changes. Optionally, you can set the `case-insensitive-name-mapping.refresh-period` to have Trino refresh the properties without requiring a restart:

```
case-insensitive-name-mapping.refresh-period=30s
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Non-transactional INSERT[#](https://trino.io/docs/current/connector/phoenix.html#non-transactional-insert) <a href="#non-transactional-insert" id="non-transactional-insert"></a>

The connector supports adding rows using [INSERT statements](https://trino.io/docs/current/sql/insert.html). By default, data insertion is performed by writing data to a temporary table. You can skip this step to improve performance and write directly to the target table. Set the `insert.non-transactional-insert.enabled` catalog property or the corresponding `non_transactional_insert` catalog session property to `true`.

Note that with this property enabled, data can be corrupted in rare cases where exceptions occur during the insert operation. With transactions disabled, no rollback can be performed.

### Querying Phoenix tables[#](https://trino.io/docs/current/connector/phoenix.html#querying-phoenix-tables) <a href="#querying-phoenix-tables" id="querying-phoenix-tables"></a>

The default empty schema in Phoenix maps to a schema named `default` in Trino. You can see the available Phoenix schemas by running `SHOW SCHEMAS`:

```
SHOW SCHEMAS FROM example;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you have a Phoenix schema named `web`, you can view the tables in this schema by running `SHOW TABLES`:

```
SHOW TABLES FROM example.web;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can see a list of the columns in the `clicks` table in the `web` schema using either of the following:

```
DESCRIBE example.web.clicks;
SHOW COLUMNS FROM example.web.clicks;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Finally, you can access the `clicks` table in the `web` schema:

```
SELECT * FROM example.web.clicks;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you used a different name for your catalog properties file, use that catalog name instead of `example` in the above examples.

### Type mapping[#](https://trino.io/docs/current/connector/phoenix.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

Because Trino and Phoenix each support types that the other does not, this connector [modifies some types](https://trino.io/docs/current/language/types.html#type-mapping-overview) when reading or writing data. Data types may not map the same way in both directions between Trino and the data source. Refer to the following sections for type mapping in each direction.

#### Phoenix type to Trino type mapping[#](https://trino.io/docs/current/connector/phoenix.html#phoenix-type-to-trino-type-mapping) <a href="#phoenix-type-to-trino-type-mapping" id="phoenix-type-to-trino-type-mapping"></a>

The connector maps Phoenix types to the corresponding Trino types following this table:

| Phoenix database type | Trino type     |
| --------------------- | -------------- |
| `BOOLEAN`             | `BOOLEAN`      |
| `TINYINT`             | `TINYINT`      |
| `UNSIGNED_TINYINT`    | `TINYINT`      |
| `SMALLINT`            | `SMALLINT`     |
| `UNSIGNED_SMALLINT`   | `SMALLINT`     |
| `INTEGER`             | `INTEGER`      |
| `UNSIGNED_INT`        | `INTEGER`      |
| `BIGINT`              | `BIGINT`       |
| `UNSIGNED_LONG`       | `BIGINT`       |
| `FLOAT`               | `REAL`         |
| `UNSIGNED_FLOAT`      | `REAL`         |
| `DOUBLE`              | `DOUBLE`       |
| `UNSIGNED_DOUBLE`     | `DOUBLE`       |
| `DECIMAL(p,s)`        | `DECIMAL(p,s)` |
| `CHAR(n)`             | `CHAR(n)`      |
| `VARCHAR(n)`          | `VARCHAR(n)`   |
| `BINARY`              | `VARBINARY`    |
| `VARBINARY`           | `VARBINARY`    |
| `DATE`                | `DATE`         |
| `UNSIGNED_DATE`       | `DATE`         |
| `ARRAY`               | `ARRAY`        |

No other types are supported.

#### Trino type to Phoenix type mapping[#](https://trino.io/docs/current/connector/phoenix.html#trino-type-to-phoenix-type-mapping) <a href="#trino-type-to-phoenix-type-mapping" id="trino-type-to-phoenix-type-mapping"></a>

The Phoenix fixed length `BINARY` data type is mapped to the Trino variable length `VARBINARY` data type. There is no way to create a Phoenix table in Trino that uses the `BINARY` data type, as Trino does not have an equivalent type.

The connector maps Trino types to the corresponding Phoenix types following this table:

| Trino database type | Phoenix type   |
| ------------------- | -------------- |
| `BOOLEAN`           | `BOOLEAN`      |
| `TINYINT`           | `TINYINT`      |
| `SMALLINT`          | `SMALLINT`     |
| `INTEGER`           | `INTEGER`      |
| `BIGINT`            | `BIGINT`       |
| `REAL`              | `FLOAT`        |
| `DOUBLE`            | `DOUBLE`       |
| `DECIMAL(p,s)`      | `DECIMAL(p,s)` |
| `CHAR(n)`           | `CHAR(n)`      |
| `VARCHAR(n)`        | `VARCHAR(n)`   |
| `VARBINARY`         | `VARBINARY`    |
| `TIME`              | `TIME`         |
| `DATE`              | `DATE`         |
| `ARRAY`             | `ARRAY`        |

No other types are supported.

#### Decimal type handling[#](https://trino.io/docs/current/connector/phoenix.html#decimal-type-handling) <a href="#decimal-type-handling" id="decimal-type-handling"></a>

`DECIMAL` types with unspecified precision or scale are ignored unless the `decimal-mapping` configuration property or the `decimal_mapping` session property is set to `allow_overflow`. Then such types are mapped to a Trino `DECIMAL` with a default precision of 38 and default scale of 0. To change the scale of the resulting type, use the `decimal-default-scale` configuration property or the `decimal_default_scale` session property. The precision is always 38.

By default, values that require rounding or truncation to fit will cause a failure at runtime. This behavior is controlled via the `decimal-rounding-mode` configuration property or the `decimal_rounding_mode` session property, which can be set to `UNNECESSARY` (the default), `UP`, `DOWN`, `CEILING`, `FLOOR`, `HALF_UP`, `HALF_DOWN`, or `HALF_EVEN` (see [RoundingMode](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/math/RoundingMode.html#enum.constant.summary)).

#### Type mapping configuration properties[#](https://trino.io/docs/current/connector/phoenix.html#type-mapping-configuration-properties) <a href="#type-mapping-configuration-properties" id="type-mapping-configuration-properties"></a>

The following properties can be used to configure how data types from the connected data source are mapped to Trino data types and how the metadata is cached in Trino.

| Property name                  | Description                                                                                                                                                                                                                                                                                                              | Default value |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `unsupported-type-handling`    | <p>Configure how unsupported column data types are handled:</p><ul><li><code>IGNORE</code>, column is not accessible.</li><li><code>CONVERT_TO_VARCHAR</code>, column is converted to unbounded <code>VARCHAR</code>.</li></ul><p>The respective catalog session property is <code>unsupported_type_handling</code>.</p> | `IGNORE`      |
| `jdbc-types-mapped-to-varchar` | Allow forced mapping of comma separated lists of data types to convert to unbounded `VARCHAR`                                                                                                                                                                                                                            |               |

### Table properties - Phoenix[#](https://trino.io/docs/current/connector/phoenix.html#table-properties-phoenix) <a href="#table-properties-phoenix" id="table-properties-phoenix"></a>

Table property usage example:

```
CREATE TABLE example_schema.scientists (
  recordkey VARCHAR,
  birthday DATE,
  name VARCHAR,
  age BIGINT
)
WITH (
  rowkeys = 'recordkey,birthday',
  salt_buckets = 10
);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following are supported Phoenix table properties from [https://phoenix.apache.org/language/index.html#options](https://phoenix.apache.org/language/index.html#options)

| Property name           | Default value | Description                                                                                                            |
| ----------------------- | ------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `rowkeys`               | `ROWKEY`      | Comma-separated list of primary key columns. See further description below                                             |
| `split_on`              | (none)        | List of keys to presplit the table on. See [Split Point](https://phoenix.apache.org/language/index.html#split\_point). |
| `salt_buckets`          | (none)        | Number of salt buckets for this table.                                                                                 |
| `disable_wal`           | false         | Whether to disable WAL writes in HBase for this table.                                                                 |
| `immutable_rows`        | false         | Declares whether this table has rows which are write-once, append-only.                                                |
| `default_column_family` | `0`           | Default column family name to use for this table.                                                                      |

#### `rowkeys`[#](https://trino.io/docs/current/connector/phoenix.html#rowkeys) <a href="#rowkeys" id="rowkeys"></a>

This is a comma-separated list of columns to be used as the table’s primary key. If not specified, a `BIGINT` primary key column named `ROWKEY` is generated , as well as a sequence with the same name as the table suffixed with `_seq` (i.e. `<schema>.<table>_seq`) , which is used to automatically populate the `ROWKEY` for each row during insertion.

### Table properties - HBase[#](https://trino.io/docs/current/connector/phoenix.html#table-properties-hbase) <a href="#table-properties-hbase" id="table-properties-hbase"></a>

The following are the supported HBase table properties that are passed through by Phoenix during table creation. Use them in the same way as above: in the `WITH` clause of the `CREATE TABLE` statement.

| Property name         | Default value | Description                                                                                                            |
| --------------------- | ------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `versions`            | `1`           | The maximum number of versions of each cell to keep.                                                                   |
| `min_versions`        | `0`           | The minimum number of cell versions to keep.                                                                           |
| `compression`         | `NONE`        | Compression algorithm to use. Valid values are `NONE` (default), `SNAPPY`, `LZO`, `LZ4`, or `GZ`.                      |
| `data_block_encoding` | `FAST_DIFF`   | Block encoding algorithm to use. Valid values are: `NONE`, `PREFIX`, `DIFF`, `FAST_DIFF` (default), or `ROW_INDEX_V1`. |
| `ttl`                 | `FOREVER`     | Time To Live for each cell.                                                                                            |
| `bloomfilter`         | `NONE`        | Bloomfilter to use. Valid values are `NONE` (default), `ROW`, or `ROWCOL`.                                             |

### SQL support[#](https://trino.io/docs/current/connector/phoenix.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides read and write access to data and metadata in Phoenix. In addition to the [globally available](https://trino.io/docs/current/language/sql-support.html#sql-globally-available) and [read operation](https://trino.io/docs/current/language/sql-support.html#sql-read-operations) statements, the connector supports the following features:

* [INSERT](https://trino.io/docs/current/sql/insert.html)
* [DELETE](https://trino.io/docs/current/sql/delete.html)
* [MERGE](https://trino.io/docs/current/sql/merge.html)
* [CREATE TABLE](https://trino.io/docs/current/sql/create-table.html)
* [CREATE TABLE AS](https://trino.io/docs/current/sql/create-table-as.html)
* [DROP TABLE](https://trino.io/docs/current/sql/drop-table.html)
* [CREATE SCHEMA](https://trino.io/docs/current/sql/create-schema.html)
* [DROP SCHEMA](https://trino.io/docs/current/sql/drop-schema.html)

#### SQL DELETE[#](https://trino.io/docs/current/connector/phoenix.html#sql-delete) <a href="#sql-delete" id="sql-delete"></a>

If a `WHERE` clause is specified, the `DELETE` operation only works if the predicate in the clause can be fully pushed down to the data source.
