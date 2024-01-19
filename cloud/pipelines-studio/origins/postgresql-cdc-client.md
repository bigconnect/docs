# PostgreSQL CDC Client

The PostgreSQL CDC Client origin processes Write-Ahead Logging (WAL) data to generate change data capture records for a PostgreSQL database.&#x20;

You might use this origin to perform database replication. You can use a separate pipeline with the JDBC Query Consumer or JDBC Multitable Consumer origin to read existing data. Then start a pipeline with the PostgreSQL CDC Client origin to process subsequent changes.

The PostgreSQL CDC Client generates a single record from each transaction. Since each transaction can include multiple CRUD operations, the PostgreSQL CDC Client origin can also include multiple operations in a record.

As a result, the origin does not write the CRUD operations to the `sdc.operation.type` record header attribute. Depending on your use case, you might use a scripting processor to convert the records as needed. Or, you might use a Field Pivoter and other processors to separate the data to create a record for each operation.

When you configure the PostgreSQL CDC Client, you configure the change capture details, such as the schema and tables to read from, the initial change to use, and the operations to include.

You define the name for the replication slot to be used, and specify whether to remove replication slots on close. You can also specify the behavior when the origin encounters an unsupported data type and include the data for those fields in the record as unparsed strings. When the source database has high-precision timestamps, you can configure the origin to write string values rather than datetime values to maintain the precision.

To determine how the origin connects to the database, you specify connection information, a query interval, number of retries, and any custom JDBC configuration properties that you need. You can configure advanced connection properties.

Note: The user that connects to the database must have the replication or superuser role.

### JDBC Driver <a href="#ariaid-title2" id="ariaid-title2"></a>

When connecting to a PostgreSQL database, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for PostgreSQL.

### PostgreSQL Prerequisite <a href="#ariaid-title3" id="ariaid-title3"></a>

To enable the PostgreSQL CDC Client origin to read Write-Ahead Logging (WAL) changed data capture information, you must install the wal2json logical decoder. Install wal2json on every PostgreSQL instance being monitored for changes.

MWARE provides the wal2json logical decoder. To install the wal2json, follow the instructions in the "Build and Install" section of the README.md file.

Then, follow the configuration instructions in the "Configuration" section of the README.md file.

### Schema, Table Name, and Exclusion Patterns <a href="#ariaid-title4" id="ariaid-title4"></a>

When you configure the PostgreSQL CDC Client origin, you specify the tables with the change capture data that you want to process. To specify the tables, you define the schema, a table name pattern, and an optional exclusion pattern.

When defining the schema and table name pattern, you can use SQL LIKE syntax to define a set of tables within a schema or across multiple schemas. For more information about valid patterns for the SQL LIKE syntax, see the [PostgreSQL documentation](https://www.postgresql.org/docs/9.5/functions-matching.html).

When needed, you can also use a regular expression as an exclusion pattern to exclude a subset of tables from the larger set.

For example, say you want to process change data capture data for all tables in the sales schema that start with SALES while excluding those that end with a dash (-) and single-character suffix. You can use the following configuration to specify the tables to process:

* Schema: `sales`
* Table Name Pattern: `SALES%`
* Exclusion Pattern: `SALES.*-.`

### Initial Change <a href="#ariaid-title5" id="ariaid-title5"></a>

The initial change is the point in the Write-Ahead Logging (WAL) data where you want to start processing. When you start the pipeline for the first time, the origin starts processing from the specified initial change. The origin only uses the specified initial change again when you reset the origin.

Note that PostgreSQL CDC Client processes only change capture data. If you need existing data, you might use a JDBC Query Consumer or a JDBC Multitable Consumer in a separate pipeline to read table data before you start a PostgreSQL CDC Client pipeline.

PostgreSQL CDC Client provides several ways to configure the initial change:From the latest changeThe origin processes all changes that occur after you start the pipeline.From a specified datetimeThe origin processes all changes that occurred at the specified datetime and later. Use the following format: `DD-MM-YYYY HH24:MI:SS`.From a specified log sequence number (LSN)The origin processes all changes that occurred in the specified LSN and later. When using the specified LSN, the origin starts processing with the timestamp associated with the LSN. If the LSN cannot be found in the WAL data, the origin continues reading from the next higher LSN that is available.Typically, a database admin can provide the LSN to use.

#### Example

You want to process all existing data in the Sales schema and then capture changed data, writing all data to Google Bigtable. To do this, you create two pipelines.

To read the existing data from the schema, you use a pipeline with the JDBC Multitable Consumer and Google Bigtable destination as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PostgreSQL-JDBCpipe.png)

