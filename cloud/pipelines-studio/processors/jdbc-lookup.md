# JDBC Lookup

The JDBC Lookup processor uses a JDBC connection to perform lookups in a database table and pass the lookup values to fields.&#x20;

Use the JDBC Lookup to enrich records with additional data. For example, you can configure the processor to use a department\_ID field as the column to look up department name values in a database table, and pass the values to a new department\_name output field.

Important: This stage does not support connecting to non-RDBMS systems, including Hive, Impala, Kudu, or Snowflake. Support for untested systems is not guaranteed. For a list of tested systems, see "Database Vendors and Drivers". Use the Kudu Lookup processor to look up Kudu data.

When a lookup results in multiple matches, the JDBC Lookup processor can return the first matching value, all matching values in a list in a single record, or all matching values in separate records.

When you configure JDBC Lookup, you specify connection information and custom JDBC configuration properties to determine how the processor connects to the database. You configure the SQL query to define the data to look up in the database, specify the output fields to write the lookup values to, and choose the multiple match behavior.

You can specify the behavior for when the lookup returns no values and optionally configure a default value for the same situation. You can also specify the behavior for when the processor encounters data of an unexpected type.

You can configure the processor to locally cache the lookup values to improve performance. When caching lookup values, you can also enable using additional threads to prepopulate the lookup cache and further increase performance.

By default, the JDBC Lookup processor requires all table columns defined in the column mappings to exist when the pipeline starts. You can configure the processor to skip this validation.

The processor generates JDBC field attributes that provide additional information about each field.

To use a JDBC version older than 4.0, you can specify the driver class name and define a health check query.

When you monitor a pipeline that includes the JDBC Lookup processor, you can view stage statistics about the number of queries the processor makes and the average time of the queries.

### Database Vendors and Drivers <a href="#ariaid-title2" id="ariaid-title2"></a>

The JDBC Lookup processor can perform lookups of database data from multiple database vendors.

The following table lists the supported and tested database versions for this stage. You can use the stage with other JDBC-compliant databases, but full support is not guaranteed.&#x20;

| Database Vendor      | Supported Versions                                                                                                                                                                                                                                    | Tested Versions                                                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| MySQL                | MySQL 5.7 and later                                                                                                                                                                                                                                   | <ul><li>MySQL 5.7 with the MySQL Connector/J 8.0.12 driver</li><li>MySQL 8.0 with the MySQL Connector/J 8.0.12 driver</li></ul>     |
| Oracle               | <ul><li>Oracle 11g, 12c, 18c, 19c, and later</li><li>Oracle Real Application Clusters (RAC) 12c, 18c, 19c, and later</li></ul><p>Also supported:</p><ul><li>Hosted systems, such as AWS RDS</li><li>Derived systems, such as Oracle Exadata</li></ul> | <ul><li>Oracle 11g, 19c with the Oracle 19.3.0 JDBC driver</li><li>Oracle RAC 12c, 19c with the Oracle 19.3.0 JDBC driver</li></ul> |
| PostgreSQL           | PostgreSQL 9.x and later                                                                                                                                                                                                                              | <ul><li>PostgreSQL 9.6.9</li><li>PostgreSQL 10.4</li><li>PostgreSQL 11.7</li><li>PostgreSQL 12.2</li><li>PostgreSQL 13.0</li></ul>  |
| Microsoft SQL Server | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                                                                                                                                             | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                           |

#### MySQL Data Types <a href="#ariaid-title3" id="ariaid-title3"></a>

The JDBC Lookup processor converts MySQL data types into Data Pipelines data types.

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

#### Oracle Data Types <a href="#ariaid-title4" id="ariaid-title4"></a>

The JDBC Lookup processor converts Oracle data types into Data Pipelines data types.

The processor supports the following Oracle data types:

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

The JDBC Lookup processor converts PostgreSQL data types into Data Pipelines data types.

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

#### SQL Server Data Types <a href="#ariaid-title6" id="ariaid-title6"></a>

The JDBC Lookup processor converts SQL Server data types into Data Pipelines data types.

The processor supports the following SQL Server data types:

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

### Installing the JDBC Driver <a href="#ariaid-title7" id="ariaid-title7"></a>

