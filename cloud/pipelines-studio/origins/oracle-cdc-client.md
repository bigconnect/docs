# Oracle CDC Client

The Oracle CDC Client origin processes change data capture (CDC) information provided by Oracle LogMiner redo logs.&#x20;

You might use this origin to perform database replication. You can use a separate pipeline with the JDBC Query Consumer or JDBC Multitable Consumer origin to read existing data. Then start a pipeline with the Oracle CDC Client origin to process subsequent changes.

Oracle CDC Client processes data based on the commit number, in ascending order.

To read the redo logs, Oracle CDC Client requires the LogMiner dictionary. The origin can use the dictionary in redo logs or in an online catalog. When using the dictionary in redo logs, the origin captures and adjusts to schema changes. The origin can also generate events when using redo log dictionaries.Note: To use Oracle CDC Client, you must enable LogMiner for the database that you want to use and complete the necessary prerequisite tasks. The origin uses JDBC to access the database.

The origin includes CDC and CRUD information in record header attributes so generated records can be easily processed by CRUD-enabled destinations.&#x20;

When you configure Oracle CDC Client, you configure change data capture details, such as the schema and tables to read from, how to read the initial change, the dictionary source location, and the operations to include. You also specify the transaction window and LogMiner session windows to use.

You can configure the origin to buffer records locally or to use database buffers. Before using local buffers, verify that the required resources are available and specify the action to take for uncommitted transactions.

You can specify the behavior when the origin encounters an unsupported data type, and you can configure the origin to pass null values when it receives them from supplemental logging data. When the source database has high-precision timestamps, you can configure the origin to write string values rather than datetime values to maintain the precision.

You also specify JDBC connection information and user credentials. If the schema was created in a pluggable database, state the pluggable database name. You can configure custom properties that the driver requires.

You can configure advanced connection properties. To use a JDBC version older than 4.0, you specify the driver class name and define a health check query.

The origin can generate events for an event stream.&#x20;

When concerned about performance, as when processing very wide tables, you can consider several alternatives to the default origin behavior. You can use the alternate PEG parser or use multiple threads for parsing. Or, you can configure the origin to not parse SQL query statements so you can pass the queries to the SQL Parser processor to be parsed.

For a list of supported operations and a description of generated records, see Parse SQL, Supported Operations, and Generated Records.

### LogMiner Dictionary Source <a href="#ariaid-title2" id="ariaid-title2"></a>

LogMiner provides dictionaries to help process redo logs. LogMiner can store dictionaries in several locations.

The Oracle CDC Client can use the following dictionary source locations:

* Online catalog - Use the online catalog when table structures are not expected to change.
* Redo logs - Use redo logs when table structures are expected to change. When reading the dictionary from redo logs, the Oracle CDC Client origin determines when schema changes occur and refreshes the schema that it uses to create records. The origin can also generate events for each DDL it reads in the redo logs.Important: When using the dictionary in redo logs, make sure to extract the latest dictionary to the redo logs each time table structures change. For more information, see Task 4. Extract a Log Miner Dictionary (Redo Logs).

Note that using the dictionary in redo logs can have significantly higher latency than using the dictionary in the online catalog. But using the online catalog does not allow for schema changes.

For more information about dictionary options and configuring LogMiner, see the Oracle LogMiner documentation.

### Oracle CDC Client Prerequisites <a href="#ariaid-title3" id="ariaid-title3"></a>

Before using the Oracle CDC Client origin, complete the following tasks:

1. Enable LogMiner.
2. Enable supplemental logging for the database or tables.
3. Create a user account with the required roles and privileges.
4. To use the dictionary in redo logs, extract the Log Miner dictionary.
5. Install the Oracle JDBC driver.

#### Task 1. Enable LogMiner <a href="#ariaid-title4" id="ariaid-title4"></a>

LogMiner provides redo logs that summarize database activity. The origin uses these logs to generate records.

LogMiner requires that the database be open, writable, and in ARCHIVELOG mode with archiving enabled.

You enable LogMiner differently based on whether you use an Oracle or Oracle RAC database:Oracle databasesUse the following steps to determine the status of the database and enable LogMiner:

1. In a SQL shell, log into the database as a user with DBA privileges and run the following commands.
2.  Check the database logging mode:

    ```
    select log_mode from v$database;
    ```

    If the command returns ARCHIVELOG, you can skip to [Task 2](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/OracleCDC.html#concept\_gvh\_3c2\_3y).

    If the command returns NOARCHIVELOG, continue with the following steps:
3.  Shut down the database:

    ```
    shutdown immediate;
    ```
4.  Start up and mount the database:

    ```
    startup mount;
    ```
5.  Enable archiving, open the database, and make it writable:

    ```
    alter database archivelog;
    alter database open read write;
    ```

Oracle RAC databasesUse the following steps to enable LogMiner and verify the status of the database:

1.  In a Terminal session on one of the database nodes, stop and mount the database:

    ```
    srvctl stop database -d <database name>
    srvctl start database -d <database name> -o mount
    ```
2.  In a SQL shell, log into the database as a user with DBA privileges and configure the database:

    ```
    alter database archivelog;
    ```
3.  In the Terminal session, restart the database:

    ```
    srvctl stop database -d <database name>
    srvctl start database -d <database name>
    ```
4.  In the SQL shell, check if the database is in logging mode:

    ```
    select log_mode from v$database;
    ```

#### Task 2. Enable Supplemental Logging <a href="#ariaid-title5" id="ariaid-title5"></a>

To retrieve data from redo logs, LogMiner requires supplemental logging for the database or tables.

Enable at least primary key or "identification key" logging at a table level for each table that you want to use. With identification key logging, records include only the primary key and changed fields.

Due to an Oracle known issue, to enable supplemental logging for a table, you must first enable minimum supplemental logging for the database.

To include all fields in the records the origin generates, enable full supplemental logging at a table or database level. Full supplemental logging provides data from all columns, those with unchanged data as well as the primary key and changed columns. For details on the data included in records based on the supplemental logging type, see Parse SQL, Supported Operations, and Generated Records.

1.  To verify if supplemental logging is enabled for the database, run the following command in a SQL shell:

    ```
    SELECT supplemental_log_data_min, supplemental_log_data_pk, supplemental_log_data_all FROM v$database;
    ```

    If the command returns `Yes` or `Implicit` for all three columns, then supplemental logging is enabled with both identification key and full supplemental logging. You can skip to [Task 3.](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/OracleCDC.html#concept\_jnz\_bd2\_3y)

    If the command returns Yes or Implicit for the first two columns, then supplemental logging is enabled with identification key logging. If this is what you want, you can skip to [Task 3](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/OracleCDC.html#concept\_jnz\_bd2\_3y).
2.  Enable identification key or full supplemental logging.For Oracle and Oracle RAC 12c, 18c, or 19c multitenant databases, best practice is to enable logging for the container for the tables, rather than the entire database. You can use the following command in a SQL shell to first apply the changes to just the container:

    ```
    ALTER SESSION SET CONTAINER=<pdb>;
    ```

    You can enable identification key or full supplemental logging to retrieve data from redo logs. You do not need to enable both:To enable identification key loggingYou can enable identification key logging for individual tables or all tables in the database:

    *   For individual tables

        In the SQL shell, use the following commands to enable minimal supplemental logging for the database, and then enable identification key logging for each table that you want to use:

        ```
        ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
        ```

        ```
        ALTER TABLE <schema name>.<table name> ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;
        ```
    *   For all tables

        In the SQL shell, use the following command to enable identification key logging for the entire database:

        ```
        ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;
        ```

    To enable full supplemental loggingYou can enable full supplemental logging for individual tables or all tables in the database:

    *   For individual tables

        In the SQL shell, use the following commands to enable minimal supplemental logging for the database, and then enable full supplemental logging for each table that you want to use:

        ```
        ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
        ```

        ```
        ALTER TABLE <schema name>.<table name> ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
        ```
    *   For all tables

        In the SQL shell, use the following command to enable full supplemental logging for the entire database:

        ```
        ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
        ```
3.  To submit the changes:

    ```
    ALTER SYSTEM ARCHIVE LOG CURRENT;
    ```

#### Task 3. Create a User Account <a href="#ariaid-title6" id="ariaid-title6"></a>

Create a user account to use with the Oracle CDC Client origin. You need the account to access the database through JDBC.

You create accounts differently based on the Oracle version that you use:12c, 18c, or 19c multitenant databasesFor multitenant Oracle and Oracle RAC 12c, 18c, and 19c databases, create a common user account. Common user accounts are created in `cdb$root` and must use the convention: `c##<name>`.

1. In a SQL shell, log into the database as a user with DBA privileges.
2.  Create the common user account using the following set of commands:

    ```
    ALTER SESSION SET CONTAINER=cdb$root;
    CREATE USER <user name> IDENTIFIED BY <password> CONTAINER=all;
    GRANT create session, alter session, set container, logmining, execute_catalog_role TO <user name> CONTAINER=all;
    GRANT select on GV_$DATABASE to <user name>;
    GRANT select on V_$DATABASE to <user name>;
    GRANT select on V_$DATABASE_INCARNATION to <user name>;
    GRANT select on V_$LOGMNR_CONTENTS to <user name>;
    GRANT select on V_$ARCHIVED_LOG to <user name>;
    GRANT select on V_$LOG to <user name>;
    GRANT select on V_$LOGFILE to <user name>;
    GRANT select on V_$LOGMNR_LOGS to <user name>;
    ALTER SESSION SET CONTAINER=<pdb>;
    GRANT select on <db>.<table> TO <user name>;
    ```

    Repeat the final command for each table that you want to use.

When you configure the origin, use this user account for the JDBC credentials. Use the entire user name, including the `c##`, as the JDBC user name.

12c, 18c, or 19c standard databasesFor standard Oracle and Oracle RAC 12c, 18c, and 19c databases, create a user account with the necessary privileges:

1. In a SQL shell, log into the database as a user with DBA privileges.
2.  Create the user account using the following set of commands:

    ```
    CREATE USER <user name> IDENTIFIED BY <password>;
    GRANT create session, alter session, logmining, execute_catalog_role TO <user name>;
    GRANT select on GV_$DATABASE to <user name>;
    GRANT select on V_$DATABASE to <user name>;
    GRANT select on V_$DATABASE_INCARNATION to <user name>;
    GRANT select on V_$LOGMNR_CONTENTS to <user name>;
    GRANT select on V_$ARCHIVED_LOG to <user name>;
    GRANT select on V_$LOG to <user name>;
    GRANT select on V_$LOGFILE to <user name>;
    GRANT select on V_$LOGMNR_LOGS to <user name>;
    GRANT select on <db>.<table> TO <user name>;
    ```

    Repeat the final command for each table that you want to use.

When you configure the origin, use this user account for the JDBC credentials.

11g databasesFor Oracle 11g databases, create a user account with the necessary privileges:

1. In a SQL shell, log into the database as a user with DBA privileges.
2.  Create the user account using the following commands:

    ```
    CREATE USER <user name> IDENTIFIED BY <password>;
    GRANT create session, alter session, execute_catalog_role, select any transaction, select any table to <user name>;
    GRANT select on GV_$DATABASE to <user name>;
    GRANT select on V_$DATABASE to <user name>;
    GRANT select on V_$DATABASE_INCARNATION to <user name>;
    GRANT select on V_$LOGMNR_CONTENTS to <user name>;
    GRANT select on V_$ARCHIVED_LOG to <user name>;
    GRANT select on V_$LOG to <user name>;
    GRANT select on V_$LOGFILE to <user name>;
    GRANT select on V_$LOGMNR_LOGS to <user name>;
    GRANT select on <db>.<table> TO <user name>;
    ```

    Repeat the final command for each table that you want to use.

When you configure the origin, use this user account for the JDBC credentials.

#### Task 4. Extract a Log Miner Dictionary (Redo Logs) <a href="#ariaid-title7" id="ariaid-title7"></a>

When using redo logs as the dictionary source, you must extract the Log Miner dictionary to the redo logs before you start the pipeline. Repeat this step periodically to ensure that the redo logs that contain the dictionary are still available.

Oracle recommends that you extract the dictionary only at off-peak hours since the extraction can consume database resources.

To extract the dictionary for all Oracle and Oracle RAC standard databases, run the following command:

```
EXECUTE DBMS_LOGMNR_D.BUILD(OPTIONS=> DBMS_LOGMNR_D.STORE_IN_REDO_LOGS);
```

To extract the dictionary for Oracle and Oracle RAC 12c, 18c, or 19c multitenant databases, run the following commands:

```
ALTER SESSION SET CONTAINER=cdb$root;
EXECUTE DBMS_LOGMNR_D.BUILD(OPTIONS=> DBMS_LOGMNR_D.STORE_IN_REDO_LOGS);
```

#### Task 5. Install the Driver <a href="#ariaid-title8" id="ariaid-title8"></a>

The Oracle CDC Client origin connects to Oracle through JDBC. You cannot access the database until you install the required driver.

Install the Oracle JDBC driver for the Oracle database version that you use.

### Table Configuration <a href="#ariaid-title9" id="ariaid-title9"></a>

When you configure the Oracle CDC Client origin, you specify the tables with the change capture data that you want to process.

A table configuration defines a group of tables with the same table name pattern, that are from one or more schemas with the same name pattern.

You can define one or more table configurations.

#### Schema, Table Name, and Exclusion Patterns <a href="#ariaid-title10" id="ariaid-title10"></a>

To specify the tables, you define the schema, a table name pattern, and an optional table exclusion pattern.

When defining the schema and table name pattern, you can use SQL LIKE syntax to define a set of tables within a schema or across multiple schemas. For more information about valid patterns for the SQL LIKE syntax, see the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/ms179859.aspx).

When needed, you can also use a regular expression as an exclusion pattern to exclude a subset of tables from the larger set.

For example, say you want to process change data capture data for all tables in the sales schema that start with `SALES` while excluding those that end with a dash (-) and single-character suffix. You can use the following configuration to specify the tables to process:

* Schema: `sales`
* Table Name Pattern: `SALES%`
* Exclusion Pattern: `SALES.*-.`

### Initial Change <a href="#ariaid-title11" id="ariaid-title11"></a>

The initial change is the point in the LogMiner redo logs where you want to start processing. When you start the pipeline for the first time, the origin starts processing from the specified initial change. The origin only uses the specified initial change again when you reset the origin.

Note that Oracle CDC Client processes only change capture data. If you need existing data, you might use a JDBC Query Consumer or a JDBC Multitable Consumer in a separate pipeline to read table data before you start an Oracle CDC Client pipeline.

Oracle CDC Client provides several ways to configure the initial change:From the latest changeThe origin processes all changes that occur after you start the pipeline.From a specified datetimeThe origin processes all changes that occurred at the specified datetime and later. Use the following format: `DD-MM-YYYY HH24:MI:SS`.Note: If the database operates in a different time zone from Data Pipelines, the DB Time Zone property must be set correctly to ensure that the appropriate changes are processed.From a specified system change number (SCN)The origin processes all changes that occurred in the specified SCN and later. When using the specified SCN, the origin starts processing with the timestamp associated with the SCN. If the SCN cannot be found in the redo logs, the origin continues reading from the next higher SCN that is available in the redo logs.Typically, a database admin can provide the SCN to use.

#### Example

You want to process all existing data in the Orders table and then capture changed data, writing all data to Amazon S3. To read the existing data, you use a pipeline with the JDBC Query Consumer and Amazon S3 destination as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/OracleCDC-JDBCpipe.png)

Once all existing data is read, you stop the JDBC Query Consumer pipeline and start the following Oracle CDC Client pipeline. This pipeline is configured to pick up changes that occur after you start the pipeline, but if you wanted to prevent any chance of data loss, you could configure the initial change for an exact datetime or earlier SCN:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/OracleCDC-pipeline.png)

