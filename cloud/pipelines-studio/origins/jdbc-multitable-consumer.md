# JDBC Multitable Consumer

The JDBC Multitable Consumer origin reads database data from multiple tables and multiple schemas through a JDBC connection.&#x20;

Use the origin to read multiple tables from one or more schemas in the same database. For example, you might use the origin to perform database replication.

Data Pipelines includes database-specific origins, such as the Oracle Bulkload and SQL Server 2019 BDC Multitable Consumer origins. When available, StreamSets recommends using a database-specific origin. Data Pipelines also provides CDC origins to process changed data, and the JDBC Query Consumer origin to use a custom SQL query for processing.Important: This stage does not support connecting to non-RDBMS systems, including Hive, Impala, Kudu, or Snowflake. Support for untested systems is not guaranteed. For a list of tested systems, see "Database Vendors and Drivers".

When you configure the origin, you specify connection information and custom JDBC configuration properties to determine how the origin connects to the database. When the source database has high-precision timestamps, such as IBM Db2 TIMESTAMP(9) fields, you can configure the origin to write strings rather than datetime values to maintain the precision.

You define groups of database tables to read. The origin generates SQL queries based on the table configurations that you define, and then returns data as a map with column names and field values.

When you define the table configurations, you can optionally override the default key column and specify the initial offset to use. By default, the origin processes tables incrementally, using primary key columns or user-defined offset columns to track its progress. You can configure the origin to perform non-incremental processing to enable it to also process tables that do not have a key or offset column.

You can configure the origin to perform multithreaded partition processing, multithreaded table processing, or use the default - a mix of both. You also specify the processing batch strategy. When configuring partitions, you can configure the offset size, number of active partitions, and offset conditions.

You can configure advanced properties, such as the initial order to read from tables, connection related properties, and transaction isolation. And you can specify what the origin does when encountering an unsupported data type: convert the data to string or stop the pipeline.

When the pipeline stops, the JDBC Multitable Consumer origin notes where it stops reading. When the pipeline starts again, the origin continues processing from where it stopped by default. You can reset the origin to process all available data, using any initial offsets that you defined.

By default, the origin generates JDBC record header and field attributes that provide additional information about each record and field.

You can configure advanced connection properties. To use a JDBC version older than 4.0, you specify the driver class name and define a health check query.

The origin can generate events for an event stream.&#x20;

## Database Vendors and Drivers <a href="#ariaid-title2" id="ariaid-title2"></a>

The JDBC Multitable Consumer origin can read database data from multiple database vendors.

The following table lists the supported and tested database versions for this stage. You can use the stage with other JDBC-compliant databases, but full support is not guaranteed.&#x20;

| Database Vendor      | Supported Versions                                                                                                                                                                                                                                    | Tested Versions                                                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| MySQL                | MySQL 5.7 and later                                                                                                                                                                                                                                   | <ul><li>MySQL 5.7 with the MySQL Connector/J 8.0.12 driver</li><li>MySQL 8.0 with the MySQL Connector/J 8.0.12 driver</li></ul>     |
| Oracle               | <ul><li>Oracle 11g, 12c, 18c, 19c, and later</li><li>Oracle Real Application Clusters (RAC) 12c, 18c, 19c, and later</li></ul><p>Also supported:</p><ul><li>Hosted systems, such as AWS RDS</li><li>Derived systems, such as Oracle Exadata</li></ul> | <ul><li>Oracle 11g, 19c with the Oracle 19.3.0 JDBC driver</li><li>Oracle RAC 12c, 19c with the Oracle 19.3.0 JDBC driver</li></ul> |
| PostgreSQL           | PostgreSQL 9.x and later                                                                                                                                                                                                                              | <ul><li>PostgreSQL 9.6.9</li><li>PostgreSQL 10.4</li><li>PostgreSQL 11.7</li><li>PostgreSQL 12.2</li><li>PostgreSQL 13.0</li></ul>  |
| Microsoft SQL Server | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                                                                                                                                             | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                           |

### MySQL Data Types <a href="#ariaid-title3" id="ariaid-title3"></a>

The JDBC Multitable Consumer origin converts MySQL data types into Data Pipelines data types.

The origin supports the following MySQL data types:

| MySQL Data Type           | Data Pipelines Data Type |
| ------------------------- | ------------------------ |
| Bigint                    | Long                     |
| Bigint Unsigned           | Decimal                  |
| Binary                    | Byte Array               |
| Blob                      | Byte Array               |
| Char                      | String                   |
| Date                      | Date                     |
| Datetime                  | Datetime                 |
| Decimal                   | Decimal                  |
| Double                    | Double                   |
| Enum                      | String                   |
| Float                     | Float                    |
| Int                       | Integer                  |
| Int Unsigned              | Long                     |
| Json                      | String                   |
| Linestring                | Byte Array               |
| Medium Int                | Integer                  |
| Medium Int Unsigned       | Long                     |
| Numeric                   | Decimal                  |
| Point                     | Byte Array               |
| Polygon                   | Byte Array               |
| Set                       | String                   |
| Smallint                  | Short                    |
| Smallint Unsigned         | Integer                  |
| Text                      | String                   |
| Time                      | Time                     |
| Timestamp                 | Datetime                 |
| Tinyint, Tinyint Unsigned | Short                    |
| Varbinary                 | Byte Array               |
| Varchar                   | String                   |
| Year                      | Date                     |

### Oracle Data Types <a href="#ariaid-title4" id="ariaid-title4"></a>

The JDBC Multitable Consumer origin converts Oracle data types into Data Pipelines data types.

The stage supports the following Oracle data types:

| Oracle Data Type               | Data Pipelines Data Type |
| ------------------------------ | ------------------------ |
| Number                         | Decimal                  |
| Char                           | String                   |
| Varchar, Varchar2              | String                   |
| Nchar, NvarChar2               | String                   |
| Binary\_float                  | Float                    |
| Binary\_double                 | Double                   |
| Date                           | Datetime                 |
| Timestamp                      | Datetime                 |
| Timestamp with time zone       | Zoned\_datetime          |
| Timestamp with local time zone | Zoned\_datetime          |
| Long                           | String                   |
| Blob                           | Byte\_array              |
| Clob                           | String                   |
| Nclob                          | String                   |
| XMLType                        | String                   |

### PostgreSQL Data Types <a href="#ariaid-title5" id="ariaid-title5"></a>

The JDBC Multitable Consumer origin converts PostgreSQL data types into Data Pipelines data types.

The origin supports the following PostgreSQL data types:

| PostgreSQL Data Type                | Data Pipelines Data Type |
| ----------------------------------- | ------------------------ |
| Array                               | String                   |
| Bigint                              | Long                     |
| Bit Varying                         | String                   |
| Boolean                             | Boolean                  |
| Box                                 | String                   |
| Bytea                               | Byte Array               |
| Char                                | String                   |
| Cidr                                | String                   |
| Circle                              | String                   |
| Composite Type                      | String                   |
| Date                                | Date                     |
| Daterange                           | String                   |
| Decimal                             | Decimal                  |
| Double Precision                    | Double                   |
| Enum                                | String                   |
| Inet                                | String                   |
| Int4range, Int8range                | String                   |
| Integer                             | Integer                  |
| Interval                            | String                   |
| Json, Jsonb                         | String                   |
| Line                                | String                   |
| Lseg                                | String                   |
| Macaddr                             | String                   |
| Money                               | Double                   |
| Numeric                             | Decimal                  |
| Numrange                            | String                   |
| Path                                | String                   |
| Point                               | String                   |
| Polygon                             | String                   |
| Real                                | Float                    |
| Smallint                            | Short                    |
| Text                                | String                   |
| Time, Time with Time Zone           | Time                     |
| Timestamp, Timestamp with Time Zone | Time                     |
| Tsrange                             | String                   |
| Txtzrange                           | String                   |
| Uuid                                | String                   |
| Varchar                             | String                   |