Before you use the JDBC Lookup processor, install the JDBC driver for the database. You cannot access the database until you install the required driver.Note: When connecting to a PostgreSQL or Microsoft SQL Server database, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for those databases.

### Lookup Cache <a href="#ariaid-title8" id="ariaid-title8"></a>

To improve pipeline performance, you can configure the JDBC Lookup processor to locally cache the values returned from a database table.

The processor caches values until the cache reaches the maximum size or the expiration time. When the first limit is reached, the processor evicts values from the cache.

You can configure the following ways to evict values from the cache:

**Size-based eviction**

Configure the maximum number of values that the processor caches. When the maximum number is reached, the processor evicts the oldest values from the cache.

**Time-based eviction**

Configure the amount of time that a value can remain in the cache without being written to or accessed. When the expiration time is reached, the processor evicts the value from the cache. The eviction policy determines whether the processor measures the expiration time since the last write of the value or since the last access of the value.

For example, you set the eviction policy to expire after the last access and set the expiration time to 60 seconds. After the processor does not access a value for 60 seconds, the processor evicts the value from the cache.

When you stop the pipeline, the processor clears the cache.

#### Using Additional Threads <a href="#ariaid-title9" id="ariaid-title9"></a>

When using local caching, you can increase the number of threads that the JDBC Lookup processor uses to prepopulate the lookup cache. After the cache is populated, the additional threads are released. This can substantially increase the performance of the processor.

By default, the Minimum Idle Connections property, on the Advanced tab, determines the minimum number of connections to the database that Data Pipelines creates and maintains.

When you enable using a local lookup cache, the Minimum Idle Connections property can also determine the number of available cores on the Data Pipelines machine that the processor uses for threads to prepopulate the cache.

When caching is enabled, the JDBC Lookup processor uses additional threads based on the smaller of the following numbers:

* The configured Minimum Idle Connections property.
* The number of available cores on the Data Pipelines machine, minus one. The processor never uses all available cores.

By increasing the setting for the Minimum Idle Connections property, you can enable the processor to use almost all available cores on the Data Pipelines machine for additional threads to prepopulate the lookup cache.

For example, say you have 8 available cores on the Data Pipelines machine when you start a pipeline, and the JDBC Lookup has local caching enabled and the Minimum Idle Connections property set to 8. Then, the JDBC Lookup can use 7 of the available cores for threads to prepopulate the lookup cache. This might be ideal if you have complex lookup processing that you want to accomplish as quickly as possible and do not need to reserve resources for other processing.

To reserve machine resources for other processing, you can limit the cores that are used by the processor by setting the Minimum Idle Connections property to a lower number. For example, if you set Minimum Idle Connections to 5, then the processor can use up to 4 available cores for threads.

#### Retry Lookups for Missing Values <a href="#ariaid-title10" id="ariaid-title10"></a>

When you enable local caching, the processor also caches the configured default value when a lookup for a given column fails and a default value is defined for that column in the Column Mappings. The processor then always returns the default value for the column to avoid unnecessary lookups.

You can configure the processor to retry lookups for known missing values by enabling the Retry on Missing Value property. Configure the processor to retry lookups when the lookup table might be updated as the pipeline runs.

For example, if you expect that new values will be inserted in the table as the pipeline runs, you’d want to configure the processor to retry the request rather than returning the cached default value.

Note: If a lookup for a given column fails and a default value is not configured for that column, then the processor handles the record based on the Missing Values Behavior property.

### JDBC Field Attributes <a href="#ariaid-title11" id="ariaid-title11"></a>

The JDBC Lookup processor generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

The following data type conversions do not include all information in the corresponding Data Pipelines data type:

* Database Decimal and Numeric data types are converted to the Data Pipelines Decimal data type, which does not store scale and precision.
* The database Timestamp data type is converted to the Data Pipelines Datetime data type, which does not store nanoseconds.

To preserve this information during data type conversion, the origin generates the following field attributes for these Data Pipelines data types:

| Data Pipelines Data Type | Generated Field Attribute | Description                                                          |
| ------------------------ | ------------------------- | -------------------------------------------------------------------- |
| Decimal                  | precision                 | Provides the original precision for every decimal or numeric column. |
| Decimal                  | scale                     | Provides the original scale for every decimal or numeric column.     |
| Datetime                 | nanoSeconds               | Provides the original nanoseconds for every timestamp column.        |