### Choosing Buffers <a href="#ariaid-title12" id="ariaid-title12"></a>

When processing data, the Oracle CDC Client can buffer data locally on the Data Pipelines machine or use Oracle LogMiner buffers:

Local buffers

When using local buffers, the origin requests the transactions for the relevant tables and time period. The origin buffers the resulting LogMiner redo SQL statements until it verifies a commit for a transaction. After seeing a commit, it parses and processes the committed data.

The origin can buffer the redo SQL statements completely in memory or write them primarily to disk while using a small amount of memory for tracking.

By default, the origin uses local buffers. In general, using local buffers should provide better performance than Oracle LogMiner buffers.

Use local buffers to process large transactions or to avoid monopolizing the Oracle PGA. Buffer information in memory for better performance when Data Pipelines resources allow. Buffer information to disk to avoid monopolizing Data Pipelines resources.

Oracle LogMiner buffersWhen using Oracle LogMiner buffers, the origin requests data from Oracle LogMiner for a particular time period. LogMiner then buffers all transactions for that time period for all tables in the database, rather than only the tables needed by the origin.LogMiner keeps the transaction information in the buffers until it reads commits in the logs, then it passes the committed data to the Oracle CDC Client origin.Depending on the size and number of the transactions and the time period being buffered, buffering the transactions can monopolize the PGA, the private memory region for the Oracle server process.Use LogMiner buffers when you do not expect the volume of transactions to overtax Oracle resources.

#### Local Buffer Resource Requirements <a href="#ariaid-title13" id="ariaid-title13"></a>

Before using local buffers, you should verify that the allocated resources are sufficient for the needs of the pipeline.

Use the following guidelines based on the local buffering option that you use:In memoryWhen buffering in memory, the origin buffers the LogMiner redo SQL statements returned by Oracle. It processes the data after receiving a commit for the statement.Before buffering in memory, increase the Data Pipelines Java heap size to accommodate the information that you expect the pipeline to receive.Use the following equation to estimate the Java heap size requirements:

```
Required Memory = (L * (30 + S) * T * 1.5) bytes
```

*   L - Maximum number of statements LogMiner generates for each transaction.

    Each changed row generates one statement with all field names and values.
*   S - Maximum length in characters of each redo SQL statement generated by LogMiner, including all field names and values.For example, the following SQL statement contains 92 characters, including spaces and punctuation.

    ```
    insert into "SYS"."Y"("ID","D") values ('690054',TO_TIMESTAMP('2017-07-18 02:00:00.389390'))
    ```
* T - Maximum number of transactions in flight for the tables at any given time.

30 represents the number of bytes in the statement ID, which is also stored.To diskWhen buffering to disk, the origin stores only the statement ID for each SQL query in memory. Then it saves the queries to disk.Note that when buffering to disk, Data Pipelines clears the buffered data when the pipeline starts and stops. When you restart a pipeline, Data Pipelines uses the last-saved offset unless you reset the origin.Before buffering to disk, ensure that enough space is available on the local disk. You might also increase the Data Pipelines Java heap size, but to a lesser degree than when buffering fully in memory.Use the following calculation to determine the amount of disk space that the pipeline requires:

```
Required Disk Space = (L * S * T * 1.5) bytes
```

*   L - Maximum number of statements LogMiner generates for each transaction.

    Each changed row generates one statement with all field names and values.
*   S - Maximum length in characters of each redo SQL statement generated by LogMiner, including all field names and values.For example, the following SQL statement contains 92 characters, including spaces and punctuation.

    ```
    insert into "SYS"."Y"("ID","D") values ('690054',TO_TIMESTAMP('2017-07-18 02:00:00.389390'))
    ```
* T - Maximum number of transactions in flight for the tables at any given time.

Use the following equation to estimate the Java heap size required by the pipeline:

```
Required Memory = (L * T * 30 * 1.5) bytes
```

*   L - Maximum number of statements LogMiner generates for each transaction.

    Each changed row generates one statement with all field names and values.
* T: Maximum number of transactions in flight for the tables at any given time.

#### Uncommitted Transaction Handling <a href="#ariaid-title14" id="ariaid-title14"></a>

You can configure how the Oracle CDC Client origin handles old uncommitted transactions when the origin uses local buffers.

When using local buffers, the Oracle CDC Client origin clears the buffers of old uncommitted transactions after each LogMiner session. Old uncommitted transactions are those that are older than the time specified for the Maximum Transaction Length property.

By default, the origin generates error records for the old uncommitted transactions. The origin converts each LogMiner redo SQL statement to a record and passes the record to the stage for error handling.

If you don't need error records for old uncommitted transactions, you can configure the origin to discard uncommitted transactions. This also reduces the overhead used to generate the error records.

For example, say the Maximum Transaction Length and the LogMiner Session Window properties are both set to one hour, and a transaction arrives in the last ten minutes of a LogMiner session. If the transaction remains uncommitted when the LogMiner session closes and the next session begins, the Oracle CDC origin retains the transaction for the next session. This is because the transaction is only ten minutes old and the maximum transaction length is an hour.

Now say the transaction is still uncommitted an hour later, when the next LogMiner session closes. Then, the transaction is considered an old uncommitted transaction because it is older than the maximum transaction length. By default, the origin generates an error record for the transaction. However, if you configure the origin to discard old uncommitted transactions, the origin simply discards the transaction instead.

To discard old uncommitted transactions, select the Discard Old Uncommitted Transactions property. This property is only available when you buffer changes locally.

### Include Nulls <a href="#ariaid-title15" id="ariaid-title15"></a>

When the Oracle LogMiner performs full supplemental logging, the resulting data includes all columns in the table with null values where no changes occurred. When the Oracle CDC Client processes this data, by default, it ignores null values when generating records.

You can configure the origin to include the null values in the record. You might need to include the null values when the destination system has required fields. To include null values, enable the Include Nulls property on the Oracle CDC tab.

### Unsupported Data Types <a href="#ariaid-title16" id="ariaid-title16"></a>

You can configure how the origin handles records that contain unsupported data types. The origin can perform the following actions:

* Pass the record to the pipeline without the unsupported data types.
* Pass the record to error without the unsupported data types.
* Discard the record.

You can configure the origin to include the unsupported data types in the record. When you include unsupported types, the origin includes the field names and passes the data as unparsed strings, when possible.

The following Oracle data types are not supported by the Oracle CDC Client origin:

* Array
* Blob
* Clob
* Datalink
* Distinct
* Interval
* Java Object
* Nclob
* Other
* Ref
* Ref Cursor
* SQLXML
* Struct
* Time with Timezone

#### Conditional Data Type Support <a href="#ariaid-title17" id="ariaid-title17"></a>

Note the following information about conditionally supported Oracle data types:

* The Oracle Raw data type is treated as a Data Pipelines Byte Array.
* The Oracle Timestamp with Timezone data type is converted to the Data Pipelines Zoned Datetime data type. To maximize efficiency while providing more precision, the origin includes only the UTC offset with the data. It omits the time zone ID.

### Parse SQL, Supported Operations, and Generated Records <a href="#ariaid-title18" id="ariaid-title18"></a>

The Oracle CDC Client origin generates records differently depending on whether the origin is configured to parse SQL queries:Parsing SQL QueriesWhen the Oracle CDC Client origin parses SQL queries, it can create records for the following operations:

* INSERT
* DELETE
* UPDATE

The origin generates records based on the Oplog operation type, the logging enabled for the database and tables, and the operations selected in the Operations stage property.For example, if you specify Insert operations, the origin generates only Insert records, ignoring all Update and Delete operations.The following table describes how the origin generates record data:

| Oplog Operation | Identification/Primary Key Logging Only                         | Full Supplemental Logging |
| --------------- | --------------------------------------------------------------- | ------------------------- |
| INSERT          | All fields that contain data, ignoring fields with null values. | All fields.               |
| UPDATE          | Primary key field and fields with updated values.               | All fields.               |
| DELETE          | Primary key field.                                              | All fields.               |

