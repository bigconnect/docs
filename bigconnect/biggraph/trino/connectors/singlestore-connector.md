# SingleStore connector

The SingleStore (formerly known as MemSQL) connector allows querying and creating tables in an external SingleStore database.

### Requirements[#](https://trino.io/docs/current/connector/singlestore.html#requirements) <a href="#requirements" id="requirements"></a>

To connect to SingleStore, you need:

* SingleStore version 7.1.4 or higher.
* Network access from the Trino coordinator and workers to SingleStore. Port 3306 is the default port.

### Configuration[#](https://trino.io/docs/current/connector/singlestore.html#configuration) <a href="#configuration" id="configuration"></a>

To configure the SingleStore connector, create a catalog properties file in `etc/catalog` named, for example, `example.properties`, to mount the SingleStore connector as the `example` catalog. Create the file with the following contents, replacing the connection properties as appropriate for your setup:

```
connector.name=singlestore
connection-url=jdbc:singlestore://example.net:3306
connection-user=root
connection-password=secret
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `connection-url` defines the connection information and parameters to pass to the SingleStore JDBC driver. The supported parameters for the URL are available in the [SingleStore JDBC driver documentation](https://docs.singlestore.com/db/v7.6/en/developer-resources/connect-with-application-development-tools/connect-with-java-jdbc/the-singlestore-jdbc-driver.html#connection-string-parameters).

The `connection-user` and `connection-password` are typically required and determine the user credentials for the connection, often a service user. You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid actual values in the catalog properties files.

#### Connection security[#](https://trino.io/docs/current/connector/singlestore.html#connection-security) <a href="#connection-security" id="connection-security"></a>

If you have TLS configured with a globally-trusted certificate installed on your data source, you can enable TLS between your cluster and the data source by appending a parameter to the JDBC connection string set in the `connection-url` catalog configuration property.

Enable TLS between your cluster and SingleStore by appending the `useSsl=true` parameter to the `connection-url` configuration property:

```
connection-url=jdbc:singlestore://example.net:3306/?useSsl=true
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

