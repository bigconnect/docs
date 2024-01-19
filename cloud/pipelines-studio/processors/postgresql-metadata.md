# PostgreSQL Metadata

The PostgreSQL Metadata processor determines the PostgreSQL table where each record should be written, compares the record structure against the table structure, then creates or alters the table as needed.

Use the PostgreSQL Metadata processor as part of the Drift Synchronization Solution for PostgreSQL.

When processing data, the PostgreSQL Metadata processor uses a table name expression to determine the name of the target table to use for each record. If the target table is not in the processor's cache, the processor queries the database for table information and caches the results. When the target table is in the cache, the processor compares the record structure against cached table structure.

When a record includes fields that do not exist in the table, the PostgreSQL Metadata processor alters the table as needed, then updates the table information in the cache. When a record should be written to a table that does not exist, the processor creates the table based on the fields in the record.

Like other database-related stages, when you configure the PostgreSQL Metadata processor, you can specify custom JDBC properties, enter connection credentials, and configure advanced properties such as an initial query and timeouts.

For more information about the Drift Synchronization Solution for PostgreSQL and a case study, see Drift Synchronization Solution for PostgreSQL.

Support for additional databases is planned for future releases.&#x20;

### JDBC Driver <a href="#ariaid-title2" id="ariaid-title2"></a>

When connecting to a PostgreSQL database, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for PostgreSQL.

### Schema and Table Names <a href="#ariaid-title3" id="ariaid-title3"></a>

When you configure the schemas and tables where records should be written, you can use the actual schema and table names or expressions that resolve to the schemas and tables to use.

Use names when all data can be written to the same schema or table. Use expressions to use information in the record to determine the schema or table to write to.

For example, the JDBC Multitable Consumer origin writes the originating table name in the `jdbc.tables` record header attribute. If you want to write records to tables of the same name, you can use `${record:attribute('jdbc.tables')}` for the table name property.

Similarly, the JDBC Query Consumer writes the originating table name in a `<user-defined prefix>.tables` record header attribute when the origin is configured to create record header attributes. So if you want to write records to tables of the same name, you can use `${record:attribute('<user defined prefix>.tables')}` for the table name property.

Tips for schema and table name expressions:

* If all records are to be written to a single schema or table, you can enter the schema or table name instead of an expression.
* If the schema or table name can be extrapolated from record data or header attributes, you can use an expression that evaluates to the schema or table name.
* When necessary, you can use an Expression Evaluator earlier in the pipeline to perform calculations and write the results to a new field or a header attribute. Then, configure the PostgreSQL Metadata processor to use that information.

### Decimal Precision and Scale Field Attributes <a href="#ariaid-title4" id="ariaid-title4"></a>

Use the Decimal Precision Attribute and Decimal Scale Attribute properties to specify the precision and scale for the Decimal columns that the PostgreSQL Metadata processor creates.

While other data types have hard coded definitions that the processor uses to create columns in database tables, Decimal columns require a specified precision and scale.

When processing data from the JDBC Query Consumer or JDBC Multitable Consumer origins, use the default attribute names, "precision" and "scale". Both origins store the precision and scale of Decimal columns in "precision" and "scale" field attributes for each Decimal field.

When processing data from other origins, you can use the Expression Evaluator processor earlier in the pipeline to create precision and scale field attributes for Decimal fields.

### Caching Information <a href="#ariaid-title5" id="ariaid-title5"></a>

When processing records, the PostgreSQL Metadata processor queries the database for the necessary table information and caches the results. After creating or altering a table, it updates the table information in the cache. The processor uses the cache for record comparison when possible, to avoid unnecessary queries.

Important: Do not alter any table that might be used by the pipeline while the pipeline runs. Since the PostgreSQL Metadata processor caches information about table structures and creates and alters tables, the processor must have accurate information about the tables.

### PostgreSQL Data Types <a href="#ariaid-title6" id="ariaid-title6"></a>

The following table lists Data Pipelines data types and the corresponding PostgreSQL data types. The PostgreSQL Metadata processor uses these conversions when generating CREATE TABLE and ALTER TABLE statements.

| Data Pipelines Data Type | PostgreSQL Data Type        |
| ------------------------ | --------------------------- |
| Boolean                  | Boolean                     |
| Byte Array               | Bytea                       |
| Char                     | Character                   |
| Date                     | Date                        |
| Datetime                 | Timestamp without time zone |
| Decimal                  | Numeric                     |
| Double                   | Double precision            |
| Float                    | Real                        |
| Integer                  | Integer                     |
| Long                     | Bigint                      |
| List                     | Not supported               |
| List-Map                 | Not supported               |
| Map                      | Not supported               |
| Short                    | Smallint                    |
| String                   | Character varying           |
| Time                     | Time without time zone      |
| Zoned Datetime           | Timestamp with time zone    |

### Configuring a PostgreSQL Metadata Processor <a href="#ariaid-title7" id="ariaid-title7"></a>

Configure a PostgreSQL Metadata processor as part of the Drift Synchronization Solution for PostgreSQL.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the JDBC tab, configure the following properties:

    | JDBC Property                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JDBC Connection String                   | <p>Connection string to use to connect to the database. Use the following format:</p><pre><code>jdbc:postgresql://&#x3C;host>:&#x3C;port>/&#x3C;database>
    </code></pre>                                                                                                                                                                                                                                                                                                    |
    | Schema Name                              | Name of the schema to use. You can enter an expression that evaluates to the schema name.                                                                                                                                                                                                                                                                                                                                                                                  |
    | Table Name                               | <p>Name of the database table to use.</p><p>Enter one of the following:</p><ul><li>Name of an existing database table.</li><li><p>An expression that evaluates to the table name. For example, to use the table name in a "tableName" record header attribute, enter the following expression:</p><pre><code>${record:attribute('tableName')}
    </code></pre></li></ul><p>The processor creates the table if the table does not exist and updates the table when needed.</p> |
    | Decimal Scale Attribute                  | <p>The field attribute that contains the scale for Decimal fields.</p><p>Use the default for data generated by the JDBC Query Consumer or the JDBC Multitable Consumer origins.</p>                                                                                                                                                                                                                                                                                        |
    | Decimal Precision Attribute              | <p>The field attribute that contains the precision for Decimal fields.</p><p>Use the default for data generated by the JDBC Query Consumer or the JDBC Multitable Consumer origins.</p>                                                                                                                                                                                                                                                                                    |
    | Use Credentials                          | <p>Enables entering credentials on the Credentials tab. Use when you do not include credentials in the JDBC connection string.</p><p>The user account used for the connection credentials must have both the Create Table and Alter Table permissions on the database.</p>                                                                                                                                                                                                 |
    | Additional JDBC Configuration Properties | <p>Additional JDBC configuration properties to use. To add properties, click Add and define the JDBC property name and value.</p><p>Use the property names and values as expected by JDBC.</p>                                                                                                                                                                                                                                                                             |
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
    | Lowercase Column Names            | <p>Creates columns with lowercased names.</p><p>By default, the processor uses the column names as they appear in the record. Select this option to create all new columns with lowercased names.</p>                                                                                                                                                                                                    |
