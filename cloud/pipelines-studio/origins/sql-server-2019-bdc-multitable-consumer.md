# SQL Server 2019 BDC Multitable Consumer

The SQL Server 2019 BDC Multitable Consumer origin reads data from Microsoft SQL Server 2019 Big Data Cluster (BDC) through a JDBC connection.&#x20;

Use the origin to read multiple tables from one or more schemas in the same database. For example, you might use the origin to replicate a database.

When you configure the origin, you specify connection information and optional custom JDBC configuration properties to determine how the origin connects to SQL Server 2019 BDC. You specify a database and then define groups of tables to read from that database. The origin generates SQL queries based on the table configurations that you define, and then returns data as a map with column names and field values.

When you define the table configurations, you can optionally override the default key column and specify the initial offset to use. By default, the origin processes tables incrementally, using primary key columns or user-defined offset columns to track its progress. You can configure the origin to perform non-incremental processing to enable it to also process tables that do not have a key or offset column.

You can configure the origin to perform multithreaded partition processing, multithreaded table processing, or use the default - a mix of both. When configuring partitions, you can configure the offset size, number of active partitions, and offset conditions.

You define the strategy that the origin uses to create each batch of data and the number of batches to create from each result set. You can configure advanced properties, such as the initial order to read from tables, connection related properties, and transaction isolation. And you can specify what the origin does when encountering an unsupported data type: convert the data to string or stop the pipeline.

When the pipeline stops, the SQL Server 2019 BDC Multitable Consumer origin notes where it stops reading. When the pipeline starts again, the origin continues processing from where it stopped by default. You can reset the origin to process all available data, using any initial offsets that you defined.

By default, the origin generates JDBC record header and field attributes that provide additional information about each record and field.

The origin can generate events for an event stream.&#x20;

Before you use the SQL Server 2019 BDC Multitable Consumer origin, you must install the SQL Server 2019 Big Data Cluster stage library and complete other prerequisite tasks.&#x20;



**Supported Versions**

The following table lists the version of the SQL Server 2019 Big Data Cluster stage library to use with specific Data Pipelines versions:

| Data Pipelines Version          | Supported Stage Library Version                           |
| ------------------------------- | --------------------------------------------------------- |
| Data Pipelines 3.12.x and later | SQL Server 2019 Big Data Cluster Enterprise Library 1.0.x |

### External Tables <a href="#ariaid-title7" id="ariaid-title7"></a>

In SQL Server 2019 BDC, you can define external tables that access data virtualized in SQL Server. For more information, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/relational-databases/polybase/data-virtualization?view=sqlallproducts-allversions).

To configure the SQL Server 2019 BDC Multitable Consumer origin to read from an external table:

* On the JDBC tab, set the Database property to the SQL Server database where SQL Server 2019 BDC virtualizes the external table.
* On the Tables tab, create a table configuration that includes the external table. For example, you can define a configuration that includes the external table name in the Table Name Pattern property.

### Table Configuration <a href="#ariaid-title8" id="ariaid-title8"></a>

When you configure a SQL Server 2019 BDC Multitable Consumer origin, you define a table configuration for each group of tables that you want to read. A table configuration defines a group of tables with the same table name pattern, that are from one or more schemas with the same name pattern, and that have proper primary keys or the same user-defined offset columns.

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

#### Schema, Table Name, and Exclusion Patterns <a href="#ariaid-title9" id="ariaid-title9"></a>

You define the group of tables that the SQL Server 2019 BDC Multitable Consumer origin reads by defining schema and table name patterns for the table configuration. The origin reads all tables with names that match the table pattern in the schemas with names that match the schema pattern.

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

#### Offset Column and Value <a href="#ariaid-title10" id="ariaid-title10"></a>

The SQL Server 2019 BDC Multitable Consumer origin uses an offset column and initial offset value to determine where to start reading data within tables and partitions.

By default, the origin uses the primary key of the tables as the offset column and uses no initial offset value. When you use multithreaded table processing and the table has a composite primary key, the origin uses each primary key as an offset column. You cannot use composite keys with multithreaded partition processing.

By default, the origin reads all available data from each table when you start the pipeline. The origin generates SQL queries using the following syntax when you start the pipeline:

