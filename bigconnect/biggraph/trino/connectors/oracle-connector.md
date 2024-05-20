# Oracle connector

The Oracle connector allows querying and creating tables in an external Oracle database. Connectors let Trino join data provided by different databases, like Oracle and Hive, or different Oracle database instances.

### Requirements[#](https://trino.io/docs/current/connector/oracle.html#requirements) <a href="#requirements" id="requirements"></a>

To connect to Oracle, you need:

* Oracle 19 or higher.
* Network access from the Trino coordinator and workers to Oracle. Port 1521 is the default port.

### Configuration[#](https://trino.io/docs/current/connector/oracle.html#configuration) <a href="#configuration" id="configuration"></a>

To configure the Oracle connector as the `example` catalog, create a file named `example.properties` in `etc/catalog`. Include the following connection properties in the file:

```
connector.name=oracle
# The correct syntax of the connection-url varies by Oracle version and
# configuration. The following example URL connects to an Oracle SID named
# "orcl".
connection-url=jdbc:oracle:thin:@example.net:1521:orcl
connection-user=root
connection-password=secret
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `connection-url` defines the connection information and parameters to pass to the JDBC driver. The Oracle connector uses the Oracle JDBC Thin driver, and the syntax of the URL may be different depending on your Oracle configuration. For example, the connection URL is different if you are connecting to an Oracle SID or an Oracle service name. See the [Oracle Database JDBC driver documentation](https://docs.oracle.com/en/database/oracle/oracle-database/19/jjdbc/data-sources-and-URLs.html) for more information.

The `connection-user` and `connection-password` are typically required and determine the user credentials for the connection, often a service user. You can use secrets to avoid actual values in the catalog properties files.

Note

Oracle does not expose metadata comment via `REMARKS` column by default in JDBC driver. You can enable it using `oracle.remarks-reporting.enabled` config option. See [Additional Oracle Performance Extensions](https://docs.oracle.com/en/database/oracle/oracle-database/19/jjdbc/performance-extensions.html) for more details.

By default, the Oracle connector uses connection pooling for performance improvement. The below configuration shows the typical default values. To update them, change the properties in the catalog configuration file:

```
oracle.connection-pool.max-size=30
oracle.connection-pool.min-size=1
oracle.connection-pool.inactive-timeout=20m
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

To disable connection pooling, update properties to include the following:

