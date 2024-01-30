# HBase Lookup

The HBase Lookup processor performs key-value lookups in HBase and passes the lookup values to fields.&#x20;

Use the HBase Lookup to enrich records with additional data. For example, you can configure the processor to use a department\_ID field as the key to look up department name values in HBase, and pass the values to a new department\_name output field.

When you configure the HBase Lookup processor, you specify whether the processor performs a bulk lookup of all keys in a batch, or performs an individual lookup of each key in a record. You define the key to look up in HBase, and specify the output field to write the lookup values to.

You can configure the processor to locally cache the key-value pairs to improve performance.

You also specify the HBase configuration properties, including the ZooKeeper Quorum, parent znode, and table name. When necessary, you can enable Kerberos authentication, specify an HBase user, and add additional HBase configuration properties.

### Lookup Key <a href="#ariaid-title2" id="ariaid-title2"></a>

When you define the lookup key, you specify the row and optionally the column and timestamp to look up in HBase.

The following table describes each lookup parameter that you can use to define the lookup key:

| Lookup Parameter | Description                                                                                                                                  |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Row              | The row to look up in HBase.                                                                                                                 |
| Column           | <p>The column of the row to use. The column must use the following format:</p><pre><code>&#x3C;column-family>:&#x3C;qualifier>
</code></pre> |
| Timestamp        | The timestamp associated with the row and column. The timestamp must be a Datetime type.                                                     |

You can define the lookup key using any of the following combinations of the lookup parameters:Row, Column, and TimestampWhen you define all of the lookup parameters, HBase Lookup processor returns the value of the specified row, column, and timestamp. The processor passes a single String value to the output field.Row and ColumnWhen you define the row and column lookup parameters, HBase Lookup processor returns the value of the specified row and column with the most recent timestamp. The processor passes a single String value to the output field.Row and TimestampWhen you define the row and timestamp lookup parameters, HBase Lookup processor looks up all values of the row in all columns with the specified timestamp. The processor passes a map of String values that contain the HBase column family, qualifier, and value for the specified row.For example, if the row exists in three columns with the specified timestamp, the processor returns a map of string values in the following format:

```
/<first column family:qualifier>: <value>
/<second column family:qualifier>: <value>
/<third column family:qualifier>: <value>
```

RowWhen you define only the row lookup parameter, HBase Lookup processor looks up all values of the row in all columns with the most recent timestamp. The processor passes a map of String values that contain the HBase column family, qualifier, and value for the specified row.For example, if the row exists in three columns, the processor returns a map of string values in the following format:

```
/<first column family:qualifier>: <value>
/<second column family:qualifier>: <value>
/<third column family:qualifier>: <value>
```

### Lookup Cache <a href="#ariaid-title3" id="ariaid-title3"></a>

To improve pipeline performance, you can configure the HBase Lookup processor to locally cache the key-value pairs returned from HBase.

The processor caches key-value pairs until the cache reaches the maximum size or the expiration time. When the first limit is reached, the processor evicts key-value pairs from the cache.

You can configure the following ways to evict key-value pairs from the cache:Size-based evictionConfigure the maximum number of key-value pairs that the processor caches. When the maximum number is reached, the processor evicts the oldest key-value pairs from the cache.Time-based evictionConfigure the amount of time that a key-value pair can remain in the cache without being written to or accessed. When the expiration time is reached, the processor evicts the key from the cache. The eviction policy determines whether the processor measures the expiration time since the last write of the value or since the last access of the value.For example, you set the eviction policy to expire after the last access and set the expiration time to 60 seconds. After the processor does not access a key-value pair for 60 seconds, the processor evicts the key-value pair from the cache.

When you stop the pipeline, the processor clears the cache.

### Kerberos Authentication <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use Kerberos authentication to connect to HBase. When you use Kerberos authentication, Data Pipelines uses the Kerberos principal and keytab to connect to HBase. By default, Data Pipelines uses the user account who started it to connect.

