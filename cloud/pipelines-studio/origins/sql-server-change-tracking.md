# SQL Server Change Tracking

The SQL Server Change Tracking origin processes data from Microsoft SQL Server change tracking tables. The origin can process data from tables with simple numeric primary keys. The origin cannot process data from tables with compound or non-numeric primary keys.&#x20;

By default, the origin generates a record with change tracking information and the latest version of each record from the data tables. You can configure it to use only the change tracking information. The origin uses multiple threads to enable parallel processing of data.

Use the SQL Server Change Tracking origin to generate records from change tracking tables. To read data from Microsoft SQL Server change data capture (CDC) tables, use the SQL Server CDC Client origin. For more information about the differences between change tracking and CDC data, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/track-data-changes-sql-server). To read data from SQL Server temporal tables, use the JDBC Multitable Consumer origin or the JDBC Query Consumer origin. For more information about temporal tables, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/relational-databases/tables/temporal-tables?view=sql-server-ver15).

The SQL Server Change Tracking origin includes the CRUD operation type in a record header attribute so generated records can be easily processed by CRUD-enabled destinations.

You might use this origin to perform database replication. You can use a separate pipeline with the JDBC Query Consumer or JDBC Multitable Consumer origin to read existing data. Then start a pipeline with the SQL Server Change Tracking origin to process subsequent changes.

When you configure the origin, you can define groups of change tracking tables in the same database and any initial offsets to use. When you omit initial offsets, the origin processes only incoming data.

To determine how the origin connects to the database, you specify connection information, a query interval, number of retries, and any custom JDBC configuration properties that you need.

You specify whether you want to include the latest version of the data in generated records or whether to include only change tracking data. You define the number of threads that the origin uses to read from the tables and the strategy that the origin uses to create each batch of data. You also define the initial order that the origin uses to read the tables.

The origin can generate events for an event stream.

### JDBC Driver <a href="#ariaid-title2" id="ariaid-title2"></a>

When connecting to Microsoft SQL Server, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for SQL Server.

### Prerequisites <a href="#ariaid-title3" id="ariaid-title3"></a>

Before you configure the SQL Server Change Tracking origin, complete the following prerequisites in the SQL Server database:

1. Enable change tracking for the database.
2. Assign the required permissions to the database user.

#### Enable Change Tracking <a href="#ariaid-title4" id="ariaid-title4"></a>

Before you configure the SQL Server Change Tracking origin, enable change tracking for the SQL Server database and for each table within that database that you want tracked.

For instructions, see the [Microsoft SQL Server documentation](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server?view=sql-server-ver15).

#### Assign Permissions <a href="#ariaid-title5" id="ariaid-title5"></a>

Before you configure the SQL Server Change Tracking origin, create a database user that the origin can use to connect to the database. Assign the database user the following permissions:

* VIEW CHANGE TRACKING permission on the database.
*   When using the default record generation to join change tracking data with the current version of the data, the user must have SELECT permission on at least the primary key column for each associated data table.

    If processing data only from the change tracking data, the user does not need this permission.

### Multithreaded Processing <a href="#ariaid-title6" id="ariaid-title6"></a>

The SQL Server Change Tracking origin performs parallel processing and enables the creation of a multithreaded pipeline.

When you start the pipeline, the SQL Server Change Tracking origin retrieves the list of change tracking-enabled tables with a valid minimum change tracking version that are defined in the table configuration. The origin then uses multiple concurrent threads based on the Number of Threads property. Each thread reads data from a single table.

Note: The Maximum Pool Size property on the Advanced tab defines the maximum number of connections the origin can make to the database. It must be equal to or greater than the value defined for the Number of Threads property.

As the pipeline runs, each thread connects to the origin system, creates a batch of data, and passes the batch to an available pipeline runner. A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin.

Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

#### Example

Say you are reading from 10 tables. You set the Number of Threads property to 5 and the Maximum Pool Size property to 6. When you start the pipeline, the origin retrieves the list of tables. The origin then creates five threads to read from the first five tables, and by default Data Pipelines creates a matching number of pipeline runners. Upon receiving data, a thread passes a batch to each of the pipeline runners for processing.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

### Batch Strategy <a href="#ariaid-title7" id="ariaid-title7"></a>

You can specify the batch strategy to use when processing data:

Process all available rows from the tableEach thread processes all available rows from a table. A thread runs a SQL query and processes all of the results for a table. Then, the thread switches to the next available table.For example, let's say the origin has a maximum batch size of 100 and uses two threads to read from four tables, each of which contains 1000 rows. Thread 1 runs a SQL query to create 10 batches of 100 rows each from table A, while thread 2 uses the same strategy to read data from table B.When table A and table B are fully read, the threads switch to table C and table D and complete the same process. When thread 1 finishes reading from table C, it switches back to the next available table to read any data that has arrived since the table was processed, starting from the last saved offset.