### SQL Server Data Types <a href="#ariaid-title6" id="ariaid-title6"></a>

The JDBC Multitable Consumer origin converts SQL Server data types into Data Pipelines data types.

The origin supports the following SQL Server data types:

| SQL Server Data Type | Data Pipelines Data Type |
| -------------------- | ------------------------ |
| Bigint               | Long                     |
| Binary               | Byte\_Array              |
| Bit                  | Boolean                  |
| Char                 | String                   |
| Date                 | Date                     |
| Datetime, Datetime2  | Datetime                 |
| Datetimeoffset       | Zoned\_datetime          |
| Decimal              | Decimal                  |
| Float                | Double                   |
| Image                | Byte\_Array              |
| Int                  | Integer                  |
| Money                | Decimal                  |
| Nchar                | String                   |
| Ntext                | String                   |
| Numeric              | Decimal                  |
| Nvarchar             | String                   |
| Real                 | Float                    |
| Smalldatetime        | Datetime                 |
| Smallint             | Short                    |
| Smallmoney           | Decimal                  |
| Text                 | String                   |
| Time                 | Time                     |
| Tinyint              | Short                    |
| Varbinary            | Byte\_Array              |
| Varchar              | String                   |
| XML                  | String                   |

The origin does not support the following SQL Server data types:

* Geography
* Geometry

## Installing the JDBC Driver <a href="#ariaid-title7" id="ariaid-title7"></a>

Before you use the JDBC Multitable Consumer origin, install the JDBC driver for the database. You cannot access the database until you install the required driver.Note: When connecting to a PostgreSQL or Microsoft SQL Server database, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for those databases.

### Working with a MySQL JDBC Driver <a href="#ariaid-title8" id="ariaid-title8"></a>

Note the following issues that can occur when using a MySQL JDBC driver with the JDBC Multitable Consumer origin:The driver returns time values to the second.

Due to a MySQL JDBC driver issue, the driver cannot return time values to the millisecond. Instead, the driver returns the values to the second. For example, if a column has a value of 20:12:50.581, the driver reads the value as 20:12:50.000.

The origin might not read new rows created in MySQL while the pipeline is running.When using the default transaction isolation level, the origin might not read new rows that are created in MySQL as the pipeline is running. To resolve this issue, configure the origin to use the read committed transaction isolation level in the Advanced tab.

## Table Configuration <a href="#ariaid-title9" id="ariaid-title9"></a>

When you configure a JDBC Multitable Consumer origin, you define a table configuration for each group of tables that you want to read. A table configuration defines a group of tables with the same table name pattern, that are from one or more schemas with the same name pattern, and that have proper primary keys or the same user-defined offset columns.

You can define one or more table configurations.

For example, you can define one table configuration to replicate a database that has a proper primary key for each table. You simply enter the schema name and use the default table name pattern `%` which matches all tables in the schema.

Let's look at an example where you need to define more than one table configuration. Say that you want to copy a set of tables to an HBase cluster. The SALES schema contains ten tables, but you want to copy only the following four tables:

* `store_a`
* `store_b`
* `store_c`
* `customers`

The three store tables use `orderID` as the primary key. You want to override the primary key for the customers table, and so need to define `customerID` as the offset column for that table. You want to read all available data in the tables, so do not need to define an initial offset value.

You define one table configuration as follows so that the origin can read the three store tables:

* Schema - SALES
* Table Name Pattern - store%

Then you define the second table configuration as follows so that the origin can read the customers table:

* Schema - SALES
* Table Name Pattern - customers
* Override Offset Columns - enabled
* Offset Columns - customerID

### Schema, Table Name, and Exclusion Patterns <a href="#ariaid-title10" id="ariaid-title10"></a>

You define the group of tables that the JDBC Multitable Consumer origin reads by defining schema and table name patterns for the table configuration. The origin reads all tables with names that match the table pattern in the schemas with names that match the schema pattern.

The schema and table name patterns use the SQL LIKE syntax. For example, the LIKE syntax uses the percentage wildcard (%) to represent any string of zero or more characters. The schema name pattern `st%` matches schemas whose names start with `st`. The default table name pattern `%` matches all tables in the specified schemas.

For more information about valid patterns for the SQL LIKE syntax, see the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/ms179859.aspx).

You can optionally define a schema or table exclusion pattern to exclude some schemas or tables from being read. The schema and table exclusion patterns use a Java-based regular expression, or regex. For more information about using regular expressions with Data Pipelines, see Regular Expressions Overview.

For example, let's say that you want to read all tables in the `US_WEST` and `US_EAST` schemas except for tables that start with `dept`. You enter the following schema, table name pattern, and table exclusion pattern:

* Schema - US%
* Table Name Pattern - %
* Table Exclusion Pattern - dept.\*

Since you do not need to exclude any schemas, you simply leave the schema exclusion pattern empty.

Or, let's say that you want to read all tables from all schemas, except for the `sys` and `system` schemas. You enter the following schema, table name pattern, and schema exclusion pattern and leave the table exclusion pattern blank:

* Schema - %
* Table Name Pattern - %
* Schema Exclusion Pattern - sys|system

### Offset Column and Value <a href="#ariaid-title11" id="ariaid-title11"></a>

The JDBC Multitable Consumer origin uses an offset column and initial offset value to determine where to start reading data within tables and partitions.

By default, the origin uses the primary key of the tables as the offset column and uses no initial offset value. When you use multithreaded table processing and the table has a composite primary key, the origin uses each primary key as an offset column. You cannot use composite keys with multithreaded partition processing.

By default, the origin reads all available data from each table when you start the pipeline. The origin generates SQL queries using the following syntax when you start the pipeline:

```
SELECT * FROM <table> ORDER BY <offset column_1>, <offset column_2>, ...
```

Where `<offset column_`_`n`_`>` represents each primary key of the table, such as when the table has a composite primary key. When you restart the pipeline or when the origin switches back to a previously read table, the origin adds a WHERE clause to the SQL query to continue reading from the last saved offset.

To use this default behavior, you do not need to configure any of the offset properties.

You can make the following changes to how the origin handles offset columns and initial offset values:Override the primary key as the offset columnYou can override the primary key and define another offset column or columns. Or if the table doesn’t have a primary key, you can define the offset column or columns to use.Important: As a best practice, a user-defined offset column should be an incremental and unique column that does not contain null values. If the column is not unique - that is, multiple rows can have the same value for this column - there is a potential for data loss upon pipeline restart. For details, see Multiple Offset Value Handling.

Having an index on this column is strongly encouraged since the underlying query uses an ORDER BY and inequality operators on this column.

**Define an initial offset value**

The initial offset value is a value within the offset column where you want the JDBC Multitable Consumer origin to start reading. When you define an initial offset value, you must first enter the offset column name and then the value. If you are using the default primary key as the offset column, enter the name of the primary key.

If you define an initial offset value for a single offset column, the origin generates SQL queries using the following syntax:

```
SELECT * FROM <table> ORDER BY <offset column> WHERE <offset column> > ${offset}
```

If you defined multiple offset columns, you must define an initial offset value for each column, in the same order that the columns are defined. The origin uses the initial offset values of all columns to determine where to start reading data. For example, you override the primary key with the following offset columns: `p1`, `p2`, `p3` and define an initial offset value for each column. The origin generates SQL queries using the following syntax:

```
SELECT * FROM <table> ORDER BY p1, p2, p3 WHERE (p1 > ${offset1}) OR (p1 = ${offset1} AND p2 > ${offset2}) OR (p1 = ${offset1} AND p2 = ${offset2} AND p3 > ${offset3})
```

**Note**: Data Pipelines stores offsets for Datetime columns as Long values. For offset columns with a Datetime data type, enter the initial value as a Long value. You can use the time functions to transform a Datetime value to a Long value. For example, the following expression converts a date entered as a String to a Date object, and then to a Long value:

```
${time:dateTimeToMilliseconds(time:extractDateFromString('2017-05-01 20:15:30.915','yyyy-MM-dd HH:mm:ss.SSS'))} 
```

**Define additional offset column conditions**

You can use the expression language to define additional conditions that the origin uses to determine where to start reading data. The origin adds the defined condition to the WHERE clause of the SQL query.

You can use the `offset:column` function in the condition to access an offset column by position. For example, if you have a table with offset columns `p1` and `p2`, then `offset:column(0)` returns the value of `p1` while `offset:column(1)` returns the value of `p2`.

Let's say that you defined a `transaction_time` column as the offset column. While the origin reads the table, multiple active transactions are being written to the table with the current timestamp for the `transaction_time` column. When the origin finishes reading the first record with the current timestamp, the origin continues reading with the next offset and skips some rows with the current timestamp. You can enter the following offset column condition to ensure that the origin reads from all offset columns with a timestamp less than the current time:

```
${offset:column(0)} < ${time:now()}
```

If your database requires the datetime in a specific format, you can use the `time:extractStringFromDate` function to specify the format. For example:

```
${offset:column(0)} < '${time:extractStringFromDate(time:now(), "yyyy-MM-dd HH:mm:ss")}'
```

### Reading from Views <a href="#ariaid-title12" id="ariaid-title12"></a>

The JDBC Multitable Consumer origin can read from views in addition to tables.

The origin reads from all tables and views that are included in the defined table configurations. If a table configuration includes views that you do not want to read, simply exclude them from the configuration.

Use the origin to read from simple views that select data from a single table.

We do not recommend using the origin to read from complex views that combine data from two or more tables using joins. If the origin reads from complex views, it runs multiple queries in parallel which can cause a heavy workload on the database.

## Multithreaded Processing Modes <a href="#ariaid-title13" id="ariaid-title13"></a>

The JDBC Multitable Consumer origin performs parallel processing and enables the creation of a multithreaded pipeline. The origin can use multiple threads to process entire tables or partitions within tables.

By default, the origin performs multithreaded partition processing for the tables that fulfill the partition processing requirements, while performing multithreaded table processing for all other tables. When using the default behavior, the origin notes the tables that allow partition processing in the Data Pipelines log. When needed, you can configure the origin to require partition processing for all tables or to perform only table processing. You can also allow the single-threaded non-incremental processing of tables when needed.

The origin provides the following multithreaded processing modes:

* Multithreaded table processing - The origin can use up to one thread per table. Can process tables with multiple offset columns.
*   Multithreaded partition processing - The origin can use up to one thread per table partition. Use to process larger volumes of data than multithreaded table processing.

    Multithreaded partition processing requires a single primary key or user-defined offset column of a supported data type, and additional details for partition creation. Tables with composite keys or a key or user-defined offset column of an unsupported data type cannot be partitioned.

When you configure the origin, you specify the tables to process and the multithreaded partition processing mode to use for each set of tables:

*   Off - Use to perform multithreaded table processing.

    Can be used to perform non-incremental loads of tables without key or offset columns, when enabled.
*   On (Best Effort) - Use to perform partition processing where possible and allow multithreaded table processing for tables with multiple key or offset columns.

    Can be used to perform non-incremental loads of tables without key or offset columns, when enabled.
*   On (Required) - Use to perform partition processing for all specified tables.

    Does not allow performing other types of processing for tables that do not meet the partition processing requirements.

### Multithreaded Table Processing <a href="#ariaid-title14" id="ariaid-title14"></a>

When performing multithreaded table processing, the JDBC Multitable Consumer origin retrieves the list of tables defined in the table configuration when you start the pipeline. The origin then uses multiple concurrent threads based on the Number of Threads property. Each thread reads data from a single table, and each table can have a maximum of one thread read from it at a time.Note: The Maximum Pool Size property on the Advanced tab defines the maximum number of connections the origin can make to the database. It must be equal to or greater than the value defined for the Number of Threads property.

As the pipeline runs, each thread connects to the origin system, creates a batch of data, and passes the batch to an available pipeline runner. A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin.

Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

The order of batch processing depends on many factors. For more information, see Processing Queue.

For more information about multithreaded pipelines, see Multithreaded Pipeline Overview.

**Example**

Say you are reading from ten tables. You set the Number of Threads property to 5 and the Maximum Pool Size property to 6. When you start the pipeline, the origin retrieves the list of tables. The origin then creates five threads to read from the first five tables, and by default Data Pipelines creates a matching number of pipeline runners. Upon receiving data, a thread passes a batch to each of the pipeline runners for processing.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

### Multithreaded Partition Processing <a href="#ariaid-title15" id="ariaid-title15"></a>

By default, the JDBC Multitable Consumer origin performs multithreaded partition processing for all tables that meet the partition processing requirements, and performs table processing for all other tables.

To perform multithreaded processing of partitions within a table, you enable partition processing in the table configuration, then specify the partition size and the maximum number of partitions to use. Limiting the number of partitions also limits the number of threads that can be dedicated to processing data in the table.

When you configure a set of tables for unlimited partitions, the origin creates up to twice as many partitions as the pipeline thread count. For example, if you have 5 threads, the table can have up to 10 partitions.

Similar to multithreaded table processing, each thread reads data from a single partition, and each partition can have a maximum of one thread read from it at a time.

When processing partitions, the processing order depends on many factors. For a full description, see Processing Queue.

**Partition Processing Requirements**

To perform multithreaded partition processing for a table, the table must meet the following requirements:Single key or offset columnThe table must have a single primary key or user-defined offset column. Performing multithreaded partition processing on a table with composite keys generates an error and stops the pipeline.If a table does not have a primary key column, you can use the Override Offset Columns property to specify a valid offset column to use. Having an ascending index on the offset column is strongly encouraged since the underlying query uses an ORDER BY and inequality operators on this column.Numeric data typeTo use partition processing, the primary key or user-defined offset column must have a numeric data type that allows arithmetic partitioning.The key or offset column must be one of the following data types:

* Integer-based: Integer, Smallint, Tinyint
*   Long-based: Bigint, Date, Time, Timestamp.

    The Oracle data type Timestamp with time zone is also supported, as long as each row has the same time zone.
* Float-based: Float, Real
* Double-based: Double
* Precision-based: Decimal, Numeric

**Multiple Offset Value Handling**

When processing partitions, the JDBC Multitable Consumer origin can process multiple records with the same offset value. For example, the origin can process multiple records with the same timestamp in a `transaction_date` offset column.

Warning: When processing multiple records with the same offset value, records can be dropped if you stop the pipeline when the origin is processing a series of records with the same offset value.

When you stop the pipeline as the origin is processing a series of records with the same offset value, the origin notes the offset. Then, when you restart the pipeline, it starts with a record with the next logical offset value, skipping any unprocessed records that use the same last-saved offset.

For example, say you specified a datetime column as a user-defined offset column, and five records in the table share the same datetime value. Now say you happen to stop the pipeline after it processes the second record. The pipeline stores the datetime value as the offset where it stopped. When you restart the pipeline, processing begins with the next datetime value, skipping the three unprocessed records with the last-saved offset value.

**Best Effort: Processing Non-Compliant Tables**

To process tables in a table configuration that might not meet the partition processing requirements, you can use the On (Best Effort) option when you configure the Multithreaded Partition Processing mode property.

