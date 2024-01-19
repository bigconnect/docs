# SQL Server CDC Client

The SQL Server CDC Client origin processes data in Microsoft SQL Server change data capture (CDC) tables. The origin fetches changes in time windows and uses multiple threads to enable parallel processing of data.&#x20;

Use the SQL Server CDC Client origin to generate records from CDC tables. To read data from Microsoft SQL Server change tracking tables, use the SQL Server Change Tracking origin. For more information about the differences between CDC and change tracking data, see the [Microsoft SQL Server documentation](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/track-data-changes-sql-server). To read data from SQL Server temporal tables, use the JDBC Multitable Consumer origin or the JDBC Query Consumer origin. For more information about temporal tables, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/relational-databases/tables/temporal-tables?view=sql-server-ver15).

The SQL Server CDC Client origin includes the CRUD operation type in a record header attribute so generated records can be easily processed by CRUD-enabled destinations. For an overview of Data Pipelines changed data processing and a list of CRUD-enabled destinations, see Processing Changed Data.

You might use this origin to perform database replication. You can use a separate pipeline with the JDBC Query Consumer or JDBC Multitable Consumer origin to read existing data. Then start a pipeline with the SQL Server CDC Client origin to process subsequent changes.

When you configure the origin, you specify the SQL Server capture instance names - the origin processes the related CDC tables. You can define groups of tables in the same database and any initial offsets to use. When you omit initial offsets, the origin processes all available data in the CDC tables.

You can enable late table processing to allow the origin to process tables that appear after the pipeline starts. You can also configure the origin to check for schema changes in processed tables and to generate an event after discovering a change.

To determine how the origin connects to the database, you specify connection information, a query interval, number of retries, and any custom JDBC configuration properties that you need. You can configure advanced connection properties.

The origin can generate events for an event stream.&#x20;

### JDBC Driver <a href="#ariaid-title2" id="ariaid-title2"></a>

When connecting to Microsoft SQL Server, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for SQL Server.

### Supported Operations <a href="#ariaid-title3" id="ariaid-title3"></a>

The SQL Server CDC Client origin supports the SQL Server insert and delete operations. Updates captured after the update operation are treated as update, and updates captured before the update operation are treated as an unsupported operation.

### Time Windows <a href="#ariaid-title4" id="ariaid-title4"></a>

The SQL Server CDC Client origin fetches changes from tables in time windows. By default, the origin fetches changes in one time window, beginning with the last committed offset and ending with the latest data. When you have large volumes of data, you can improve performance by configuring multiple time windows.

To configure multiple time windows, set the Maximum Transaction Length property to the desired size of the time windows. The first time window starts at the initial offset or last saved offset and ends the specified number of seconds later. The next window starts where the previous ended and ends the specified number of seconds later, and so on. Within each time window, the origin creates batches based on the batch settings.

When you enable event generation, the origin produces a no-more-data event at the end of each time window, even when subsequent time windows remain for processing.

Previewing data shows no values when there are no changes during the first time window.

### Multithreaded Processing <a href="#ariaid-title5" id="ariaid-title5"></a>

The SQL Server CDC Client origin performs parallel processing and enables the creation of a multithreaded pipeline.

When you start the pipeline, the SQL Server CDC Client origin retrieves the list of CDC tables associated with the source tables defined in the table configurations. The origin then uses multiple concurrent threads based on the Number of Threads property. Each thread reads data from a single table.

Note: The Maximum Pool Size property on the Advanced tab defines the maximum number of connections the origin can make to the database. It must be equal to or greater than the value defined for the Number of Threads property.

As the pipeline runs, each thread connects to the origin system, creates a batch of data, and passes the batch to an available pipeline runner. A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin.

Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

#### Example

Say you are reading from 10 tables. You set the Number of Threads property to 5 and the Maximum Pool Size property to 6. When you start the pipeline, the origin retrieves the list of tables. The origin then creates five threads to read from the first five tables, and by default Data Pipelines creates a matching number of pipeline runners. Upon receiving data, a thread passes a batch to each of the pipeline runners for processing.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

### Batch Strategy <a href="#ariaid-title6" id="ariaid-title6"></a>

You can specify the batch strategy to use when processing data:

Process all available rows from the tableEach thread processes all available rows from a table. A thread runs a SQL query and processes all of the results for a table. Then, the thread switches to the next available table.

For example, let's say the origin has a maximum batch size of 100 and uses two threads to read from four tables, each of which contains 1000 rows. Thread 1 runs a SQL query to create 10 batches of 100 rows each from table A, while thread 2 uses the same strategy to read data from table B.When table A and table B are fully read, the threads switch to table C and table D and complete the same process. When thread 1 finishes reading from table C, it switches back to the next available table to read any data that has arrived since the table was processed, starting from the last saved offset.