Once all existing data is read, you stop the JDBC Multitable Consumer pipeline and start the following PostgreSQL CDC Client pipeline. This pipeline is configured to pick up changes that occur after you start the pipeline, but if you wanted to prevent any chance of data loss, you could configure the initial change for an exact datetime or earlier LSN:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PostgreSQL-PtoG.png)

### Generated Record <a href="#ariaid-title6" id="ariaid-title6"></a>

The PostgreSQL CDC Client generates a single record from each transaction. Since each transaction can include multiple CRUD operations, the PostgreSQL CDC Client origin can also include multiple operations in a record.

As a result, the origin does not write the CRUD operations to the `sdc.operation.type` record header attribute. Depending on your use case, you might use a scripting processor to convert the records as needed. Or, you might use a Field Pivoter and other processors to separate the data to create a record for each operation.

PostgreSQL CDC Client origin records include the following fields:

| Field Name | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| xid        | Transaction ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| nextlsn    | Next Logical Sequence Number (LSN).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| timestamp  | Timestamp with sub-second granularity, including the time zone offset from UTC.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| change     | <p>A list field that includes the following details about each data change:</p><ul><li>kind - Operation type: Insert, Update, or Delete.</li><li>schema - Schema name.</li><li>table - Table name.</li><li>columnnames - List of columns associated with the change.</li><li>columntypes - List of data types for the columns.</li><li>columnvalues - List of new values for the columns.</li><li><p>oldkeys - A map field that includes the previous information for the changed fields. Includes the following fields:</p><ul><li>keynames - List of names for the columns.</li><li>keytypes - List of data types for the columns.</li><li>keyvalues - List of values for the columns.</li></ul></li></ul> |

#### Sample Record

Below is a sample generated record. Notice how this single xid includes six operations:

<pre class="language-json"><code class="lang-json">{
  "xid": 598,
  "nextlsn": "0/16751E8",
  "timestamp": "2018-07-13 13:24:44.152109-07",
  "change": [
    {
<strong>      "kind": "update",
</strong>      "schema": "public",
      "table": "table1",
      "columnnames": [
        "id",
        "value"
      ],
      "columntypes": [
        "integer",
        "character(33)"
      ],
      "columnvalues": [
        1,
        "a"
      ],
      "oldkeys": {
        "keynames": [
          "value"
        ],
        "keytypes": [
          "character(33)"
        ],
        "keyvalues": [
          "a"
        ]
      }
    },
  {
<strong>      "kind": "update",
</strong>      "schema": "public",
      "table": "table1",
      "columnnames": [
        "id",
        "value"
      ],
      "columntypes": [
        "integer",
        "character(33)"
      ],
      "columnvalues": [
        2,
        "b                                "
      ],
      "oldkeys": {
        "keynames": [
          "value"
        ],
        "keytypes": [
          "character(33)"
        ],
        "keyvalues": [
          "b                                "
        ]
      }
    },
    {
<strong>      "kind": "update",
</strong>      "schema": "public",
      "table": "table1",
      "columnnames": [
        "id",
        "value"
      ],
      "columntypes": [
        "integer",
        "character(33)"
      ],
      "columnvalues": [
        3,
        "c                                "
      ],
      "oldkeys": {
        "keynames": [
          "value"
        ],
        "keytypes": [
          "character(33)"
        ],
        "keyvalues": [
          "c                                "
        ]
      }
    },
    {
<strong>      "kind": "update",
</strong>      "schema": "public",
      "table": "table2",
      "columnnames": [
        "id",
        "name"
      ],
      "columntypes": [
        "integer",
        "character varying(255)"
      ],
      "columnvalues": [
        1,
        "a"
      ],
      "oldkeys": {
        "keynames": [
          "id"
        ],
        "keytypes": [
          "integer"
        ],
        "keyvalues": [
          1
        ]
      }
    },
    {
<strong>      "kind": "update",
</strong>      "schema": "public",
      "table": "table2",
      "columnnames": [
        "id",
        "name"
      ],
      "columntypes": [
        "integer",
        "character varying(255)"
      ],
      "columnvalues": [
        2,
        "b"
      ],
      "oldkeys": {
        "keynames": [
          "id"
        ],
        "keytypes": [
          "integer"
        ],
        "keyvalues": [
          2
        ]
      }
    },
    {
<strong>      "kind": "update",
</strong>      "schema": "public",
      "table": "table2",
      "columnnames": [
        "id",
        "name"
      ],
      "columntypes": [
        "integer",
        "character varying(255)"
      ],
      "columnvalues": [
        3,
        "c"
      ],
      "oldkeys": {
        "keynames": [
          "id"
        ],
        "keytypes": [
          "integer"
        ],
        "keyvalues": [
          3
        ]
      }
    }
  ]
}
</code></pre>