The number of threads that can process the tables is limited by the Number of Threads property for the origin.Switch tablesWhen the origin performs multithreaded table processing for all tables, each thread processes one batch of data, then switches to an available table and repeats the process. When a pipeline starts, each thread runs a SQL query, generates a result set, and processes a batch of records from the result set. The database driver caches the remaining records for the same thread to access again. Then, the thread switches to the next available table.A table is available in the following situations:

*   The table does not have a result set created by another thread.

    In this case, the thread runs a SQL query to generate a result set and processes a batch of records. The database driver caches the remaining records for later use.
*   The table has a cached result set created by the same thread.

    In this case, the thread creates the batch from the result set, rather than running another SQL query.

A table is not available when the table has a result set created by another thread. A table with a cached result set can only be processed by the thread that created the result set. And a table cannot have multiple cached result sets.The Max Batch Size property limits the number of records included in a batch. The Batches from Result Set property limits the number of batches that a thread can create from the result set. After a thread creates the configured number of batches, the thread closes the result set. This allows a different thread to read from the table.

For example, let's say an origin uses two concurrent threads to read from three tables. Max Batch Size is set to 100 records, and Batches from Result Set is set to 2.Thread 1 runs a SQL query on table A, which contains 80 records. The thread processes the 80 records in a single batch. In the meantime, thread 2 runs a SQL query on table B, which contains 5000 records, and caches the result set. The thread creates a batch to process the first 100 records in the table.After processing table A, thread 1 looks for the next table to process. Thread 1 skips table B because the table has a cached result set from thread 2, so thread 1 runs a SQL query on table C, which contains 1000 records, and caches the result set. Thread 1 processes the first 100 records in the table.Similarly, when thread 2 completes the batch on table B, it skips table C because thread 1 has a result set cached for the table. If table A had new data that came in, thread 2 would process that data, since there is no result set for the table. But if table A has no additional data, thread 2 returns to table B, where it has a cached result set.Since thread 2 has a cached result set on table B, it creates a new batch from the result set instead of running a SQL query. Since this is the second batch from the result set, thread 2 closes the result set after processing the batch. This allows either thread to process data from table B the next time they check the table.

### Table Configuration <a href="#ariaid-title8" id="ariaid-title8"></a>

When you configure the SQL Server Change Tracking origin, you define multiple tables using a single set of table configuration properties and you can define multiple table configurations to process multiple groups of change tables. When you define table configurations you can define the following properties for each set of tables:

* Schema - The schema where the tables reside.
*   Table name pattern - Use SQL LIKE syntax to define a set of tables to process. For example, the table name pattern st% matches tables whose names start with "st". The default pattern, %, matches all tables in the schema.

    For more information about valid patterns for the SQL LIKE syntax, see the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/ms179859.aspx).
*   Table exclusion pattern - When necessary, use a regex pattern to exclude certain tables that match the table name pattern from being read.

    For example, say you want to process all change tracking tables in the schema except for those that start with "dept". You can use the default % for the table name pattern, and enter dept\* for the table exclusion pattern.
*   Initial offset - The SQL Server Change Tracking origin uses the SYS\_CHANGE\_VERSION column as the offset column. To process existing data, define the offset value to use. The offset is used for all tables included in the table configuration.

    When not set, the origin processes only incoming data.

    **Important**: When processing an offset, the origin starts with the first value greater than the specified offset.

### Initial Table Order Strategy <a href="#ariaid-title9" id="ariaid-title9"></a>

You can define the initial order that the origin uses to read the tables.

Define one of the following initial table order strategies:NoneReads the tables in the order that they are listed in the database.AlphabeticalReads the tables in alphabetical order.

The origin uses the table order strategy only for the initial reading of the tables. When threads switch back to previously read tables, they read from the next available table, regardless of the defined order.

### Generated Record <a href="#ariaid-title10" id="ariaid-title10"></a>

The SQL Server Change Tracking origin can generate a record in the following ways:Change tracking and current data

By default, when the SQL Server Change Tracking origin generates a record, it includes the data from the change tracking table and performs an outer join with the current version of the table.

The resulting record includes the following:

* Change tracking fields such as SYS\_CHANGE\_VERSION, SYS\_CHANGE\_CREATION\_VERSION, SYS\_CHANGE\_OPERATION, etc.
* The latest version of the record, when available.

Important: Unlike a record generated by a CDC origin, the change tracking record includes the latest version of the record, not the version of the record created by the change.Change tracking onlyYou can configure the origin to omit the join and produce a record with only the change tracking data. The resulting record includes the following:

* Change tracking fields such as SYS\_CHANGE\_VERSION, SYS\_CHANGE\_CREATION\_VERSION, etc.
* Primary key field for the changed record, provided by the change tracking table.

