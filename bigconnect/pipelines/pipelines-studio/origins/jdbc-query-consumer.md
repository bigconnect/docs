# JDBC Query Consumer

The JDBC Query Consumer origin reads database data using a user-defined SQL query through a JDBC connection. The origin returns data as a map with column names and field values.&#x20;

Data Pipelines includes database-specific origins, such as the Oracle Bulkload and Teradata Consumer origins. When available, StreamSets recommends using a database-specific origin. Data Pipelines also provides CDC origins to process changed data and the JDBC Multitable Consumer origin to perform database replication or to read from multiple tables in the same database.

The ability to process Microsoft SQL Server CDC data is deprecated in this origin and will be removed in a future release. To process data from Microsoft SQL Server CDC tables, use the SQL Server CDC Client origin. To process data from Microsoft SQL Server change tracking tables, use the SQL Server Change Tracking origin.

**Important**: This stage does not support connecting to non-RDBMS systems, including Hive, Impala, Kudu, or Snowflake. Support for untested systems is not guaranteed. For a list of tested systems, see "Database Vendors and Drivers".

When you configure the JDBC Query Consumer origin, you define the SQL query that the origin uses to read data from a single table or from a join of tables.

When you configure JDBC Query Consumer, you specify connection information, query interval, and custom JDBC configuration properties to determine how the origin connects to the database. You configure the query mode and SQL query to define the data returned by the database. When in full query mode and reading from certain databases, you can use a stored procedure instead of a SQL query. When the source database has high-precision timestamps, such as IBM Db2 TIMESTAMP(9) fields, you can configure the origin to write strings rather than datetime values to maintain the precision.

You can configure JDBC Query Consumer to perform change data capture for databases that store the information in a table. And you can specify what the origin does when encountering an unsupported data type: convert the data to string or stop the pipeline.

You can specify custom properties that your driver requires. You can configure advanced connection properties. To use a JDBC version older than 4.0, you specify the driver class name and define a health check query.

By default, the origin generates JDBC record header and field attributes that provide additional information about each record and field.

The origin can generate events for an event stream. For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

### Database Vendors and Drivers <a href="#ariaid-title2" id="ariaid-title2"></a>

The JDBC Query Consumer origin can read database data from multiple database vendors.

The following table lists the supported and tested database versions for this stage. You can use the stage with other JDBC-compliant databases, but full support is not guaranteed.&#x20;

| Database Vendor      | Supported Versions                                                                                                                                                                                                                                    | Tested Versions                                                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| MySQL                | MySQL 5.7 and later                                                                                                                                                                                                                                   | <ul><li>MySQL 5.7 with the MySQL Connector/J 8.0.12 driver</li><li>MySQL 8.0 with the MySQL Connector/J 8.0.12 driver</li></ul>     |
| Oracle               | <ul><li>Oracle 11g, 12c, 18c, 19c, and later</li><li>Oracle Real Application Clusters (RAC) 12c, 18c, 19c, and later</li></ul><p>Also supported:</p><ul><li>Hosted systems, such as AWS RDS</li><li>Derived systems, such as Oracle Exadata</li></ul> | <ul><li>Oracle 11g, 19c with the Oracle 19.3.0 JDBC driver</li><li>Oracle RAC 12c, 19c with the Oracle 19.3.0 JDBC driver</li></ul> |
| PostgreSQL           | PostgreSQL 9.x and later                                                                                                                                                                                                                              | <ul><li>PostgreSQL 9.6.9</li><li>PostgreSQL 10.4</li><li>PostgreSQL 11.7</li><li>PostgreSQL 12.2</li><li>PostgreSQL 13.0</li></ul>  |
| Microsoft SQL Server | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                                                                                                                                             | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                           |

#### MySQL Data Types <a href="#ariaid-title3" id="ariaid-title3"></a>

The JDBC Query Consumer origin converts MySQL data types into Data Pipelines data types.

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