```
oracle.connection-pool.enabled=false
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Data source authentication[#](https://trino.io/docs/current/connector/oracle.html#data-source-authentication) <a href="#data-source-authentication" id="data-source-authentication"></a>

The connector can provide credentials for the data source connection in multiple ways:

* inline, in the connector configuration file
* in a separate properties file
* in a key store file
* as extra credentials set when connecting to Trino

You can use secrets to avoid storing sensitive values in the catalog properties files.

The following table describes configuration properties for connection credentials:

| Property name                           | Description                                                                                                                                                                                            |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `credential-provider.type`              | Type of the credential provider. Must be one of `INLINE`, `FILE`, or `KEYSTORE`; defaults to `INLINE`.                                                                                                 |
| `connection-user`                       | Connection user name.                                                                                                                                                                                  |
| `connection-password`                   | Connection password.                                                                                                                                                                                   |
| `user-credential-name`                  | Name of the extra credentials property, whose value to use as the user name. See `extraCredentials` in [Parameter reference](https://trino.io/docs/current/client/jdbc.html#jdbc-parameter-reference). |
| `password-credential-name`              | Name of the extra credentials property, whose value to use as the password.                                                                                                                            |
| `connection-credential-file`            | Location of the properties file where credentials are present. It must contain the `connection-user` and `connection-password` properties.                                                             |
| `keystore-file-path`                    | The location of the Java Keystore file, from which to read credentials.                                                                                                                                |
| `keystore-type`                         | File format of the keystore file, for example `JKS` or `PEM`.                                                                                                                                          |
| `keystore-password`                     | Password for the key store.                                                                                                                                                                            |
| `keystore-user-credential-name`         | Name of the key store entity to use as the user name.                                                                                                                                                  |
| `keystore-user-credential-password`     | Password for the user name key store entity.                                                                                                                                                           |
| `keystore-password-credential-name`     | Name of the key store entity to use as the password.                                                                                                                                                   |
| `keystore-password-credential-password` | Password for the password key store entity.                                                                                                                                                            |

#### Multiple Oracle servers[#](https://trino.io/docs/current/connector/oracle.html#multiple-oracle-servers) <a href="#multiple-oracle-servers" id="multiple-oracle-servers"></a>

If you want to connect to multiple Oracle servers, configure another instance of the Oracle connector as a separate catalog.

To add another Oracle catalog, create a new properties file. For example, if you name the property file `sales.properties`, Trino creates a catalog named `sales`.

#### General configuration properties[#](https://trino.io/docs/current/connector/oracle.html#general-configuration-properties) <a href="#general-configuration-properties" id="general-configuration-properties"></a>

The following table describes general catalog configuration properties for the connector:

| Property name                                               | Description                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `case-insensitive-name-matching`                            | Support case insensitive schema and table names. Defaults to `false`.                                                                                                                                                                                                                                                                                                     |
| `case-insensitive-name-matching.cache-ttl`                  | Duration for which case insensitive schema and table names are cached. Defaults to `1m`.                                                                                                                                                                                                                                                                                  |
| `case-insensitive-name-matching.config-file`                | Path to a name mapping configuration file in JSON format that allows Trino to disambiguate between schemas and tables with similar names in different cases. Defaults to `null`.                                                                                                                                                                                          |
| `case-insensitive-name-matching.config-file.refresh-period` | Frequency with which Trino checks the name matching configuration file for changes. The duration value defaults to `0s` (refresh disabled).                                                                                                                                                                                                                               |
| `metadata.cache-ttl`                                        | Duration for which metadata, including table and column statistics, is cached. Defaults to `0s` (caching disabled).                                                                                                                                                                                                                                                       |
| `metadata.cache-missing`                                    | Cache the fact that metadata, including table and column statistics, is not available. Defaults to `false`.                                                                                                                                                                                                                                                               |
| `metadata.schemas.cache-ttl`                                | Duration for which schema metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                                                                                              |
| `metadata.tables.cache-ttl`                                 | Duration for which table metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                                                                                               |
| `metadata.statistics.cache-ttl`                             | Duration for which tables statistics are cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                                                                                           |
| `metadata.cache-maximum-size`                               | Maximum number of objects stored in the metadata cache. Defaults to `10000`.                                                                                                                                                                                                                                                                                              |
| `write.batch-size`                                          | Maximum number of statements in a batched execution. Do not change this setting from the default. Non-default values may negatively impact performance. Defaults to `1000`.                                                                                                                                                                                               |
| `dynamic-filtering.enabled`                                 | Push down dynamic filters into JDBC queries. Defaults to `true`.                                                                                                                                                                                                                                                                                                          |
| `dynamic-filtering.wait-timeout`                            | Maximum [duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which Trino waits for dynamic filters to be collected from the build side of joins before starting a JDBC query. Using a large timeout can potentially result in more detailed dynamic filters. However, it can also increase latency for some queries. Defaults to `20s`. |

#### Appending query metadata[#](https://trino.io/docs/current/connector/oracle.html#appending-query-metadata) <a href="#appending-query-metadata" id="appending-query-metadata"></a>

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

#### Domain compaction threshold[#](https://trino.io/docs/current/connector/oracle.html#domain-compaction-threshold) <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improve performance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large dynamic filters. The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` catalog session property can be used to adjust the default value of `32` for this threshold.

#### Procedures[#](https://trino.io/docs/current/connector/oracle.html#procedures) <a href="#procedures" id="procedures"></a>

*   `system.flush_metadata_cache()`

    Flush JDBC metadata caches. For example, the following system call flushes the metadata caches for all schemas in the `example` catalog

    ```
    USE example.example_schema;
    CALL system.flush_metadata_cache()
    ```

#### Case insensitive matching[#](https://trino.io/docs/current/connector/oracle.html#case-insensitive-matching) <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

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



#### Non-transactional INSERT[#](https://trino.io/docs/current/connector/oracle.html#non-transactional-insert) <a href="#non-transactional-insert" id="non-transactional-insert"></a>