```
SELECT * FROM <table> ORDER BY <offset column_1>, <offset column_2>, ...
```

Where `<offset column_`_`n`_`>` represents each primary key of the table, such as when the table has a composite primary key. When you restart the pipeline or when the origin switches back to a previously read table, the origin adds a WHERE clause to the SQL query to continue reading from the last saved offset.

To use this default behavior, you do not need to configure any of the offset properties.

You can make the following changes to how the origin handles offset columns and initial offset values:

Override the primary key as the offset columnYou can override the primary key and define another offset column or columns. Or if the table doesn’t have a primary key, you can define the offset column or columns to use.

Important: As a best practice, a user-defined offset column should be an incremental and unique column that does not contain null values. If the column is not unique - that is, multiple rows can have the same value for this column - there is a potential for data loss upon pipeline restart. For details, see Multiple Offset Value Handling.

Having an index on this column is strongly encouraged since the underlying query uses an ORDER BY and inequality operators on this column.

**Define an initial offset value**

The initial offset value is a value within the offset column where you want the SQL Server 2019 BDC Multitable Consumer origin to start reading. When you define an initial offset value, you must first enter the offset column name and then the value. If you are using the default primary key as the offset column, enter the name of the primary key.If you define an initial offset value for a single offset column, the origin generates SQL queries using the following syntax:

```
SELECT * FROM <table> ORDER BY <offset column> WHERE <offset column> > ${offset}
```

If you defined multiple offset columns, you must define an initial offset value for each column, in the same order that the columns are defined. The origin uses the initial offset values of all columns to determine where to start reading data. For example, you override the primary key with the following offset columns: `p1`, `p2`, `p3` and define an initial offset value for each column. The origin generates SQL queries using the following syntax:

```
SELECT * FROM <table> ORDER BY p1, p2, p3 WHERE (p1 > ${offset1}) OR (p1 = ${offset1} AND p2 > ${offset2}) OR (p1 = ${offset1} AND p2 = ${offset2} AND p3 > ${offset3})
```

Note: Data Pipelines stores offsets for Datetime columns as Long values. For offset columns with a Datetime data type, enter the initial value as a Long value. You can use the time functions to transform a Datetime value to a Long value. For example, the following expression converts a date entered as a String to a Date object, and then to a Long value:

```
${time:dateTimeToMilliseconds(time:extractDateFromString('2017-05-01 20:15:30.915','yyyy-MM-dd HH:mm:ss.SSS'))} 
```

**Define additional offset column conditions**

You can use the expression language to define additional conditions that the origin uses to determine where to start reading data. The origin adds the defined condition to the WHERE clause of the SQL query.You can use the `offset:column` function in the condition to access an offset column by position. For example, if you have a table with offset columns `p1` and `p2`, then `offset:column(0)` returns the value of `p1` while `offset:column(1)` returns the value of `p2`.Let's say that you defined a `transaction_time` column as the offset column. While the origin reads the table, multiple active transactions are being written to the table with the current timestamp for the `transaction_time` column. When the origin finishes reading the first record with the current timestamp, the origin continues reading with the next offset and skips some rows with the current timestamp. You can enter the following offset column condition to ensure that the origin reads from all offset columns with a timestamp less than the current time:

```
${offset:column(0)} < ${time:now()}
```

If your database requires the datetime in a specific format, you can use the `time:extractStringFromDate` function to specify the format. For example:

```
${offset:column(0)} < '${time:extractStringFromDate(time:now(), "yyyy-MM-dd HH:mm:ss")}'
```

#### Reading from Views <a href="#ariaid-title11" id="ariaid-title11"></a>

The SQL Server 2019 BDC Multitable Consumer origin can read from views in addition to tables.

The origin reads from all tables and views that are included in the defined table configurations. If a table configuration includes views that you do not want to read, simply exclude them from the configuration.

Use the origin to read from simple views that select data from a single table.

We do not recommend using the origin to read from complex views that combine data from two or more tables using joins. If the origin reads from complex views, it runs multiple queries in parallel which can cause a heavy workload on the database.

### Multithreaded Processing Modes <a href="#ariaid-title12" id="ariaid-title12"></a>

The SQL Server 2019 BDC Multitable Consumer origin performs parallel processing and enables the creation of a multithreaded pipeline. The origin can use multiple threads to process entire tables or partitions within tables.

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