The number of threads that can process the tables is limited by the Number of Threads property for the origin.Switch tablesWhen the origin performs multithreaded table processing for all tables, each thread processes one batch of data, then switches to an available table and repeats the process. When a pipeline starts, each thread runs a SQL query, generates a result set, and processes a batch of records from the result set. The database driver caches the remaining records for the same thread to access again. Then, the thread switches to the next available table.A table is available in the following situations:

*   The table does not have a result set created by another thread.

    In this case, the thread runs a SQL query to generate a result set and processes a batch of records. The database driver caches the remaining records for later use.
*   The table has a cached result set created by the same thread.

    In this case, the thread creates the batch from the result set, rather than running another SQL query.

A table is not available when the table has a result set created by another thread. A table with a cached result set can only be processed by the thread that created the result set. And a table cannot have multiple cached result sets.

The Max Batch Size property limits the number of records included in a batch. The Batches from Result Set property limits the number of batches that a thread can create from the result set. After a thread creates the configured number of batches, the thread closes the result set. This allows a different thread to read from the table.

For example, let's say an origin uses two concurrent threads to read from three tables. Max Batch Size is set to 100 records, and Batches from Result Set is set to 2.

Thread 1 runs a SQL query on table A, which contains 80 records. The thread processes the 80 records in a single batch. In the meantime, thread 2 runs a SQL query on table B, which contains 5000 records, and caches the result set. The thread creates a batch to process the first 100 records in the table.After processing table A, thread 1 looks for the next table to process. Thread 1 skips table B because the table has a cached result set from thread 2, so thread 1 runs a SQL query on table C, which contains 1000 records, and caches the result set. Thread 1 processes the first 100 records in the table.

Similarly, when thread 2 completes the batch on table B, it skips table C because thread 1 has a result set cached for the table. If table A had new data that came in, thread 2 would process that data, since there is no result set for the table. But if table A has no additional data, thread 2 returns to table B, where it has a cached result set.

Since thread 2 has a cached result set on table B, it creates a new batch from the result set instead of running a SQL query. Since this is the second batch from the result set, thread 2 closes the result set after processing the batch. This allows either thread to process data from table B the next time they check the table.

### Table Configuration <a href="#ariaid-title7" id="ariaid-title7"></a>

When you configure SQL Server CDC Client, you can define multiple CDC tables using a single set of table configuration properties. You can also define multiple table configurations to process multiple groups of CDC tables.

When you define table configurations you can define the following properties for each set of tables:Capture Instance NameDetermines the CDC tables to process. The naming convention for Microsoft SQL Server CDC tables is \<capture instance name>\_CT. When specifying this property, use the capture instance name, not the names of the CDC tables to be processed. For example, specify the `dbo.customer` source table, not the associated CDC table, `dbo_customer_CT`.

When you configure the Capture Instance Name property, you can specify a pattern that describes a set of capture instance names to use. If capture instance names are not set up in the database, then you specify a pattern for the table names to process.

