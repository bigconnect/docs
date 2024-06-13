# Snowflake connector

The Snowflake connector allows querying and creating tables in an external [Snowflake](https://www.snowflake.com/) account. This can be used to join data between different systems like Snowflake and Hive, or between two different Snowflake accounts.

### Configuration <a href="#configuration" id="configuration"></a>

To configure the Snowflake connector, create a catalog properties file in `etc/catalog` named, for example, `example.properties`, to mount the Snowflake connector as the `snowflake` catalog. Create the file with the following contents, replacing the connection properties as appropriate for your setup:

```
connector.name=snowflake
connection-url=jdbc:snowflake://<account>.snowflakecomputing.com
connection-user=root
connection-password=secret
snowflake.account=account
snowflake.database=database
snowflake.role=role
snowflake.warehouse=warehouse
```



#### Arrow serialization support <a href="#arrow-serialization-support" id="arrow-serialization-support"></a>

This is an experimental feature which introduces support for using Apache Arrow as the serialization format when reading from Snowflake. Please note there are a few caveats:

* Using Apache Arrow serialization is disabled by default. In order to enable it, add `--add-opens=java.base/java.nio=ALL-UNNAMED` to the Trino JVM config.

#### Multiple Snowflake databases or accounts <a href="#multiple-snowflake-databases-or-accounts" id="multiple-snowflake-databases-or-accounts"></a>

The Snowflake connector can only access a single database within a Snowflake account. Thus, if you have multiple Snowflake databases, or want to connect to multiple Snowflake accounts, you must configure multiple instances of the Snowflake connector.

### Type mapping <a href="#type-mapping" id="type-mapping"></a>

Trino supports the following Snowflake data types:

| Snowflake Type | Trino Type     |
| -------------- | -------------- |
| `boolean`      | `boolean`      |
| `tinyint`      | `bigint`       |
| `smallint`     | `bigint`       |
| `byteint`      | `bigint`       |
| `int`          | `bigint`       |
| `integer`      | `bigint`       |
| `bigint`       | `bigint`       |
| `float`        | `real`         |
| `real`         | `real`         |
| `double`       | `double`       |
| `decimal`      | `decimal(P,S)` |
| `varchar(n)`   | `varchar(n)`   |
| `char(n)`      | `varchar(n)`   |
| `binary(n)`    | `varbinary`    |
| `varbinary`    | `varbinary`    |
| `date`         | `date`         |
| `time`         | `time`         |
| `timestampntz` | `timestamp`    |

Complete list of [Snowflake data types](https://docs.snowflake.com/en/sql-reference/intro-summary-data-types.html).

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides read access and write access to data and metadata in a Snowflake database. In addition to the globally available and read operation statements, the connector supports the following features:

* INSERT
* DELETE
* TRUNCATE
* CREATE TABLE
* CREATE TABLE AS
* DROP TABLE
* ALTER TABLE
* CREATE SCHEMA
* DROP SCHEMA

### Performance <a href="#performance" id="performance"></a>

The connector includes a number of performance improvements, detailed in the following sections.

#### Pushdown <a href="#pushdown" id="pushdown"></a>

The connector supports pushdown for a number of operations:

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

Note

The connector performs pushdown where performance may be improved, but in order to preserve correctness an operation may not be pushed down. When pushdown of an operation may result in better performance but risks correctness, the connector prioritizes correctness.

\