The connector supports adding rows using [INSERT statements](https://trino.io/docs/current/sql/insert.html). By default, data insertion is performed by writing data to a temporary table. You can skip this step to improve performance and write directly to the target table. Set the `insert.non-transactional-insert.enabled` catalog property or the corresponding `non_transactional_insert` catalog session property to `true`.

Note that with this property enabled, data can be corrupted in rare cases where exceptions occur during the insert operation. With transactions disabled, no rollback can be performed.

### Querying Oracle[#](https://trino.io/docs/current/connector/oracle.html#querying-oracle) <a href="#querying-oracle" id="querying-oracle"></a>

The Oracle connector provides a schema for every Oracle database.

Run `SHOW SCHEMAS` to see the available Oracle databases:

```
SHOW SCHEMAS FROM example;
```



If you used a different name for your catalog properties file, use that catalog name instead of `example`.

Note

The Oracle user must have access to the table in order to access it from Trino. The user configuration, in the connection properties file, determines your privileges in these schemas.

#### Examples[#](https://trino.io/docs/current/connector/oracle.html#examples) <a href="#examples" id="examples"></a>

If you have an Oracle database named `web`, run `SHOW TABLES` to see the tables it contains:

```
SHOW TABLES FROM example.web;
```



To see a list of the columns in the `clicks` table in the `web` database, run either of the following:

```
DESCRIBE example.web.clicks;
SHOW COLUMNS FROM example.web.clicks;
```



To access the clicks table in the web database, run the following:

```
SELECT * FROM example.web.clicks;
```



### Type mapping[#](https://trino.io/docs/current/connector/oracle.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

Because Trino and Oracle each support types that the other does not, this connector modifies some types when reading or writing data. Data types may not map the same way in both directions between Trino and the data source. Refer to the following sections for type mapping in each direction.

#### Oracle to Trino type mapping[#](https://trino.io/docs/current/connector/oracle.html#oracle-to-trino-type-mapping) <a href="#oracle-to-trino-type-mapping" id="oracle-to-trino-type-mapping"></a>

Trino supports selecting Oracle database types. This table shows the Oracle to Trino data type mapping:

| Oracle database type          | Trino type                 | Notes                      |
| ----------------------------- | -------------------------- | -------------------------- |
| `NUMBER(p, s)`                | `DECIMAL(p, s)`            | See Mapping numeric types  |
| `NUMBER(p)`                   | `DECIMAL(p, 0)`            | See Mapping numeric types  |
| `FLOAT[(p)]`                  | `DOUBLE`                   |                            |
| `BINARY_FLOAT`                | `REAL`                     |                            |
| `BINARY_DOUBLE`               | `DOUBLE`                   |                            |
| `VARCHAR2(n CHAR)`            | `VARCHAR(n)`               |                            |
| `VARCHAR2(n BYTE)`            | `VARCHAR(n)`               |                            |
| `NVARCHAR2(n)`                | `VARCHAR(n)`               |                            |
| `CHAR(n)`                     | `CHAR(n)`                  |                            |
| `NCHAR(n)`                    | `CHAR(n)`                  |                            |
| `CLOB`                        | `VARCHAR`                  |                            |
| `NCLOB`                       | `VARCHAR`                  |                            |
| `RAW(n)`                      | `VARBINARY`                |                            |
| `BLOB`                        | `VARBINARY`                |                            |
| `DATE`                        | `TIMESTAMP(0)`             | See Mapping datetime types |
| `TIMESTAMP(p)`                | `TIMESTAMP(p)`             | See Mapping datetime types |
| `TIMESTAMP(p) WITH TIME ZONE` | `TIMESTAMP WITH TIME ZONE` | See Mapping datetime types |

No other types are supported.

#### Trino to Oracle type mapping[#](https://trino.io/docs/current/connector/oracle.html#trino-to-oracle-type-mapping) <a href="#trino-to-oracle-type-mapping" id="trino-to-oracle-type-mapping"></a>

Trino supports creating tables with the following types in an Oracle database. The table shows the mappings from Trino to Oracle data types:

Note

For types not listed in the table below, Trino can’t perform the `CREATE TABLE <table> AS SELECT` operations. When data is inserted into existing tables, `Oracle to Trino` type mapping is used.

| Trino type                 | Oracle database type          | Notes                       |
| -------------------------- | ----------------------------- | --------------------------- |
| `TINYINT`                  | `NUMBER(3)`                   |                             |
| `SMALLINT`                 | `NUMBER(5)`                   |                             |
| `INTEGER`                  | `NUMBER(10)`                  |                             |
| `BIGINT`                   | `NUMBER(19)`                  |                             |
| `DECIMAL(p, s)`            | `NUMBER(p, s)`                |                             |
| `REAL`                     | `BINARY_FLOAT`                |                             |
| `DOUBLE`                   | `BINARY_DOUBLE`               |                             |
| `VARCHAR`                  | `NCLOB`                       |                             |
| `VARCHAR(n)`               | `VARCHAR2(n CHAR)` or `NCLOB` | See Mapping character types |
| `CHAR(n)`                  | `CHAR(n CHAR)` or `NCLOB`     | See Mapping character types |
| `VARBINARY`                | `BLOB`                        |                             |
| `DATE`                     | `DATE`                        | See Mapping character types |
| `TIMESTAMP`                | `TIMESTAMP(3)`                | See Mapping character types |
| `TIMESTAMP WITH TIME ZONE` | `TIMESTAMP(3) WITH TIME ZONE` | See Mapping character types |

No other types are supported.

#### Mapping numeric types[#](https://trino.io/docs/current/connector/oracle.html#mapping-numeric-types) <a href="#mapping-numeric-types" id="mapping-numeric-types"></a>

An Oracle `NUMBER(p, s)` maps to Trino’s `DECIMAL(p, s)` except in these conditions:

* No precision is specified for the column (example: `NUMBER` or `NUMBER(*)`), unless `oracle.number.default-scale` is set.
* Scale (`s` ) is greater than precision.
* Precision (`p` ) is greater than 38.
* Scale is negative and the difference between `p` and `s` is greater than 38, unless `oracle.number.rounding-mode` is set to a different value than `UNNECESSARY`.

If `s` is negative, `NUMBER(p, s)` maps to `DECIMAL(p + s, 0)`.

For Oracle `NUMBER` (without precision and scale), you can change `oracle.number.default-scale=s` and map the column to `DECIMAL(38, s)`.

#### Mapping datetime types[#](https://trino.io/docs/current/connector/oracle.html#mapping-datetime-types) <a href="#mapping-datetime-types" id="mapping-datetime-types"></a>

Writing a timestamp with fractional second precision (`p`) greater than 9 rounds the fractional seconds to nine digits.

Oracle `DATE` type stores hours, minutes, and seconds, so it is mapped to Trino `TIMESTAMP(0)`.

Warning

Due to date and time differences in the libraries used by Trino and the Oracle JDBC driver, attempting to insert or select a datetime value earlier than `1582-10-15` results in an incorrect date inserted.

#### Mapping character types[#](https://trino.io/docs/current/connector/oracle.html#mapping-character-types) <a href="#mapping-character-types" id="mapping-character-types"></a>

Trino’s `VARCHAR(n)` maps to `VARCHAR2(n CHAR)` if `n` is no greater than 4000. A larger or unbounded `VARCHAR` maps to `NCLOB`.

Trino’s `CHAR(n)` maps to `CHAR(n CHAR)` if `n` is no greater than 2000. A larger `CHAR` maps to `NCLOB`.

Using `CREATE TABLE AS` to create an `NCLOB` column from a `CHAR` value removes the trailing spaces from the initial values for the column. Inserting `CHAR` values into existing `NCLOB` columns keeps the trailing spaces. For example:

```
CREATE TABLE vals AS SELECT CAST('A' as CHAR(2001)) col;
INSERT INTO vals (col) VALUES (CAST('BB' as CHAR(2001)));
SELECT LENGTH(col) FROM vals;
```



```
 _col0
-------
  2001
     1
(2 rows)
```



Attempting to write a `CHAR` that doesn’t fit in the column’s actual size fails. This is also true for the equivalent `VARCHAR` types.

#### Type mapping configuration properties[#](https://trino.io/docs/current/connector/oracle.html#type-mapping-configuration-properties) <a href="#type-mapping-configuration-properties" id="type-mapping-configuration-properties"></a>

The following properties can be used to configure how data types from the connected data source are mapped to Trino data types and how the metadata is cached in Trino.

| Property name                  | Description                                                                                                                                                                                                                                                                                                              | Default value |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `unsupported-type-handling`    | <p>Configure how unsupported column data types are handled:</p><ul><li><code>IGNORE</code>, column is not accessible.</li><li><code>CONVERT_TO_VARCHAR</code>, column is converted to unbounded <code>VARCHAR</code>.</li></ul><p>The respective catalog session property is <code>unsupported_type_handling</code>.</p> | `IGNORE`      |
| `jdbc-types-mapped-to-varchar` | Allow forced mapping of comma separated lists of data types to convert to unbounded `VARCHAR`                                                                                                                                                                                                                            |               |

#### Number to decimal configuration properties[#](https://trino.io/docs/current/connector/oracle.html#number-to-decimal-configuration-properties) <a href="#number-to-decimal-configuration-properties" id="number-to-decimal-configuration-properties"></a>

| Configuration property name   | Session property name  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Default       |
| ----------------------------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| `oracle.number.default-scale` | `number_default_scale` | Default Trino `DECIMAL` scale for Oracle `NUMBER` (without precision and scale) date type. When not set then such column is treated as not supported.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | not set       |
| `oracle.number.rounding-mode` | `number_rounding_mode` | <p>Rounding mode for the Oracle <code>NUMBER</code> data type. This is useful when Oracle <code>NUMBER</code> data type specifies higher scale than is supported in Trino. Possible values are:</p><ul><li><code>UNNECESSARY</code> - Rounding mode to assert that the requested operation has an exact result, hence no rounding is necessary.</li><li><code>CEILING</code> - Rounding mode to round towards positive infinity.</li><li><code>FLOOR</code> - Rounding mode to round towards negative infinity.</li><li><code>HALF_DOWN</code> - Rounding mode to round towards <code>nearest neighbor</code> unless both neighbors are equidistant, in which case rounding down is used.</li><li><code>HALF_EVEN</code> - Rounding mode to round towards the <code>nearest neighbor</code> unless both neighbors are equidistant, in which case rounding towards the even neighbor is performed.</li><li><code>HALF_UP</code> - Rounding mode to round towards <code>nearest neighbor</code> unless both neighbors are equidistant, in which case rounding up is used</li><li><code>UP</code> - Rounding mode to round towards zero.</li><li><code>DOWN</code> - Rounding mode to round towards zero.</li></ul> | `UNNECESSARY` |

### SQL support[#](https://trino.io/docs/current/connector/oracle.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides read access and write access to data and metadata in Oracle. In addition to the globally available and read operation statements, the connector supports the following statements:

* INSERT
* UPDATE
* DELETE
* TRUNCATE
* CREATE TABLE
* CREATE TABLE AS
* DROP TABLE
* ALTER TABLE
* COMMENT

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

#### SQL DELETE[#](https://trino.io/docs/current/connector/oracle.html#sql-delete) <a href="#sql-delete" id="sql-delete"></a>

If a `WHERE` clause is specified, the `DELETE` operation only works if the predicate in the clause can be fully pushed down to the data source.

#### ALTER TABLE RENAME TO[#](https://trino.io/docs/current/connector/oracle.html#alter-table-rename-to) <a href="#alter-table-rename-to" id="alter-table-rename-to"></a>

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

### Fault-tolerant execution support[#](https://trino.io/docs/current/connector/oracle.html#fault-tolerant-execution-support) <a href="#fault-tolerant-execution-support" id="fault-tolerant-execution-support"></a>

The connector supports Fault-tolerant execution of query processing. Read and write operations are both supported with any retry policy.

### Table functions[#](https://trino.io/docs/current/connector/oracle.html#table-functions) <a href="#table-functions" id="table-functions"></a>

The connector provides specific table functions to access Oracle.

#### `query(varchar) -> table`[#](https://trino.io/docs/current/connector/oracle.html#query-varchar-table) <a href="#query-varchar-table" id="query-varchar-table"></a>

The `query` function allows you to query the underlying database directly. It requires syntax native to Oracle, because the full query is pushed down and processed in Oracle. This can be useful for accessing native features which are not available in Trino or for improving query performance in situations where running a query natively may be faster.

The native query passed to the underlying data source is required to return a table as a result set. Only the data source performs validation or security checks for these queries using its own configuration. Trino does not perform these tasks. Only use passthrough queries to read data.

As a simple example, query the `example` catalog and select an entire table:

```
SELECT
  *
FROM
  TABLE(
    example.system.query(
      query => 'SELECT
        *
      FROM
        tpch.nation'
    )
  );
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

As a practical example, you can use the [MODEL clause from Oracle SQL](https://docs.oracle.com/cd/B19306\_01/server.102/b14223/sqlmodel.htm):

```
SELECT
  SUBSTR(country, 1, 20) country,
  SUBSTR(product, 1, 15) product,
  year,
  sales
FROM
  TABLE(
    example.system.query(
      query => 'SELECT
        *
      FROM
        sales_view
      MODEL
        RETURN UPDATED ROWS
        MAIN
          simple_model
        PARTITION BY
          country
        MEASURES
          sales
        RULES
          (sales['Bounce', 2001] = 1000,
          sales['Bounce', 2002] = sales['Bounce', 2001] + sales['Bounce', 2000],
          sales['Y Box', 2002] = sales['Y Box', 2001])
      ORDER BY
        country'
    )
  );
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note

The query engine does not preserve the order of the results of this function. If the passed query contains an `ORDER BY` clause, the function result may not be ordered as expected.

### Performance[#](https://trino.io/docs/current/connector/oracle.html#performance) <a href="#performance" id="performance"></a>

The connector includes a number of performance improvements, detailed in the following sections.

#### Synonyms[#](https://trino.io/docs/current/connector/oracle.html#synonyms) <a href="#synonyms" id="synonyms"></a>

Based on performance reasons, Trino disables support for Oracle `SYNONYM`. To include `SYNONYM`, add the following configuration property:

```
oracle.synonyms.enabled=true
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Pushdown[#](https://trino.io/docs/current/connector/oracle.html#pushdown) <a href="#pushdown" id="pushdown"></a>

The connector supports pushdown for a number of operations:

* Join pushdown
* Limit pushdown
* Top-N pushdown

In addition, the connector supports Aggregation pushdown for the following functions:

* `avg()`
* `count()`, also `count(distinct x)`
* `max()`
* `min()`
* `sum()`

Pushdown is only supported for `DOUBLE` type columns with the following functions:

* `stddev()` and `stddev_samp()`
* `stddev_pop()`
* `var_pop()`
* `variance()` and `var_samp()`

Pushdown is only supported for `REAL` or `DOUBLE` type column with the following functions:

* `covar_samp()`
* `covar_pop()`

Note

The connector performs pushdown where performance may be improved, but in order to preserve correctness an operation may not be pushed down. When pushdown of an operation may result in better performance but risks correctness, the connector prioritizes correctness.

**Join pushdown**[**#**](https://trino.io/docs/current/connector/oracle.html#join-pushdown)

The `join-pushdown.enabled` catalog configuration property or `join_pushdown_enabled` catalog session property control whether the connector pushes down join operations. The property defaults to `false`, and enabling join pushdowns may negatively impact performance for some queries.

**Predicate pushdown support**[**#**](https://trino.io/docs/current/connector/oracle.html#predicate-pushdown-support)

The connector does not support pushdown of any predicates on columns that use the `CLOB`, `NCLOB`, `BLOB`, or `RAW(n)` Oracle database types, or Trino data types that [map](https://trino.io/docs/current/connector/oracle.html#oracle-type-mapping) to these Oracle database types.

In the following example, the predicate is not pushed down for either query since `name` is a column of type `VARCHAR`, which maps to `NCLOB` in Oracle:

```
SHOW CREATE TABLE nation;

--             Create Table
----------------------------------------
-- CREATE TABLE oracle.trino_test.nation (
--    name VARCHAR
-- )
-- (1 row)

SELECT * FROM nation WHERE name > 'CANADA';
SELECT * FROM nation WHERE name = 'CANADA';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

In the following example, the predicate is pushed down for both queries since `name` is a column of type `VARCHAR(25)`, which maps to `VARCHAR2(25)` in Oracle:

```
SHOW CREATE TABLE nation;

--             Create Table
----------------------------------------
-- CREATE TABLE oracle.trino_test.nation (
--    name VARCHAR(25)
-- )
-- (1 row)

SELECT * FROM nation WHERE name > 'CANADA';
SELECT * FROM nation WHERE name = 'CANADA';
```