You can use SQL-like syntax to define the set of CDC tables to process. For example, the pattern `east%` matches tables whose names start with "east". For more information about SQL LIKE syntax, see [https://msdn.microsoft.com/en-us/library/ms179859.aspx](https://msdn.microsoft.com/en-us/library/ms179859.aspx).

When configuring the Capture Instance Name property, you can use one of the following formats:

*   To process the CDC tables that match the specified capture instance name pattern, use the following format:

    ```
    <capture instance name pattern>
    ```

    Use this format when CDC tables are created based on capture instance names. You can use the pattern to process a full set of CDC tables or to exclude some CDC tables from processing.

    For example, say you have a `Sales.Accounts` table with a CDC table named `Sales_Accounts_CT`. After adding several columns to the table, you create a new CDC table called `Sales_Accounts2_CT`.

    To process both CDC tables, you can specify the following capture instance name pattern: `Sales_Accounts%`. To process only the CDC data that occurred after the schema change, you can specify the following capture instance name: `Sales_Accounts2`.
*   To process all available CDC tables for the specified data tables, use the following format:

    ```
    <schema name>_<data table name pattern>
    ```

    Use this format when CDC tables are created based on data tables instead of capture instance names.

    For example, to process all available CDC tables for data tables in a Sales schema, you might use `Sales_%`. Or, to process the CDC tables associated with a set of data tables with the Transact prefix, you might use `Sales_Transact%`.
*   To process all CDC tables associated with the schema, use the following format:

    ```
    <schema name>_%
    ```

    For example, to process all tables in the sales schema, enter `sales_%`.

Table exclusion patternOptionally specify a regex pattern for the table names that you want to exclude from the query.

For example, say you want to process all CDC tables in the schema except for those that start with "dept". You can use the default % for the table name pattern, and enter dept\* for the table exclusion pattern.

Initial offsetTo process existing data, specify an initial offset. When not set, the origin processes all available CDC data.The SQL Server CDC Client origin uses the `__$start_lsn` column as the offset column. To process existing data, define the offset value to use. The offset is used for all tables included in the table configuration.Important: When you specify an initial offset, the origin starts with the first value greater than or equal to the specified offset.

### Initial Table Order Strategy <a href="#ariaid-title8" id="ariaid-title8"></a>

You can define the initial order that the origin uses to read the tables.

Define one of the following initial table order strategies:NoneReads the tables in the order that they are listed in the database.AlphabeticalReads the tables in alphabetical order.

The origin uses the table order strategy only for the initial reading of the tables. When threads switch back to previously read tables, they read from the next available table, regardless of the defined order.

### Allow Late Table Processing <a href="#ariaid-title9" id="ariaid-title9"></a>

You can configure the SQL Server CDC Client to process data in CDC tables that appear after the pipeline starts.

When you allow late table processing, the SQL Server CDC Client origin uses a background thread to check for late CDC tables. The origin checks at regular user-defined intervals.

To enable late table processing, configure the following properties:

* On the JDBC tab, select the Allow Late Tables property.
* To define the time to wait before checking for new tables, configure the New Table Discovery Interval property.
* On the Advanced tab, set the Maximum Pool Size and Minimum Idle Connections properties to one thread more than the Number of Threads property.

### Checking for Schema Changes <a href="#ariaid-title10" id="ariaid-title10"></a>

You can configure the SQL Server CDC Client origin to check for schema changes in the tables being processed. When checking for schema changes, the origin includes a schema check statement in the SQL query.

When the origin checks for schema changes, it performs the following tasks:

1. Compares current table schemas with the original table schemas at regular intervals, based on the Query Interval property.
2.  If it determines that the schema of a table has changed, it generates a schema-change event that states the table or capture instance name with the changed schema.

    The origin generates a schema-change event each time that it finds a schema change: one for each table with a schema change.

    Note: Since the origin continues to check for schema changes at regular intervals until the pipeline stops, a single schema change can generate a large volume of events.
3. It can write the exact column name or data type change to the Data Pipelines log. To enable writing to the log, the log level must be set to Trace. For information about changing the log level, see Modifying the Log Level.

To enable the SQL Server CDC Client origin to check for schema changes and generate events when discovering them, enable both of the following properties:

* On the General tab, select the Produce Events property.
* On the JDBC tab, select the Enable Schema Changes Event property.

### Generated Record <a href="#ariaid-title11" id="ariaid-title11"></a>

When the SQL Server CDC Client origin generates a record, it places the CDC information, such as the CDC operation and start LSN values, in record header attributes. It places row data provided by the CDC tables into the fields of the generated records.

The origin also generates field attributes that provide additional information about field.

#### Record Header Attributes <a href="#ariaid-title12" id="ariaid-title12"></a>

The SQL Server CDC Client origin generates JDBC record header attributes that provide the SQL Server CDC data for each record, such as the start or end log sequence numbers (LSN).

The origin also includes the sdc.operation.type attribute and information from the SQL Server CDC tables. The SQL Server CDC Client header attributes are prefixed with `jdbc`. The names of the SQL Server CDC column names are included in the header attribute name, as follows: `jdbc.<CDC column name>`.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes.

The origin provides the following header attributes:

| Header Attribute Name              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.operation.type                 | <p>The origin uses the following values to represent the operation type:</p><ul><li>1 for Insert</li><li>2 for Delete</li><li>3 for Update, including updates captured after the update operation</li><li>5 for unsupported operations, including updates captured before the update operation</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| jdbc.cdc.source\_schema\_name      | Provides the source schema for the CDC data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| jdbc.cdc.source\_name              | Provides the source table for the CDC data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| jdbc.tables                        | Provides a comma-separated list of source tables for the fields in the record.Note: Not all JDBC drivers provide this information.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| jdbc.\<column name>.jdbcType       | <p>Provides the numeric value of the original SQL data type for each field in the record. See the <a href="https://docs.oracle.com/javase/8/docs/api/constant-values.html#java.sql.Types.ARRAY">Java documentation</a> for a list of the data types that correspond to numeric values.</p><p>Because the record read from the SQL Server CDC table includes CDC columns, the generated record also includes corresponding jdbc.&#x3C;column name>.jdbcType header attributes for those columns.</p><p>For example, since the original data includes a __$start_lsn column, the resulting record has a jdbc.__$start_lsn.jdbc.Type header attribute. It also has a jdbc.__$start_lsn attribute that is generated separately by the origin, as described below.</p> |
| jdbc.\<column name>.jdbc.precision | Provides the original precision for all numeric and decimal fields.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| jdbc.\<column name>.jdbc.scale     | Provides the original scale for all numeric and decimal fields.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| jdbc. \_\_$command\_id             | Data from the SQL Server CDC \_\_$command\_id column.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| jdbc.\_\_$end\_lsn                 | Data from the SQL Server CDC \_\_$end\_lsn column.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| jdbc.\_\_$operation                | The CRUD operation type using SQL Server codes, as defined in the SQL Server CDC \_\_$operation column.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| jdbc.\_\_$seqval                   | Data from the SQL Server CDC \_\_$seqval column.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| jdbc.\_\_$start\_lsn               | Data from the SQL Server CDC \_\_$start\_lsn column.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| jdbc.\_\_$update\_mask             | Data from the SQL Server CDC \_\_$update\_mask column.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

For details about the CDC attributes, see the [SQL Server documentation](https://docs.microsoft.com/en-us/sql/relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql).

#### CRUD Operation Header Attributes <a href="#ariaid-title13" id="ariaid-title13"></a>

When generating records, the SQL Server CDC Client origin specifies the operation type in both of the following record header attributes:sdc.operation.typeThe SQL Server CDC Client origin writes the operation type to the sdc.operation.type record header attribute.The origin uses the following values in the sdc.operation.type record header attribute to represent the operation type:

* 1 for Insert
* 2 for Delete
* 3 for Update, including updates captured after the update operation
* 5 for unsupported operations, including updates captured before the update operation

If you use a CRUD-enabled destination in the pipeline such as JDBC Producer or Elasticsearch, the destination can use the operation type when writing to destination systems. When necessary, you can use an Expression Evaluator or scripting processors to manipulate the value in the `sdc.operation.type` header attribute.&#x20;

When using CRUD-enabled destinations, the destination looks for the operation type in the sdc.operation.type attribute only.jdbc.\_\_$operationThe SQL Server CDC Client origin places the values from the SQL Server \_\_$operation column in the jdbc.\_\_$operation record header attribute. As a result, the jdbc.\_\_$operation record header attribute contains the CRUD operation type as defined using SQL Server CDC codes.Note that CRUD-enabled stages only use the sdc.operation.type header attribute, they do not check the jdbc.\_\_$operation header attribute.SQL Server CDC uses the following codes to define the operation type:

* 1 for delete
* 2 for insert
* 3 for updates captured before the update operation
* 4 for updates captured after the update operation

#### Field Attributes <a href="#ariaid-title14" id="ariaid-title14"></a>

The SQL Server CDC Client origin generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

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

### Event Generation <a href="#ariaid-title15" id="ariaid-title15"></a>

The SQL Server CDC Client origin can generate events that you can use in an event stream. When you enable event generation, the origin generates an event when it completes processing the data returned by the specified queries for all tables.

If you enable schema change event generation, the origin also generates an event each time it finds a schema change.

SQL Server CDC Client events can be used in any logical way. For example:

*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin continues processing from the last-saved offset unless you reset the origin.
* With a destination to store event information.

#### Event Record <a href="#ariaid-title16" id="ariaid-title16"></a>

Event records generated by SQL Server CDC Client origin have the following event-related record header attributes:

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                                                                                |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>no-more-data - Generated when the origin completes processing all the requested CDC data in a time window.</li><li>schema-change - Generated only when checking for schema changes is enabled and when the origin determines that a schema change has occurred for one of the tables being processed.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                                                                               |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                                                                          |

The SQL Server CDC Client origin can generate the following types of event records:no-more-data

The origin generates a no-more-data event record when the origin completes processing all available data in a time window and the number of seconds configured for event generation delay elapses without any new files appearing to be processed. The origin generates the event record even when subsequent time windows remain for processing.

The no-more-data event record generated by the origin has the sdc.event.type set to no-more-data and does not include any additional fields.

schema-change

The origin generates a schema-change event record only when you enable the origin to check for schema changes, and the origin discovers a schema change.

The schema-change event record generated by the origin has the sdc.event.type set to schema-change and includes the following fields:

| Event Record Field       | Description                                                                                     |
| ------------------------ | ----------------------------------------------------------------------------------------------- |
| capture-instance-name    | The name of the capture instance or CDC table associated with the table with the schema change. |
| source-table-schema-name | The name of the schema that contains the data table.                                            |
| source-table-name        | The name of the data table that has a schema changes.                                           |

### &#x20;<a href="#ariaid-title17" id="ariaid-title17"></a>

\