#### Multithreaded Table Processing <a href="#ariaid-title13" id="ariaid-title13"></a>

When performing multithreaded table processing, the SQL Serer 2019 BDC Multitable Consumer origin retrieves the list of tables defined in the table configuration when you start the pipeline. The origin then uses multiple concurrent threads based on the Number of Threads property. Each thread reads data from a single table, and each table can have a maximum of one thread read from it at a time.Note: The Maximum Pool Size property on the Advanced tab defines the maximum number of connections the origin can make to the database. It must be equal to or greater than the value defined for the Number of Threads property.

As the pipeline runs, each thread connects to the origin system, creates a batch of data, and passes the batch to an available pipeline runner. A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin.

Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

The order of batch processing depends on many factors. For more information, see Processing Queue.

**Example**

Say you are reading from ten tables. You set the Number of Threads property to 5 and the Maximum Pool Size property to 6. When you start the pipeline, the origin retrieves the list of tables. The origin then creates five threads to read from the first five tables, and by default Data Pipelines creates a matching number of pipeline runners. Upon receiving data, a thread passes a batch to each of the pipeline runners for processing.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

#### Multithreaded Partition Processing <a href="#ariaid-title14" id="ariaid-title14"></a>

By default, the SQL Server 2019 BDC Multitable Consumer origin performs multithreaded partition processing for all tables that meet the partition processing requirements, and performs table processing for all other tables.

To perform multithreaded processing of partitions within a table, you enable partition processing in the table configuration, then specify the partition size and the maximum number of partitions to use. Limiting the number of partitions also limits the number of threads that can be dedicated to processing data in the table.

When you configure a set of tables for unlimited partitions, the origin creates up to twice as many partitions as the pipeline thread count. For example, if you have 5 threads, the table can have up to 10 partitions.

Similar to multithreaded table processing, each thread reads data from a single partition, and each partition can have a maximum of one thread read from it at a time.

When processing partitions, the processing order depends on many factors. For a full description, see Processing Queue.

**Partition Processing Requirements**

To perform multithreaded partition processing for a table, the table must meet the following requirements:Single key or offset columnThe table must have a single primary key or user-defined offset column. Performing multithreaded partition processing on a table with composite keys generates an error and stops the pipeline.If a table does not have a primary key column, you can use the Override Offset Columns property to specify a valid offset column to use. Having an ascending index on the offset column is strongly encouraged since the underlying query uses an ORDER BY and inequality operators on this column.Numeric data typeTo use partition processing, the primary key or user-defined offset column must have a numeric data type that allows arithmetic partitioning.The key or offset column must be one of the following data types:

* Integer-based: Integer, Smallint, Tinyint
* Long-based: Bigint, Date, Time, Timestamp.
* Float-based: Float, Real
* Double-based: Double
* Precision-based: Decimal, Numeric

**Multiple Offset Value Handling**

When processing partitions, the SQL Server 2019 BDC Multitable Consumer origin can process multiple records with the same offset value. For example, the origin can process multiple records with the same timestamp in a `transaction_date` offset column.

Warning: When processing multiple records with the same offset value, records can be dropped if you stop the pipeline when the origin is processing a series of records with the same offset value.

When you stop the pipeline as the origin is processing a series of records with the same offset value, the origin notes the offset. Then, when you restart the pipeline, it starts with a record with the next logical offset value, skipping any unprocessed records that use the same last-saved offset.

For example, say you specified a datetime column as a user-defined offset column, and five records in the table share the same datetime value. Now say you happen to stop the pipeline after it processes the second record. The pipeline stores the datetime value as the offset where it stopped. When you restart the pipeline, processing begins with the next datetime value, skipping the three unprocessed records with the last-saved offset value.

**Best Effort: Processing Non-Compliant Tables**

To process tables in a table configuration that might not meet the partition processing requirements, you can use the On (Best Effort) option when you configure the Multithreaded Partition Processing mode property.

When you select the best effort option, the origin performs multithreaded partition processing for all tables that meet the partition processing requirements. The origin performs multithreaded table processing for tables that include multiple key or offset columns. And if you enable non-incremental processing, the origin can also process all tables that do not include key or offset columns.