#### CDC Header Attributes <a href="#ariaid-title7" id="ariaid-title7"></a>

The PostgreSQL CDC Client includes PostgreSQL change data capture information in the following record header attributes:

| CDC Header Attribute   | Description                             |
| ---------------------- | --------------------------------------- |
| postgres.cdc.lsn       | Logical Sequence Number of this record. |
| postgres.cdc.xid       | Transaction ID.                         |
| postgres.cdc.timestamp | Timestamp of transaction.               |

### Configuring a PostgreSQL CDC Client Origin <a href="#ariaid-title8" id="ariaid-title8"></a>

Configure a PostgreSQL CDC Client origin to process WAL change data capture data from a PostgreSQL database.

Before you use the origin, complete the prerequisite task.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul> |
2.  On the PostgreSQL CDC tab, configure the following properties:

    | PostgreSQL CDC Property          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Tables                           | <p>Tables to track. Specify related properties as needed.</p><p>Using simple or bulk edit mode, click the Add icon to define another table configuration.</p>                                                                                                                                                                                                                                                                                                                                                                                      |
    | Schema                           | Schema to use. You can enter a schema name or use SQL LIKE syntax to specify a set of schemas.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Table Name Pattern               | Table name pattern that specifies the tables to track. You can enter a table name or use SQL LIKE syntax to specify a set of tables.                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Exclusion Pattern                | <p>An optional table exclusion pattern to define a subset of tables to exclude. You can enter a table name or use a regular expression to specify a subset of tables to exclude.</p><p>To view and configure this option, click Show Advanced Options.</p>                                                                                                                                                                                                                                                                                         |
    | Initial Change                   | <p>The starting point for the read. When you start the pipeline for the first time, the origin starts processing from the specified initial change. The origin only uses the specified initial change again when you reset the origin.</p><p>Use one of the following options:</p><ul><li>From Latest Change - Processes changes that arrive after you start the pipeline.</li><li>From Date - Processes changes starting from the specified date.</li><li>From LSN - Processes changes starting from the specified log sequence number.</li></ul> |
    | Start Date                       | <p>The datetime to read from when you start the pipeline.</p><p>Use the following format: <code>DD-MM-YYYY HH24:MI:SS</code>.</p><p>For a date-based initial change only.</p>                                                                                                                                                                                                                                                                                                                                                                      |
    | DB Time Zone                     | Time zone of the database. For a date-based initial change only.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Start LSN                        | <p>The log sequence number to start reading from when you start the pipeline. If the LSN cannot be found, the origin continues reading from the next higher LSN that is available.</p><p>For an LSN-based initial change only.</p>                                                                                                                                                                                                                                                                                                                 |
    | Remove Replication Slot on Close | Removes the replication slot after each use. Keeping the replication slot results in the continued production of WAL records for this slot, but may cause a degradation in performance.Note: This option is not supported in PostgreSQL versions earlier than 9.5. Earlier versions of PostgreSQL do not include the required feature.                                                                                                                                                                                                             |
    | Replication Slot                 | <p>Name for the replication slot. Include only lowercase letters and numbers.</p><p>Default is <code>sdc</code>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Operations                       | Operations to include.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Add Unsupported Fields           | Includes fields with unsupported data types in the record. When enabled, the origin adds the field name and passes the data as an unparsed string, when possible.                                                                                                                                                                                                                                                                                                                                                                                  |
    | Convert Timestamp To String      | <p>Enables the origin to write timestamps as string values rather than datetime values. Strings maintain the precision stored in the source database.</p><p>When writing timestamps to Data Pipelines date or time data types that do not store nanoseconds, the origin stores any nanoseconds from the timestamp in a <a href="https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Pipeline_Design/FieldAttributes.html#concept_xfm_wtp_1z">field attribute</a>.</p>                                             |
    | Query Timeout                    | <p>Time to wait before timing out a WAL query and returning the batch.</p><p>Default is <code>${ 5 * MINUTES }</code> which is 300 seconds.</p>                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Poll Interval                    | <p>Interval to wait before checking for additional data.</p><p>Default is 1 second.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Status Interval                  | <p>Interval between status updates. Use to ensure that the wal_sender process does not time out.</p><p>This property should be set to less than the wal_sender_timeout property in the PostgreSQL <code>postgresql.conf</code> file. Ideally, it should be set to half of the value of the wal_sender_timeout property.</p><p>For example, you can use the default status interval of 30 seconds with the default wal_sender_timeout value of 60000 milliseconds, or 1 minute.</p>                                                                 |
    | CDC Generator Queue Size         | <p>Buffer size for storing records in memory before passing them downstream.</p><p>Set this property larger than the maximum batch size.</p>                                                                                                                                                                                                                                                                                                                                                                                                       |
