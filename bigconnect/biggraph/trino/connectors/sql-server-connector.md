# SQL Server connector

The SQL Server connector allows querying and creating tables in an external [Microsoft SQL Server](https://www.microsoft.com/sql-server/) database. This can be used to join data between different systems like SQL Server and Hive, or between two different SQL Server instances.

### Requirements[#](https://trino.io/docs/current/connector/sqlserver.html#requirements) <a href="#requirements" id="requirements"></a>

To connect to SQL Server, you need:

* SQL Server 2012 or higher, or Azure SQL Database.
* Network access from the Trino coordinator and workers to SQL Server. Port 1433 is the default port.

### Configuration[#](https://trino.io/docs/current/connector/sqlserver.html#configuration) <a href="#configuration" id="configuration"></a>

The connector can query a single database on a given SQL Server instance. Create a catalog properties file that specifies the SQL server connector by setting the `connector.name` to `sqlserver`.

For example, to access a database as `example`, create the file `etc/catalog/example.properties`. Replace the connection properties as appropriate for your setup:

```
connector.name=sqlserver
connection-url=jdbc:sqlserver://<host>:<port>;databaseName=<databaseName>;encrypt=false
connection-user=root
connection-password=secret
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `connection-url` defines the connection information and parameters to pass to the SQL Server JDBC driver. The supported parameters for the URL are available in the [SQL Server JDBC driver documentation](https://docs.microsoft.com/sql/connect/jdbc/building-the-connection-url).

The `connection-user` and `connection-password` are typically required and determine the user credentials for the connection, often a service user. You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid actual values in the catalog properties files.

#### Connection security[#](https://trino.io/docs/current/connector/sqlserver.html#connection-security) <a href="#connection-security" id="connection-security"></a>

The JDBC driver, and therefore the connector, automatically use Transport Layer Security (TLS) encryption and certificate validation. This requires a suitable TLS certificate configured on your SQL Server database host.

If you do not have the necessary configuration established, you can disable encryption in the connection string with the `encrypt` property:

```
connection-url=jdbc:sqlserver://<host>:<port>;databaseName=<databaseName>;encrypt=false
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further parameters like `trustServerCertificate`, `hostNameInCertificate`, `trustStore`, and `trustStorePassword` are details in the [TLS section of SQL Server JDBC driver documentation](https://docs.microsoft.com/sql/connect/jdbc/using-ssl-encryption).

#### Data source authentication[#](https://trino.io/docs/current/connector/sqlserver.html#data-source-authentication) <a href="#data-source-authentication" id="data-source-authentication"></a>

The connector can provide credentials for the data source connection in multiple ways:

* inline, in the connector configuration file
* in a separate properties file
* in a key store file
* as extra credentials set when connecting to Trino

You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid storing sensitive values in the catalog properties files.

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

#### Multiple SQL Server databases or servers[#](https://trino.io/docs/current/connector/sqlserver.html#multiple-sql-server-databases-or-servers) <a href="#multiple-sql-server-databases-or-servers" id="multiple-sql-server-databases-or-servers"></a>

The SQL Server connector can only access a single SQL Server database within a single catalog. Thus, if you have multiple SQL Server databases, or want to connect to multiple SQL Server instances, you must configure multiple instances of the SQL Server connector.

To add another catalog, simply add another properties file to `etc/catalog` with a different name, making sure it ends in `.properties`. For example, if you name the property file `sales.properties`, Trino creates a catalog named `sales` using the configured connector.

#### General configuration properties[#](https://trino.io/docs/current/connector/sqlserver.html#general-configuration-properties) <a href="#general-configuration-properties" id="general-configuration-properties"></a>

The following table describes general catalog configuration properties for the connector:

| Property name                                               | Description                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `case-insensitive-name-matching`                            | Support case insensitive schema and table names. Defaults to `false`.                                                                                                                                                                                                                           |
| `case-insensitive-name-matching.cache-ttl`                  | Duration for which case insensitive schema and table names are cached. Defaults to `1m`.                                                                                                                                                                                                        |
| `case-insensitive-name-matching.config-file`                | Path to a name mapping configuration file in JSON format that allows Trino to disambiguate between schemas and tables with similar names in different cases. Defaults to `null`.                                                                                                                |
| `case-insensitive-name-matching.config-file.refresh-period` | Frequency with which Trino checks the name matching configuration file for changes. The duration value defaults to `0s` (refresh disabled).                                                                                                                                                     |
| `metadata.cache-ttl`                                        | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which metadata, including table and column statistics, is cached. Defaults to `0s` (caching disabled).                                                                                                   |
| `metadata.cache-missing`                                    | Cache the fact that metadata, including table and column statistics, is not available. Defaults to `false`.                                                                                                                                                                                     |
| `metadata.schemas.cache-ttl`                                | Duration for which schema metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                    |
| `metadata.tables.cache-ttl`                                 | Duration for which table metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                     |
| `metadata.statistics.cache-ttl`                             | Duration for which tables statistics are cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                 |
| `metadata.cache-maximum-size`                               | Maximum number of objects stored in the metadata cache. Defaults to `10000`.                                                                                                                                                                                                                    |
| `write.batch-size`                                          | Maximum number of statements in a batched execution. Do not change this setting from the default. Non-default values may negatively impact performance. Defaults to `1000`.                                                                                                                     |
| `dynamic-filtering.enabled`                                 | Push down dynamic filters into JDBC queries. Defaults to `true`.                                                                                                                                                                                                                                |
| `dynamic-filtering.wait-timeout`                            | Maximum duration for which Trino waits for dynamic filters to be collected from the build side of joins before starting a JDBC query. Using a large timeout can potentially result in more detailed dynamic filters. However, it can also increase latency for some queries. Defaults to `20s`. |

#### Appending query metadata[#](https://trino.io/docs/current/connector/sqlserver.html#appending-query-metadata) <a href="#appending-query-metadata" id="appending-query-metadata"></a>

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

#### Domain compaction threshold[#](https://trino.io/docs/current/connector/sqlserver.html#domain-compaction-threshold) <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improve performance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large dynamic filters. The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` catalog session property can be used to adjust the default value of `32` for this threshold.

#### Specific configuration properties# <a href="#specific-configuration-properties" id="specific-configuration-properties"></a>

The SQL Server connector supports additional catalog properties to configure the behavior of the connector and the issues queries to the database.

| Property name                           | Description                                                                                                                                                          |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sqlserver.snapshot-isolation.disabled` | Control the automatic use of snapshot isolation for transactions issued by Trino in SQL Server. Defaults to `false`, which means that snapshot isolation is enabled. |

#### Procedures[#](https://trino.io/docs/current/connector/sqlserver.html#procedures) <a href="#procedures" id="procedures"></a>

*   `system.flush_metadata_cache()`

    Flush JDBC metadata caches. For example, the following system call flushes the metadata caches for all schemas in the `example` catalog

    ```
    USE example.example_schema;
    CALL system.flush_metadata_cache();
    ```

    ![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Case insensitive matching[#](https://trino.io/docs/current/connector/sqlserver.html#case-insensitive-matching) <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

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

#### Non-transactional INSERT[#](https://trino.io/docs/current/connector/sqlserver.html#non-transactional-insert) <a href="#non-transactional-insert" id="non-transactional-insert"></a>

The connector supports adding rows using [INSERT statements](https://trino.io/docs/current/sql/insert.html). By default, data insertion is performed by writing data to a temporary table. You can skip this step to improve performance and write directly to the target table. Set the `insert.non-transactional-insert.enabled` catalog property or the corresponding `non_transactional_insert` catalog session property to `true`.

Note that with this property enabled, data can be corrupted in rare cases where exceptions occur during the insert operation. With transactions disabled, no rollback can be performed.

### Querying SQL Server[#](https://trino.io/docs/current/connector/sqlserver.html#querying-sql-server) <a href="#querying-sql-server" id="querying-sql-server"></a>

The SQL Server connector provides access to all schemas visible to the specified user in the configured database. For the following examples, assume the SQL Server catalog is `example`.

You can see the available schemas by running `SHOW SCHEMAS`:

```
SHOW SCHEMAS FROM example;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you have a schema named `web`, you can view the tables in this schema by running `SHOW TABLES`:

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

Finally, you can query the `clicks` table in the `web` schema:

```
SELECT * FROM example.web.clicks;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you used a different name for your catalog properties file, use that catalog name instead of `example` in the above examples.

### Type mapping[#](https://trino.io/docs/current/connector/sqlserver.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

Because Trino and SQL Server each support types that the other does not, this connector [modifies some types](https://trino.io/docs/current/language/types.html#type-mapping-overview) when reading or writing data. Data types may not map the same way in both directions between Trino and the data source. Refer to the following sections for type mapping in each direction.

#### SQL Server type to Trino type mapping[#](https://trino.io/docs/current/connector/sqlserver.html#sql-server-type-to-trino-type-mapping) <a href="#sql-server-type-to-trino-type-mapping" id="sql-server-type-to-trino-type-mapping"></a>

The connector maps SQL Server types to the corresponding Trino types following this table:

| SQL Server database type                      | Trino type                    | Notes                                                                                                        |
| --------------------------------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `BIT`                                         | `BOOLEAN`                     |                                                                                                              |
| `TINYINT`                                     | `SMALLINT`                    | SQL Server `TINYINT` is actually `unsigned TINYINT`                                                          |
| `SMALLINT`                                    | `SMALLINT`                    |                                                                                                              |
| `INTEGER`                                     | `INTEGER`                     |                                                                                                              |
| `BIGINT`                                      | `BIGINT`                      |                                                                                                              |
| `DOUBLE PRECISION`                            | `DOUBLE`                      |                                                                                                              |
| `FLOAT[(n)]`                                  | `REAL` or `DOUBLE`            | See [Numeric type mapping](https://trino.io/docs/current/connector/sqlserver.html#sqlserver-numeric-mapping) |
| `REAL`                                        | `REAL`                        |                                                                                                              |
| `DECIMAL[(p[, s])]`, `NUMERIC[(p[, s])]`      | `DECIMAL(p, s)`               |                                                                                                              |
| `CHAR[(n)]`                                   | `CHAR(n)`                     | `1 <= n <= 8000`                                                                                             |
| `NCHAR[(n)]`                                  | `CHAR(n)`                     | `1 <= n <= 4000`                                                                                             |
| `VARCHAR[(n \| max)]`, `NVARCHAR[(n \| max)]` | `VARCHAR(n)`                  | `1 <= n <= 8000`, `max = 2147483647`                                                                         |
| `TEXT`                                        | `VARCHAR(2147483647)`         |                                                                                                              |
| `NTEXT`                                       | `VARCHAR(1073741823)`         |                                                                                                              |
| `VARBINARY[(n \| max)]`                       | `VARBINARY`                   | `1 <= n <= 8000`, `max = 2147483647`                                                                         |
| `DATE`                                        | `DATE`                        |                                                                                                              |
| `TIME[(n)]`                                   | `TIME(n)`                     | `0 <= n <= 7`                                                                                                |
| `DATETIME2[(n)]`                              | `TIMESTAMP(n)`                | `0 <= n <= 7`                                                                                                |
| `SMALLDATETIME`                               | `TIMESTAMP(0)`                |                                                                                                              |
| `DATETIMEOFFSET[(n)]`                         | `TIMESTAMP(n) WITH TIME ZONE` | `0 <= n <= 7`                                                                                                |

#### Trino type to SQL Server type mapping[#](https://trino.io/docs/current/connector/sqlserver.html#trino-type-to-sql-server-type-mapping) <a href="#trino-type-to-sql-server-type-mapping" id="trino-type-to-sql-server-type-mapping"></a>

The connector maps Trino types to the corresponding SQL Server types following this table:

| Trino type      | SQL Server type                  | Notes                                                      |
| --------------- | -------------------------------- | ---------------------------------------------------------- |
| `BOOLEAN`       | `BIT`                            |                                                            |
| `TINYINT`       | `TINYINT`                        | Trino only supports writing values belonging to `[0, 127]` |
| `SMALLINT`      | `SMALLINT`                       |                                                            |
| `INTEGER`       | `INTEGER`                        |                                                            |
| `BIGINT`        | `BIGINT`                         |                                                            |
| `REAL`          | `REAL`                           |                                                            |
| `DOUBLE`        | `DOUBLE PRECISION`               |                                                            |
| `DECIMAL(p, s)` | `DECIMAL(p, s)`                  |                                                            |
| `CHAR(n)`       | `NCHAR(n)` or `NVARCHAR(max)`    | See Character type mapping                                 |
| `VARCHAR(n)`    | `NVARCHAR(n)` or `NVARCHAR(max)` | See Character type mapping                                 |
| `VARBINARY`     | `VARBINARY(max)`                 |                                                            |
| `DATE`          | `DATE`                           |                                                            |
| `TIME(n)`       | `TIME(n)`                        | `0 <= n <= 7`                                              |
| `TIMESTAMP(n)`  | `DATETIME2(n)`                   | `0 <= n <= 7`                                              |

Complete list of [SQL Server data types](https://msdn.microsoft.com/library/ms187752.aspx).

#### Numeric type mapping[#](https://trino.io/docs/current/connector/sqlserver.html#numeric-type-mapping) <a href="#numeric-type-mapping" id="numeric-type-mapping"></a>

For SQL Server `FLOAT[(n)]`:

* If `n` is not specified maps to Trino `Double`
* If `1 <= n <= 24` maps to Trino `REAL`
* If `24 < n <= 53` maps to Trino `DOUBLE`

#### Character type mapping[#](https://trino.io/docs/current/connector/sqlserver.html#character-type-mapping) <a href="#character-type-mapping" id="character-type-mapping"></a>

For Trino `CHAR(n)`:

* If `1 <= n <= 4000` maps SQL Server `NCHAR(n)`
* If `n > 4000` maps SQL Server `NVARCHAR(max)`

For Trino `VARCHAR(n)`:

* If `1 <= n <= 4000` maps SQL Server `NVARCHAR(n)`
* If `n > 4000` maps SQL Server `NVARCHAR(max)`

#### Type mapping configuration properties[#](https://trino.io/docs/current/connector/sqlserver.html#type-mapping-configuration-properties) <a href="#type-mapping-configuration-properties" id="type-mapping-configuration-properties"></a>

The following properties can be used to configure how data types from the connected data source are mapped to Trino data types and how the metadata is cached in Trino.

| Property name                  | Description                                                                                                                                                                                                                                                                                                              | Default value |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `unsupported-type-handling`    | <p>Configure how unsupported column data types are handled:</p><ul><li><code>IGNORE</code>, column is not accessible.</li><li><code>CONVERT_TO_VARCHAR</code>, column is converted to unbounded <code>VARCHAR</code>.</li></ul><p>The respective catalog session property is <code>unsupported_type_handling</code>.</p> | `IGNORE`      |
| `jdbc-types-mapped-to-varchar` | Allow forced mapping of comma separated lists of data types to convert to unbounded `VARCHAR`                                                                                                                                                                                                                            |               |

### SQL support[#](https://trino.io/docs/current/connector/sqlserver.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides read access and write access to data and metadata in SQL Server. In addition to the globally available and read operation statements, the connector supports the following features:

* INSERT
* UPDATE
* DELETE
* TRUNCATE
* Schema and table management

#### UPDATE# <a href="#update" id="update"></a>

Only `UPDATE` statements with constant assignments and predicates are supported. For example, the following statement is supported because the values assigned are constants:

```
UPDATE table SET col1 = 1 WHERE col3 = 1
```



Arithmetic expressions, function calls, and other non-constant `UPDATE` statements are not supported. For example, the following statement is not supported because arithmetic expressions cannot be used with the `SET` command:

```
UPDATE table SET col1 = col2 + 2 WHERE col3 = 1
```



The `=`, `!=`, `>`, `<`, `>=`, `<=`, `IN`, `NOT IN` operators are supported in predicates. The following statement is not supported because the `AND` operator cannot be used in predicates:

```
UPDATE table SET col1 = 1 WHERE col3 = 1 AND col2 = 3
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

All column values of a table row cannot be updated simultaneously. For a three column table, the following statement is not supported:

```
UPDATE table SET col1 = 1, col2 = 2, col3 = 3 WHERE col3 = 1
```



#### SQL DELETE[#](https://trino.io/docs/current/connector/sqlserver.html#sql-delete) <a href="#sql-delete" id="sql-delete"></a>

If a `WHERE` clause is specified, the `DELETE` operation only works if the predicate in the clause can be fully pushed down to the data source.

#### ALTER TABLE RENAME TO[#](https://trino.io/docs/current/connector/sqlserver.html#alter-table-rename-to) <a href="#alter-table-rename-to" id="alter-table-rename-to"></a>

The connector does not support renaming tables across multiple schemas. For example, the following statement is supported:

```
ALTER TABLE example.schema_one.table_one RENAME TO example.schema_one.table_two
```



The following statement attempts to rename a table across schemas, and therefore is not supported:

```
ALTER TABLE example.schema_one.table_one RENAME TO example.schema_two.table_two
```



### Fault-tolerant execution support[#](https://trino.io/docs/current/connector/sqlserver.html#fault-tolerant-execution-support) <a href="#fault-tolerant-execution-support" id="fault-tolerant-execution-support"></a>

The connector supports Fault-tolerant execution of query processing. Read and write operations are both supported with any retry policy.

### Table functions# <a href="#table-functions" id="table-functions"></a>

The connector provides specific table functions to access SQL Server.

#### `query(varchar) -> table`# <a href="#query-varchar-table" id="query-varchar-table"></a>

The `query` function allows you to query the underlying database directly. It requires syntax native to SQL Server, because the full query is pushed down and processed in SQL Server. This can be useful for accessing native features which are not implemented in Trino or for improving query performance in situations where running a query natively may be faster.

The native query passed to the underlying data source is required to return a table as a result set. Only the data source performs validation or security checks for these queries using its own configuration. Trino does not perform these tasks. Only use passthrough queries to read data.

For example, query the `example` catalog and select the top 10 percent of nations by population:

```
SELECT
  *
FROM
  TABLE(
    example.system.query(
      query => 'SELECT
        TOP(10) PERCENT *
      FROM
        tpch.nation
      ORDER BY
        population DESC'
    )
  );
```



#### `procedure(varchar) -> table`[#](https://trino.io/docs/current/connector/sqlserver.html#procedure-varchar-table) <a href="#procedure-varchar-table" id="procedure-varchar-table"></a>

The `procedure` function allows you to run stored procedures on the underlying database directly. It requires syntax native to SQL Server, because the full query is pushed down and processed in SQL Server. In order to use this table function set `sqlserver.experimental.stored-procedure-table-function-enabled` to `true`.

Note

The `procedure` function does not support running StoredProcedures that return multiple statements, use a non-select statement, use output parameters, or use conditional statements.

Warning

This feature is experimental only. The function has security implication and syntax might change and be backward incompatible.

The follow example runs the stored procedure `employee_sp` in the `example` catalog and the `example_schema` schema in the underlying SQL Server database:

```
SELECT
  *
FROM
  TABLE(
    example.system.procedure(
      query => 'EXECUTE example_schema.employee_sp'
    )
  );
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If the stored procedure `employee_sp` requires any input append the parameter value to the procedure statement:

```
SELECT
  *
FROM
  TABLE(
    example.system.procedure(
      query => 'EXECUTE example_schema.employee_sp 0'
    )
  );
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note

The query engine does not preserve the order of the results of this function. If the passed query contains an `ORDER BY` clause, the function result may not be ordered as expected.

### Performance[#](https://trino.io/docs/current/connector/sqlserver.html#performance) <a href="#performance" id="performance"></a>

The connector includes a number of performance improvements, detailed in the following sections.

#### Table statistics[#](https://trino.io/docs/current/connector/sqlserver.html#table-statistics) <a href="#table-statistics" id="table-statistics"></a>

The SQL Server connector can use table and column statistics for cost based optimizations, to improve query processing performance based on the actual data in the data source.

The statistics are collected by SQL Server and retrieved by the connector.

The connector can use information stored in single-column statistics. SQL Server Database can automatically create column statistics for certain columns. If column statistics are not created automatically for a certain column, you can create them by executing the following statement in SQL Server Database.

```
CREATE STATISTICS example_statistics_name ON table_schema.table_name (column_name);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

SQL Server Database routinely updates the statistics. In some cases, you may want to force statistics update (e.g. after defining new column statistics or after changing data in the table). You can do that by executing the following statement in SQL Server Database.

```
UPDATE STATISTICS table_schema.table_name;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Refer to SQL Server documentation for information about options, limitations and additional considerations.

#### Pushdown[#](https://trino.io/docs/current/connector/sqlserver.html#pushdown) <a href="#pushdown" id="pushdown"></a>

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
* `stddev()`
* `stddev_pop()`
* `stddev_samp()`
* `variance()`
* `var_pop()`
* `var_samp()`

Note

The connector performs pushdown where performance may be improved, but in order to preserve correctness an operation may not be pushed down. When pushdown of an operation may result in better performance but risks correctness, the connector prioritizes correctness.

**Cost-based join pushdown#**

The connector supports cost-based Join pushdown to make intelligent decisions about whether to push down a join operation to the data source.

When cost-based join pushdown is enabled, the connector only pushes down join operations if the available Table statistics suggest that doing so improves performance. Note that if no table statistics are available, join operation pushdown does not occur to avoid a potential decrease in query performance.

The following table describes catalog configuration properties for join pushdown:

| Property name            | Description                                                                                                                                                                                                                                                                                                                                                                                         | Default value |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| `join-pushdown.enabled`  | Enable join pushdown. Equivalent catalog session property is `join_pushdown_enabled`.                                                                                                                                                                                                                                                                                                               | `true`        |
| `join-pushdown.strategy` | Strategy used to evaluate whether join operations are pushed down. Set to `AUTOMATIC` to enable cost-based join pushdown, or `EAGER` to push down joins whenever possible. Note that `EAGER` can push down joins even when table statistics are unavailable, which may result in degraded query performance. Because of this, `EAGER` is only recommended for testing and troubleshooting purposes. | `AUTOMATIC`   |

**Predicate pushdown support**[**#**](https://trino.io/docs/current/connector/sqlserver.html#predicate-pushdown-support)

The connector supports pushdown of predicates on `VARCHAR` and `NVARCHAR` columns if the underlying columns in SQL Server use a case-sensitive [collation](https://learn.microsoft.com/en-us/sql/relational-databases/collations/collation-and-unicode-support?view=sql-server-ver16).

The following operators are pushed down:

* `=`
* `<>`
* `IN`
* `NOT IN`

To ensure correct results, operators are not pushed down for columns using a case-insensitive collation.

#### Bulk insert[#](https://trino.io/docs/current/connector/sqlserver.html#bulk-insert) <a href="#bulk-insert" id="bulk-insert"></a>

You can optionally use the [bulk copy API](https://docs.microsoft.com/sql/connect/jdbc/use-bulk-copy-api-batch-insert-operation) to drastically speed up write operations.

Enable bulk copying and a lock on the destination table to meet [minimal logging requirements](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import).

The following table shows the relevant catalog configuration properties and their default values:

| Property name                                          | Description                                                                                                                                                                                                                        | Default |
| ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `sqlserver.bulk-copy-for-write.enabled`                | Use the SQL Server bulk copy API for writes. The corresponding catalog session property is `bulk_copy_for_write`.                                                                                                                  | `false` |
| `sqlserver.bulk-copy-for-write.lock-destination-table` | Obtain a bulk update lock on the destination table for write operations. The corresponding catalog session property is `bulk_copy_for_write_lock_destination_table`. Setting is only used when `bulk-copy-for-write.enabled=true`. | `false` |

Limitations:

* Column names with leading and trailing spaces are not supported.

### Data compression[#](https://trino.io/docs/current/connector/sqlserver.html#data-compression) <a href="#data-compression" id="data-compression"></a>

You can specify the [data compression policy for SQL Server tables](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) with the `data_compression` table property. Valid policies are `NONE`, `ROW` or `PAGE`.

Example:

```
CREATE TABLE example_schema.scientists (
  recordkey VARCHAR,
  name VARCHAR,
  age BIGINT,
  birthday DATE
)
WITH (
  data_compression = 'ROW'
);
```