#### Oracle Data Types <a href="#ariaid-title4" id="ariaid-title4"></a>

The JDBC Query Consumer origin converts Oracle data types into Data Pipelines data types.

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

#### PostgreSQL Data Types <a href="#ariaid-title5" id="ariaid-title5"></a>

The JDBC Query Consumer origin converts PostgreSQL data types into Data Pipelines data types.

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

#### SQL Server Data Types <a href="#ariaid-title6" id="ariaid-title6"></a>

The JDBC Query Consumer origin converts SQL Server data types into Data Pipelines data types.

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

### Installing the JDBC Driver <a href="#ariaid-title7" id="ariaid-title7"></a>

Before you use the JDBC Query Consumer origin, install the JDBC driver for the database. You cannot access the database until you install the required driver.Note: When connecting to a PostgreSQL or Microsoft SQL Server database, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for those databases.

### Offset Column and Offset Value <a href="#ariaid-title8" id="ariaid-title8"></a>

JDBC Query Consumer uses an offset column and initial offset value to determine where to start reading data within a table. Include both the offset column and the offset value in the WHERE clause of the SQL query.

The offset column must be a column in the table with unique non-null values, such as a primary key or indexed column. The initial offset value is a value within the offset column where you want the origin to start reading.

When the origin performs an incremental query, you must configure the offset column and offset value. For full queries, you can optionally configure them.

### Full and Incremental Mode <a href="#ariaid-title9" id="ariaid-title9"></a>

JDBC Query Consumer can perform queries in two modes:

**Incremental mode**

When the JDBC Query Consumer performs an incremental query, it uses the initial offset as the offset value in the first SQL query. As the origin completes processing the results of the first query, it saves the last offset value that it processes. Then it waits the specified query interval before performing a subsequent query.

When the origin performs a subsequent query, it returns data based on the last-saved offset. You can reset the origin to use the initial offset value.

Use incremental mode for append-only tables or when you do not need to capture changes to older rows. By default, the origin uses incremental mode.

**Full mode**

When the JDBC Query Consumer origin performs a full query, it runs the specified SQL query. If you optionally configure the offset column and initial offset value, the origin uses the initial offset as the offset value in the SQL query each time it requests data.

When the origin completes processing the results of the full query, it waits the specified query interval, and then performs the same query again.Use full mode to capture all row updates. You might use a Record Deduplicator in the pipeline to minimize repeated rows. Not ideal for large tables.

**Tip**: If you want to process the results from a single full query and then stop the pipeline, you can enable the origin to generate events and use the Pipeline Finisher to stop the pipeline automatically. For more information, see Event Generation.

When using full mode with certain databases, you can alternatively call a stored procedure instead of defining a SQL query.

### Recovery <a href="#ariaid-title10" id="ariaid-title10"></a>

JDBC Query Consumer supports recovery after a deliberate or unexpected stop when it performs incremental queries. Recovery is not supported for full queries.

In incremental mode, the origin uses offset values in the offset column to determine where to continue processing after a deliberate or unexpected stop. To ensure seamless recovery in incremental mode, use a primary key or indexed column as the offset column. As JDBC Query Consumer processes data, it tracks the offset value internally. When the pipeline stops, the origin notes where it stopped processing data. When you restart the pipeline, it continues from the last-saved offset.

When JDBC Query Consumer performs full queries, the origin runs the full query again after you restart the pipeline.

### SQL Query <a href="#ariaid-title11" id="ariaid-title11"></a>

The SQL query defines the data returned from the database. You define the query in the SQL Query property on the JDBC tab.