All generated records include change tracking information in record header attributes.

The origin also generates field attributes that provide additional information about field.

#### Record Header Attributes <a href="#ariaid-title11" id="ariaid-title11"></a>

The SQL Server Change Tracking origin generates JDBC record header attributes that provide additional information about each record, such as the original data type of a field or the source tables for the record.

The origin also includes the sdc.operation.type attribute and information from the SQL Server change tracking table. The SQL Server Change Tracking header attributes are prefixed with "jdbc".

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes.&#x20;

The origin provides the following header attributes:

| Header Attribute Name               | Description                                                                                                                                                                                                                                                              |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| sdc.operation.type                  | <p>The origin uses the following values to represent the operation type:</p><ul><li>1 for INSERT</li><li>2 for DELETE</li><li>3 for UPDATE</li></ul>                                                                                                                     |
| jdbc.tables                         | Provides a comma-separated list of source tables for the fields in the record.Note: Not all JDBC drivers provide this information.                                                                                                                                       |
| jdbc.\<column name>.jdbcType        | Provides the numeric value of the original SQL data type for each field in the record. See the [Java documentation](https://docs.oracle.com/javase/8/docs/api/constant-values.html#java.sql.Types.ARRAY) for a list of the data types that correspond to numeric values. |
| jdbc.\<column name>.jdbc.precision  | Provides the original precision for all numeric and decimal fields.                                                                                                                                                                                                      |
| jdbc.\<column name>.jdbc.scale      | Provides the original scale for all numeric and decimal fields.                                                                                                                                                                                                          |
| jdbc.SYS\_CHANGE\_COLUMNS           | <p>Lists the columns that have changed since the last sync.</p><p>Returns NULL when column change tracking is not enabled, when the operation is insert or delete, or when all non-primary key columns were updated at once.</p>                                         |
| jdbc.SYS\_CHANGE\_CONTEXT           | Provides change context information when available.                                                                                                                                                                                                                      |
| jdbc.SYS\_CHANGE\_CREATION\_VERSION | Provides the version number associated with the last insert operation.                                                                                                                                                                                                   |
| jdbc.SYS\_CHANGE\_OPERATION         | <p>Indicates the type of change that occurred:</p><ul><li>I for insert</li><li>D for delete</li><li>U for update</li></ul>                                                                                                                                               |
| jdbc.SYS\_CHANGE\_VERSION           | Provides the version number of the most recent change to the row.                                                                                                                                                                                                        |

For details about the SYS\_CHANGE change tracking attributes, see the [SQL Server documentation](https://docs.microsoft.com/en-us/sql/relational-databases/system-functions/changetable-transact-sql).

#### CRUD Operation Header Attributes <a href="#ariaid-title12" id="ariaid-title12"></a>

When generating records, the SQL Server Change Tracking origin specifies the operation type in both of the following record header attributes:sdc.operation.typeThe SQL Server Change Tracking origin writes the operation type to the sdc.operation.type record header attribute.The origin uses the following values in the sdc.operation.type record header attribute to represent the operation type:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE

If you use a CRUD-enabled destination in the pipeline such as JDBC Producer or Elasticsearch, the destination can use the operation type when writing to destination systems. When necessary, you can use an Expression Evaluator or scripting processors to manipulate the value in the `sdc.operation.type` header attribute.&#x20;

When using CRUD-enabled destinations, the destination looks for the operation type in the sdc.operation.type attribute only.jdbc.SYS\_CHANGE\_OPERATIONThe SQL Server Change Tracking origin also writes the CRUD operation type to the jdbc.SYS\_CHANGE\_OPERATION record header attribute. However note that CRUD-enabled stages only use the sdc.operation.type header attribute, they do not check the jdbc.SYS\_CHANGE\_OPERATION attribute.

#### Field Attributes <a href="#ariaid-title13" id="ariaid-title13"></a>

The SQL Server Change Tracking origin generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

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

### Event Generation <a href="#ariaid-title14" id="ariaid-title14"></a>

The SQL Server Change Tracking origin can generate events that you can use in an event stream. When you enable event generation, the origin generates an event when it completes processing the data returned by the specified queries for all tables.

SQL Server Change Tracking events can be used in any logical way. For example:

*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin continues processing from the last-saved offset unless you reset the origin.

#### Event Record <a href="#ariaid-title15" id="ariaid-title15"></a>

Event records generated by SQL Server Change Tracking origin have the following event-related record header attributes:

| Record Header Attribute       | Description                                                                                                                                                         |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following type:</p><ul><li>no-more-data - Generated when the origin completes processing all data in the specified change tables.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                        |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                   |

The no-more-data event record includes no record fields.

### &#x20;<a href="#ariaid-title16" id="ariaid-title16"></a>

\
