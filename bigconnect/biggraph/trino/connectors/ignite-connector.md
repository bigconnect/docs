# Ignite connector

The Ignite connector allows querying an [Apache Ignite](https://ignite.apache.org/) database from Trino.

### Requirements[#](https://trino.io/docs/current/connector/ignite.html#requirements) <a href="#requirements" id="requirements"></a>

To connect to a Ignite server, you need:

* Ignite version 2.9.0 or latter
* Network access from the Trino coordinator and workers to the Ignite server. Port 10800 is the default port.
* Specify `--add-opens=java.base/java.nio=ALL-UNNAMED` in the `jvm.config` when starting the Trino server.

### Configuration[#](https://trino.io/docs/current/connector/ignite.html#configuration) <a href="#configuration" id="configuration"></a>

The Ignite connector expose `public` schema by default.

The connector can query a Ignite instance. Create a catalog properties file that specifies the Ignite connector by setting the `connector.name` to `ignite`.

For example, to access an instance as `example`, create the file `etc/catalog/example.properties`. Replace the connection properties as appropriate for your setup:

```
connector.name=ignite
connection-url=jdbc:ignite:thin://host1:10800/
connection-user=exampleuser
connection-password=examplepassword
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `connection-url` defines the connection information and parameters to pass to the Ignite JDBC driver. The parameters for the URL are available in the [Ignite JDBC driver documentation](https://ignite.apache.org/docs/latest/SQL/JDBC/jdbc-driver). Some parameters can have adverse effects on the connector behavior or not work with the connector.

The `connection-user` and `connection-password` are typically required and determine the user credentials for the connection, often a service user. You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid actual values in the catalog properties files.

#### Multiple Ignite servers[#](https://trino.io/docs/current/connector/ignite.html#multiple-ignite-servers) <a href="#multiple-ignite-servers" id="multiple-ignite-servers"></a>

If you have multiple Ignite servers you need to configure one catalog for each server. To add another catalog:

* Add another properties file to `etc/catalog`
* Save it with a different name that ends in `.properties`

For example, if you name the property file `sales.properties`, Trino uses the configured connector to create a catalog named `sales`.

#### General configuration properties[#](https://trino.io/docs/current/connector/ignite.html#general-configuration-properties) <a href="#general-configuration-properties" id="general-configuration-properties"></a>

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

#### Appending query metadata[#](https://trino.io/docs/current/connector/ignite.html#appending-query-metadata) <a href="#appending-query-metadata" id="appending-query-metadata"></a>

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

#### Domain compaction threshold[#](https://trino.io/docs/current/connector/ignite.html#domain-compaction-threshold) <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improve performance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large dynamic filters. The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` catalog session property can be used to adjust the default value of `1000` for this threshold.

#### Procedures[#](https://trino.io/docs/current/connector/ignite.html#procedures) <a href="#procedures" id="procedures"></a>

*   `system.flush_metadata_cache()`

    Flush JDBC metadata caches. For example, the following system call flushes the metadata caches for all schemas in the `example` catalog

    ```
    USE example.example_schema;
    CALL system.flush_metadata_cache();
    ```

    ![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Case insensitive matching[#](https://trino.io/docs/current/connector/ignite.html#case-insensitive-matching) <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

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

#### Non-transactional INSERT[#](https://trino.io/docs/current/connector/ignite.html#non-transactional-insert) <a href="#non-transactional-insert" id="non-transactional-insert"></a>

The connector supports adding rows using [INSERT statements](https://trino.io/docs/current/sql/insert.html). By default, data insertion is performed by writing data to a temporary table. You can skip this step to improve performance and write directly to the target table. Set the `insert.non-transactional-insert.enabled` catalog property or the corresponding `non_transactional_insert` catalog session property to `true`.

Note that with this property enabled, data can be corrupted in rare cases where exceptions occur during the insert operation. With transactions disabled, no rollback can be performed.

### Table properties[#](https://trino.io/docs/current/connector/ignite.html#table-properties) <a href="#table-properties" id="table-properties"></a>

Table property usage example:

```
CREATE TABLE public.person (
  id BIGINT NOT NULL,
  birthday DATE NOT NULL,
  name VARCHAR(26),
  age BIGINT,
  logdate DATE
)
WITH (
  primary_key = ARRAY['id', 'birthday']
);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following are supported Ignite table properties from [https://ignite.apache.org/docs/latest/sql-reference/ddl](https://ignite.apache.org/docs/latest/sql-reference/ddl)

| Property name | Required | Description                                                                                                                            |
| ------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `primary_key` | No       | The primary key of the table, can chose multi columns as the table primary key. Table at least contains one column not in primary key. |

#### `primary_key`[#](https://trino.io/docs/current/connector/ignite.html#primary-key) <a href="#primary-key" id="primary-key"></a>

This is a list of columns to be used as the table’s primary key. If not specified, a `VARCHAR` primary key column named `DUMMY_ID` is generated, the value is derived from the value generated by the `UUID` function in Ignite.

### Type mapping[#](https://trino.io/docs/current/connector/ignite.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

The following are supported Ignite SQL data types from [https://ignite.apache.org/docs/latest/sql-reference/data-types](https://ignite.apache.org/docs/latest/sql-reference/data-types)

| Ignite SQL data type name | Map to Trino type | Possible values                                     |
| ------------------------- | ----------------- | --------------------------------------------------- |
| `BOOLEAN`                 | `BOOLEAN`         | `TRUE` and `FALSE`                                  |
| `BIGINT`                  | `BIGINT`          | `-9223372036854775808`, `9223372036854775807`, etc. |
| `DECIMAL`                 | `DECIMAL`         | Data type with fixed precision and scale            |
| `DOUBLE`                  | `DOUBLE`          | `3.14`, `-10.24`, etc.                              |
| `INT`                     | `INT`             | `-2147483648`, `2147483647`, etc.                   |
| `REAL`                    | `REAL`            | `3.14`, `-10.24`, etc.                              |
| `SMALLINT`                | `SMALLINT`        | `-32768`, `32767`, etc.                             |
| `TINYINT`                 | `TINYINT`         | `-128`, `127`, etc.                                 |
| `CHAR`                    | `CHAR`            | `hello`, `Trino`, etc.                              |
| `VARCHAR`                 | `VARCHAR`         | `hello`, `Trino`, etc.                              |
| `DATE`                    | `DATE`            | `1972-01-01`, `2021-07-15`, etc.                    |
| `BINARY`                  | `VARBINARY`       | Represents a byte array.                            |

### SQL support[#](https://trino.io/docs/current/connector/ignite.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides read access and write access to data and metadata in Ignite. In addition to the globally available and read operation statements, the connector supports the following features:

* INSERT
* UPDATE
* DELETE
* CREATE TABLE
* CREATE TABLE AS
* DROP TABLE
* ALTER TABLE

#### UPDATE# <a href="#update" id="update"></a>

Only `UPDATE` statements with constant assignments and predicates are supported. For example, the following statement is supported because the values assigned are constants:

```
UPDATE table SET col1 = 1 WHERE col3 = 1
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Arithmetic expressions, function calls, and other non-constant `UPDATE` statements are not supported. For example, the following statement is not supported because arithmetic expressions cannot be used with the `SET` command:

```
UPDATE table SET col1 = col2 + 2 WHERE col3 = 1
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `=`, `!=`, `>`, `<`, `>=`, `<=`, `IN`, `NOT IN` operators are supported in predicates. The following statement is not supported because the `AND` operator cannot be used in predicates:

```
UPDATE table SET col1 = 1 WHERE col3 = 1 AND col2 = 3
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

All column values of a table row cannot be updated simultaneously. For a three column table, the following statement is not supported:

```
UPDATE table SET col1 = 1, col2 = 2, col3 = 3 WHERE col3 = 1
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### ALTER TABLE RENAME TO[#](https://trino.io/docs/current/connector/ignite.html#alter-table-rename-to) <a href="#alter-table-rename-to" id="alter-table-rename-to"></a>

The connector does not support renaming tables across multiple schemas. For example, the following statement is supported:

```
ALTER TABLE example.schema_one.table_one RENAME TO example.schema_one.table_two
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following statement attempts to rename a table across schemas, and therefore is not supported:

```
ALTER TABLE example.schema_one.table_one RENAME TO example.schema_two.table_two
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Pushdown[#](https://trino.io/docs/current/connector/ignite.html#pushdown) <a href="#pushdown" id="pushdown"></a>

The connector supports pushdown for a number of operations:

* Join pushdown
* Limit pushdown
* Top-N pushdown

Aggregate pushdown for the following functions:

* `avg()`
* `count()`
* `max()`
* `min()`
* `sum()`

**Predicate pushdown support#**

The connector does not support pushdown of any predicates on columns with textual types like `CHAR` or `VARCHAR`. This ensures correctness of results since the data source may compare strings case-insensitively.

In the following example, the predicate is not pushed down for either query since `name` is a column of type `VARCHAR`:

```
SELECT * FROM nation WHERE name > 'CANADA';
SELECT * FROM nation WHERE name = 'CANADA';
```