When parsing the SQL query, the origin also includes CDC and CRUD information in record header attributes.To parse the SQL query, on the **Oracle CDC** tab, select the **Parse SQL Query** property. This property is enabled by default.Then, configure the Operations property to specify the operations that you want to process.Not parsing SQL queries

When the Oracle CDC Client origin does not parse the SQL query, it writes each LogMiner SQL statement to a field named `sql`. The origin also generates field attributes that provide additional information about field.

You might write LogMiner SQL statements to generated records to pass the queries to the [SQL Parser processor](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Processors/SQLParser.html#concept\_zh2\_kfj\_tdb) to be parsed or to archive that information.

When not parsing the SQL query, the origin can create records for the following operations:

* INSERT
* DELETE
* UPDATE
* SELECT\_FOR\_UPDATE

The records are created based on the selected operations in the Operations stage property.To write LogMiner SQL statements to generated records, on the **Oracle CDC** tab, clear the **Parse SQL Query** property.Then, configure the Operations property to specify the operations that you want to include.

#### CRUD Operation Header Attributes <a href="#ariaid-title19" id="ariaid-title19"></a>

When the Oracle CDC Client is configured to parse the SQL query, it specifies the operation type in both of the following record header attributes when generating records:sdc.operation.typeThe Oracle CDC Client evaluates the Oplog operation type associated with each entry that it processes and, when appropriate, it writes the operation type to the `sdc.operation.type` record header attribute.The origin uses the following values in the `sdc.operation.type` record header attribute to represent the operation type:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE and SELECT\_FOR\_UPDATE

If you use a CRUD-enabled destination in the pipeline such as JDBC Producer or Elasticsearch, the destination can use the operation type when writing to destination systems. When necessary, you can use an Expression Evaluator or scripting processors to manipulate the value in the `sdc.operation.type` header attribute.&#x20;

When using CRUD-enabled destinations, the destination looks for the operation type in the `sdc.operation.type` attribute before checking the `oracle.cdc.operation` attribute.oracle.cdc.operationThe Oracle CDC Client also writes the Oplog CRUD operation type to the `oracle.cdc.operation` record header attribute. This attribute was implemented in an earlier release and is supported for backward compatibility.The origin writes the Oplog operation type to the `oracle.cdc.operation` attribute as the following strings:

* INSERT
* UPDATE
* SELECT\_FOR\_ UPDATE
* DELETE

CRUD-enabled destinations check for this attribute for the operation type after checking the `sdc.operation.type` attribute.

#### CDC Header Attributes <a href="#ariaid-title20" id="ariaid-title20"></a>

When the Oracle CDC Client is configured to parse the SQL query, it provides the following CDC record header attributes for each record:

* oracle.cdc.operation
*   oracle.cdc.precisionTimestamp - Timestamp, to the nanosecond, when the database change occurred.

    Used when the database provides the timestamp in nanoseconds.
* oracle.cdc.query
* oracle.cdc.redoValue
* oracle.cdc.rowId
* oracle.cdc.scn
* oracle.cdc.sequence.oracle - Includes sequence numbers that indicate the order in which statements were processed within the transaction.This sequence number is generated by Oracle. Use to keep statements in order within a single transaction.Note: When the origin uses local buffering, Oracle provides the default value of `1` for all records, so should not be used to order statements. Use the `oracle.cdc.sequence.internal` attribute when using local buffering.
*   oracle.cdc.sequence.internal - Includes sequence numbers equivalent to those in the `oracle.cdc.sequence.oracle` attribute, but created by the Oracle CDC Client origin.

    Like the `oracle.cdc.sequence.oracle` attribute, you can use these values to keep statements in order within a single transaction.

    Best practice is to use this attribute only when using local buffering, when the `oracle.cdc.sequence.oracle` attribute does not provide the expected data.
* oracle.cdc.table
*   oracle.cdc.timestamp - Timestamp, to the second, when the change occurred.

    Used when the database provides the timestamp in seconds.
* oracle.cdc.undoValue
* oracle.cdc.user

And it includes the following record header attributes for each decimal field in the record:

* jdbc.\<fieldname>.precision
* jdbc.\<fieldname>.scale

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes.&#x20;

#### Field Attributes <a href="#ariaid-title21" id="ariaid-title21"></a>

The Oracle CDC Client origin generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

The following data type conversions do not include all information in the corresponding Data Pipelines type:

* The Oracle Number data type is converted to the Data Pipelines Decimal data type, which does not store scale and precision.
* The Oracle Timestamp data type is converted to the Data Pipelines Datetime data type, which does not store nanoseconds.

To preserve this information during data type conversion, the origin generates the following field attributes for these Data Pipelines data types:

| Data Pipelines Data Type | Generated Field Attribute | Description                                                   |
| ------------------------ | ------------------------- | ------------------------------------------------------------- |
| Decimal                  | precision                 | Provides the original precision for every number column.      |
| Decimal                  | scale                     | Provides the original scale for every number column.          |
| Datetime                 | nanoSeconds               | Provides the original nanoseconds for every timestamp column. |

You can use the `record:fieldAttribute` or `record:fieldAttributeOrDefault` functions to access the information in the attributes.&#x20;

### Event Generation <a href="#ariaid-title22" id="ariaid-title22"></a>

The Oracle CDC Client origin can generate events that you can use in an event stream when the origin uses redo logs as the dictionary source. The origin does not generate events when using the online catalog as the dictionary source.

When you use redo logs as the dictionary source and enable event generation, the Oracle CDC Client generates events when it reads DDL statements. It generates events for ALTER, CREATE, DROP, and TRUNCATE statements.

When you start the pipeline, the origin queries the database and caches the schemas for all tables listed in the origin, and then generates an initial event record for each table. Each event record describes the current schema for each table. The origin uses the cached schemas to generate records when processing data-related redo log entries.

The origin then generates an event record for each DDL statement it encounters in the redo logs. Each event record includes the DDL statement and the related table in record header attributes.

The origin includes table schema information in the event record for new and updated tables. When the origin encounters an ALTER or CREATE statement, it queries the database for the latest schema for the table.

If the ALTER statement is an update to a cached schema, the origin updates the cache and includes the updated table schema in the event record. If the ALTER statement is older than the cached schema, the origin does not include the table schema in the event record. Similarly, if the CREATE statement is for a "new" table, the origin caches the new table and includes the table schema in the event record. Because the origin verifies that all specified tables exist when the pipeline starts, this can occur only when the table is dropped and created after the pipeline starts. If the CREATE statement is for a table that is already cached, the origin does not include the table schema in the event record.

Oracle CDC Client events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title23" id="ariaid-title23"></a>

Event records generated by the Oracle CDC Client origin have the following event-related record header attributes. Record header attributes are stored as strings:

| Record Header Attribute       | Description                                                                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>STARTUP</li><li>ALTER</li><li>CREATE</li><li>DROP</li><li>TRUNCATE</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                            |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                       |
| oracle.cdc.table              | Name of the Oracle database table that changed.                                                                                         |
| oracle.cdc.ddl                | The DDL statement that triggered the event.                                                                                             |

STARTUP event records are generated when the pipeline starts. The origin creates one event record for each table. The event record includes the current table schema.

DROP and TRUNCATE event records include just the record header attributes listed above.

CREATE event records include the schema for the new table when the table has been dropped and recreated. ALTER event records include the table schema when the statement updates a cached schema. For more information about the behavior for CREATE and ALTER statements, see [Event Generation](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/OracleCDC.html#concept\_h2t\_hx1\_vy).

For example, the following ALTER event record displays the three fields in an updated schema - NICK, ID, and NAME:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/OracleCDC-eventrecord.png)

In the list of record header attributes, notice the DDL statement that added the NICK field, the name of the updated table, and the ALTER event type.

### PEG Parser <a href="#ariaid-title24" id="ariaid-title24"></a>

The Oracle CDC Client origin provides an alternate PEG parser that you can try when concerned about pipeline performance.

The Oracle CDC Client origin provides a PEG parser as an alternate to the default parser. You can try the PEG parser when you feel that performance with the default parser is inadequate, as when processing very wide tables.

For more information about PEG processing, see [https://en.wikipedia.org/wiki/Parsing\_expression\_grammar](https://en.wikipedia.org/wiki/Parsing\_expression\_grammar).

To use the PEG parser, enable the Parse SQL Query property on the Oracle CDC tab, and then enable the Use PEG Parser property on the Advanced tab.

### Multithreaded Parsing <a href="#ariaid-title25" id="ariaid-title25"></a>

When you configure the origin to use local buffering and to parse the SQL query, you can configure the Oracle CDC Client origin to use multiple threads to parse transactions. You can use multithreaded parsing with both the default Oracle CDC Client parser and the alternate PEG parser.

When performing multithreaded parsing, the origin uses multiple threads to generate records from committed SQL statements in a transaction. It does not perform multithreaded processing of the resulting records.

The Oracle CDC Client origin uses multiple threads for parsing based on the Parse Thread Pool Size property. When you start the pipeline, the origin creates the number of threads specified in the property. The origin connects to Oracle, creates a LogMiner session, and processes a single transaction at a time.

When the origin processes a transaction, it reads and buffers all SQL statements in the transaction to an in-memory queue and waits for statements to be committed before processing them. Once committed, the SQL statements are parsed using all available threads and the original order of the SQL statements is retained.

The resulting records are passed to the rest of the pipeline. Note that enabling multithreaded parsing does not enable multithreaded processing – the pipeline uses a single thread for reading data.

### Working with the SQL Parser Processor <a href="#ariaid-title26" id="ariaid-title26"></a>

For very wide tables, those with hundreds of columns, reading the redo logs and parsing the SQL query using the Oracle CDC Client origin may take longer than expected and can cause the Oracle redo logs to be rotated out before they are read. When this happens, data is lost.

To avoid this, you can use multiple pipelines and the SQL Parser processor. The first pipeline contains the Oracle CDC Client and an intermediate endpoint. Configure the origin to not parse the SQL query. The second pipeline passes records from the intermediate endpoint to the SQL Parser processor to both parse the SQL query and to update the fields. Using this approach, the origin can read the redo logs without waiting for the SQL Parser to finish and therefore no data is lost.

The Oracle CDC Client continues to perform the following tasks:

* Reads the change data logs.
* Generates records that contain only the SQL query.
* Generates events.

The SQL Parser processor performs the following tasks:

* Parses the SQL query.
* Generates CDC and CRUD record header attributes.

### Working with the Drift Synchronization Solution <a href="#ariaid-title27" id="ariaid-title27"></a>

If you use the Oracle CDC Client origin as part of a Drift Synchronization Solution for Hive pipeline, make sure to pass only records flagged for Insert to the Hive Metadata processor.

The Drift Synchronization Solution for Hive enables automatic updates of Hive tables based on incoming data. The Hive Metadata processor, the first stage in the solution, expects Insert records only. Below are some recommended methods to ensure that the processor receives only Insert records:

* Configure the Oracle CDC Client to process only Insert records.
* If you want to process additional record types in the pipeline, use a Stream Selector to route only Insert records to the Hive Metadata processor.

### Data Preview with Oracle CDC Client <a href="#ariaid-title28" id="ariaid-title28"></a>

When using data preview with the Oracle CDC Client origin, you might need to increase the Preview Timeout data preview property.

By default, data preview waits 10,000 milliseconds, 10 seconds, to establish a connection to the origin system before timing out. Due to the complex nature of this origin, the initial startup can take longer than the default.

If data preview times out, try increasing the timeout property to 120,000 milliseconds. If preview continues to time out, increase the timeout incrementally.

### Configuring an Oracle CDC Client <a href="#ariaid-title29" id="ariaid-title29"></a>

Configure an Oracle CDC Client origin to process LogMiner change data capture information from an Oracle database.

Before you use the origin, complete the prerequisite tasks.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                |
    | Produce Events   | When the origin uses redo logs as the dictionary source, can generate event records when the origin reads DDL statements. Use for event handling.                                                                    |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul> |
2.  On the Oracle CDC tab, configure the following change data capture properties:

    | Oracle CDC Property                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Tables                               | <p>Tables to track. Specify related properties as needed.</p><p>Using simple or bulk edit mode, click the Add icon to define another table configuration.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Schema Name                          | <p>Schema to use. You can enter a schema name or use <a href="https://docs.oracle.com/database/121/SQLRF/conditions007.htm#SQLRF00501">SQL LIKE syntax</a> to specify a set of schemas.</p><p>The origin submits the schema name in all caps by default. To use a lower or mixed-case name, select the Case-Sensitive Names property.</p>                                                                                                                                                                                                                                                                                                                                                                                               |
    | Table Name Pattern                   | Table name pattern that specifies the tables to track. You can enter a table name or use [SQL LIKE syntax](https://docs.oracle.com/database/121/SQLRF/conditions007.htm#SQLRF00501) to specify a set of tables. The origin submits table names in all caps by default. To use lower or mixed-case names, select the Case-Sensitive Names property.                                                                                                                                                                                                                                                                                                                                                                                      |
    | Exclusion Pattern                    | <p>Optional table exclusion pattern to define a subset of tables to exclude. You can enter a table name or use a regular expression to specify a subset of tables to exclude.</p><p>To view and configure this option, click Show Advanced Options.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Case-Sensitive Names                 | <p>Submits case-sensitive schema, table, and column names. When not selected, the origin submits names in all caps.</p><p>Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.</p>                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Initial Change                       | <p>Starting point for the read. When you start the pipeline for the first time, the origin starts processing from the specified initial change. The origin only uses the specified initial change again when you reset the origin.</p><p>Use one of the following options:</p><ul><li>From Latest Change - Processes changes that arrive after you start the pipeline.</li><li>From Date - Processes changes starting from the specified date.Note: If the database operates in a different time zone from Data Pipelines, the DB Time Zone property must be set correctly to ensure that the appropriate changes are processed.</li><li>From SCN - Processes changes starting from the specified system change number (SCN).</li></ul> |
    | Start Date                           | <p>Datetime to read from when you start the pipeline. For a date-based initial change.</p><p>Use the following format: <code>DD-MM-YYYY HH24:MI:SS</code>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | Start SCN                            | <p>System change number to start reading from when you start the pipeline. If the SCN cannot be found in the redo logs, the origin continues reading from the next higher SCN that is available in the redo logs.</p><p>For an SCN-based initial change.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Operations                           | <p>Operations to include when creating records. All unlisted operations are ignored.</p><p>Operations are supported based on whether the Parse SQL Query property is selected. </p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Dictionary Source                    | <p>Location of the LogMiner dictionary:</p><ul><li>Redo logs - Use when schemas can change. Allows the origin to adapt to schema changes and to generate events for DDL statements.</li><li>Online catalog - Use for better performance when schemas are not expected to change.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Buffer Changes Locally               | <p>Determines the buffers that the origin uses. Select to use local Data Pipelines buffers. Clear to use Oracle LogMiner buffers. Generally, using local buffers will enhance pipeline performance.</p><p>By default, the origin uses local buffers.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Buffer Location                      | <p>Location of local buffers:</p><ul><li>In memory</li><li>To disk</li></ul><p>Before running the pipeline, note the local buffer resource requirements.</p><p>Available when using local buffers.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Discard Old Uncommitted Transactions | <p>Discards old uncommitted transactions at the end of a LogMiner session instead of processing them into error records.</p><p>Old uncommitted transactions are those that are older than the time specified for the Maximum Transaction Length property.</p><p>Available when using local buffers.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Unsupported Field Type               | <p>Action taken when the origin encounters unsupported data types in the record:</p><ul><li>Ignore and Send Record to Pipeline - The origin ignores unsupported data types and passes the record with only supported data types to the pipeline.</li><li>Send Record to Error - The origin handles the record based on the error record handling configured for the stage. The error record contains only the supported data types.</li><li>Discard Record - The origin discards the record.</li></ul>                                                                                                                                                                                                                                  |
    | Add Unsupported Fields to Records    | Includes fields with unsupported data types in the record. Includes the field names and the unparsed string values of the unsupported fields, when possible.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Include Nulls                        | Includes null values in records generated from full supplemental logging that include null values. By default, the origin generates a record without null values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Convert Timestamp To String          | <p>Enables the origin to write timestamps as string values rather than datetime values. Strings maintain the precision stored in the source database. For example, strings can maintain the precision of a high-precision Timestamp(6) field.</p><p>When writing timestamps to Data Pipelines date or time data types that do not store nanoseconds, the origin stores any nanoseconds from the timestamp in a field attribute.</p>                                                                                                                                                                                                                                                                                                     |
    | Maximum Transaction Length           | <p>Time in seconds to wait for changes for a transaction. Enter the longest period of time that you expect a transaction to require.</p><p>Transactions older than the specified time are treated as old uncommitted transactions each time a LogMiner session closes.</p><p>Default is <code>${ 1 * HOURS }</code> which is 3600 seconds.</p><p>Used for local buffers only.</p>                                                                                                                                                                                                                                                                                                                                                       |
    | LogMiner Session Window              | <p>Time in seconds to keep a LogMiner session open. Set to larger than the maximum transaction length. Reduce when not using local buffering to reduce LogMiner resource use.</p><p>Default is <code>${ 2 * HOURS }</code> which is 7200 seconds.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Parse SQL Query                      | <p>Parses SQL queries to generate records.</p><p>If set to false, the origin writes the SQL query to a <code>sql</code> field that can be parsed later by the SQL Parser processor.</p><p>By default, the origin parses the SQL queries.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Send Redo Query in Headers           | Includes the LogMiner redo query in the `oracle.cdc.query` record header attribute.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | DB Time Zone                         | Time zone of the database. Specify when the database operates in a different time zone from Data Pipelines.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
3.  On the JDBC tab, configure the following JDBC properties:

    | JDBC Property                            | Description                                                                                                                                                                                                                                                                                                                          |
    | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | JDBC Connection String                   | Connection string used to connect to the database.Note: If you include the JDBC credentials in the connection string, use the user account created for the origin. Common user accounts for Oracle and Oracle RAC 12c, 18c, and 19c multitenant databases start with `c##`. For more information, see Task 3. Create a User Account. |
    | Max Batch Size (records)                 | <p>Maximum number of records processed at one time. Honors values up to the Data Pipelines maximum batch size.</p><p>Default is 1000. The Data Pipelines default is 1000.</p>                                                                                                                                                        |
    | PDB                                      | <p>Name of the pluggable database that contains the schema you want to use. Use only when the schema was created in a pluggable database.</p><p>Required for schemas created in pluggable databases.</p>                                                                                                                             |
    | Use Credentials                          | Enables entering credentials on the Credentials tab. Use when you do not include credentials in the JDBC connection string.                                                                                                                                                                                                          |
    | Additional JDBC Configuration Properties | <p>Additional JDBC configuration properties to use. To add properties, click Add and define the JDBC property name and value.</p><p>Use the property names and values as expected by JDBC.</p>                                                                                                                                       |
4.  To enter JDBC credentials separately from the JDBC connection string, on the Credentials tab, configure the following properties:

    | Credentials Property | Description                                                                                                                                                                                                                                                               |
    | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Username             | <p>User name for the JDBC connection. Use the user account created for the origin. Common user accounts for Oracle and Oracle RAC 12c, 18c, and 19c multitenant databases start with <code>c##</code>.</p><p>For more information, see Task 3. Create a User Account.</p> |
    | Password             | <p>Password for the account.</p><p>Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores.</p>                                                                                                         |
5.  When using JDBC versions older than 4.0, on the Legacy Drivers tab, optionally configure the following properties:

    | Legacy Drivers Property      | Description                                                                                                  |
    | ---------------------------- | ------------------------------------------------------------------------------------------------------------ |
    | JDBC Class Driver Name       | Class name for the JDBC driver. Required for JDBC versions older than version 4.0.                           |
    | Connection Health Test Query | Optional query to test the health of a connection. Recommended only when the JDBC version is older than 4.0. |
6.  On the Advanced tab, optionally configure advanced properties.The defaults for these properties should work in most cases:

    | Advanced Property                  | Description                                                                                                                                                                                                                                                                                                                                                                                              |
    | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JDBC Fetch Size for Current Window | <p>Number of redo logs to fetch and process together when the LogMiner session includes the current time.</p><p>At the end of the session, the origin fetches and processes any remaining redo logs.</p><p>Set to 1 to make data available as soon as the origin receives a redo log.</p><p>Default is 1.</p>                                                                                            |
    | JDBC Fetch Size for Past Windows   | <p>Number of redo logs to fetch and process together when the LogMiner session is entirely in the past.</p><p>When the fetch size exceeds available redo logs, the origin fetches and processes the remaining redo logs. Lower fetch sizes can increase throughput when writes to the destination system are slow.</p><p>Default is 1.</p>                                                               |
    | Use PEG Parser                     | Enables use of the PEG parser instead of the default Oracle CDC Client origin parser. Can improve performance.                                                                                                                                                                                                                                                                                           |
    | Parse Thread Pool Size             | <p>Number of threads that the origin uses for multithreaded parsing. Available only when performing local buffering and parsing the SQL query.</p><p>Can be used with both the default parser and PEG parser.</p>                                                                                                                                                                                        |
    | Disable Continuous Mine            | Disables the LogMiner `CONTINUOUS_MINE` option. The `CONTINUOUS_MINE` option is available in Oracle and Oracle RAC 18c and earlier.                                                                                                                                                                                                                                                                      |
    | Maximum Pool Size                  | <p>Maximum number of connections to create.</p><p>Default is 1. The recommended value is 1.</p>                                                                                                                                                                                                                                                                                                          |
    | Minimum Idle Connections           | <p>Minimum number of connections to create and maintain. To define a fixed connection pool, set to the same value as Maximum Pool Size.</p><p>Default is 1.</p>                                                                                                                                                                                                                                          |
    | Connection Timeout (seconds)       | <p>Maximum time to wait for a connection. Use a time constant in an expression to define the time increment.Default is 30 seconds, defined as follows:</p><pre><code>${30 * SECONDS}
    </code></pre>                                                                                                                                                                                                       |
    | Idle Timeout (seconds)             | <p>Maximum time to allow a connection to idle. Use a time constant in an expression to define the time increment.</p><p>Use 0 to avoid removing any idle connections.</p><p>When the entered value is close to or more than the maximum lifetime for a connection, Data Pipelines ignores the idle timeout.</p><p>Default is 10 minutes, defined as follows:</p><pre><code>${10 * MINUTES}
    </code></pre> |
    | Max Connection Lifetime (seconds)  | <p>Maximum lifetime for a connection. Use a time constant in an expression to define the time increment.</p><p>Use 0 to set no maximum lifetime.</p><p>When a maximum lifetime is set, the minimum valid value is 30 minutes.</p><p>Default is 30 minutes, defined as follows:</p><pre><code>${30 * MINUTES}
    </code></pre>                                                                               |
    | Enforce Read-only Connection       | <p>Creates read-only connections to avoid any type of write.</p><p>Default is enabled. Disabling this property is not recommended.</p>                                                                                                                                                                                                                                                                   |
    | Transaction Isolation              | <p>Transaction isolation level used to connect to the database.</p><p>Default is the default transaction isolation level set for the database. You can override the database default by setting the level to any of the following:</p><ul><li>Read committed</li><li>Read uncommitted</li><li>Repeatable read</li><li>Serializable</li></ul>                                                             |