### Non-Incremental Processing <a href="#ariaid-title18" id="ariaid-title18"></a>

You can configure the SQL Server 2019 BDC Multitable Consumer origin to perform non-incremental processing for tables with no primary keys or user-defined offset columns. By default, the origin performs incremental processing and does not process tables without a key or offset column.

You can enable non-incremental processing for the set of tables defined in a table configuration on the Tables tab.

Note: When enabling non-incremental processing for a table without a key or offset column, you cannot require multithreaded partition processing for the table configuration. That is, you cannot run the pipeline with the Multithreaded Partition Processing Mode property set to On (Required).

Use On (Best Effort) or Off to perform non-incremental processing of the table. With either option selected, table is processed using a single thread, like multithreaded table processing.

When you enable non-incremental processing, the origin processes any table without a key or offset column as follows:

* The origin uses a single thread to process all available data in the table.
*   After the origin processes all available data, it notes that the table has been processed as an offset. So, if you stop and restart the pipeline after the origin completes all processing, the origin does not reprocess the table.

    If you want to reprocess data in the table, you can reset the origin before restarting the pipeline. This resets the origin for all tables that the origin processes.
* If the pipeline stops while the origin is still processing available data, when the pipeline restarts, the origin reprocesses the entire table. This occurs because the table has no key or offset column to allow for tracking progress.

For example, say you configure the origin to use five threads and process a set of tables that includes a table with no key or offset column. To process data in this table, you enable the Enable Non-Incremental Load table configuration property. You also set Multithreaded Partition Processing Mode to On (Best Effort) to allow the origin to use multithreaded partition processing when possible and allow both non-incremental processing and multithreaded table processing when needed.

When you start the pipeline, the origin allocates one thread to the table that requires non-incremental processing. It processes the table data using multithreaded table processing until all data is processed. When the thread completes processing all available data, the origin notes this as part of the offset and the thread becomes available to process data from other tables. In the meantime, the four other threads process data from the rest of the tables using multithreaded partition processing when possible.

### Batch Strategy <a href="#ariaid-title19" id="ariaid-title19"></a>

You can specify the batch strategy to use when processing data. The batch strategy behaves differently depending on whether you use multithreaded table processing or multithreaded partition processing.

#### Process All Available Rows <a href="#ariaid-title20" id="ariaid-title20"></a>

The Process All Available Rows from the Table batch strategy differs slightly depending on whether the origin is processing full tables or partitions within a table:

**Multithreaded table processing**

When the origin performs multithreaded table processing for all tables, each thread processes all available rows from a table. A thread runs a SQL query and processes all of the results for a table. Then, the thread switches to the next available table.

For example, let's say the origin has a maximum batch size of 100 and uses two threads to read from four tables, each of which contains 1000 rows. Thread 1 runs a SQL query to create 10 batches of 100 rows each from table A, while thread 2 uses the same strategy to read data from table B.When table A and table B are fully read, the threads switch to table C and table D and complete the same process. When thread 1 finishes reading from table C, it switches back to the next available table to read any data that has arrived since the table was processed, starting from the last saved offset.The number of threads that can process the tables is limited by the Number of Threads property for the origin.When the tables being processed use both table and partition processing, the threads query the partitions as described below. For details on how the tables and partitions rotate through the processing queue, see Processing Queue.

**Multithreaded partition processing**

Multithreaded partition processing is similar to multithreaded table processing, except that it works at a partition level. Each thread runs a SQL query for a partition and processes multiple batches of data from the results. When all data in the partition is processed, the thread switches to the next available partition.The number of threads that can process partitions for each table is limited by the Number of Threads property for the origin and the Max Active Partitions table property.

#### Switch Tables <a href="#ariaid-title21" id="ariaid-title21"></a>

The Switch Tables batch strategy differs depending on whether the origin performs full table or partition processing. The number of batches that the origin creates before closing a result set is based on the Batches from Result Set property.

Note: The Switch Tables batch strategy can use a large amount of heap memory, depending on the Fetch Size property and the JDBC driver being used. For example, some drivers prefetch the rows for each table to reduce the number of round trips to the database, which can result in a large amount of heap memory usage.

**Multithreaded table processing**

