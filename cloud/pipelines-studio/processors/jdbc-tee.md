# JDBC Tee

The JDBC Tee processor uses a JDBC connection to write data to a MySQL or PostgreSQL database table, and then pass generated database column values to fields.&#x20;

Use the JDBC Tee processor to write some or all record fields to a database table and then enrich records with additional data.

Important: Use this processor only with MySQL or PostgreSQL databases.

To use the processor, the database table must be configured to generate column values as data is inserted.

When you configure the JDBC Tee processor, you specify connection information to the MySQL or PostgreSQL database, table name, and optionally define field mappings. By default, the processor writes data to the table based on the matching field names. You can override the default field mappings by defining specific mappings.

You define generated column mappings to specify the output fields to pass the generated database column values to.

You can configure the stage to rollback an entire batch if an error occurs while writing part of the batch. You can also configure custom properties that your driver requires.

The JDBC Tee processor can use CRUD operations defined in the `sdc.operation.type` record header attribute to write data. You can define a default operation for records without the header attribute or value. You can also configure whether to use multi-row operations for inserts and deletes, and how to handle records with unsupported operations.

When processing data from a CDC-enabled origin, you can specify the origin change log to aid record processing. For information about Data Pipelines change data processing and a list of CDC-enabled origins, see Processing Changed Data.

To use a JDBC version older than 4.0, you can specify the driver class name and define a health check query.

### Example

Let's assume that you are processing customer orders. You have a customer database table with an `ID` column as the primary key. The customer table is configured to generate a sequential number for the `ID` column as each row is inserted into the table. For example, the first customer row is assigned an `ID` of 001, and the second is assigned an `ID` of 002.

When you process a new customer’s order, the JDBC Tee processor inserts the customer data to the customer table and the database returns the generated ID for that customer. The JDBC Tee processor passes the generated ID value to a new `cust_ID` field in the record. The processor passes all record fields to the next stage in the pipeline for additional processing.

The following image displays a high-level overview of how the stage processes our customer order example:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/JDBCTee.png)

### Database Vendors and Drivers <a href="#ariaid-title2" id="ariaid-title2"></a>

The JDBC Tee processor can write data to a MySQL or PostgreSQL database.

The following table lists the supported and tested vendors, versions, and drivers:

| Database Vendor | Supported Version        | Tested Version                                                                                                                     |
| --------------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| MySQL           | MySQL 5.7 and later      | <ul><li>MySQL 5.7 with the MySQL Connector/J 8.0.12 driver</li><li>MySQL 8.0 with the MySQL Connector/J 8.0.12 driver</li></ul>    |
| PostgreSQL      | PostgreSQL 9.x and later | <ul><li>PostgreSQL 9.6.9</li><li>PostgreSQL 10.4</li><li>PostgreSQL 11.7</li><li>PostgreSQL 12.2</li><li>PostgreSQL 13.0</li></ul> |

#### MySQL Data Types <a href="#ariaid-title3" id="ariaid-title3"></a>

The JDBC Tee processor converts MySQL data types into Data Pipelines data types.

The processor supports the following MySQL data types:

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

#### PostgreSQL Data Types <a href="#ariaid-title4" id="ariaid-title4"></a>

The JDBC Tee processor converts PostgreSQL data types into Data Pipelines data types.

The processor supports the following PostgreSQL data types:

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

### Installing the JDBC Driver <a href="#ariaid-title5" id="ariaid-title5"></a>

Before you use the JDBC Tee processor, install the JDBC driver for the database. You cannot access the database until you install the required driver.Note: When connecting to a PostgreSQL database, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for PostgreSQL.

### Define the CRUD Operation <a href="#ariaid-title6" id="ariaid-title6"></a>

The JDBC Tee processor can insert, update, or delete data. The processor writes the records based on the CRUD operation defined in a CRUD operation header attribute or in operation-related stage properties.

You can define the CRUD operation in the following ways:CRUD operation header attributeYou can define the CRUD operation in a CRUD operation record header attribute. The processor looks for the CRUD operation to use in the sdc.operation.type record header attribute.The attribute can contain one of the following numeric values:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE

If your pipeline includes a CRUD-enabled origin that processes changed data, the processor simply reads the operation type from the sdc.operation.type header attribute that the origin generates. If your pipeline uses a non-CDC origin, you can use the Expression Evaluator or a scripting processor to define the record header attribute. For more information about Data Pipelines changed data processing and a list of CDC-enabled origins, see Processing Changed Data.

Operation stage propertiesYou define a default operation in the processor properties. The processor uses the default operation when the sdc.operation.type record header attribute is not set.You can also define how to handle records with unsupported operations defined in the sdc.operation.type header attribute. The processor can discard them, send them to error, or use the default operation.

### Single and Multi-row Operations <a href="#ariaid-title7" id="ariaid-title7"></a>

The JDBC Tee processor performs single-row operations by default. That is, it executes a SQL statement for each record. When supported by the destination database, you can configure the JDBC Tee processor to perform multi-row operations. Depending on the sequence of the data, multi-row operations can improve pipeline performance.

When performing multi-row operations, the JDBC Tee processor creates a single SQL statement for sequential insert rows and for sequential delete rows. The processor does not perform multi-row update operations. You can configure the Statement Parameter Limit property to limit the number of parameters in an insert operation - that is, you can limit the number of records included in an insert statement.

For example, say the pipeline generates three insert records, followed by two update records, and four delete records. If you enable multi-row operations and do not set a statement parameter limit, the JDBC Tee processor generates a single insert SQL statement for the three insert records, two update statements - one for each of the update records, and a single delete statement for the four delete records. On the other hand, if you enable multi-row operations and set the statement parameter limit to two, the JDBC Tee processor generates two insert SQL statements - one for two insert records and one for the third insert record, two update statements - one for each of the update records, and a single delete statement for the four delete records.

Important: Before enabling multi-row operations, verify that the database supports the SQL statements that the JDBC Tee processor uses.

Error handling for multi-row operations depends on the database. If the database reports the individual record that causes an error in a multi-row statement, the stage sends that record to the error stream. If the database does not report which record causes an error, the stage sends all the records from the statement to the error stream.

For multi-row inserts, the JDBC Tee processor uses the following SQL statement:

```
INSERT INTO <table name> (<col1>, <col2>, <col3>) 
     VALUES (<record1 field1>,<record1 field2>,<record1 field3>), 
     (<r2 f1>,<r2 f2>,<r2 f3>), (<r3 f1>,<r3 f2>,<r3 f3>),...;
```

For multi-row deletes, the processor uses the following SQL statement for tables with a single primary key:

```
DELETE FROM <table name> WHERE <primary key> IN (<key1>, <key2>, <key3>,...);
```

For multi-row deletes, the processor uses the following SQL statement for tables with multiple primary keys:

```
DELETE FROM <table name> WHERE (<pkey1>, <pkey2>, <pkey3>)
      IN ((<key1-1>, <key1-2>, <key1-3>),(<key2-1>, <key2-2>, <key2-2>),...);
```

### Configuring a JDBC Tee Processor <a href="#ariaid-title8" id="ariaid-title8"></a>

