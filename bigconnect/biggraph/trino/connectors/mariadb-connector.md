# MariaDB connector

The MariaDB connector allows querying and creating tables in an external MariaDB database.

### Requirements <a href="#requirements" id="requirements"></a>

To connect to MariaDB, you need:

* MariaDB version 10.10 or higher.
* Network access from the Trino coordinator and workers to MariaDB. Port 3306 is the default port.

### Configuration <a href="#configuration" id="configuration"></a>

To configure the MariaDB connector, create a catalog properties file in `etc/catalog` named, for example, `example.properties`, to mount the MariaDB connector as the `example` catalog. Create the file with the following contents, replacing the connection properties as appropriate for your setup:

```
connector.name=mariadb
connection-url=jdbc:mariadb://example.net:3306
connection-user=root
connection-password=secret
```

The `connection-user` and `connection-password` are typically required and determine the user credentials for the connection, often a service user. You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid actual values in the catalog properties files.

#### Data source authentication <a href="#data-source-authentication" id="data-source-authentication"></a>

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

#### General configuration properties <a href="#general-configuration-properties" id="general-configuration-properties"></a>

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

#### Domain compaction threshold <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improvperformance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large dynamic filters. The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` catalog session property can be used to adjust the default value of `32` for this threshold.

#### Case insensitive matching <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

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

### Querying MariaDB <a href="#querying-mariadb" id="querying-mariadb"></a>

The MariaDB connector provides a schema for every MariaDB _database_. You can see the available MariaDB databases by running `SHOW SCHEMAS`:

```
SHOW SCHEMAS FROM example;
```

If you have a MariaDB database named `web`, you can view the tables in this database by running `SHOW TABLES`:

```
SHOW TABLES FROM example.web;
```

You can see a list of the columns in the `clicks` table in the `web` database using either of the following:

```
DESCRIBE example.web.clicks;
SHOW COLUMNS FROM example.web.clicks;
```

Finally, you can access the `clicks` table in the `web` database:

```
SELECT * FROM example.web.clicks;
```

If you used a different name for your catalog properties file, use that catalog name instead of `example` in the above examples.

### Type mapping <a href="#type-mapping" id="type-mapping"></a>

Because Trino and MariaDB each support types that the other does not, this connector modifies some types when reading or writing data. Data types may not map the same way in both directions between Trino and the data source. Refer to the following sections for type mapping in each direction.

#### MariaDB type to Trino type mapping <a href="#mariadb-type-to-trino-type-mapping" id="mariadb-type-to-trino-type-mapping"></a>

The connector maps MariaDB types to the corresponding Trino types according to the following table:

| MariaDB type        | Trino type          | Notes                                                                                                                                                                                                                                                           |
| ------------------- | ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BOOLEAN`           | `TINYINT`           | `BOOL` and `BOOLEAN` are aliases of `TINYINT(1)`                                                                                                                                                                                                                |
| `TINYINT`           | `TINYINT`           |                                                                                                                                                                                                                                                                 |
| `TINYINT UNSIGNED`  | `SMALLINT`          |                                                                                                                                                                                                                                                                 |
| `SMALLINT`          | `SMALLINT`          |                                                                                                                                                                                                                                                                 |
| `SMALLINT UNSIGNED` | `INTEGER`           |                                                                                                                                                                                                                                                                 |
| `INT`               | `INTEGER`           |                                                                                                                                                                                                                                                                 |
| `INT UNSIGNED`      | `BIGINT`            |                                                                                                                                                                                                                                                                 |
| `BIGINT`            | `BIGINT`            |                                                                                                                                                                                                                                                                 |
| `BIGINT UNSIGNED`   | `DECIMAL(20, 0)`    |                                                                                                                                                                                                                                                                 |
| `FLOAT`             | `REAL`              |                                                                                                                                                                                                                                                                 |
| `DOUBLE`            | `DOUBLE`            |                                                                                                                                                                                                                                                                 |
| `DECIMAL(p,s)`      | `DECIMAL(p,s)`      |                                                                                                                                                                                                                                                                 |
| `CHAR(n)`           | `CHAR(n)`           |                                                                                                                                                                                                                                                                 |
| `TINYTEXT`          | `VARCHAR(255)`      |                                                                                                                                                                                                                                                                 |
| `TEXT`              | `VARCHAR(65535)`    |                                                                                                                                                                                                                                                                 |
| `MEDIUMTEXT`        | `VARCHAR(16777215)` |                                                                                                                                                                                                                                                                 |
| `LONGTEXT`          | `VARCHAR`           |                                                                                                                                                                                                                                                                 |
| `VARCHAR(n)`        | `VARCHAR(n)`        |                                                                                                                                                                                                                                                                 |
| `TINYBLOB`          | `VARBINARY`         |                                                                                                                                                                                                                                                                 |
| `BLOB`              | `VARBINARY`         |                                                                                                                                                                                                                                                                 |
| `MEDIUMBLOB`        | `VARBINARY`         |                                                                                                                                                                                                                                                                 |
| `LONGBLOB`          | `VARBINARY`         |                                                                                                                                                                                                                                                                 |
| `VARBINARY(n)`      | `VARBINARY`         |                                                                                                                                                                                                                                                                 |
| `DATE`              | `DATE`              |                                                                                                                                                                                                                                                                 |
| `TIME(n)`           | `TIME(n)`           |                                                                                                                                                                                                                                                                 |
| `TIMESTAMP(n)`      | `TIMESTAMP(n)`      | MariaDB stores the current timestamp by default. Enable [explicit\_defaults\_for\_timestamp](https://mariadb.com/docs/reference/mdb/system-variables/explicit\_defaults\_for\_timestamp/) to avoid implicit default values and use `NULL` as the default value. |
| `DATETIME(n)`       | `TIMESTAMP(n)`      |                                                                                                                                                                                                                                                                 |

No other types are supported.

#### Trino type mapping to MariaDB type mapping <a href="#trino-type-mapping-to-mariadb-type-mapping" id="trino-type-mapping-to-mariadb-type-mapping"></a>

The connector maps Trino types to the corresponding MariaDB types according to the following table:

| Trino type          | MariaDB type   | Notes                                                                                                                                                                                                                                                          |
| ------------------- | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BOOLEAN`           | `BOOLEAN`      |                                                                                                                                                                                                                                                                |
| `TINYINT`           | `TINYINT`      |                                                                                                                                                                                                                                                                |
| `SMALLINT`          | `SMALLINT`     |                                                                                                                                                                                                                                                                |
| `INTEGER`           | `INT`          |                                                                                                                                                                                                                                                                |
| `BIGINT`            | `BIGINT`       |                                                                                                                                                                                                                                                                |
| `REAL`              | `FLOAT`        |                                                                                                                                                                                                                                                                |
| `DOUBLE`            | `DOUBLE`       |                                                                                                                                                                                                                                                                |
| `DECIMAL(p,s)`      | `DECIMAL(p,s)` |                                                                                                                                                                                                                                                                |
| `CHAR(n)`           | `CHAR(n)`      |                                                                                                                                                                                                                                                                |
| `VARCHAR(255)`      | `TINYTEXT`     | Maps on `VARCHAR` of length 255 or less.                                                                                                                                                                                                                       |
| `VARCHAR(65535)`    | `TEXT`         | Maps on `VARCHAR` of length between 256 and 65535, inclusive.                                                                                                                                                                                                  |
| `VARCHAR(16777215)` | `MEDIUMTEXT`   | Maps on `VARCHAR` of length between 65536 and 16777215, inclusive.                                                                                                                                                                                             |
| `VARCHAR`           | `LONGTEXT`     | `VARCHAR` of length greater than 16777215 and unbounded `VARCHAR` map to `LONGTEXT`.                                                                                                                                                                           |
| `VARBINARY`         | `MEDIUMBLOB`   |                                                                                                                                                                                                                                                                |
| `DATE`              | `DATE`         |                                                                                                                                                                                                                                                                |
| `TIME(n)`           | `TIME(n)`      |                                                                                                                                                                                                                                                                |
| `TIMESTAMP(n)`      | `TIMESTAMP(n)` | MariaDB stores the current timestamp by default. Enable `explicit_defaults_for_timestamp   <https://mariadb.com/docs/reference/mdb/system-variables/explicit_defaults_for_timestamp/>`\_ to avoid implicit default values and use `NULL` as the default value. |

No other types are supported.