When the origin performs multithreaded table processing for all tables, each thread creates a set of batches from one table, and then switches to the next available table to create the next set of batches. The thread runs an initial SQL query to create the first set of batches from the table. The database caches the remaining rows in a result set in the database for the same thread to access again, and then the thread switches to the next available table. A table is available in the following situations:

* The table does not have an open result set cache. In this case, the thread runs an initial SQL query to create the first batch, caching the remaining rows in a result set in the database.
* The table has an open result set cache created by that same thread. In this case, the thread creates the batch from the result set cache in the database rather than running another SQL query.

A table is not available when the table has an open result set cache created by another thread. No other thread can read from that table until the result set is closed.

When you configure a switch table strategy, define the result set cache size and the number of batches that a thread can create from the result set. After a thread creates the configured number of batches, a different thread can read from the table.

Note: By default, the origin instructs the database to cache an unlimited number of result sets. A thread can create an unlimited number of batches from that result set.

For example, let's say an origin has a batch size of 100 and uses two concurrent threads to read from four tables, each of which contains 10,000 rows. You set the result set cache size to 500 and set the number of batches read from the result set to 5.

Thread1 runs an SQL query on table1, which returns all 10,000 rows. The thread creates a batch when it reads the first 100 rows. The next 400 rows are cached as a result set in the database. Since thread2 is similarly processing table2, thread1 switches to the next available table, table3, and repeats the same process. After creating a batch from table3, thread1 switches back to table1 and retrieves the next batch of rows from the result set that it previously cached in the database.