For more information on TLS configuration options, see the [JDBC driver documentation](https://docs.singlestore.com/db/v7.6/en/developer-resources/connect-with-application-development-tools/connect-with-java-jdbc/the-singlestore-jdbc-driver.html#tls-parameters).

#### Multiple SingleStore servers[#](https://trino.io/docs/current/connector/singlestore.html#multiple-singlestore-servers) <a href="#multiple-singlestore-servers" id="multiple-singlestore-servers"></a>

You can have as many catalogs as you need, so if you have additional SingleStore servers, simply add another properties file to `etc/catalog` with a different name (making sure it ends in `.properties`). For example, if you name the property file `sales.properties`, Trino will create a catalog named `sales` using the configured connector.

#### General configuration properties[#](https://trino.io/docs/current/connector/singlestore.html#general-configuration-properties) <a href="#general-configuration-properties" id="general-configuration-properties"></a>

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

#### Appending query metadata[#](https://trino.io/docs/current/connector/singlestore.html#appending-query-metadata) <a href="#appending-query-metadata" id="appending-query-metadata"></a>

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

#### Domain compaction threshold[#](https://trino.io/docs/current/connector/singlestore.html#domain-compaction-threshold) <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improve performance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large [dynamic filters](https://trino.io/docs/current/admin/dynamic-filtering.html). The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` [catalog session property](https://trino.io/docs/current/sql/set-session.html#session-properties-definition) can be used to adjust the default value of `32` for this threshold.

#### Procedures[#](https://trino.io/docs/current/connector/singlestore.html#procedures) <a href="#procedures" id="procedures"></a>

*   `system.flush_metadata_cache()`

    Flush JDBC metadata caches. For example, the following system call flushes the metadata caches for all schemas in the `example` catalog

    ```
    USE example.example_schema;
    CALL system.flush_metadata_cache();
    ```

    ![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Case insensitive matching[#](https://trino.io/docs/current/connector/singlestore.html#case-insensitive-matching) <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

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

#### Non-transactional INSERT[#](https://trino.io/docs/current/connector/singlestore.html#non-transactional-insert) <a href="#non-transactional-insert" id="non-transactional-insert"></a>

The connector supports adding rows using [INSERT statements](https://trino.io/docs/current/sql/insert.html). By default, data insertion is performed by writing data to a temporary table. You can skip this step to improve performance and write directly to the target table. Set the `insert.non-transactional-insert.enabled` catalog property or the corresponding `non_transactional_insert` catalog session property to `true`.

Note that with this property enabled, data can be corrupted in rare cases where exceptions occur during the insert operation. With transactions disabled, no rollback can be performed.

### Querying SingleStore[#](https://trino.io/docs/current/connector/singlestore.html#querying-singlestore) <a href="#querying-singlestore" id="querying-singlestore"></a>

The SingleStore connector provides a schema for every SingleStore _database_. You can see the available SingleStore databases by running `SHOW SCHEMAS`:

```
SHOW SCHEMAS FROM example;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you have a SingleStore database named `web`, you can view the tables in this database by running `SHOW TABLES`:

```
SHOW TABLES FROM example.web;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can see a list of the columns in the `clicks` table in the `web` database using either of the following:

```
DESCRIBE example.web.clicks;
SHOW COLUMNS FROM example.web.clicks;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Finally, you can access the `clicks` table in the `web` database:

```
SELECT * FROM example.web.clicks;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you used a different name for your catalog properties file, use that catalog name instead of `example` in the above examples.

### Type mapping[#](https://trino.io/docs/current/connector/singlestore.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

Because Trino and Singlestore each support types that the other does not, this connector [modifies some types](https://trino.io/docs/current/language/types.html#type-mapping-overview) when reading or writing data. Data types may not map the same way in both directions between Trino and the data source. Refer to the following sections for type mapping in each direction.

#### Singlestore to Trino type mapping[#](https://trino.io/docs/current/connector/singlestore.html#singlestore-to-trino-type-mapping) <a href="#singlestore-to-trino-type-mapping" id="singlestore-to-trino-type-mapping"></a>

The connector maps Singlestore types to the corresponding Trino types following this table:

| Singlestore type    | Trino type          | Notes                                                                                                                          |
| ------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `BIT`               | `BOOLEAN`           |                                                                                                                                |
| `BOOLEAN`           | `BOOLEAN`           |                                                                                                                                |
| `TINYINT`           | `TINYINT`           |                                                                                                                                |
| `TINYINT UNSIGNED`  | `SMALLINT`          |                                                                                                                                |
| `SMALLINT`          | `SMALLINT`          |                                                                                                                                |
| `SMALLINT UNSIGNED` | `INTEGER`           |                                                                                                                                |
| `INTEGER`           | `INTEGER`           |                                                                                                                                |
| `INTEGER UNSIGNED`  | `BIGINT`            |                                                                                                                                |
| `BIGINT`            | `BIGINT`            |                                                                                                                                |
| `BIGINT UNSIGNED`   | `DECIMAL(20, 0)`    |                                                                                                                                |
| `DOUBLE`            | `DOUBLE`            |                                                                                                                                |
| `REAL`              | `DOUBLE`            |                                                                                                                                |
| `DECIMAL(p, s)`     | `DECIMAL(p, s)`     | See [Singlestore DECIMAL type handling](https://trino.io/docs/current/connector/singlestore.html#singlestore-decimal-handling) |
| `CHAR(n)`           | `CHAR(n)`           |                                                                                                                                |
| `TINYTEXT`          | `VARCHAR(255)`      |                                                                                                                                |
| `TEXT`              | `VARCHAR(65535)`    |                                                                                                                                |
| `MEDIUMTEXT`        | `VARCHAR(16777215)` |                                                                                                                                |
| `LONGTEXT`          | `VARCHAR`           |                                                                                                                                |
| `VARCHAR(n)`        | `VARCHAR(n)`        |                                                                                                                                |
| `LONGBLOB`          | `VARBINARY`         |                                                                                                                                |
| `DATE`              | `DATE`              |                                                                                                                                |
| `TIME`              | `TIME(0)`           |                                                                                                                                |
| `TIME(6)`           | `TIME(6)`           |                                                                                                                                |
| `DATETIME`          | `TIMESTAMP(0)`      |                                                                                                                                |
| `DATETIME(6)`       | `TIMESTAMP(6)`      |                                                                                                                                |
| `JSON`              | `JSON`              |                                                                                                                                |

No other types are supported.

#### Trino to Singlestore type mapping[#](https://trino.io/docs/current/connector/singlestore.html#trino-to-singlestore-type-mapping) <a href="#trino-to-singlestore-type-mapping" id="trino-to-singlestore-type-mapping"></a>

The connector maps Trino types to the corresponding Singlestore types following this table:

| Trino type          | Singlestore type | Notes                                                                                                                          |
| ------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `BOOLEAN`           | `BOOLEAN`        |                                                                                                                                |
| `TINYINT`           | `TINYINT`        |                                                                                                                                |
| `SMALLINT`          | `SMALLINT`       |                                                                                                                                |
| `INTEGER`           | `INTEGER`        |                                                                                                                                |
| `BIGINT`            | `BIGINT`         |                                                                                                                                |
| `DOUBLE`            | `DOUBLE`         |                                                                                                                                |
| `REAL`              | `FLOAT`          |                                                                                                                                |
| `DECIMAL(p, s)`     | `DECIMAL(p, s)`  | See [Singlestore DECIMAL type handling](https://trino.io/docs/current/connector/singlestore.html#singlestore-decimal-handling) |
| `CHAR(n)`           | `CHAR(n)`        |                                                                                                                                |
| `VARCHAR(65535)`    | `TEXT`           |                                                                                                                                |
| `VARCHAR(16777215)` | `MEDIUMTEXT`     |                                                                                                                                |
| `VARCHAR`           | `LONGTEXT`       |                                                                                                                                |
| `VARCHAR(n)`        | `VARCHAR(n)`     |                                                                                                                                |
| `VARBINARY`         | `LONGBLOB`       |                                                                                                                                |
| `DATE`              | `DATE`           |                                                                                                                                |
| `TIME(0)`           | `TIME`           |                                                                                                                                |
| `TIME(6)`           | `TIME(6)`        |                                                                                                                                |
| `TIMESTAMP(0)`      | `DATETIME`       |                                                                                                                                |
| `TIMESTAMP(6)`      | `DATETIME(6)`    |                                                                                                                                |
| `JSON`              | `JSON`           |                                                                                                                                |

No other types are supported.

#### Decimal type handling[#](https://trino.io/docs/current/connector/singlestore.html#decimal-type-handling) <a href="#decimal-type-handling" id="decimal-type-handling"></a>

`DECIMAL` types with unspecified precision or scale are ignored unless the `decimal-mapping` configuration property or the `decimal_mapping` session property is set to `allow_overflow`. Then such types are mapped to a Trino `DECIMAL` with a default precision of 38 and default scale of 0. To change the scale of the resulting type, use the `decimal-default-scale` configuration property or the `decimal_default_scale` session property. The precision is always 38.

By default, values that require rounding or truncation to fit will cause a failure at runtime. This behavior is controlled via the `decimal-rounding-mode` configuration property or the `decimal_rounding_mode` session property, which can be set to `UNNECESSARY` (the default), `UP`, `DOWN`, `CEILING`, `FLOOR`, `HALF_UP`, `HALF_DOWN`, or `HALF_EVEN` (see [RoundingMode](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/math/RoundingMode.html#enum.constant.summary)).

#### Type mapping configuration properties[#](https://trino.io/docs/current/connector/singlestore.html#type-mapping-configuration-properties) <a href="#type-mapping-configuration-properties" id="type-mapping-configuration-properties"></a>

The following properties can be used to configure how data types from the connected data source are mapped to Trino data types and how the metadata is cached in Trino.

| Property name                  | Description                                                                                                                                                                                                                                                                                                              | Default value |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `unsupported-type-handling`    | <p>Configure how unsupported column data types are handled:</p><ul><li><code>IGNORE</code>, column is not accessible.</li><li><code>CONVERT_TO_VARCHAR</code>, column is converted to unbounded <code>VARCHAR</code>.</li></ul><p>The respective catalog session property is <code>unsupported_type_handling</code>.</p> | `IGNORE`      |
| `jdbc-types-mapped-to-varchar` | Allow forced mapping of comma separated lists of data types to convert to unbounded `VARCHAR`                                                                                                                                                                                                                            |               |

### SQL support[#](https://trino.io/docs/current/connector/singlestore.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides read access and write access to data and metadata in a SingleStore database. In addition to the [globally available](https://trino.io/docs/current/language/sql-support.html#sql-globally-available) and [read operation](https://trino.io/docs/current/language/sql-support.html#sql-read-operations) statements, the connector supports the following features:

* [INSERT](https://trino.io/docs/current/sql/insert.html)
* [UPDATE](https://trino.io/docs/current/sql/update.html)
* [DELETE](https://trino.io/docs/current/sql/delete.html)
* [TRUNCATE](https://trino.io/docs/current/sql/truncate.html)
* [CREATE TABLE](https://trino.io/docs/current/sql/create-table.html)
* [CREATE TABLE AS](https://trino.io/docs/current/sql/create-table-as.html)
* [DROP TABLE](https://trino.io/docs/current/sql/drop-table.html)
* [ALTER TABLE](https://trino.io/docs/current/sql/alter-table.html)
* [CREATE SCHEMA](https://trino.io/docs/current/sql/create-schema.html)
* [DROP SCHEMA](https://trino.io/docs/current/sql/drop-schema.html)

#### UPDATE[#](https://trino.io/docs/current/connector/singlestore.html#update) <a href="#update" id="update"></a>

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

#### SQL DELETE[#](https://trino.io/docs/current/connector/singlestore.html#sql-delete) <a href="#sql-delete" id="sql-delete"></a>

If a `WHERE` clause is specified, the `DELETE` operation only works if the predicate in the clause can be fully pushed down to the data source.

#### ALTER TABLE RENAME TO[#](https://trino.io/docs/current/connector/singlestore.html#alter-table-rename-to) <a href="#alter-table-rename-to" id="alter-table-rename-to"></a>

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

### Performance[#](https://trino.io/docs/current/connector/singlestore.html#performance) <a href="#performance" id="performance"></a>

The connector includes a number of performance improvements, detailed in the following sections.

#### Pushdown[#](https://trino.io/docs/current/connector/singlestore.html#pushdown) <a href="#pushdown" id="pushdown"></a>

The connector supports pushdown for a number of operations:

* [Join pushdown](https://trino.io/docs/current/optimizer/pushdown.html#join-pushdown)
* [Limit pushdown](https://trino.io/docs/current/optimizer/pushdown.html#limit-pushdown)
* [Top-N pushdown](https://trino.io/docs/current/optimizer/pushdown.html#topn-pushdown)

Note

The connector performs pushdown where performance may be improved, but in order to preserve correctness an operation may not be pushed down. When pushdown of an operation may result in better performance but risks correctness, the connector prioritizes correctness.

**Join pushdown**[**#**](https://trino.io/docs/current/connector/singlestore.html#join-pushdown)

The `join-pushdown.enabled` catalog configuration property or `join_pushdown_enabled` [catalog session property](https://trino.io/docs/current/sql/set-session.html#session-properties-definition) control whether the connector pushes down join operations. The property defaults to `false`, and enabling join pushdowns may negatively impact performance for some queries.

**Predicate pushdown support**[**#**](https://trino.io/docs/current/connector/singlestore.html#predicate-pushdown-support)

The connector does not support pushdown of any predicates on columns with [textual types](https://trino.io/docs/current/language/types.html#string-data-types) like `CHAR` or `VARCHAR`. This ensures correctness of results since the data source may compare strings case-insensitively.

In the following example, the predicate is not pushed down for either query since `name` is a column of type `VARCHAR`:

```
SELECT * FROM nation WHERE name > 'CANADA';
SELECT * FROM nation WHERE name = 'CANADA';
```