You can also define the query in a [runtime resource](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Pipeline\_Configuration/RuntimeValues.html#concept\_bs4\_5nm\_2s), and then use the `runtime:loadResource` function in the SQL Query property to load the query from the resource file at runtime. For example, you might enter the following expression for the property:

```
${runtime:loadResource("myquery.sql", false)}
```

When running the origin in full query mode and reading from certain databases, you can define a stored procedure, then call the stored procedure using the SQL Query property.

The SQL query guidelines that you use depend on whether you configure the origin to perform an incremental or full query.Note: Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.

#### SQL Query for Incremental Mode <a href="#ariaid-title12" id="ariaid-title12"></a>

When you define the SQL query for incremental mode, JDBC Query Consumer requires a WHERE and ORDER BY clause in the query.

Use the following guidelines when you define the WHERE and ORDER BY clauses in the query:

In the WHERE clause, include the offset column and the offset valueThe origin uses an offset column and value to determine the data that is returned. Include both in the WHERE clause of the query.Use the OFFSET constant to represent the offset valueIn the WHERE clause, use ${OFFSET} to represent the offset value.

For example, when you start a pipeline, the following query returns all data from the table where the data in the offset column is greater than the initial offset value:

```
SELECT * FROM <tablename> WHERE <offset column> > ${OFFSET}
```

Tip: When the offset values are strings, enclose ${OFFSET} in single quotation marks.In the ORDER BY clause, include the offset column as the first columnTo avoid returning duplicate data, use the offset column as the first column in the ORDER BY clause.

**Note**: Using column that is not a primary key or indexed column in the ORDER BY clause can slow performance.

For example, the following query for incremental mode returns data from an Invoice table where the ID column is the offset column. The query returns all data where the ID is greater than the offset and orders the data by the ID:

```
 SELECT * FROM invoice WHERE id > ${OFFSET} ORDER BY id
```

#### SQL Query for Full Mode <a href="#ariaid-title13" id="ariaid-title13"></a>

You can define any type of SQL query for full mode.

For example, you can run the following query to return all data from an Invoice table:

```
SELECT * FROM invoice
```

When you define the SQL query for full mode, you can optionally include the WHERE and ORDER BY clauses using the same guidelines as for incremental mode. However, using these clauses to read from large tables can cause performance issues.

#### Stored Procedure in Full Mode <a href="#ariaid-title14" id="ariaid-title14"></a>

When reading from certain databases, you can call a stored procedure from the JDBC Query Consumer origin. Currently, you can use stored procedures with MySQL, PostgreSQL, and SQL Server databases.

You can call a stored procedure when using JDBC Query Consumer in full mode. Using stored procedures in other modes is not supported.

To use a stored procedure complete the following tasks:

1. In your database, define the stored procedure.
2. In the origin, on the JDBC tab, configure the SQL Query property to call the stored procedure. Use the appropriate syntax for your database.
3. Also on the JDBC tab, clear the Incremental Mode property, which is selected by default.
4. Test the pipeline to ensure that the procedure performs as expected.

**Examples**

MySQL databaseTo read all data from a MySQL table, you might create a stored procedure as follows:

```
CREATE PROCEDURE <procedure_name>()
BEGIN
SELECT * FROM <table_name>;
END;
```

Then, on the JDBC tab, you clear the Incremental Mode property and enter the following in the SQL Query property to call the procedure:

```
"CALL <procedure_name>()"
```

PostgreSQL databaseTo read all data from a PostgreSQL table, you might create a stored procedure as follows:

```
create or replace function <procedure_name>()
	returns table (id int)
	language plpgsql
    as $$
    begin
	return query 
	select * from <table_name>;
    end;$$
```

Then, on the JDBC tab, you clear the Incremental Mode property and enter the following in the SQL Query property to call the procedure:

```
"SELECT * FROM <procedure_name>()"
```

SQL Server databaseTo read all data from a SQL Server table, you might create a stored procedure as follows:

```
CREATE PROCEDURE <procedure_name>
AS
    SELECT * FROM <table_name>
RETURN
```

Then, on the JDBC tab, you clear the Incremental Mode property and enter the following in the SQL Query property to call the procedure:

```
"EXEC <procedure_name>"
```

### JDBC Attributes <a href="#ariaid-title15" id="ariaid-title15"></a>

The JDBC Query Consumer origin generates record header attributes and field attributes that provide additional information about each record and field.

The origin receives these details from the JDBC driver.

#### JDBC Header Attributes <a href="#ariaid-title16" id="ariaid-title16"></a>

By default, the JDBC Query Consumer generates JDBC record header attributes that provide additional information about each record, such as the original data type of a field or the source tables for the record. The origin receives these details from the JDBC driver.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes. For more information about working with record header attributes, see Working with Header Attributes.

JDBC header attributes include a user-defined prefix to differentiate the JDBC header attributes from other record header attributes. By default, the prefix is `jdbc`.

You can change the prefix that the origin uses and you can configure the origin not to create JDBC header attributes using the Create JDBC Header Attributes and JDBC Header Prefix properties on the Advanced tab.

The origin can provide the following JDBC header attributes:

| JDBC Header Attribute                   | Description                                                                                                                                                                                                                                                                                                                                   |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \<JDBC prefix>.tables                   | <p>Provides a comma-separated list of source tables for the fields in the record.Note: Not all JDBC drivers provide this information.</p><p>Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.</p> |
| \<JDBC prefix>.\<column name>.jdbcType  | Provides the numeric value of the original SQL data type for each field in the record. See the [Java documentation](https://docs.oracle.com/javase/8/docs/api/constant-values.html#java.sql.Types.ARRAY) for a list of the data types that correspond to numeric values.                                                                      |
| \<JDBC prefix>.\<column name>.precision | Provides the original precision for all numeric and decimal fields.                                                                                                                                                                                                                                                                           |
| \<JDBC prefix>.\<column name>.scale     | Provides the original scale for all numeric and decimal fields.                                                                                                                                                                                                                                                                               |

**Header Attributes with the Drift Synchronization Solution**

When you use the JDBC Query Consumer with the Drift Synchronization Solution, ensure that the origin creates JDBC header attributes.

JDBC header attributes allow the Hive Metadata processor to use the precision and scale information in the attributes to define decimal fields.

To enable the Hive Metadata processor to define decimal fields as needed, perform the following steps:

1. In the JDBC Query Consumer, on the Advanced tab, make sure that Create JDBC Header Attributes is selected.
2. On the same tab, you can optionally configure JDBC Header Prefix.
3.  In the Hive Metadata processor, if necessary, configure the Decimal Precision Expression and Decimal Scale Expression properties on the Hive tab.

    If you changed the default value for JDBC Header Prefix in the JDBC Query Consumer, then update the "jdbc." string in the expressions to use the correct JDBC Header Prefix.

    If you did not change the JDBC Header Prefix default value, then use the default expressions for the properties.

#### JDBC Field Attributes <a href="#ariaid-title18" id="ariaid-title18"></a>

The JDBC Query Consumer origin generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

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

### CDC for Microsoft SQL Server <a href="#ariaid-title19" id="ariaid-title19"></a>

Important: This functionality is deprecated and will be removed in a future release. To process data from Microsoft SQL Server CDC tables, use the SQL Server CDC Client origin. To process data from Microsoft SQL Server change tracking tables, use the SQL Server Change Tracking origin.

You can use the JDBC Query Consumer to process change capture data from Microsoft SQL Server.

To process Microsoft SQL Server changed capture data, perform the following tasks:

1. In the JDBC Query Consumer origin, on the JDBC tab, make sure Incremental Mode is enabled.
2.  Configure the Offset Column property to use \_\_$start\_lsn.

    Microsoft SQL Server uses \_$start\_lsn as the offset column in change data capture tables.
3.  Configure the Initial Offset property.

    This determines where the origin starts the read when you start the pipeline. To read all available data, set it to 0.
4.  Configure the SQL Query property:

    * In the SELECT statement, use the CDC table name.
    * In the WHERE clause, use \_\_$start\_lsn as the offset column, and since \_\_$start\_lsn stores the offset in binary format, add a command to convert the integer offset to Binary(10).
    * In the ORDER BY clause, use \_\_$start\_lsn as the offset column and optionally specify reading in ascending or descending order. By default, the origin reads in ascending order.

    The following query summarizes these points:

    ```
    SELECT * from <CDC table name>
    WHERE __$start_lsn > CAST(0x${OFFSET} as binary(10))
    ORDER BY __$start_lsn <ASC | DESC>
    ```
5. If you want to group row updates from the same transaction, configure the properties on the Change Data Capture tab:
   * For the Transaction ID Column Name use \_\_$start\_lsn. The \_\_$start\_lsn column includes transaction information in the offset.
   * Set the Max Transaction Size. This property overrides the Data Pipelines maximum batch size. For more information about both of these properties, see Group Rows by Transaction.

#### CRUD Record Header Attribute <a href="#ariaid-title20" id="ariaid-title20"></a>

When reading change capture data from Microsoft SQL Server, the JDBC Query Consumer origin includes the CRUD operation type in the sdc.operation.type record header attribute.

If you use a CRUD-enabled destination in the pipeline such as JDBC Producer or Elasticsearch, the destination can use the operation type when writing to destination systems. When necessary, you can use an Expression Evaluator or scripting processors to manipulate the value in the `sdc.operation.type` header attribute. For an overview of Data Pipelines changed data processing and a list of CRUD-enabled destinations, see Processing Changed Data.

The origin uses the following values in the sdc.operation.type record header attribute to represent the operation type:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE
* 5 for unsupported codes

#### Group Rows by Transaction <a href="#ariaid-title21" id="ariaid-title21"></a>

When reading from Microsoft SQL Server, JDBC Query Consumer can group row updates from the same transaction when reading from a change log table. This maintains consistency when performing change data capture.

To enable this feature, specify the transaction ID column and maximum transaction size. When these properties are defined, JDBC Query Consumer processes data as a batch up to the maximum transaction size, overriding the Data Pipelines maximum batch size.

When the transaction is larger than the maximum transaction size, JDBC Query Consumer uses multiple batches as needed.

To preserve transactional integrity, increase the maximum transaction size as necessary. Note that setting this property too high can cause out of memory errors.

### Event Generation <a href="#ariaid-title22" id="ariaid-title22"></a>

The JDBC Query Consumer origin can generate events that you can use in an event stream. When you enable event generation, the origin generates an event when it completes processing the data returned by the specified query. The origin also generates an event when a query completes successfully and when it fails to complete.

JDBC Query Consumer events can be used in any logical way. For example:

*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin processes data based on how you configured the origin. For example, if you configure the origin to run in incremental mode, the origin saves the offset when the executor stops the pipeline. When it restarts, the origin continues processing from the last-saved offset. In contrast, if you configure the origin to run in full mode, when you restart the pipeline, the origin uses the initial offset, if specified.

    For an example, see Stopping a Pipeline After Processing All Available Data.
*   With the Email executor to send a custom email after receiving an event.

    For an example, see Sending Email During Pipeline Processing.
*   With a destination to store information about completed queries.

    For an example, see Preserving an Audit Trail of Events.

For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

#### Event Record <a href="#ariaid-title23" id="ariaid-title23"></a>

Event records generated by JDBC Query Consumer origin have the following event-related record header attributes:

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                                               |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>no-more-data - Generated when the origin completes processing all data returned by a query.</li><li>jdbc-query-success - Generated when the origin successfully completes a query.</li><li>jdbc-query-failure - Generated when the origin fails to complete a query.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                                              |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                                         |

The origin can generate the following types of event records:No-more-dataThe origin generates a no-more-data event record when it completes processing all data returned by a query.No-more-data event records generated by the origin have the `sdc.event.type` set to `no-more-data` and include the following field:

| Event Record Field | Description                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------- |
| record-count       | Number of records successfully generated since the pipeline started or since the last no-more-data event was created. |

Query successThe origin generates a query success event record when it completes processing the data returned from a query.The query success event records have the `sdc.event.type` record header attribute set to `jdbc-query-success` and include the following fields:

| Field         | Description                         |
| ------------- | ----------------------------------- |
| query         | Query that completed successfully.  |
| timestamp     | Timestamp when the query completed. |
| row-count     | Number of processed rows.           |
| source-offset | Offset after the query completed.   |

Query failureThe origin generates a query failure event record when it fails to complete processing the data returned from a query.The query failure event records have the `sdc.event.type` record header attribute set to jdbc-query-failure and include the following fields:

| Field         | Description                                           |
| ------------- | ----------------------------------------------------- |
| query         | Query that failed to complete.                        |
| timestamp     | Timestamp when the query failed to complete.          |
| row-count     | Number of records from the query that were processed. |
| source-offset | Origin offset after query failure.                    |
| error         | First error message.                                  |

### Configuring a JDBC Query Consumer <a href="#ariaid-title24" id="ariaid-title24"></a>

Configure a JDBC Query Consumer origin to use a single configured SQL query to read database data through a JDBC connection.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                |
    | Produce Events   | Generates event records when events occur. Use for event handling.                                                                                                                                                   |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul> |
2.  On the JDBC tab, configure the following properties:

    | JDBC Property                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JDBC Connection String                   | <p>Connection string used to connect to the database. Use the connection string format required by the database vendor.</p><p>For example, use the following formats for these database vendors:</p><ul><li>MySQL - <code>jdbc:mysql://&#x3C;host>:&#x3C;port>/&#x3C;database_name></code></li><li>Oracle - <code>jdbc:oracle:&#x3C;driver_type>:@&#x3C;host>:&#x3C;port>:&#x3C;service_name></code></li><li>PostgreSQL - <code>jdbc:postgresql://&#x3C;host>:&#x3C;port>/&#x3C;database_name></code></li><li>SQL Server - <code>jdbc:sqlserver://&#x3C;host>:&#x3C;port>;databaseName=&#x3C;database_name></code></li></ul><p>You can optionally include the user name and password in the connection string.</p>                    |
    | SQL Query                                | <p>SQL query to use when reading data from the database. you can use several methods to specify the query:</p><ul><li>Define the query in the property.</li><li>Define the query in a runtime resource, and then use the <code>runtime:loadResource</code> function in the property to load the query from the resource file at runtime.</li><li>When in full query mode and reading from certain databases, you can define a stored procedure in the database, then call the stored procedure from the property.</li></ul><p>Note: Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.</p> |
    | Initial Offset                           | <p>Offset value to use when the pipeline starts. When you start the pipeline for the first time, the origin starts processing from the specified initial offset. The origin only uses the specified initial offset again when you reset the origin.</p><p>Required in incremental mode.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Offset Column                            | <p>Column to use for the offset value.</p><p>As a best practice, an offset column should be an incremental and unique column that does not contain null values. Having an index on this column is strongly encouraged since the underlying query uses an ORDER BY and inequality operators on this column.</p><p>Required in incremental mode.</p>                                                                                                                                                                                                                                                                                                                                                                                    |
    | Incremental Mode                         | <p>Defines how JDBC Query Consumer queries the database. Select to perform incremental queries. Clear to perform full queries.</p><p>To process CDC data from Microsoft SQL Server, select this option. For more information about CDC for Microsoft SQL Server, see CDC for Microsoft SQL Server.</p><p>Default is incremental mode.</p>                                                                                                                                                                                                                                                                                                                                                                                             |
    | Use Credentials                          | Enables entering credentials on the Credentials tab. Use when you do not include credentials in the JDBC connection string.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Root Field Type                          | Root field type to use for generated records. Use the default List-Map option unless using the origin in a pipeline built with Data Pipelines version 1.1.0 or earlier.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Query Interval                           | <p>Amount of time to wait between queries. Enter an expression based on a unit of time. You can use SECONDS, MINUTES, or HOURS.</p><p>Default is 10 seconds: ${10 * SECONDS}.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Max Batch Size (records)                 | Maximum number of records to include in a batch.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Max Clob Size (characters)               | Maximum number of characters to be read in a Clob field. Larger data is truncated.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Max Blob Size (bytes)                    | Maximum number of bytes to be read in a Blob field.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Number of Retries on SQL Error           | <p>Number of times the origin tries to execute the query after receiving an SQL error. After retrying this number of times, the origin handles the error based on the error handling configured for the origin.</p><p>Use to handle transient network or connection issues that prevent the origin from submitting a query.</p><p>Default is 0.</p>                                                                                                                                                                                                                                                                                                                                                                                   |
    | Convert Timestamp To String              | <p>Enables the origin to write timestamps as string values rather than datetime values. Strings maintain the precision stored in the source database. For example, strings can maintain the precision of a high-precision IBM Db2 TIMESTAMP(9) field.</p><p>When writing timestamps to Data Pipelines date or time data types that do not store nanoseconds, the origin stores any nanoseconds from the timestamp in a field attribute.</p>                                                                                                                                                                                                                                                                                           |
    | Additional JDBC Configuration Properties | <p>Additional JDBC configuration properties to use. To add properties, click Add and define the JDBC property name and value.</p><p>Use the property names and values as expected by JDBC.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
3.  If you configured the origin to enter JDBC credentials separately from the JDBC connection string on the JDBC tab, then configure the following properties on the Credentials tab:

    | Credentials Property | Description                                                                                                                                                |
    | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Username             | <p>User name for the JDBC connection.</p><p>The user account must have the correct permissions or privileges in the database.</p>                          |
    | Password             | Password for the JDBC user name.Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores. |
4.  To process change capture data from Microsoft SQL Server, on the Change Data Capture tab, optionally configure the following properties to group rows by transaction:

    | Change Data Capture Property | Description                                                                                                 |
    | ---------------------------- | ----------------------------------------------------------------------------------------------------------- |
    | Transaction ID Column Name   | Transaction ID column name, typically `__$start_lsn`.                                                       |
    | Max Transaction Size (rows)  | <p>Maximum number of rows to include in a batch.</p><p>Overrides the Data Pipelines maximum batch size.</p> |
5.  When using JDBC versions older than 4.0, on the Legacy Drivers tab, optionally configure the following properties:

    | Legacy Drivers Property      | Description                                                                                                  |
    | ---------------------------- | ------------------------------------------------------------------------------------------------------------ |
    | JDBC Class Driver Name       | Class name for the JDBC driver. Required for JDBC versions older than version 4.0.                           |
    | Connection Health Test Query | Optional query to test the health of a connection. Recommended only when the JDBC version is older than 4.0. |
6.  On the Advanced tab, optionally configure advanced properties.The defaults for these properties should work in most cases:

    | Advanced Property                 | Description                                                                                                                                                                                                                                                                                                                                                                                              |
    | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Maximum Pool Size                 | <p>Maximum number of connections to create.</p><p>Default is 1. The recommended value is 1.</p>                                                                                                                                                                                                                                                                                                          |
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
    | Create JDBC Header Attributes     | Adds JDBC header attributes to records. The origin creates JDBC header attributes by default.Note: When using the origin with a Drift Synchronization Solution, make sure this property is selected.                                                                                                                                                                                                     |
    | JDBC Header Prefix                | Prefix for JDBC header attributes.                                                                                                                                                                                                                                                                                                                                                                       |
    | Disable Query Validation          | Disables the query validation that occurs by default. Use to avoid time consuming query validation situations.Warning: Query validation prevents running a pipeline with invalid queries. Use this option with care.                                                                                                                                                                                     |
    | On Unknown Type                   | <p>Action to take when encountering an unsupported data type:</p><ul><li>Stop Pipeline - Stops the pipeline after completing processing the previous records.</li><li>Convert to String - When possible, converts the data to string and continues processing.</li></ul>                                                                                                                                 |