After thread1 creates five batches using the result set cache for table1. Thread1 then switches to the next available table. A different thread runs an SQL query to read additional rows from table1, beginning from the last saved offset.When the tables being processed use both table and partition processing, the threads query the partitions as described below. For details on how the tables and partitions rotate through the processing queue, see [Processing Queue](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/SQLServerBDCMultitable.html#concept\_czt\_ql2\_r1c).

**Multithreaded partition processing**

Multithreaded partition processing is similar to multithreaded table processing, with a twist - each thread creates a set of batches from one partition for a table, then all partitions from the same table are moved to the end of the processing queue. This allows the origin to switch to the next available table.The behavior around caching the result set and the number of batches to process from the result set is the same, but at a partition level.

### Initial Table Order Strategy <a href="#ariaid-title22" id="ariaid-title22"></a>

You can define the initial order that the origin uses to read the tables.

Define one of the following initial table order strategies:

**None**

Reads the tables in the order that they are listed in the database.

**Alphabetical**

Reads the tables in alphabetical order.Referential ConstraintsReads the tables based on the dependencies between the tables. The origin reads the parent table first, and then reads the child tables that refer to the parent table with a foreign key.

You cannot use the referential constraints order when the tables to be read have a cyclic dependency. When the origin detects a cyclic dependency, the pipeline fails to validate with the following error:

```
JDBC_68 Tables referring to each other in a cyclic fashion.
```

Note that the referential constraints order can cause pipeline validation or initialization to slow down because the origin has to sort the tables before reading them.

The origin uses this table order only for the initial reading of the tables. When threads switch back to previously read tables, they read from the next available table, regardless of the defined order.

### Processing Queue <a href="#ariaid-title23" id="ariaid-title23"></a>

The SQL Server 2019 BDC Multitable Consumer origin maintains a virtual queue to determine the data to process from different tables. The queue includes each table defined in the origin. When a table is to be processed by partition, multiple partitions for the table are added to the queue, limited by the Max Partitions property defined for each table configuration on the Tables tab.

The origin rotates and reorganizes the queue based on the Per Batch Strategy property. And it processes data from the queue with the threads specified in the Number of Threads property and the Batches from Result Set property. These three properties are defined for the origin on the JDBC tab.

Below are some scenarios to help clarify how the queue works.

#### Multithreaded Table Processing Only <a href="#ariaid-title24" id="ariaid-title24"></a>

The origin performs only multithreaded table processing in either of the following situations:

* Multithreaded Partition Processing Mode property is set to Off.
* Multithreaded Partition Processing Mode property is set to On (Best Effort) and no tables meet the partition processing requirements.

Say you have tables A, B, C and D that you configure for table processing. When you start the pipeline, the origin adds all tables to the queue. If configured, the Initial Table Order Strategy advanced property can affect the order. Let's assume we have no referential constraints and use alphabetical order:

```
A  B  C  D
```

When a thread becomes available, it processes data from the first table in the queue. The number of batches is based on the Batches from Result Set property. The processing of the tables depends on how you define the Per Batch Strategy property:

**Process All Available Rows in the Table**

With this batch strategy, threads do not start processing data in the next table until all available data is processed for the preceding table.

That is, table A remains at the front of the queue until all available data is processed. Then processing begins on table B. Table A moves to the back, remaining in the queue in case more data appears, as follows:

```
B  C  D  A  
```

**Switch Tables**

With this batch strategy, the order of the queue remains the same, but each thread performs a SQL query to create a set of batches based on the Batches from Result Set property. When it completes processing, it performs the same process with the next table in the queue.

After a thread takes a set of batches from table A, table A moves to the back of the queue:

```
B  C  D  A
```

The next thread takes a set of batches from table B. Then B moves to the back of the queue:

```
C  D  A  B  
```

So after processing 4 sets of batches, the queue looks like it did in the beginning:

```
A  B  C  D
```

#### Multithreaded Partition Processing Only <a href="#ariaid-title25" id="ariaid-title25"></a>

The origin performs only multithreaded partition processing in either of the following situations:

* Multithreaded Partition Processing Mode property is set to On (Required).
* Multithreaded Partition Processing Mode property is set to On (Best Effort) and all tables meet the partition processing requirements.

Say you have table A, B, and C and all three tables are loaded up with lots of data to process. Tables A and B are configured with a maximum of 3 active partitions. And since table C has the largest volume of data, you allow an unlimited number of partitions. Again, let's use the alphabetical initial table ordering.

When you start the pipeline, each table is queued up with the maximum number of active partitions. And for table C, that means double the number of threads for the pipeline. So if we configure the pipeline for 4 threads, table C can have up to 8 partitions in the queue at any given time. So the initial queue looks like this:

```
A1  A2  A3  B1  B2  B3  C1  C2  C3  C4  C5  C6  C7  C8 
```

A partition remains in the queue until the origin confirms that there is no more data in the partition. When a thread becomes available, it creates a set of batches from the first partition of the first table in the queue. The number of batches is based on the Batches from Result Set property. The order of tables and partitions in the queue depends on how you define the Per Batch Strategy, as follows:

**Process All Available Rows in the Table**

When processing partitions, this batch strategy retains the original order of the queue, but rotates through the partitions as each thread processes a set of batches.

Note: In practice, this means that rows from subsequent tables can be processed before a previous table is completed, since available threads continue to pick up partitions from the queue.

For example, the four threads start processing on the first four partitions in the queue: A1, A2, A3, and B1. This puts B2 at the front of the queue, ready for the next available thread. And since the four partitions being processed have additional data to process, they go to the back of the queue. So processing of table B data begins before table A is fully processed.

The rest of the partitions remain in the original order as follows:

```
B2  B3  C1  C2  C3  C4  C5  C6  C7  C8  A1  A2  A3  B1
```

After the four threads process another four sets of batches, the queue looks like this:

```
C3  C4  C5  C6  C7  C8  A1  A2  A3  B1  B2  B3  C1  C2
```

**Switch Tables**

When processing partitions, this batch strategy forces all subsequent, consecutive partitions from the same table to the end of the queue each time a thread processes a set of batches from a partition.

Let's start again with the initial batch order:

```
A1  A2  A3  B1  B2  B3  C1  C2  C3  C4  C5  C6  C7  C8 
```

When a thread processes a set of batches from A1, it pushes the rest of the table A partitions to the end of the queue. This queues up the next table, table B, for processing. And since A1 still contains data, it takes the last spot, as follows:

```
B1  B2  B3  C1  C2  C3  C4  C5  C6  C7  C8  A2  A3  A1
```

As the second thread processes a set of batches from B1, the other B partitions are sent to the back, and since B1 still contains data, it takes the last spot as follows:

```
C1  C2  C3  C4  C5  C6  C7  C8  A2  A3  A1  B2  B3  B1
```

And as the third thread takes a set of batches from C1, the rest of the C partitions are pushed to the back, so the queue looks like this:

```
A2  A3  A1  B2  B3  B1  C2  C3  C4  C5  C6  C7  C8  C1
```

#### Both Multithreaded Partition and Table Processing <a href="#ariaid-title26" id="ariaid-title26"></a>

The origin performs both multithreaded partition processing and multithreaded table processing in the following situation:

* Multithreaded Partition Processing Mode property is set to On (Best Effort) and some tables meet the partition processing requirements while other tables do not.

When processing a mix of full tables and partitioned tables, the queue basically behaves the same as when processing only partitions, with full tables being processed as partitioned tables with a single partition. Let's walk through it.

Say we have table A being processed without partitions, and table B configured with a maximum of 3 partitions, and table C with no limit. As in the example above, the pipeline has 4 threads to work with which allows 8 partitions to table C. Using the alphabetical initial table ordering, the initial queue looks like this:

```
A  B1  B2  B3  C1  C2  C3  C4  C5  C6  C7  C8 
```

When a thread becomes available, it processes a set of batches from the first table or partition in the queue. The number of batches is based on the Batches from Result Set property. The order of the queue depends on how you define the Per Batch Strategy, as follows:

**Process All Available Rows in the Table**

With this batch strategy, the queue remains in the basic initial order and rotates as each thread claims a set of batches from the next table or partition. The unpartitioned table A is processed like a table with a single partition.

Note that unpartitioned tables are not processed in full when they move to the front of the queue. For this behavior, configure all tables to be processed without partitions. Or, set the Batches from Result Set property to -1.

When the pipeline starts, the 4 threads process a set of batches from the A table and from partitions B1, B2, and B3. Since the table and partitions all still contain data, they then move to the end of the queue as follows:

```
C1  C2  C3  C4  C5  C6  C7  C8  A  B1  B2  B3 
```

As each thread completes processing, it processes a set of batches from the front of the queue. After each of the 4 threads takes another set of batches, the queue looks like this:

```
C5  C6  C7  C8  A  B1  B2  B3  C1  C2  C3  C4 
```

**Switch Tables**

When processing tables and partitions, this batch strategy forces all subsequent, consecutive partitions from the same table to the end of the queue. And it treats unpartitioned tables as a table with a single partition. As a result, the queue rotation is a simplified version of processing only partitioned tables.

So we have this initial order:

```
A  B1  B2  B3  C1  C2  C3  C4  C5  C6  C7  C8 
```

The first thread processes a set of batches from table A, and since there are no related partitions, it simply goes to the end of the queue:

```
B1  B2  B3  C1  C2  C3  C4  C5  C6  C7  C8  A
```

The second thread processes a set of batches from B1, pushes the rest of the table B partitions to the end of the queue, and B1 lands at the end because it contains more data to be processed:

```
C1  C2  C3  C4  C5  C6  C7  C8  A  B2  B3  B1
```

The third thread processes a set of batches from C1, pushes the rest of the table C partitions to the end, and C1 takes the last slot:

```
A  B2  B3  B1  C2  C3  C4  C5  C6  C7  C8  C1
```

And then the fourth thread processes another set of batches from table A, and moves A to the end of the queue:

```
B2  B3  B1  C2  C3  C4  C5  C6  C7  C8  C1  A
```

### JDBC Attributes <a href="#ariaid-title27" id="ariaid-title27"></a>

The SQL Server 2019 BDC Multitable Consumer origin generates record header attributes and field attributes that provide additional information about each record and field.

The origin receives these details from the JDBC driver.

#### JDBC Header Attributes <a href="#ariaid-title28" id="ariaid-title28"></a>

The SQL Server 2019 BDC Multitable Consumer origin generates JDBC record header attributes that provide additional information about each record, such as the original data type of a field or the source tables for the record.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes.&#x20;

JDBC record header attributes include a `jdbc` prefix to differentiate the JDBC attributes from other record header attributes.

The origin can provide the following JDBC header attributes:

| JDBC Header Attribute         | Description                                                                                                                                                                                                                                                              |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| jdbc.tables                   | Provides a comma-separated list of source tables for the fields in the record.                                                                                                                                                                                           |
| jdbc.partition                | Provides the full offset key for the partition that produced the record                                                                                                                                                                                                  |
| jdbc.threadNumber             | Provides the number of the thread that produced the record.                                                                                                                                                                                                              |
| jdbc.\<column name>.jdbcType  | Provides the numeric value of the original SQL data type for each field in the record. See the [Java documentation](https://docs.oracle.com/javase/8/docs/api/constant-values.html#java.sql.Types.ARRAY) for a list of the data types that correspond to numeric values. |
| jdbc.\<column name>.precision | Provides the original precision for all numeric and decimal fields.                                                                                                                                                                                                      |
| jdbc.\<column name>.scale     | Provides the original scale for all numeric and decimal fields.                                                                                                                                                                                                          |

#### JDBC Field Attributes <a href="#ariaid-title29" id="ariaid-title29"></a>

The SQL Server 2019 BDC Multitable Consumer origin generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

The following data type conversions do not include all information in the corresponding Data Pipelines type:

* SQL Server Decimal and Numeric data types are converted to the Data Pipelines Decimal data type, which does not store scale and precision.
* SQL Server Datetime, Datetime2, and Smalldatetime data types are converted to the Data Pipelines Datetime data type, which does not store nanoseconds.

To preserve this information during data type conversion, the origin generates the following field attributes for these Data Pipelines data types:

| Data Pipelines Data Type | Generated Field Attribute | Description                                                                               |
| ------------------------ | ------------------------- | ----------------------------------------------------------------------------------------- |
| Decimal                  | precision                 | Provides the original precision for every decimal or numeric column.                      |
| Decimal                  | scale                     | Provides the original scale for every decimal or numeric column.                          |
| Datetime                 | nanoSeconds               | Provides the original nanoseconds for every datetime, datetime2, or smalldatetime column. |

You can use the `record:fieldAttribute` or `record:fieldAttributeOrDefault` functions to access the information in the attributes.&#x20;

### Event Generation <a href="#ariaid-title30" id="ariaid-title30"></a>

The SQL Server 2019 BDC Multitable Consumer origin can generate events that you can use in an event stream. When you enable event generation, the origin generates an event when it completes processing the data returned by the specified queries for all tables. It also generates events when it completes processing the data returned from a table and the data returned from a schema.

SQL Server 2019 BDC Multitable Consumer events can be used in any logical way. For example:

*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin continues processing from the last-saved offset unless you reset the origin.
* With the Email executor to send a custom email after receiving an event.
* With a destination to store information about completed queries.

#### Event Record <a href="#ariaid-title31" id="ariaid-title31"></a>

Event records generated by SQL Server 2019 BDC Multitable Consumer origin have the following event-related record header attributes:

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following type:</p><ul><li>no-more-data - Generated when the origin completes processing all data returned by the queries for all tables.</li><li>schema-finished - Generated when the origin completes processing all rows within a schema.</li><li>table-finished - Generated when the origin completes processing all rows within a table.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                                                                                    |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                                                                               |

The SQL Server 2019 BDC Multitable Consumer origin can generate the following event record:

no-more-dataThe SQL Server 2019 BDC Multitable Consumer origin generates a no-more-data event record when the origin completes processing all data returned by the queries for all tables.You can configure the origin to delay the generation of the no-more-data event by a specified number of seconds. You might configure a delay to ensure that the schema-finished or table-finished events are generated and delivered to the pipeline before the no-more-data event record.

To use a delay, configure the No-more-data Event Generation Delay property.

The no-more-data event record generated by the origin has the `sdc.event.type` record header attribute set to `no-more-data` and does not include any additional fields.

schema-finishedThe SQL Server 2019 BDC Multitable Consumer origin generates a schema-finished event record when the origin completes processing all data within a schema.The schema-finished event record has the following additional fields:

| Event Record Field | Description                                                         |
| ------------------ | ------------------------------------------------------------------- |
| schema             | The schema that has returned no remaining data to be processed.     |
| tables             | A list of the tables within the schema that have no remaining data. |

table-finishedThe SQL Server 2019 BDC Multitable Consumer origin generates a table-finished event record when the origin completes processing all data within a table.The table-finished event record has the following additional fields:

| Event Record Field | Description                                                                      |
| ------------------ | -------------------------------------------------------------------------------- |
| schema             | The schema associated with the table that has no remaining data to be processed. |
| table              | The table that has no remaining data to be processed.                            |