The Kerberos principal and keytab are defined in the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`. To use Kerberos authentication, configure all Kerberos properties in the Data Pipelines configuration file.

For more information about enabling Kerberos authentication for Data Pipelines, see Kerberos Authentication.

### Using an HBase User <a href="#ariaid-title5" id="ariaid-title5"></a>

Data Pipelines can either use the currently logged in Data Pipelines user or a user configured in the processor to look up data in HBase.

A Data Pipelines configuration property can be set that requires using the currently logged in Data Pipelines user. When this property is not set, you can specify a user in the origin. For more information about Hadoop impersonation and the Data Pipelines property, see Hadoop Impersonation Mode.

Note that the processor uses a different user account to connect to HDFS. By default, Data Pipelines uses the user account who started it to connect to external systems. When using Kerberos, Data Pipelines uses the Kerberos principal.

To configure a user in the processor to look up data in HBase, perform the following tasks:

1.  On HBase, configure the user as a proxy user and authorize the user to impersonate the HBase user.

    For more information, see the HBase documentation.
2. In the HBase Lookup processor, enter the HBase user name.

### HDFS Properties and Configuration File <a href="#ariaid-title6" id="ariaid-title6"></a>

You can configure the HBase Lookup processor to use individual HDFS properties or HDFS configuration files:

HBase configuration fileYou can use the following HDFS configuration file with the HBase configuration file:

* hbase-site.xml

To use HDFS configuration files:

1. Store the files or a symlink to the files in the Data Pipelines resources directory.
2. In the HBase Lookup processor, specify the location of the files.Note: For a Cloudera Manager installation, Data Pipelines automatically creates a symlink to the files named `hbase-conf`. Enter `hbase-conf` for the location of the files in the HBase Lookup processor.

Individual propertiesYou can configure individual HBase properties in the HBase Lookup processor. To add an HBase property, you specify the exact property name and the value. The HBase Lookup processor does not validate the property names or values.Note: Individual properties override properties defined in the HBase configuration file.

### Configuring an HBase Lookup <a href="#ariaid-title7" id="ariaid-title7"></a>

Configure an HBase Lookup processor to perform key-value lookups in HBase.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Lookup tab, configure the following properties:

    | Lookup Property          | Description                                                                                                                                                                                                                                                                                                                                                                                          |
    | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Mode                     | <p>Mode used to perform the lookups:</p><ul><li>Per Batch - Performs a bulk lookup of all keys in a batch. The processor performs a single lookup for each batch.</li><li>Per Key in Each Record - Performs individual lookups of each key in each record. If you configure multiple key expressions, the processor performs multiple lookups for each record.</li></ul><p>Default is Per Batch.</p> |
    | Row Expression           | <p>Row to look up in HBase. Enter a row name or enter an expression that defines the row.For example, enter the following expression to use the data in the department_id field as the row:</p><pre><code>${record:value('/department_id')}
    </code></pre>                                                                                                                                            |
    | Column Expression        | <p>Optional column family and qualifier of the row for the lookup. Enter a column name or enter an expression that defines the column. The column name must use the following format:</p><pre><code>&#x3C;column-family>:&#x3C;qualifier>
    </code></pre><p>If empty, the processor returns the values of the row for each column.</p>                                                                 |
    | Timestamp Expression     | <p>Optional timestamp of the row and column for the lookup. Enter value with a Datetime type or an expression that evaluates to a Datetime type.</p><p>If empty, the processor returns the value with the most recent timestamp.</p>                                                                                                                                                                 |
    | Output Field             | Name of the field in the record to pass the lookup value. You can specify an existing field or a new field. If the field does not exist, HBase Lookup creates the field.                                                                                                                                                                                                                             |
    | Enable Local Caching     | Specifies whether to locally cache the returned key-value pairs.                                                                                                                                                                                                                                                                                                                                     |
    | Maximum Entries to Cache | <p>Maximum number of key-value pairs to cache. When the maximum number is reached, the processor evicts the oldest key-value pairs from the cache.</p><p>Default is -1, which means unlimited.</p>                                                                                                                                                                                                   |
    | Eviction Policy Type     | <p>Policy used to evict key-value pairs from the local cache when the expiration time has passed:</p><ul><li>Expire After Last Access - Measures the expiration time since the key-value pair was last accessed by a read or a write.</li><li>Expire After Last Write - Measures the expiration time since the key-value pair was created, or since the value was last replaced.</li></ul>           |
    | Expiration Time          | <p>Amount of time that a key-value pair can remain in the local cache without being accessed or written to.</p><p>Default is 1 second.</p>                                                                                                                                                                                                                                                           |
    | Time Unit                | <p>Unit of time for the expiration time.</p><p>Default is seconds.</p>                                                                                                                                                                                                                                                                                                                               |
3.  On the HBase tab, configure the following properties:

    | HBase Property                | Description                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | ZooKeeper Quorum              | <p>Comma-separated list of servers in the ZooKeeper quorum. Use the following format:</p><pre><code>&#x3C;host>.&#x3C;domain>.com
    </code></pre><p>To ensure a connection, enter additional broker URIs.</p>                                                                                                                                                                                                                        |
    | ZooKeeper Client Port         | Port number clients use to connect to the ZooKeeper servers.                                                                                                                                                                                                                                                                                                                                                                       |
    | ZooKeeper Parent Znode        | Root node that contains all znodes used by the HBase cluster.                                                                                                                                                                                                                                                                                                                                                                      |
    | Table Name                    | <p>Name of the HBase table to use. Enter a table name or a namespace and table name as follows: &#x3C;namespace>.&#x3C;tablename>.</p><p>If you do not enter a table name, HBase uses the default namespace.</p>                                                                                                                                                                                                                   |
    | Kerberos Authentication       | <p>Uses Kerberos credentials to connect to HBase.</p><p>When selected, uses the Kerberos principal and keytab defined in the Data Pipelines configuration file, <code>$SDC_CONF/sdc.properties</code>.</p>                                                                                                                                                                                                                         |
    | HBase User                    | <p>The HBase user to use to look up data from HBase. When using this property, make sure HBase is configured appropriately.</p><p>When not configured, the pipeline uses the currently logged in Data Pipelines user.</p><p>Not configurable when Data Pipelines is configured to use the currently logged in Data Pipelines user. For more information, see Hadoop Impersonation Mode.</p>                                        |
    | HBase Configuration Directory | <p>Location of the HDFS configuration files.</p><p>For a Cloudera Manager installation, enter <code>hbase-conf</code>. For all other installations, use a directory or symlink within the Data Pipelines resources directory.</p><p>You can use the following file with HBase:</p><ul><li>hbase-site.xml</li></ul><p>Note: Properties in the configuration files are overridden by individual properties defined in the stage.</p> |
    | HBase Configuration           | <p>Additional HBase configuration properties to use.</p><p>To add properties, click Add and define the property name and value. Use the property names and values as expected by HBase.</p>                                                                                                                                                                                                                                        |