You can use the `record:fieldAttribute` or `record:fieldAttributeOrDefault` functions to access the information in the attributes.&#x20;

### Monitoring a JDBC Lookup <a href="#ariaid-title12" id="ariaid-title12"></a>

When you monitor a pipeline that includes the JDBC Lookup processor, the Summary tab displays statistics about the queries that the JDBC Lookup processor performs. Use the statistics to help identify any performance bottlenecks encountered by the pipeline.

When you select the JDBC Lookup processor in the canvas while monitoring a running pipeline, the Summary tab displays the following stage statistics:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/JDBCLookupStatistics.png)

The Select Queries Meter displays the number of queries that the processor makes per second. The Select Queries Timer displays the average amount of time that the queries take to complete.

For more information about monitoring pipelines, see Pipeline Monitoring Overview.

### Configuring a JDBC Lookup <a href="#ariaid-title13" id="ariaid-title13"></a>

Configure a JDBC Lookup processor to perform lookups in a database table.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the JDBC tab, configure the following properties:

    | JDBC Property                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JDBC Connection String                   | <p>Connection string used to connect to the database. Use the connection string format required by the database vendor.</p><p>For example, use the following formats for these database vendors:</p><ul><li>MySQL - <code>jdbc:mysql://&#x3C;host>:&#x3C;port>/&#x3C;database_name></code></li><li>Oracle - <code>jdbc:oracle:&#x3C;driver_type>:@&#x3C;host>:&#x3C;port>:&#x3C;service_name></code></li><li>PostgreSQL - <code>jdbc:postgresql://&#x3C;host>:&#x3C;port>/&#x3C;database_name></code></li><li>SQL Server - <code>jdbc:sqlserver://&#x3C;host>:&#x3C;port>;databaseName=&#x3C;database_name></code></li></ul><p>You can optionally include the user name and password in the connection string.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | SQL Query                                | <p>SQL query to use to look up data in the database. Use the following syntax for the query:</p><pre><code>SELECT &#x3C;column1 name>, &#x3C;column2 name> FROM &#x3C;table name> WHERE &#x3C;column3 name> =
        '${record:value(&#x3C;field path>)}'
    </code></pre><p>For example, to use the department ID field to look up the department name column, use the following query:</p><pre><code>SELECT DeptName FROM Departments WHERE DeptID = '${record:value('/dept_ID')}'
    </code></pre>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | Column Mappings                          | <p>Use to override the default column to field mappings. By default, columns are written to fields of the same name.Enter the following:</p><ul><li>Column Name - Name of the database column that contains the lookup value. Enter a column name or enter an expression that defines the column name.</li><li>SDC Field - Name of the field in the record that receives the lookup value. You can specify an existing field or a new field. If the field does not exist, JDBC Lookup creates the field.</li><li><p>Default Value - Optional default value to use when the query does not return a value for the field. If the query returns no value and this property is not defined, the processor handles the record based on the Missing Values Behavior property.</p><p>To enter a default value for the Date data type, use the following format: yyyy/MM/dd. To enter a default value for the Datetime data type, use the following format: yyyy/MM/dd HH:mm:ss.</p></li><li>Data Type - Data type to use for the SDC Field. Required when you specify a default value. The processor uses the database column data type by default.</li></ul><p>Using simple or bulk edit mode, click the Add icon to create additional column mappings.</p> |
    | Multiple Values Behavior                 | <p>Action to take upon finding multiple matching values:</p><ul><li>First value only - Returns the first value.</li><li>All values as a list - Returns every matching value in a list in a single record.</li><li>Split into multiple records - Returns every matching value in a separate record.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | Missing Values Behavior                  | <p>Action to take upon finding no return values in fields with no default value defined:</p><ul><li>Send to error - Sends the record to error.</li><li>Pass the record along the pipeline unchanged - Passes the record without a lookup return value.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Max Clob Size (characters)               | Maximum number of characters to be read in a Clob field. Larger data is truncated.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Max Blob Size (bytes)                    | Maximum number of bytes to be read in a Blob field.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Retry on Missing Value                   | Specifies whether to retry lookups for known missing values. By default, the processor caches and then always returns the default value for known missing values to avoid unnecessary lookups.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Use Credentials                          | Enables entering credentials on the Credentials tab. Use when you do not include credentials in the JDBC connection string.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Validate Column Mappings                 | <p>Requires that all column names defined in the column mappings exist in the database when the pipeline starts.</p><p>When enabled, if a column in the mapping does not exist, the pipeline fails to start. When disabled, if a column in the mapping does not exist, the mapping is not used during processing.</p><p>By default, this property is enabled.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Enable Local Caching                     | Specifies whether to locally cache the returned values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Maximum Entries to Cache                 | <p>Maximum number of values to cache. When the maximum number is reached, the processor evicts the oldest values from the cache.</p><p>Default is -1, which means unlimited.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Eviction Policy Type                     | <p>Policy used to evict values from the local cache when the expiration time has passed:</p><ul><li>Expire After Last Access - Measures the expiration time since the value was last accessed by a read or a write.</li><li>Expire After Last Write - Measures the expiration time since the value was created, or since the value was last replaced.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Expiration Time                          | <p>Amount of time that a value can remain in the local cache without being accessed or written to.</p><p>Default is 1 second.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Time Unit                                | <p>Unit of time for the expiration time.</p><p>Default is seconds.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Additional JDBC Configuration Properties | <p>Additional JDBC configuration properties to use. To add properties, click Add and define the JDBC property name and value.</p><p>Use the property names and values as expected by JDBC.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
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

    | Advanced Property                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Maximum Pool Size                 | <p>Maximum number of connections to create.</p><p>Default is 1. The recommended value is 1.</p>                                                                                                                                                                                                                                                                                                                                                            |
    | Minimum Idle Connections          | <p>When local caching is not enabled, determines the minimum number of connections to the database to create and maintain.</p><p>When local caching is enabled, also determines the number of available cores that can be used for additional threads for processing.</p><p>Note: When local caching is enabled, configure this property carefully to avoid monopolizing Data Pipelines resources. For more information, see Using Additional Threads.</p> |
    | Connection Timeout (seconds)      | <p>Maximum time to wait for a connection. Use a time constant in an expression to define the time increment.Default is 30 seconds, defined as follows:</p><pre><code>${30 * SECONDS}
    </code></pre>                                                                                                                                                                                                                                                         |
    | Idle Timeout (seconds)            | <p>Maximum time to allow a connection to idle. Use a time constant in an expression to define the time increment.</p><p>Use 0 to avoid removing any idle connections.</p><p>When the entered value is close to or more than the maximum lifetime for a connection, Data Pipelines ignores the idle timeout.</p><p>Default is 10 minutes, defined as follows:</p><pre><code>${10 * MINUTES}
    </code></pre>                                                   |
    | Max Connection Lifetime (seconds) | <p>Maximum lifetime for a connection. Use a time constant in an expression to define the time increment.</p><p>Use 0 to set no maximum lifetime.</p><p>When a maximum lifetime is set, the minimum valid value is 30 minutes.</p><p>Default is 30 minutes, defined as follows:</p><pre><code>${30 * MINUTES}
    </code></pre>                                                                                                                                 |
    | Auto Commit                       | <p>Determines if auto-commit mode is enabled. In auto-commit mode, the database commits the data for each record.</p><p>Default is disabled.</p>                                                                                                                                                                                                                                                                                                           |
    | Enforce Read-only Connection      | <p>Creates read-only connections to avoid any type of write.</p><p>Default is enabled. Disabling this property is not recommended.</p>                                                                                                                                                                                                                                                                                                                     |
    | Transaction Isolation             | <p>Transaction isolation level used to connect to the database.</p><p>Default is the default transaction isolation level set for the database. You can override the database default by setting the level to any of the following:</p><ul><li>Read committed</li><li>Read uncommitted</li><li>Repeatable read</li><li>Serializable</li></ul>                                                                                                               |
    | Init Query                        | <p>SQL query to perform immediately after the stage connects to the database. Use to set up the database session as needed.</p><p>For example, the following query sets the time zone for the session for a MySQL database: <code>SET time_zone = timezone;</code></p>                                                                                                                                                                                     |
    | On Unknown Type                   | <p>Action to take when encountering an unsupported data type:</p><ul><li>Stop Pipeline - Stops the pipeline after completing processing the previous records.</li><li>Convert to String - When possible, converts the data to string and continues processing.</li></ul>                                                                                                                                                                                   |