Configure a JDBC Tee processor to write data to a MySQL or PostgreSQL database table and enrich records with data from generated database columns.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the JDBC tab, configure the following properties:

    | JDBC Property                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JDBC Connection String                   | <p>Connection string used to connect to the database. Use the connection string format required by the database vendor.</p><p>For example, use the following formats for these database vendors:</p><ul><li>MySQL - <code>jdbc:mysql://&#x3C;host>:&#x3C;port>/&#x3C;database_name></code></li><li>PostgreSQL - <code>jdbc:postgresql://&#x3C;host>:&#x3C;port>/&#x3C;database_name></code></li></ul><p>You can optionally include the user name and password in the connection string.</p>                                                                                                                                                                                                                                                                                                                                                        |
    | Table Name                               | <p>Database table to write to. Some databases require a fully-qualified table name, such as <code>&#x3C;schema | database>:&#x3C;tablename></code>. Use the table name format required by the database.Enter one of the following:</p><ul><li>Name of an existing database table.</li><li><p>Expression that evaluates to the name of an existing database table. For example, if the table name is stored in the "tableName" record attribute, enter the following expression:</p><pre><code>${record:attribute('tableName')}
    </code></pre></li></ul><p>The processor creates the table if it does not exist.</p>                                                                                                                                                                                                                                 |
    | Field to Column Mapping                  | <p>Use to override the default field to column mappings. By default, fields are written to columns of the same name.When you override the mappings, you can define parameterized values to apply SQL functions to the field values before writing them to columns. For example, to convert a field value to an integer, enter the following for the parameterized value:</p><pre><code>CAST(? AS INTEGER)
    </code></pre><p>The question mark (?) is substituted with the value of the field. Leave the default value of ? if you do not need to apply a SQL function.</p><p>Using <a href="https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Pipeline_Configuration/SimpleBulkEdit.html#concept_alb_b3y_cbb">simple or bulk edit mode</a>, click the Add icon to create additional field to column mappings.</p> |
    | Generated Column Mappings                | <p>Map the generated database columns to fields in the record. Enter the following:</p><ul><li>Column Name. Name of the database column that contains the generated value. Enter a column name or enter an expression that defines the column.</li><li>SDC Field. Name of the field in the record that receives the generated column value. You can specify an existing field or a new field. If the field does not exist, JDBC Tee creates the field.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                   |
    | Change Log Format                        | Format of change capture data. Use when processing change capture data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Default Operation                        | Default CRUD operation to perform if the `sdc.operation.type` record header attribute is not set.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Unsupported Operation Handling           | <p>Action to take when the CRUD operation type defined in the <code>sdc.operation.type</code> record header attribute is not supported:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Use Default Operation - Writes the record to the destination system using the default operation.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Use Multi-Row Operation                  | <p>Combines sequential insert operations and sequential delete operations into single statements. Select to insert and delete multiple records at the same time. Before enabling this option, verify that the database supports the multi-row SQL statements used by the stage.</p><p>By default, the stage performs single-row operations.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Statement Parameter Limit                | <p>Number of parameters allowed in the prepared statement for multi-row inserts.</p><p>Use -1 to disable the parameter limit. Default is -1.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Rollback Batch On Error                  | Rolls back the entire batch when an error occurs within the batch.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Use Credentials                          | Enables entering credentials on the Credentials tab. Use when you do not include credentials in the JDBC connection string.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Additional JDBC Configuration Properties | <p>Additional JDBC configuration properties to use. To add properties, click Add and define the JDBC property name and value.</p><p>Use the property names and values as expected by JDBC.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
3.  If you configured the origin to enter JDBC credentials separately from the JDBC connection string on the JDBC tab, then configure the following properties on the Credentials tab:

    | Credentials Property | Description                                                                                                                       |
    | -------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
    | Username             | <p>User name for the JDBC connection.</p><p>The user account must have the correct permissions or privileges in the database.</p> |
    | Password             | Password for the JDBC user name.                                                                                                  |
4.  When using JDBC versions older than 4.0, on the Legacy Drivers tab, optionally configure the following properties:

    | Legacy Drivers Property      | Description                                                                                                  |
    | ---------------------------- | ------------------------------------------------------------------------------------------------------------ |
    | JDBC Class Driver Name       | Class name for the JDBC driver. Required for JDBC versions older than version 4.0.                           |
    | Connection Health Test Query | Optional query to test the health of a connection. Recommended only when the JDBC version is older than 4.0. |
5.  On the Advanced tab, optionally configure advanced properties.The defaults for these properties should work in most cases:

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
    | Transaction Isolation             | <p>Transaction isolation level used to connect to the database.</p><p>Default is the default transaction isolation level set for the database. You can override the database default by setting the level to any of the following:</p><ul><li>Read committed</li><li>Read uncommitted</li><li>Repeatable read</li><li>Serializable</li></ul>                                                             |
    | Init Query                        | <p>SQL query to perform immediately after the stage connects to the database. Use to set up the database session as needed.</p><p>For example, the following query sets the time zone for the session for a MySQL database: <code>SET time_zone = timezone;</code></p>                                                                                                                                   |