When you select the best effort option, the origin performs multithreaded partition processing for all tables that meet the partition processing requirements. The origin performs multithreaded table processing for tables that include multiple key or offset columns. And if you enable [non-incremental processing](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/MultiTableJDBCConsumer.html#concept\_xwr\_bhm\_nbb), the origin can also process all tables that do not include key or offset columns.

## Non-Incremental Processing <a href="#ariaid-title19" id="ariaid-title19"></a>

You can configure the JDBC Multitable Consumer origin to perform non-incremental processing for tables with no primary keys or user-defined offset columns. By default, the origin performs incremental processing and does not process tables without a key or offset column.

You can enable non-incremental processing for the set of tables defined in a table configuration on the Tables tab.

Note: When enabling non-incremental processing for a table without a key or offset column, you cannot require [multithreaded partition processing](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/MultiTableJDBCConsumer.html#concept\_gvy\_yws\_p1b) for the table configuration. That is, you cannot run the pipeline with the Multithreaded Partition Processing Mode property set to On (Required).

Use On (Best Effort) or Off to perform non-incremental processing of the table. With either option selected, table is processed using a single thread, like [multithreaded table processing](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/MultiTableJDBCConsumer.html#concept\_tz5\_fw5\_gz).

When you enable non-incremental processing, the origin processes any table without a key or offset column as follows:

* The origin uses a single thread to process all available data in the table.
*   After the origin processes all available data, it notes that the table has been processed as an offset. So, if you stop and restart the pipeline after the origin completes all processing, the origin does not reprocess the table.

    If you want to reprocess data in the table, you can reset the origin before restarting the pipeline. This resets the origin for all tables that the origin processes.
* If the pipeline stops while the origin is still processing available data, when the pipeline restarts, the origin reprocesses the entire table. This occurs because the table has no key or offset column to allow for tracking progress.

For example, say you configure the origin to use five threads and process a set of tables that includes a table with no key or offset column. To process data in this table, you enable the Enable Non-Incremental Load table configuration property. You also set Multithreaded Partition Processing Mode to On (Best Effort) to allow the origin to use multithreaded partition processing when possible and allow both non-incremental processing and multithreaded table processing when needed.

When you start the pipeline, the origin allocates one thread to the table that requires non-incremental processing. It processes the table data using multithreaded table processing until all data is processed. When the thread completes processing all available data, the origin notes this as part of the offset and the thread becomes available to process data from other tables. In the meantime, the four other threads process data from the rest of the tables using multithreaded partition processing when possible.

## Batch Strategy <a href="#ariaid-title20" id="ariaid-title20"></a>

You can specify the batch strategy to use when processing data. The batch strategy behaves differently depending on whether you use multithreaded table processing or multithreaded partition processing.

### Process All Available Rows <a href="#ariaid-title21" id="ariaid-title21"></a>

The Process All Available Rows from the Table batch strategy differs slightly depending on whether the origin is processing full tables or partitions within a table.Multithreaded table processing

When the origin performs multithreaded table processing for all tables, each thread processes all available rows from a table. A thread runs a SQL query and processes all of the results for a table. Then, the thread switches to the next available table.

For example, let's say the origin has a maximum batch size of 100 and uses two threads to read from four tables, each of which contains 1000 rows. Thread 1 runs a SQL query to create 10 batches of 100 rows each from table A, while thread 2 uses the same strategy to read data from table B.

When table A and table B are fully read, the threads switch to table C and table D and complete the same process. When thread 1 finishes reading from table C, it switches back to the next available table to read any data that has arrived since the table was processed, starting from the last saved offset.

The number of threads that can process the tables is limited by the Number of Threads property for the origin.

When the tables being processed use both table and partition processing, the threads query the partitions as described below. For details on how the tables and partitions rotate through the processing queue, see Processing Queue.

Multithreaded partition processingMultithreaded partition processing is similar to multithreaded table processing, except that it works at a partition level. Each thread runs a SQL query for a partition and processes multiple batches of data from the results. When all data in the partition is processed, the thread switches to the next available partition.

The number of threads that can process partitions for each table is limited by the Number of Threads property for the origin and the Max Active Partitions table property.For details on how the tables and partitions rotate through the processing queue, see Processing Queue.

### Switch Tables <a href="#ariaid-title22" id="ariaid-title22"></a>

The Switch Tables batch strategy differs depending on whether the origin performs full table or partition processing. The number of batches that the origin creates before closing a result set is based on the Batches from Result Set property.

**Note**: The Switch Tables batch strategy can use a large amount of heap memory, depending on the Fetch Size property and the JDBC driver being used. For example, some drivers prefetch the rows for each table to reduce the number of round trips to the database, which can result in a large amount of heap memory usage.

**Multithreaded table processing**

When the origin performs multithreaded table processing for all tables, each thread processes one batch of data, then switches to an available table and repeats the process. When a pipeline starts, each thread runs a SQL query, generates a result set, and processes a batch of records from the result set. The database driver caches the remaining records for the same thread to access again. Then, the thread switches to the next available table.A table is available in the following situations:

*   The table does not have a result set created by another thread.

    In this case, the thread runs a SQL query to generate a result set and processes a batch of records. The database driver caches the remaining records for later use.
*   The table has a cached result set created by the same thread.

    In this case, the thread creates the batch from the result set, rather than running another SQL query.

A table is not available when the table has a result set created by another thread. A table with a cached result set can only be processed by the thread that created the result set. And a table cannot have multiple cached result sets.

The Max Batch Size property limits the number of records included in a batch. The Batches from Result Set property limits the number of batches that a thread can create from the result set. After a thread creates the configured number of batches, the thread closes the result set. This allows a different thread to read from the table.For example, let's say an origin uses two concurrent threads to read from three tables. Max Batch Size is set to 100 records, and Batches from Result Set is set to 2.Thread 1 runs a SQL query on table A, which contains 80 records. The thread processes the 80 records in a single batch. In the meantime, thread 2 runs a SQL query on table B, which contains 5000 records, and caches the result set. The thread creates a batch to process the first 100 records in the table.After processing table A, thread 1 looks for the next table to process. Thread 1 skips table B because the table has a cached result set from thread 2, so thread 1 runs a SQL query on table C, which contains 1000 records, and caches the result set. Thread 1 processes the first 100 records in the table.Similarly, when thread 2 completes the batch on table B, it skips table C because thread 1 has a result set cached for the table. If table A had new data that came in, thread 2 would process that data, since there is no result set for the table. But if table A has no additional data, thread 2 returns to table B, where it has a cached result set.Since thread 2 has a cached result set on table B, it creates a new batch from the result set instead of running a SQL query. Since this is the second batch from the result set, thread 2 closes the result set after processing the batch. This allows either thread to process data from table B the next time they check the table.

When the tables being processed use both table and partition processing, the threads query the partitions as described below. For details on how the tables and partitions rotate through the processing queue, see Processing Queue.

**Multithreaded partition processing**

Multithreaded partition processing is similar to multithreaded table processing. The behavior around caching the result set and the number of batches to process from the result set is the same, but at a partition level. Threads also skip processing partitions that already have a result set cached from a different thread.

The difference is, when a thread works on a partition, all partitions from the same table are moved to the end of the processing queue. This allows the origin to switch to the next available table.

For examples of how tables and partitions rotate through the processing queue, see Processing Queue.

## Initial Table Order Strategy <a href="#ariaid-title23" id="ariaid-title23"></a>

You can define the initial order that the origin uses to read the tables.

Define one of the following initial table order strategies:NoneReads the tables in the order that they are listed in the database.AlphabeticalReads the tables in alphabetical order.Referential ConstraintsReads the tables based on the dependencies between the tables. The origin reads the parent table first, and then reads the child tables that refer to the parent table with a foreign key.You cannot use the referential constraints order when the tables to be read have a cyclic dependency. When the origin detects a cyclic dependency, the pipeline fails to validate with the following error:

```
JDBC_68 Tables referring to each other in a cyclic fashion.
```

Note that the referential constraints order can cause pipeline validation or initialization to slow down because the origin has to sort the tables before reading them.

The origin uses this table order only for the initial reading of the tables. When threads switch back to previously read tables, they read from the next available table, regardless of the defined order.

## Processing Queue <a href="#ariaid-title24" id="ariaid-title24"></a>

The JDBC Multitable Consumer origin maintains a virtual queue to determine the data to process from different tables. The queue includes each table defined in the origin. When a table is to be processed by partition, multiple partitions for the table are added to the queue, limited by the Max Partitions property defined for each table configuration on the Tables tab.

The origin rotates and reorganizes the queue based on the Per Batch Strategy property. The origin processes data from the queue with the threads specified in the Number of Threads property and the Batches from Result Set property.

Below are some scenarios to show how the queue works.

#### Multithreaded Table Processing Only <a href="#ariaid-title25" id="ariaid-title25"></a>

The origin performs only multithreaded table processing in either of the following situations:

* Multithreaded Partition Processing Mode property is set to Off.
* Multithreaded Partition Processing Mode property is set to On (Best Effort) and no tables meet the partition processing requirements.

Say you have tables A, B, C and D that you configure for table processing. When you start the pipeline, the origin adds all tables to the queue. If configured, the Initial Table Order Strategy advanced property can affect the order. Let's assume we have no referential constraints and use alphabetical order:

```
A  B  C  D
```

When a thread becomes available, it processes data from the first table in the queue. The processing of the tables depends on how you define the Per Batch Strategy property:

**Process All Available Rows in the Table**

With this batch strategy, threads process all data in a table before proceeding to the next table.

For example, say you have two threads. Thread 1 processes table A, thread 2 processes table B. That leaves the remaining tables in the queue, as follows:

```
C  D
```

The first available thread then processes table C, and the next processes table D.

If additional data is written to one of the processed tables, the table is placed at the end the queue.

**Switch Tables**

With this batch strategy, each thread processes a single batch from a table before moving to the next available table, so the threads cycle through the tables quickly.After a thread performs a SQL query, it creates a batch and caches a result set for future use. As long as a thread has a result set cached for a table, other threads skip that table. The result set is closed based on the Batches from Result Set property. So, if the property is set to 2, then the thread must cycle back and create a second batch for the table before closing the result set. Closing the result set enables any thread to process the table.

Say you have two threads, one processes table A, the other processes table B. They each create one batch and cache a result set. Then, those tables move to the back of the queue, as follows:

```
C  D  A  B
```

The threads repeat the same process for table C and D, and those tables move to the back of the queue:

```
A  B  C  D  
```

So, after four batches are processed, the queue looks like it did in the beginning.

When cycling back to table A and B, each thread returns to the original table that they processed, creating a batch from its cached result set. That is, if thread 1 cached a result set on table A and thread 2 cached a result set on table B, they both go back to those tables to process a second batch.

After each thread processes a second batch from their result sets, they close the result sets. Then, the next time the tables appear at the front of the queue, either thread can process them.

For more information about batch strategies, see Batch Strategy.

### Multithreaded Partition Processing Only <a href="#ariaid-title26" id="ariaid-title26"></a>

The origin performs only multithreaded partition processing in either of the following situations:

* Multithreaded Partition Processing Mode property is set to On (Required).
* Multithreaded Partition Processing Mode property is set to On (Best Effort) and all tables meet the partition processing requirements.

Say you have tables A, B, and C, and all three tables have a lot of data to process. Tables A and B are configured with a maximum of 3 active partitions. And since table C has the largest volume of data, you allow an unlimited number of partitions, which means double the number of threads for the pipeline. Again, let's use the alphabetical initial table ordering.

A partition remains in the queue until the origin confirms that there is no more data in the partition. When a thread becomes available, it creates a batch from the first available partition of the first table in the queue. The order of tables and partitions in the queue depends on how you define the Per Batch Strategy, as follows:

**Process All Available Rows in the Table**

As when processing tables, when processing partitions with this batch strategy, threads process all data in a partition before proceeding to the next partition.

**Note**: In practice, this means that rows from subsequent tables can be processed before a previous table is completed, since available threads continue to pick up partitions from the queue.

For example, say we configure the pipeline for 4 threads. That means table C can have up to 8 partitions in the queue at any given time. So the initial queue looks like this:

```
A1  A2  A3  B1  B2  B3  C1  C2  C3  C4  C5  C6  C7  C8 
```

The four threads start processing on the first four partitions in the queue: A1, A2, A3, and B1. This puts B2 at the front of the queue, ready for the next available thread.

```
B2  B3  C1  C2  C3  C4  C5  C6  C7  C8
```

Note that in this situation, the processing of table B data begins before table A is fully processed.After the four threads process another four sets of batches, the queue looks like this:

```
C3  C4  C5  C6  C7  C8
```

If additional data is written to the processed partitions, those partitions are placed at the end of the queue. So if data is written to A3, the queue looks like this:

```
C3  C4  C5  C6  C7  C8  A3
```

**Switch Tables**

As when processing tables, when processing partitions with this batch strategy, each thread processes a single batch from a partition before moving to the next available partition, so the threads cycle through the partitions quickly.

Also like processing tables, when a thread performs a SQL query to create a batch, it also caches a result set for future use. As long as a thread has a result set cached for a partition, other threads skip that partition. The result set is closed based on the Batches from Result Set property. So, if the property is set to 2, then the thread must cycle back and create a second batch for the partition before closing the result set. Closing the result set enables any thread to access the partition.

Unique to partition processing, this batch strategy forces all subsequent, consecutive partitions from the same table to the end of the queue each time a thread processes a batch from a partition.

Let's start again with the initial batch order, but this time let's say we have three threads, so table C is limited to 6 partitions:

```
A1  A2  A3  B1  B2  B3  C1  C2  C3  C4  C5  C6
```

When the pipeline starts, thread 1 takes A1, processing one batch and caching a result set. To switch tables, the rest of the table A partitions move to the end of the queue, and A1 takes the last position because it still has additional data to be processed:

```
B1  B2  B3  C1  C2  C3  C4  C5  C6  A2  A3  A1 
```

Similarly, thread 2 takes B1, processing one batch and caching a result set. The rest of the table B partitions move to the end of the queue, with B1 at the very end:

```
C1  C2  C3  C4  C5  C6  A2  A3  A1  B2  B3  B1
```

Thread 3 takes C1, processing one batch and caching the rest of the data in a result set. The rest of the table C partitions move to the end of the queue, with C1 at the very end:

```
A2  A3  A1  B2  B3  B1  C2  C3  C4  C5  C6  C1
```

Available threads continue taking the first partition in the queue as follows:

```
B2  B3  B1  C2  C3  C4  C5  C6  C1  A3  A1  A2
C2  C3  C4  C5  C6  C1  A3  A1  A2  B3  B1  B2
A3  A1  A2  B3  B1  B2  C3  C4  C5  C6  C1  C2
B3  B1  B2  C3  C4  C5  C6  C1  C2  A1  A2  A3
C3  C4  C5  C6  C1  C2  A1  A2  A3  B1  B2  B3
A1  A2  A3  B1  B2  B3  C4  C5  C6  C1  C2  C3
```

At this point, all of the A and B partitions, and partitions C1, C2, and C3, have one batch of data processed and cached result sets. The remaining C partitions have yet to be touched.

A1 was processed by thread 1. Say, A2 and A3 were processed by thread 3, and thread 2 is now available. Which partition does thread 2 take?

Thread 2 skips all of the A partitions because they have existing cached result sets from other threads. Instead, thread 2 takes B1, which has a result set that it created. Then, the other B partitions move to the end with B1 following. The A partitions remain at the front of the queue:

```
A1  A2  A3  C4  C5  C6  C7  C8  C1  C2  C3  B2  B3  B1
```

After processing this second batch from the B1 result set, thread 2 closes the result set. This allows any thread to process data from B1 the next time it comes to the front of the queue.

If thread 3 becomes available next, it also skips A1 and takes A2, which has a result set that it created. This results in the following order:

```
C4  C5  C6  C7  C8  C1  C2  C3  B2  B3  B1  A1  A3  A2
```

After processing this second batch from the A2 result set, thread 3 closes the result set. This allows any thread to process data from A2 the next time it comes to the front of the queue.

For more information about batch strategies, see Batch Strategy.

### Both Multithreaded Partition and Table Processing <a href="#ariaid-title27" id="ariaid-title27"></a>

The origin performs both multithreaded partition processing and multithreaded table processing in the following situation:

* Multithreaded Partition Processing Mode property is set to On (Best Effort) and some tables meet the partition processing requirements while other tables do not.

When processing a mix of full tables and partitioned tables, the queue basically behaves the same as when processing only partitions, with full tables being processed as a single partition. Let's walk through it....

Say we have table A being processed without partitions, and table B configured with a maximum of 3 partitions, and table C with no limit. As in the example above, the pipeline has 3 threads to work with which grants 6 partitions to table C. Using the alphabetical initial table ordering, the initial queue looks like this:

```
A  B1  B2  B3  C1  C2  C3  C4  C5  C6
```

When a thread becomes available, it processes the first table or partition in the queue. The order of the queue depends on how you define the Per Batch Strategy, as follows:

**Process All Available Rows in the Table**

When processing tables and partitions with this batch strategy, threads process all data in a table or partition before proceeding to the next partition. An unpartitioned table, like table A, is processed like a table with a single partition.

When the pipeline starts, the 3 threads process a set of batches from table A and from partitions B1 and B2, leaving the queue like this:

```
B3  C1  C2  C3  C4  C5  C6
```

As each thread completes processing, it picks up the next table or partition at the front of the queue. After each of the 3 threads takes another table or partition, the queue looks like this:

```
C3  C4  C5  C6  
```

If additional data is written to one of the processed tables or partitions, the table or partition is placed at the end of the queue.

**Switch Tables**

When processing tables and partitions with this batch strategy, each thread processes a single batch from a table or partition before moving to the next available table or partition, so the threads cycle through the queue quickly.

When a thread performs a SQL query to create a batch, it also caches a result set for future use. As long as a thread has a result set cached for a table or partition, other threads skip that table or partition. The result set is closed based on the Batches from Result Set property. So, if the property is set to 2, then the thread must cycle back and create a second batch for the table or partition before closing the result set. This enables any thread to access the table or partition.

When processing tables and partitions, this batch strategy forces all subsequent, consecutive partitions from the same table to the end of the queue. It also treats unpartitioned tables as a single partition. As a result, the queue rotation is a simplified version of processing only partitioned tables.

So we have this initial order:

```
A  B1  B2  B3  C1  C2  C3  C4  C5  C6
```

Thread 1 processes a batch from table A and caches a result set. Then, table A moves to the end of the queue:

```
B1  B2  B3  C1  C2  C3  C4  C5  C6  A
```

Thread 2 processes batch from B1 and caches a result set. The rest of the table B partitions move to the end of the queue, with B1 at the end because it contains more data to be processed:

```
C1  C2  C3  C4  C5  C6  A  B2  B3  B1
```

Thread 3 processes a batch from C1 and caches a result set. The rest of the table C partitions move to the end of the queue, with C1 at the end because it contains more data to be processed:

```
A  B2  B3  B1  C2  C3  C4  C5  C6  C7  C8  C1
```

Now, say thread 2 is the first thread to become available. Though table A is at the front of the queue, table A also has a result set cached by thread 1. Only thread 1 can process table A while the result set remains open, so thread 2 skips table A and takes B2.

This leaves A in the front of the queue. The B partitions get pushed to the end of the queue, with B2 at the very end, as follows:

```
A C2  C3  C4  C5  C6  C7  C8  C1  B3  B1  B2
```

Thread 1 then becomes available. Since it has a cached result set on table A, it creates a batch from the result set and moves A to the end of the queue:

```
B2  B3  B1  C2  C3  C4  C5  C6  C7  C8  C1  A
```

After processing this second batch from the table A result set, thread 1 closes the result set. This allows any thread to process data from table A the next time it comes to the front of the queue.

For more information about batch strategies, see Batch Strategy.

## JDBC Attributes <a href="#ariaid-title28" id="ariaid-title28"></a>

The JDBC Multitable Consumer origin generates record header attributes and field attributes that provide additional information about each record and field.

The origin receives these details from the JDBC driver.

### JDBC Header Attributes <a href="#ariaid-title29" id="ariaid-title29"></a>

By default, the JDBC Multitable Consumer origin generates JDBC record header attributes that provide additional information about each record, such as the original data type of a field or the source tables for the record.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes. For more information about working with record header attributes, see Working with Header Attributes.

JDBC record header attributes include a `jdbc` prefix to differentiate the JDBC attributes from other record header attributes.

You can configure the origin to skip creating JDBC header attributes by clearing the Create JDBC Header Attributes property on the Advanced tab.

The origin can provide the following JDBC header attributes:

| JDBC Header Attribute         | Description                                                                                                                                                                                                                                                                                                                                   |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| jdbc.tables                   | <p>Provides a comma-separated list of source tables for the fields in the record.Note: Not all JDBC drivers provide this information.</p><p>Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.</p> |
| jdbc.partition                | Provides the full offset key for the partition that produced the record                                                                                                                                                                                                                                                                       |
| jdbc.threadNumber             | Provides the number of the thread that produced the record.                                                                                                                                                                                                                                                                                   |
| jdbc.\<column name>.jdbcType  | Provides the numeric value of the original SQL data type for each field in the record. See the [Java documentation](https://docs.oracle.com/javase/8/docs/api/constant-values.html#java.sql.Types.ARRAY) for a list of the data types that correspond to numeric values.                                                                      |
| jdbc.\<column name>.precision | Provides the original precision for all numeric and decimal fields.                                                                                                                                                                                                                                                                           |
| jdbc.\<column name>.scale     | Provides the original scale for all numeric and decimal fields.                                                                                                                                                                                                                                                                               |

### JDBC Field Attributes <a href="#ariaid-title30" id="ariaid-title30"></a>

The JDBC Multitable Consumer origin generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

The following data type conversions do not include all information in the corresponding Data Pipelines data type:

* Database Decimal and Numeric data types are converted to the Data Pipelines Decimal data type, which does not store scale and precision.
* The database Timestamp data type is converted to the Data Pipelines Datetime data type, which does not store nanoseconds.

To preserve this information during data type conversion, the origin generates the following field attributes for these Data Pipelines data types:

| Data Pipelines Data Type | Generated Field Attribute | Description                                                          |
| ------------------------ | ------------------------- | -------------------------------------------------------------------- |
| Decimal                  | precision                 | Provides the original precision for every decimal or numeric column. |
| Decimal                  | scale                     | Provides the original scale for every decimal or numeric column.     |
| Datetime                 | nanoSeconds               | Provides the original nanoseconds for every timestamp column.        |

You can use the `record:fieldAttribute` or `record:fieldAttributeOrDefault` functions to access the information in the attributes. For more information about working with field attributes, see Field Attributes.

## Event Generation <a href="#ariaid-title31" id="ariaid-title31"></a>

The JDBC Multitable Consumer origin can generate events that you can use in an event stream. When you enable event generation, the origin generates an event when it completes processing the data returned by the specified queries for all tables. It also generates events when it completes processing the data returned from a table and the data returned from a schema.

JDBC Multitable Consumer events can be used in any logical way. For example:

*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin continues processing from the last-saved offset unless you reset the origin.

    For an example, see Stopping a Pipeline After Processing All Available Data.
*   With the Email executor to send a custom email after receiving an event.

    For an example, see Sending Email During Pipeline Processing.
*   With a destination to store information about completed queries.

    For an example, see Preserving an Audit Trail of Events.

For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

### Event Record <a href="#ariaid-title32" id="ariaid-title32"></a>

Event records generated by JDBC Multitable Consumer origin have the following event-related record header attributes:

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following type:</p><ul><li>no-more-data - Generated when the origin completes processing all data returned by the queries for all tables.</li><li>schema-finished - Generated when the origin completes processing all rows within a schema.</li><li>table-finished - Generated when the origin completes processing all rows within a table.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                                                                                    |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                                                                               |

The JDBC Multitable Consumer origin can generate the following event record:

no-more-dataThe JDBC Multitable Consumer origin generates a no-more-data event record when the origin completes processing all data returned by the queries for all tables.

You can configure the origin to delay the generation of the no-more-data event by a specified number of seconds. You might configure a delay to ensure that the schema-finished or table-finished events are generated and delivered to the pipeline before the no-more-data event record.

To use a delay, configure the No-more-data Event Generation Delay property.

The no-more-data event record generated by the origin has the `sdc.event.type` record header attribute set to `no-more-data` and does not include any additional fields.

**schema-finished**

The JDBC Multitable Consumer origin generates a schema-finished event record when the origin completes processing all data within a schema.The schema-finished event record has the following additional fields:

| Event Record Field | Description                                                         |
| ------------------ | ------------------------------------------------------------------- |
| schema             | The schema that has returned no remaining data to be processed.     |
| tables             | A list of the tables within the schema that have no remaining data. |

**table-finished**

The JDBC Multitable Consumer origin generates a table-finished event record when the origin completes processing all data within a table.The table-finished event record has the following additional fields:

| Event Record Field | Description                                                                      |
| ------------------ | -------------------------------------------------------------------------------- |
| schema             | The schema associated with the table that has no remaining data to be processed. |
| table              | The table that has no remaining data to be processed.                            |

## Configuring a JDBC Multitable Consumer <a href="#ariaid-title33" id="ariaid-title33"></a>

Configure a JDBC Multitable Consumer origin to use a JDBC connection to read database data from multiple tables.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                |
    | Produce Events   | Generates event records when events occur. Use for event handling.                                                                                                                                                   |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul> |
2.  On the JDBC tab, configure the following properties:

    | JDBC Property                                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JDBC Connection String                        | <p>Connection string used to connect to the database. Use the connection string format required by the database vendor.</p><p>For example, use the following formats for these database vendors:</p><ul><li>MySQL - <code>jdbc:mysql://&#x3C;host>:&#x3C;port>/&#x3C;database_name></code></li><li>Oracle - <code>jdbc:oracle:&#x3C;driver_type>:@&#x3C;host>:&#x3C;port>:&#x3C;service_name></code></li><li>PostgreSQL - <code>jdbc:postgresql://&#x3C;host>:&#x3C;port>/&#x3C;database_name></code></li><li>SQL Server - <code>jdbc:sqlserver://&#x3C;host>:&#x3C;port>;databaseName=&#x3C;database_name></code></li></ul><p>You can optionally include the user name and password in the connection string.</p>                                                                                                                                                     |
    | Use Credentials                               | Enables entering credentials on the Credentials tab. Use when you do not include credentials in the JDBC connection string.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Queries per Second                            | <p>Maximum number of queries to run in a second across all partitions and tables. Use 0 for no limit.</p><p>Default is 10.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Number of Threads                             | <p>Number of threads the origin generates and uses for multithreaded processing.</p><p>Configure the Maximum Pool Size property on the Advanced tab to be equal to or greater than this value.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Per Batch Strategy                            | <p>Strategy to create each batch of data:</p><ul><li>Switch Tables - When performing only multithreaded table processing, each thread creates a batch of data from one table, and then switches to the next available table to create the next batch. Define the Batches from Result Set property when you configure a Switch Tables strategy.</li><li>Process All Available Rows From the Table - When performing only multithreaded table processing, each thread processes all data in a table before moving to the next table.</li></ul><p>When performing multithreaded partition processing or a mix of table and partition processing, the behavior for each batch strategy is more complicated. For details, see Batch Strategy or Processing Queue.</p>                                                                                                       |
    | Max Batch Size (records)                      | Maximum number of records to include in a batch.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Batches from Result Set                       | <p>Maximum number of batches to create from a result set. After a thread creates this number of batches from a result set, the result set closes. Then, any available thread can read from the table or partition.</p><p>Use a positive integer to set a limit on the number of batches created from the result set. Use -1 to create an unlimited number of batches from a result set.</p><p>By default, the origin creates an unlimited number of batches from the result set, keeping the result set open as long as possible.</p><p>Available when using the Switch Tables batch strategy.</p>                                                                                                                                                                                                                                                                     |
    | Result Set Cache Size                         | <p>Number of result sets to cache in the database. Use a positive integer to set a limit on the number of cached result sets. Use -1 to opt out of this property.</p><p>By default, the origin caches an unlimited number of result sets.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | Max Clob Size (characters)                    | Maximum number of characters to be read in a Clob field. Larger data is truncated.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Max Blob Size (bytes)                         | Maximum number of bytes to be read in a Blob field.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Number of Retries on SQL Error                | <p>Number of times a thread tries to read a batch of data after receiving an SQL error. After a thread retries this number of times, the thread handles the error based on the error handling configured for the origin.</p><p>Use to handle transient network or connection issues that prevent a thread from reading a batch of data.</p><p>Default is 0.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Data Time Zone                                | Time zone to use to evaluate datetime-based offset column conditions.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | No-more-data Event Generation Delay (seconds) | Number of seconds to delay generation of the no-more-data event after processing all rows. Use to allow time for additional data to arrive before generating the no-more-data event.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Quote Character                               | <p>Quote character to use around schema, table, and column names in the query. Select the character that the database uses to permit lower case, mixed-case, or special characters in schema, table, or column names:</p><ul><li>None - Uses no character around names in the query. For example: <code>select * from mySchema.myTable order by myOffsetColumn</code>.</li><li>Backtick - Uses a backtick around names in the query. For example: <code>select * from `mySchema`.`myTable` order by `myOffsetColumn`</code>.</li><li>Double Quotes - Uses double quotes around names in the query. For example: <code>select * from "mySchema"."myTable" order by "myOffsetColumn"</code>.</li><li>Square Brackets - Uses square brackets around names in the query. For example: <code>select * from [mySchema].[myTable] order by [myOffsetColumn]</code>.</li></ul> |
    | Convert Timestamp To String                   | <p>Enables the origin to write timestamps as string values rather than datetime values. Strings maintain the precision stored in the source database. For example, strings can maintain the precision of a high-precision IBM Db2 TIMESTAMP(9) field.</p><p>When writing timestamps to Data Pipelines date or time data types that do not store nanoseconds, the origin stores any nanoseconds from the timestamp in a field attribute.</p>                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Fetch Size                                    | <p>Maximum number of rows to fetch and store in memory on the Data Pipelines machine. The size cannot be zero.</p><p>Default is 1,000.</p><p>Note: By default, MySQL fetches and stores the complete result set in memory on the Data Pipelines machine. If the result sets have a large number of rows or large values that exceed available memory, specify a fetch size of <code>Integer.MIN_VALUE</code> so that MySQL streams the results to the Data Pipelines machine one row at a time.</p><p>For more information about configuring a fetch size, see your database documentation.</p>                                                                                                                                                                                                                                                                        |
    | Additional JDBC Configuration Properties      | <p>Additional JDBC configuration properties to use. To add properties, click Add and define the JDBC property name and value.</p><p>Use the property names and values as expected by JDBC.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
3.  On the Tables tab, define one or more table configurations. Using simple or bulk edit mode, click the Add icon to define another table configuration.Configure the following properties for each table configuration:

    | Tables Property                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Schema                                  | <p>Pattern of the schema names included in this table configuration. Use the SQL LIKE syntax to define the pattern. Enter % to match all schemas. If you enter no value, the origin only reads from tables without a specified schema.</p><p>Required for Oracle tables.</p><p><strong>Note</strong>: Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.</p>                                                                                                                                                                                                                                                                                                                                                                          |
    | Table Name Pattern                      | <p>Pattern of the table names to read for this table configuration. Use the SQL LIKE syntax to define the pattern.Note: Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.</p><p>Default is the percentage wildcard (%) which matches all tables in the schema.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Table Exclusion Pattern                 | <p>Pattern of the table names to exclude from being read for this table configuration. Use a Java-based regular expression, or regex, to define the pattern.</p><p>Leave empty if you do not need to exclude any tables.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Schema Exclusion Pattern                | <p>Pattern of the schema names to exclude from being read for this table configuration. Use a Java-based regular expression, or regex, to define the pattern.</p><p>Leave empty if you do not need to exclude any schemas.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Override Offset Columns                 | <p>Determines whether to use the primary keys or other columns as the offset columns for this table configuration.</p><p>Select to override the primary keys and define other offset columns. Clear to use existing primary keys as the offset columns.</p><p>To perform multithreaded partition processing on a table with multiple key columns or a key column with unsupported data types, select this option and specify a valid offset column. For more information about partition processing requirements, see Partition Processing Requirements.</p>                                                                                                                                                                                                                                                                                                                     |
    | Offset Columns                          | <p>Offset columns to use.</p><p>As a best practice, an offset column should be an incremental and unique column that does not contain null values. Having an index on this column is strongly encouraged since the underlying query uses an ORDER BY and inequality operators on this column.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Initial Offset                          | <p>Offset value to use for this table configuration when the pipeline starts. Enter the primary key name or offset column name and value. For Datetime columns, enter a Long value.</p><p>When you define multiple offset columns, you must define an initial offset value for each column, in the same order that the columns are defined.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Enable Non-Incremental Load             | Enables non-incremental processing of tables that do not include a primary key or offset column. Do not use when requiring multithreaded partition processing.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Multithreaded Partition Processing Mode | <p>Determines how the origin performs multithreaded processing. Select one of the following options:</p><ul><li><p>Off - The origin performs multithreaded table processing.</p><p>Can be used to perform non-incremental processing of tables without key or offset columns.</p></li><li><p>On (Best Effort) - The origin performs multithreaded partition processing for all tables that meet the partition processing requirements and performs multithreaded table partitioning tables with multiple key or offset columns.</p><p>Can be used to perform non-incremental processing of tables without key or offset columns.</p></li><li><p>On (Required) - The origin performs multithreaded partition processing for all tables.</p><p>Generates an error if the table configuration includes tables that do not meet the partition processing requirements.</p></li></ul> |
    | Partition Size                          | <p>Range of values in the offset column to use to create partitions.</p><p>If the offset column is a Datetime column, provide the partition size in milliseconds. For example, to create a partition for every hour, enter 3,600,000.</p><p>Available when using multithreaded partition processing.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Max Partitions                          | <p>Maximum number of partitions to be maintained or processed at one time for a single table. Adjusting this value can increase throughput depending on various factors, including the machine running Data Pipelines and the database server type and capacity.</p><p>The minimum positive value is 2, to ensure the origin can make progress through the partitions.</p><p>Enter -1 to use the default behavior, allowing the origin to create up to twice as many partitions for each table as threads used by the origin. Best practice is to start with the default behavior and adjust to tune performance.</p><p>Available when using multithreaded partition processing.</p>                                                                                                                                                                                             |
    | Offset Column Conditions                | <p>Additional conditions that the origin uses to determine where to start reading data for this table configuration. The origin adds the defined condition to the WHERE clause of the SQL query.</p><p>Use the expression language to define the conditions. For example, you can use the offset:column function to compare the value of an offset column.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
4.  If you configured the origin to enter JDBC credentials separately from the JDBC connection string on the JDBC tab, then configure the following properties on the Credentials tab:

    | Credentials Property | Description                                                                                                                                                |
    | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Username             | <p>User name for the JDBC connection.</p><p>The user account must have the correct permissions or privileges in the database.</p>                          |
    | Password             | Password for the JDBC user name.Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores. |
5.  When using JDBC versions older than 4.0, on the Legacy Drivers tab, optionally configure the following properties:

    | Legacy Drivers Property      | Description                                                                                                  |
    | ---------------------------- | ------------------------------------------------------------------------------------------------------------ |
    | JDBC Class Driver Name       | Class name for the JDBC driver. Required for JDBC versions older than version 4.0.                           |
    | Connection Health Test Query | Optional query to test the health of a connection. Recommended only when the JDBC version is older than 4.0. |
6.  On the Advanced tab, optionally configure advanced properties.The defaults for these properties should work in most cases:

    | Advanced Property                 | Description                                                                                                                                                                                                                                                                                                                                                                                              |
    | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Maximum Pool Size                 | <p>Maximum number of connections to create. Must be equal to or greater than the value of the Number of Threads property.</p><p>Default is 1.</p>                                                                                                                                                                                                                                                        |
    | Minimum Idle Connections          | <p>Minimum number of connections to create and maintain. To define a fixed connection pool, set to the same value as Maximum Pool Size.</p><p>Default is 1.</p>                                                                                                                                                                                                                                          |
    | Connection Timeout (seconds)      | <p>Maximum time to wait for a connection. Use a time constant in an expression to define the time increment.Default is 30 seconds, defined as follows:</p><pre><code>${30 * SECONDS}
    </code></pre>                                                                                                                                                                                                       |
    | Idle Timeout (seconds)            | <p>Maximum time to allow a connection to idle. Use a time constant in an expression to define the time increment.</p><p>Use 0 to avoid removing any idle connections.</p><p>When the entered value is close to or more than the maximum lifetime for a connection, Data Pipelines ignores the idle timeout.</p><p>Default is 10 minutes, defined as follows:</p><pre><code>${10 * MINUTES}
    </code></pre> |
    | Max Connection Lifetime (seconds) | <p>Maximum lifetime for a connection. Use a time constant in an expression to define the time increment.</p><p>Use 0 to set no maximum lifetime.</p><p>When a maximum lifetime is set, the minimum valid value is 30 minutes.</p><p>Default is 30 minutes, defined as follows:</p><pre><code>${30 * MINUTES}
    </code></pre>                                                                               |
    | Auto Commit                       | <p>Determines if auto-commit mode is enabled. In auto-commit mode, the database commits the data for each record.</p><p>Default is disabled.</p>                                                                                                                                                                                                                                                         |
    | Enforce Read-only Connection      | <p>Creates read-only connections to avoid any type of write.</p><p>Default is enabled. Disabling this property is not recommended.</p>                                                                                                                                                                                                                                                                   |
    | Transaction Isolation             | <p>Transaction isolation level used to connect to the database.</p><p>Default is the default transaction isolation level set for the database. You can override the database default by setting the level to any of the following:</p><ul><li>Read committed</li><li>Read uncommitted</li><li>Repeatable read</li><li>Serializable</li></ul>                                                             |
    | Init Query                        | <p>SQL query to perform immediately after the stage connects to the database. Use to set up the database session as needed.</p><p>For example, the following query sets the time zone for the session for a MySQL database: <code>SET time_zone = timezone;</code></p>                                                                                                                                   |
    | Initial Table Order Strategy      | <p>Initial order used to read the tables:</p><ul><li>None - Reads the tables in the order that they are listed in the database.</li><li>Alphabetical - Reads the tables in alphabetical order.</li><li>Referential Constraints - Reads the tables based on the dependencies between the tables.</li></ul>                                                                                                |
    | Create JDBC Header Attributes     | Adds JDBC header attributes to records. The origin creates JDBC header attributes by default.Note: When using the origin with a Drift Synchronization Solution, make sure this property is selected.                                                                                                                                                                                                     |
    | On Unknown Type                   | <p>Action to take when encountering an unsupported data type:</p><ul><li>Stop Pipeline - Stops the pipeline after completing processing the previous records.</li><li>Convert to String - When possible, converts the data to string and continues processing.</li></ul>                                                                                                                                 |