3.  On the JDBC tab, configure the following JDBC properties:

    | JDBC Property                            | Description                                                                                                                                                                                                                                                                                                                                    |
    | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JDBC Connection String                   | <p>Connection string to use to connect to the database. Use the following syntax:</p><p><code>jdbc:postgresql://&#x3C;host>:&#x3C;port>/&#x3C;dbname></code></p><p>Note: If you include the JDBC credentials in the connection string, use the user account created for the origin. That user must have the superuser or replication role.</p> |
    | Max Batch Size (records)                 | <p>Maximum number of records processed at one time. Honors values up to the Data Pipelines maximum batch size.</p><p>Default is 1000. The Data Pipelines default is 1000.</p>                                                                                                                                                                  |
    | Use Credentials                          | Enables entering credentials on the Credentials tab. Use when you do not include credentials in the JDBC connection string.                                                                                                                                                                                                                    |
    | Additional JDBC Configuration Properties | <p>Additional JDBC configuration properties to use. To add properties, click Add and define the JDBC property name and value.</p><p>Use the property names and values as expected by JDBC.</p>                                                                                                                                                 |
4.  If you configured the origin to enter JDBC credentials separately from the JDBC connection string on the JDBC tab, then configure the following properties on the Credentials tab:

    | Credentials Property | Description                                                                                                                                                         |
    | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Username             | <p>PostgreSQL username.</p><p>The specified user must have the superuser or replication role.</p>                                                                   |
    | Password             | <p>PostgreSQL password.</p><p></p><p>Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores.</p> |
5.  When using JDBC versions older than 4.0, on the Legacy Drivers tab, optionally configure the following properties:

    | Legacy Drivers Property      | Description                                                                                                  |
    | ---------------------------- | ------------------------------------------------------------------------------------------------------------ |
    | JDBC Class Driver Name       | Class name for the JDBC driver. Required for JDBC versions older than version 4.0.                           |
    | Connection Health Test Query | Optional query to test the health of a connection. Recommended only when the JDBC version is older than 4.0. |
6.  On the Advanced tab, optionally configure advanced options.The defaults for these properties should work in most cases:

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
    | Enforce Read-only Connection      | <p>Creates read-only connections to avoid any type of write.</p><p>Default is enabled. Disabling this property is not recommended.</p>                                                                                                                                                                                                                                                                   |
    | Transaction Isolation             | <p>Transaction isolation level used to connect to the database.</p><p>Default is the default transaction isolation level set for the database. You can override the database default by setting the level to any of the following:</p><ul><li>Read committed</li><li>Read uncommitted</li><li>Repeatable read</li><li>Serializable</li></ul>                                                             |
    | Init Query                        | SQL query to perform immediately after the stage connects to the database. Use to set up the database session as needed.                                                                                                                                                                                                                                                                                 |