Complete list of [MariaDB data types](https://mariadb.com/kb/en/data-types/).

#### Type mapping configuration properties <a href="#type-mapping-configuration-properties" id="type-mapping-configuration-properties"></a>

The following properties can be used to configure how data types from the connected data source are mapped to Trino data types and how the metadata is cached in Trino.

| Property name                  | Description                                                                                                                                                                                                                                                                                                              | Default value |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `unsupported-type-handling`    | <p>Configure how unsupported column data types are handled:</p><ul><li><code>IGNORE</code>, column is not accessible.</li><li><code>CONVERT_TO_VARCHAR</code>, column is converted to unbounded <code>VARCHAR</code>.</li></ul><p>The respective catalog session property is <code>unsupported_type_handling</code>.</p> | `IGNORE`      |
| `jdbc-types-mapped-to-varchar` | Allow forced mapping of comma separated lists of data types to convert to unbounded `VARCHAR`                                                                                                                                                                                                                            |               |

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides read access and write access to data and metadata in a MariaDB database. In addition to the globally available and read operation statements, the connector supports the following features:

* INSERT
* UPDATE
* DELETE
* TRUNCATE
* CREATE TABLE
* CREATE TABLE AS
* DROP TABLE
* ALTER TABLE
* CREATE SCHEMA
* DROP SCHEMA

#### UPDATE <a href="#update" id="update"></a>

Only `UPDATE` statements with constant assignments and predicates are supported. For example, the following statement is supported because the values assigned are constants:

```
UPDATE table SET col1 = 1 WHERE col3 = 
```

Arithmetic expressions, function calls, and other non-constant `UPDATE` statements are not supported. For example, the following statement is not supported because arithmetic expressions cannot be used with the `SET` command:

```
UPDATE table SET col1 = col2 + 2 WHERE col3 = 1
```

The `=`, `!=`, `>`, `<`, `>=`, `<=`, `IN`, `NOT IN` operators are supported in predicates. The following statement is not supported because the `AND` operator cannot be used in predicates:

```
UPDATE table SET col1 = 1 WHERE col3 = 1 AND col2 = 3
```

All column values of a table row cannot be updated simultaneously. For a three column table, the following statement is not supported:

```
UPDATE table SET col1 = 1, col2 = 2, col3 = 3 WHERE col3 = 1
```

#### SQL DELETE <a href="#sql-delete" id="sql-delete"></a>

If a `WHERE` clause is specified, the `DELETE` operation only works if the predicate in the clause can be fully pushed down to the data source.

### Table functions <a href="#table-functions" id="table-functions"></a>

The connector provides specific table functions to access MariaDB.

#### `query(varchar) -> table` <a href="#query-varchar-table" id="query-varchar-table"></a>

The `query` function allows you to query the underlying database directly. It requires syntax native to MariaDB, because the full query is pushed down and processed in MariaDB. This can be useful for accessing native features which are not available in Trino or for improving query performance in situations where running a query natively may be faster.

The native query passed to the underlying data source is required to return a table as a result set. Only the data source performs validation or security checks for these queries using its own configuration. Trino does not perform these tasks. Only use passthrough queries to read data.

As an example, query the `example` catalog and select the age of employees by using `TIMESTAMPDIFF` and `CURDATE`:

```
SELECT
  age
FROM
  TABLE(
    example.system.query(
      query => 'SELECT
        TIMESTAMPDIFF(
          YEAR,
          date_of_birth,
          CURDATE()
        ) AS age
      FROM
        tiny.employees'
    )
  );
```



Note

The query engine does not preserve the order of the results of this function. If the passed query contains an `ORDER BY` clause, the function result may not be ordered as expected.

### Performance <a href="#performance" id="performance"></a>

The connector includes a number of performance improvements, detailed in the following sections.

#### Table statistics <a href="#table-statistics" id="table-statistics"></a>

The MariaDB connector can use table and column statistics for cost based optimizations to improve query processing performance based on the actual data in the data source.

The statistics are collected by MariaDB and retrieved by the connector.

To collect statistics for a table, execute the following statement in MariaDB.

```
ANALYZE TABLE table_name;
```



Refer to MariaDB documentation for additional information.

#### Pushdown <a href="#pushdown" id="pushdown"></a>

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

**Predicate pushdown support**

The connector does not support pushdown of any predicates on columns with [textual types](https://trino.io/docs/current/language/types.html#string-data-types) like `CHAR` or `VARCHAR`. This ensures correctness of results since the data source may compare strings case-insensitively.

In the following example, the predicate is not pushed down for either query since `name` is a column of type `VARCHAR`:

```
SELECT * FROM nation WHERE name > 'CANADA';
SELECT * FROM nation WHERE name = 'CANADA';
```
