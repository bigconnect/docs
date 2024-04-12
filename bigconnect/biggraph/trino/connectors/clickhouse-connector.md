# ClickHouse connector

The ClickHouse connector allows querying tables in an external [ClickHouse](https://clickhouse.com/) server. This can be used to query data in the databases on that server, or combine it with other data from different catalogs accessing ClickHouse or any other supported data source.

### Requirements <a href="#requirements" id="requirements"></a>

To connect to a ClickHouse server, you need:

* ClickHouse (version 21.8 or higher) or Altinity (version 20.8 or higher).
* Network access from the Trino coordinator and workers to the ClickHouse server. Port 8123 is the default port.

### Configuration <a href="#configuration" id="configuration"></a>

The connector can query a ClickHouse server. Create a catalog properties file that specifies the ClickHouse connector by setting the `connector.name` to `clickhouse`.

For example, create the file `etc/catalog/example.properties`. Replace the connection properties as appropriate for your setup:

```
connector.name=clickhouse
connection-url=jdbc:clickhouse://host1:8123/
connection-user=exampleuser
connection-password=examplepassword
```

The `connection-url` defines the connection information and parameters to pass to the ClickHouse JDBC driver. The supported parameters for the URL are available in the ClickHouse JDBC driver configuration.

The `connection-user` and `connection-password` are typically required and determine the user credentials for the connection, often a service user. You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid actual values in the catalog properties files.

#### Connection security[#](https://trino.io/docs/current/connector/clickhouse.html#connection-security) <a href="#connection-security" id="connection-security"></a>

If you have TLS configured with a globally-trusted certificate installed on your data source, you can enable TLS between your cluster and the data source by appending a parameter to the JDBC connection string set in the `connection-url` catalog configuration property.

For example, with version 2.6.4 of the ClickHouse JDBC driver, enable TLS by appending the `ssl=true` parameter to the `connection-url` configuration property:

```
connection-url=jdbc:clickhouse://host1:8443/?ssl=true
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

For more information on TLS configuration options, see the Clickhouse JDBC driver documentation

#### Data source authentication[#](https://trino.io/docs/current/connector/clickhouse.html#data-source-authentication) <a href="#data-source-authentication" id="data-source-authentication"></a>

The connector can provide credentials for the data source connection in multiple ways:

* inline, in the connector configuration file
* in a separate properties file
* in a key store file
* as extra credentials set when connecting to Trino

You can use secrets to avoid storing sensitive values in the catalog properties files.

The following table describes configuration properties for connection credentials:

| Property name                           | Description                                                                                                                                |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `credential-provider.type`              | Type of the credential provider. Must be one of `INLINE`, `FILE`, or `KEYSTORE`; defaults to `INLINE`.                                     |
| `connection-user`                       | Connection user name.                                                                                                                      |
| `connection-password`                   | Connection password.                                                                                                                       |
| `user-credential-name`                  | Name of the extra credentials property, whose value to use as the user name. See `extraCredentials` in Parameter reference.                |
| `password-credential-name`              | Name of the extra credentials property, whose value to use as the password.                                                                |
| `connection-credential-file`            | Location of the properties file where credentials are present. It must contain the `connection-user` and `connection-password` properties. |
| `keystore-file-path`                    | The location of the Java Keystore file, from which to read credentials.                                                                    |
| `keystore-type`                         | File format of the keystore file, for example `JKS` or `PEM`.                                                                              |
| `keystore-password`                     | Password for the key store.                                                                                                                |
| `keystore-user-credential-name`         | Name of the key store entity to use as the user name.                                                                                      |
| `keystore-user-credential-password`     | Password for the user name key store entity.                                                                                               |
| `keystore-password-credential-name`     | Name of the key store entity to use as the password.                                                                                       |
| `keystore-password-credential-password` | Password for the password key store entity.                                                                                                |

#### Multiple ClickHouse servers[#](https://trino.io/docs/current/connector/clickhouse.html#multiple-clickhouse-servers) <a href="#multiple-clickhouse-servers" id="multiple-clickhouse-servers"></a>

If you have multiple ClickHouse servers you need to configure one catalog for each server. To add another catalog:

* Add another properties file to `etc/catalog`
* Save it with a different name that ends in `.properties`

For example, if you name the property file `sales.properties`, Trino uses the configured connector to create a catalog named `sales`.

#### General configuration properties[#](https://trino.io/docs/current/connector/clickhouse.html#general-configuration-properties) <a href="#general-configuration-properties" id="general-configuration-properties"></a>

The following table describes general catalog configuration properties for the connector:

| Property name                                               | Description                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `case-insensitive-name-matching`                            | Support case insensitive schema and table names. Defaults to `false`.                                                                                                                                                                                                                           |
| `case-insensitive-name-matching.cache-ttl`                  | Duration for which case insensitive schema and table names are cached. Defaults to `1m`.                                                                                                                                                                                                        |
| `case-insensitive-name-matching.config-file`                | Path to a name mapping configuration file in JSON format that allows Trino to disambiguate between schemas and tables with similar names in different cases. Defaults to `null`.                                                                                                                |
| `case-insensitive-name-matching.config-file.refresh-period` | Frequency with which Trino checks the name matching configuration file for changes. The duration value defaults to `0s` (refresh disabled).                                                                                                                                                     |
| `metadata.cache-ttl`                                        | Duration for which metadata, including table and column statistics, is cached. Defaults to `0s` (caching disabled).                                                                                                                                                                             |
| `metadata.cache-missing`                                    | Cache the fact that metadata, including table and column statistics, is not available. Defaults to `false`.                                                                                                                                                                                     |
| `metadata.schemas.cache-ttl`                                | Duration for which schema metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                    |
| `metadata.tables.cache-ttl`                                 | Duration for which table metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                     |
| `metadata.statistics.cache-ttl`                             | Duration for which tables statistics are cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                 |
| `metadata.cache-maximum-size`                               | Maximum number of objects stored in the metadata cache. Defaults to `10000`.                                                                                                                                                                                                                    |
| `write.batch-size`                                          | Maximum number of statements in a batched execution. Do not change this setting from the default. Non-default values may negatively impact performance. Defaults to `1000`.                                                                                                                     |
| `dynamic-filtering.enabled`                                 | Push down dynamic filters into JDBC queries. Defaults to `true`.                                                                                                                                                                                                                                |
| `dynamic-filtering.wait-timeout`                            | Maximum duration for which Trino waits for dynamic filters to be collected from the build side of joins before starting a JDBC query. Using a large timeout can potentially result in more detailed dynamic filters. However, it can also increase latency for some queries. Defaults to `20s`. |

#### Appending query metadata[#](https://trino.io/docs/current/connector/clickhouse.html#appending-query-metadata) <a href="#appending-query-metadata" id="appending-query-metadata"></a>

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

With this configuration, a query such as `SELECT * FROM example_table;` is sent to the datasource with the comment appended:

```
SELECT * FROM example_table; /*Query sent by Trino.*/
```

The following example improves on the preceding example by using metadata:

```
query.comment-format=Query $QUERY_ID sent by user $USER from Trino.
```

If `Jane` sent the query with the query identifier `20230622_180528_00000_bkizg`, the following comment string is sent to the datasource:

```
SELECT * FROM example_table; /*Query 20230622_180528_00000_bkizg sent by user Jane from Trino.*/
```

Note

Certain JDBC driver settings and logging configurations might cause the comment to be removed.

#### Domain compaction threshold <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improve performance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large dynamic filters. The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` catalog session property can be used to adjust the default value of `1000` for this threshold.

#### Procedures <a href="#procedures" id="procedures"></a>

*   `system.flush_metadata_cache()`

    Flush JDBC metadata caches. For example, the following system call flushes the metadata caches for all schemas in the `example` catalog

    ```
    USE example.example_schema;
    CALL system.flush_metadata_cache();
    ```

#### Case insensitive matching[#](https://trino.io/docs/current/connector/clickhouse.html#case-insensitive-matching) <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

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

Queries against one of the tables or schemes defined in the `mapping` attributes are run against the corresponding remote entity. For example, a query against tables in the `case_insensitive_1` schema is forwarded to the CaseSensitiveName schema and a query against `case_insensitive_2` is forwarded to the `cASEsENSITIVEnAME` schema.

At the table mapping level, a query on `case_insensitive_1.table_1` as configured above is forwarded to `CaseSensitiveName.tablex`, and a query on `case_insensitive_1.table_2` is forwarded to `CaseSensitiveName.TABLEX`.

By default, when a change is made to the mapping configuration file, Trino must be restarted to load the changes. Optionally, you can set the `case-insensitive-name-mapping.refresh-period` to have Trino refresh the properties without requiring a restart:

```
case-insensitive-name-mapping.refresh-period=30s
```

#### Non-transactional INSERT <a href="#non-transactional-insert" id="non-transactional-insert"></a>

The connector supports adding rows using INSERT statements. By default, data insertion is performed by writing data to a temporary table. You can skip this step to improve performance and write directly to the target table. Set the `insert.non-transactional-insert.enabled` catalog property or the corresponding `non_transactional_insert` catalog session property to `true`.

Note that with this property enabled, data can be corrupted in rare cases where exceptions occur during the insert operation. With transactions disabled, no rollback can be performed.

### Querying ClickHouse <a href="#querying-clickhouse" id="querying-clickhouse"></a>

The ClickHouse connector provides a schema for every ClickHouse _database_. Run `SHOW SCHEMAS` to see the available ClickHouse databases:

```
SHOW SCHEMAS FROM example;
```

If you have a ClickHouse database named `web`, run `SHOW TABLES` to view the tables in this database:

```
SHOW TABLES FROM example.web;
```

Run `DESCRIBE` or `SHOW COLUMNS` to list the columns in the `clicks` table in the `web` databases:

```
DESCRIBE example.web.clicks;
SHOW COLUMNS FROM example.web.clicks;
```

Run `SELECT` to access the `clicks` table in the `web` database:

```
SELECT * FROM example.web.clicks;
```

Note

If you used a different name for your catalog properties file, use that catalog name instead of `example` in the above examples.

### Table properties[#](https://trino.io/docs/current/connector/clickhouse.html#table-properties) <a href="#table-properties" id="table-properties"></a>

Table property usage example:

```
CREATE TABLE default.trino_ck (
  id int NOT NULL,
  birthday DATE NOT NULL,
  name VARCHAR,
  age BIGINT,
  logdate DATE NOT NULL
)
WITH (
  engine = 'MergeTree',
  order_by = ARRAY['id', 'birthday'],
  partition_by = ARRAY['toYYYYMM(logdate)'],
  primary_key = ARRAY['id'],
  sample_by = 'id'
);
```

The following are supported ClickHouse table properties from [https://clickhouse.tech/docs/en/engines/table-engines/mergetree-family/mergetree/](https://clickhouse.tech/docs/en/engines/table-engines/mergetree-family/mergetree/)

| Property name  | Default value | Description                                                                                                             |
| -------------- | ------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `engine`       | `Log`         | Name and parameters of the engine.                                                                                      |
| `order_by`     | (none)        | Array of columns or expressions to concatenate to create the sorting key. Required if `engine` is `MergeTree`.          |
| `partition_by` | (none)        | Array of columns or expressions to use as nested partition keys. Optional.                                              |
| `primary_key`  | (none)        | Array of columns or expressions to concatenate to create the primary key. Optional.                                     |
| `sample_by`    | (none)        | An expression to use for [sampling](https://clickhouse.tech/docs/en/sql-reference/statements/select/sample/). Optional. |

Currently the connector only supports `Log` and `MergeTree` table engines in create table statement. `ReplicatedMergeTree` engine is not yet supported.

### Type mapping <a href="#type-mapping" id="type-mapping"></a>

Because Trino and ClickHouse each support types that the other does not, this connector modifies some types when reading or writing data. Data types may not map the same way in both directions between Trino and the data source. Refer to the following sections for type mapping in each direction.

#### ClickHouse type to Trino type mapping[#](https://trino.io/docs/current/connector/clickhouse.html#clickhouse-type-to-trino-type-mapping) <a href="#clickhouse-type-to-trino-type-mapping" id="clickhouse-type-to-trino-type-mapping"></a>

The connector maps ClickHouse types to the corresponding Trino types according to the following table:

| ClickHouse type        | Trino type                      | Notes                                                                                        |
| ---------------------- | ------------------------------- | -------------------------------------------------------------------------------------------- |
| `Int8`                 | `TINYINT`                       | `TINYINT`, `BOOL`, `BOOLEAN`, and `INT1` are aliases of `Int8`                               |
| `Int16`                | `SMALLINT`                      | `SMALLINT` and `INT2` are aliases of `Int16`                                                 |
| `Int32`                | `INTEGER`                       | `INT`, `INT4`, and `INTEGER` are aliases of `Int32`                                          |
| `Int64`                | `BIGINT`                        | `BIGINT` is an alias of `Int64`                                                              |
| `UInt8`                | `SMALLINT`                      |                                                                                              |
| `UInt16`               | `INTEGER`                       |                                                                                              |
| `UInt32`               | `BIGINT`                        |                                                                                              |
| `UInt64`               | `DECIMAL(20,0)`                 |                                                                                              |
| `Float32`              | `REAL`                          | `FLOAT` is an alias of `Float32`                                                             |
| `Float64`              | `DOUBLE`                        | `DOUBLE` is an alias of `Float64`                                                            |
| `Decimal`              | `DECIMAL`                       |                                                                                              |
| `FixedString`          | `VARBINARY`                     | Enabling `clickhouse.map-string-as-varchar` config property changes the mapping to `VARCHAR` |
| `String`               | `VARBINARY`                     | Enabling `clickhouse.map-string-as-varchar` config property changes the mapping to `VARCHAR` |
| `Date`                 | `DATE`                          |                                                                                              |
| `DateTime[(timezone)]` | `TIMESTAMP(0) [WITH TIME ZONE]` |                                                                                              |
| `IPv4`                 | `IPADDRESS`                     |                                                                                              |
| `IPv6`                 | `IPADDRESS`                     |                                                                                              |
| `Enum8`                | `VARCHAR`                       |                                                                                              |
| `Enum16`               | `VARCHAR`                       |                                                                                              |
| `UUID`                 | `UUID`                          |                                                                                              |

No other types are supported.

#### Trino type to ClickHouse type mapping <a href="#trino-type-to-clickhouse-type-mapping" id="trino-type-to-clickhouse-type-mapping"></a>

The connector maps Trino types to the corresponding ClickHouse types according to the following table:

| Trino type     | ClickHouse type | Notes                                                                                        |
| -------------- | --------------- | -------------------------------------------------------------------------------------------- |
| `BOOLEAN`      | `UInt8`         |                                                                                              |
| `TINYINT`      | `Int8`          | `TINYINT`, `BOOL`, `BOOLEAN`, and `INT1` are aliases of `Int8`                               |
| `SMALLINT`     | `Int16`         | `SMALLINT` and `INT2` are aliases of `Int16`                                                 |
| `INTEGER`      | `Int32`         | `INT`, `INT4`, and `INTEGER` are aliases of `Int32`                                          |
| `BIGINT`       | `Int64`         | `BIGINT` is an alias of `Int64`                                                              |
| `REAL`         | `Float32`       | `FLOAT` is an alias of `Float32`                                                             |
| `DOUBLE`       | `Float64`       | `DOUBLE` is an alias of `Float64`                                                            |
| `DECIMAL(p,s)` | `Decimal(p,s)`  |                                                                                              |
| `VARCHAR`      | `String`        |                                                                                              |
| `CHAR`         | `String`        |                                                                                              |
| `VARBINARY`    | `String`        | Enabling `clickhouse.map-string-as-varchar` config property changes the mapping to `VARCHAR` |
| `DATE`         | `Date`          |                                                                                              |
| `TIMESTAMP(0)` | `DateTime`      |                                                                                              |
| `UUID`         | `UUID`          |                                                                                              |

No other types are supported.

#### Type mapping configuration properties[#](https://trino.io/docs/current/connector/clickhouse.html#type-mapping-configuration-properties) <a href="#type-mapping-configuration-properties" id="type-mapping-configuration-properties"></a>

The following properties can be used to configure how data types from the connected data source are mapped to Trino data types and how the metadata is cached in Trino.

| Property name                  | Description                                                                                                                                                                                                                                                                                                              | Default value |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `unsupported-type-handling`    | <p>Configure how unsupported column data types are handled:</p><ul><li><code>IGNORE</code>, column is not accessible.</li><li><code>CONVERT_TO_VARCHAR</code>, column is converted to unbounded <code>VARCHAR</code>.</li></ul><p>The respective catalog session property is <code>unsupported_type_handling</code>.</p> | `IGNORE`      |
| `jdbc-types-mapped-to-varchar` | Allow forced mapping of comma separated lists of data types to convert to unbounded `VARCHAR`                                                                                                                                                                                                                            |               |

### SQL support[#](https://trino.io/docs/current/connector/clickhouse.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides read and write access to data and metadata in a ClickHouse catalog. In addition to the globally available and read operation statements, the connector supports the following features:

* INSERT
* TRUNCATE
* Schema and table management

#### ALTER SCHEMA# <a href="#alter-schema" id="alter-schema"></a>

The connector supports renaming a schema with the `ALTER SCHEMA RENAME` statement. `ALTER SCHEMA SET AUTHORIZATION` is not supported.

### Performance[#](https://trino.io/docs/current/connector/clickhouse.html#performance) <a href="#performance" id="performance"></a>

The connector includes a number of performance improvements, detailed in the following sections.

#### Pushdown[#](https://trino.io/docs/current/connector/clickhouse.html#pushdown) <a href="#pushdown" id="pushdown"></a>

The connector supports pushdown for a number of operations:

* Limit pushdown

Aggregate pushdown for the following functions:

* `avg()`
* `count()`
* `max()`
* `min()`
* `sum()`

Note

The connector performs pushdown where performance may be improved, but in order to preserve correctness an operation may not be pushed down. When pushdown of an operation may result in better performance but risks correctness, the connector prioritizes correctness.

**Predicate pushdown support**[**#**](https://trino.io/docs/current/connector/clickhouse.html#predicate-pushdown-support)

The connector does not support pushdown of any predicates on columns with [textual types](https://trino.io/docs/current/language/types.html#string-data-types) like `CHAR` or `VARCHAR`. This ensures correctness of results since the data source may compare strings case-insensitively.

In the following example, the predicate is not pushed down for either query since `name` is a column of type `VARCHAR`:

```
SELECT * FROM nation WHERE name > 'CANADA';
SELECT * FROM nation WHERE name = 'CANADA';
```
