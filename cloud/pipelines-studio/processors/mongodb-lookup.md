# MongoDB Lookup

The MongoDB Lookup processor performs lookups in MongoDB and passes all values from the returned document to a new list-map field in the record.&#x20;

Use the MongoDB Lookup processor to enrich records with additional data. For example, you have multiple department documents in MongoDB that list the employees in the department. You configure the processor to use the `department_ID` field in the record to look up a department document, and pass all values from the matching document to a new `department_employees` field in the record.

When you configure the MongoDB Lookup processor, you define connection information, such as the connection string and MongoDB credentials. You define the fields to look up and the field for the return values.

When a lookup results in multiple matched documents, the MongoDB Lookup processor can return values from the first matching document or return values from all matching documents in separate records.

To improve performance, you can configure the processor to locally cache the document values.

You can optionally configure advanced options that determine how the processor connects to MongoDB, including enabling SSL/TLS for the processor.

### Field Mappings <a href="#ariaid-title2" id="ariaid-title2"></a>

When you configure the MongoDB Lookup processor, you define the document fields to look up in MongoDB. You map these document fields to fields in the record that contain the values to look up.

When you define a document field, use the dot notation to define a field in an embedded document as follows:

```
<embedded document>.<field name>.<embedded field name>
```

When you define a field in the record, reference the field as follows:

```
/<field name>
```

You can define multiple field mappings. The processor uses the configured field mappings to generate and run a `find()` query in MongoDB.

After defining the field mappings, define a new list-map field to store all values from the returned document.

For example, your MongoDB collection contains customer documents with the following structure:

```
{
  _id: 123,
  customer: {
       name: "Ed Martinez",
       status: "gold",
	phone: "123-456-7891",
	location: {
	  city: "San Francisco",
	  state: "California"
	}
  }
}
```

Your pipeline reads from an origin that contains customer names and cities, but you want to enrich that customer data with the customer status and phone number. When you configure the processor, you map the `customer.name` and `customer.location.city` document fields to the values stored in the `name` and `city` fields in the record. To store the lookup result, you define a new field named `customer_details`. The following image shows the configured field mappings and the result field:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/MongoDBLookup.png)

When you run the pipeline, the processor uses the field mappings to generate and run a `find()` query in MongoDB. The processor passes all values from the returned document to the new result field.

### Lookup Cache <a href="#ariaid-title3" id="ariaid-title3"></a>

To improve pipeline performance, you can configure the MongoDB Lookup processor to locally cache the document values returned from MongoDB.

The processor caches values until the cache reaches the maximum size or the expiration time. When the first limit is reached, the processor evicts values from the cache.

You can configure the following ways to evict values from the cache:

**Size-based eviction**

Configure the maximum number of values that the processor caches. When the maximum number is reached, the processor evicts the oldest values from the cache.

**Time-based eviction**

Configure the amount of time that a value can remain in the cache without being written to or accessed. When the expiration time is reached, the processor evicts the value from the cache. The eviction policy determines whether the processor measures the expiration time since the last write of the value or since the last access of the value.

For example, you set the eviction policy to expire after the last access and set the expiration time to 60 seconds. After the processor does not access a value for 60 seconds, the processor evicts the value from the cache.

When you stop the pipeline, the processor clears the cache.

### Credentials <a href="#ariaid-title4" id="ariaid-title4"></a>

Based on the authentication used by the MongoDB server, configure the processor to use no authentication, username/password authentication, or LDAP authentication. When using username/password authentication, you can also use delegated authentication.

By default, the processor uses no authentication.

To use username/password or LDAP authentication, enter the required credentials in one of the following ways:Connection string in the MongoDB tabEnter credentials in the connection string in the MongoDB tab.To enter credentials for username/password authentication, enter the username and password before the host name. Use the following format:

<pre><code><strong>mongodb://username:password@host[:port][/[database][?options]]
</strong></code></pre>

To enter credentials for LDAP authentication, enter the username and password before the host name, and set the authMechanism option to PLAIN. Use the following format:

<pre><code><strong>mongodb://username:password@host[:port][/[database]?authMechanism=PLAIN
</strong></code></pre>

Credentials tabSelect either the Username/Password or LDAP authentication type in the Credentials tab. Then enter the username and password for the authentication type.

If you enter credentials in both the connection string and in the Credentials tab, the Credentials tab takes precedence.

### Read Preference <a href="#ariaid-title5" id="ariaid-title5"></a>

You can configure the read preference that the MongoDB Lookup processor uses.

The read preference determines how the processor reads data from different members of the MongoDB replica set.

You can use the following MongoDB read preferences:

* Primary - Requires reading from the primary member.
* Primary Preferred - Prefers reading from the primary, but allows reads from a secondary member.
* Secondary - Requires reading from a secondary member.
* Secondary Preferred - Prefers reading from a secondary, but allows reads from a primary when necessary.
* Nearest - Reads from the member with the least network latency.

By default, the origin uses Secondary Preferred to avoid making unnecessary requests to the primary member.

### BSON Timestamp <a href="#ariaid-title6" id="ariaid-title6"></a>

When processing data from MongoDB version 2.6 and later, the MongoDB Lookup processor supports the MongoDB BSON Timestamp data type.

The MongoDB BSON Timestamp is a MongoDB data type that includes a timestamp and ordinal as follows:

```
<BSON Timestamp field name>:Timestamp(<timestamp>, <ordinal>)
```

The MongoDB processor converts the BSON Timestamp to a map as follows:

```
<BSON Timestamp field name>{MAP}:
    Timestamp{DATETIME}:<UTC timestamp>
    Ordinal{INTEGER}:<integer ordinal>
```

For example, a Transaction BSON timestamp of `(1485449409, 1)`, is converted to the following Transaction map field:

```
"Transaction":{
    "Timestamp":Jan 26, 2016 14:50:09PM
    "Ordinal":1
}
```

### Enabling SSL/TLS <a href="#ariaid-title7" id="ariaid-title7"></a>

You can enable the MongoDB Lookup processor to use SSL/TLS to connect to MongoDB.

1. In the Advanced tab for the stage, select the SSL Enabled property.
2.  If the MongoDB certificate is signed by a private CA or not trusted by the default Java truststore, create a custom truststore file or modify a copy of the default Java truststore file to add the CA to the file. Then configure Data Pipelines to use the modified truststore file.

    By default, Data Pipelines uses the Java truststore file located in $JAVA\_HOME/jre/lib/security/cacerts. If your certificate is signed by a CA that is included in the default Java truststore file, you do not need to create a truststore file and can skip this step.

    In these steps, we show how to modify the default truststore file to add an additional CA to the list of trusted CAs. If you prefer to create a custom truststore file, see the [keytool documentation](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html).\
    Note: If Data Pipelines is already configured to use a custom truststore file to enable HTTPS or for secure connections to an LDAP server, then simply add this additional CA to the same modified truststore file.

    1.  Use the following command to set the JAVA\_HOME environment variable:

        ```
        export JAVA_HOME=<Java home directory>
        ```
    2.  Use the following command to set the SDC\_CONF environment variable:

        ```
        export SDC_CONF=<Data Pipelines configuration directory>
        ```

        For example, for an RPM installation use:

        ```
        export SDC_CONF=/etc/sdc
        ```
    3.  Use the following command to copy the default Java truststore file to the Data Pipelines configuration directory:

        ```
        cp "${JAVA_HOME}/jre/lib/security/cacerts" "${SDC_CONF}/truststore.jks"
        ```
    4.  Use the following keytool command to import the CA certificate into the truststore file:

        ```
        keytool -import -file <MongoDB certificate> -trustcacerts -noprompt -alias <MongoDB alias> -storepass <password> -keystore "${SDC_CONF}/truststore.jks"
        ```

        For example:

        ```
        keytool -import -file  myMongoDB.pem -trustcacerts -noprompt -alias MyMongoDB -storepass changeit -keystore "${SDC_CONF}/truststore.jks"
        ```
    5.  Define the following options in the SDC\_JAVA\_OPTS environment variable:

        * javax.net.ssl.trustStore - Path to the truststore file on the Data Pipelines machine.
        * javax.net.ssl.trustStorePassword - Truststore password.

        Modify environment variables using the method required by your installation type.

        For example, define the options as follows:

        ```
        export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Djavax.net.ssl.trustStore=/etc/sdc/truststore.jks -Djavax.net.ssl.trustStorePassword=mypassword -Xmx1024m -Xms1024m -server -XX:-OmitStackTraceInFastThrow"
        ```

        Or to avoid saving the password in the export command, save the password in a text file and then define the truststore password option as follows: -Djavax.net.ssl.trustStorePassword=$(cat passwordfile.txt)

        Then ensure that the password file is readable only by the user executing the export command.
    6. Restart Data Pipelines to enable the changes.

### Configuring a MongoDB Lookup <a href="#ariaid-title8" id="ariaid-title8"></a>

Configure a MongoDB Lookup processor to perform lookups in MongoDB.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the MongoDB tab, configure the following properties:

    | MongoDB Property   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Connection String  | <p>Connection string for the MongoDB instance. Use the following format:</p><pre><code>mongodb://host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
    </code></pre><p>When connecting to a cluster, enter additional node information to ensure a connection.</p><p>If the MongoDB server uses username/password or LDAP authentication, you can include the credentials in the connection string, as described in Credentials.</p> |
    | Enable Single Mode | <p>Select to connect to a single MongoDB server or node. If multiple nodes are defined in the connection string, the stage connects only to the first node.</p><p>Use this option with care. If the stage cannot connect or the connection fails, the pipeline stops.</p>                                                                                                                                                                               |
    | Database           | Name of the MongoDB database.                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Collection         | Name of the MongoDB collection to use.                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Read Preference    | Determines how the processor reads data from different members of the MongoDB replica set.                                                                                                                                                                                                                                                                                                                                                              |
3.  On the Lookup tab, configure the following properties:

    | Lookup Property               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Document to DP Field Mappings | <p>List of document fields to look up in MongoDB mapped to fields in the record that contain the lookup values.Enter the following:</p><ul><li><p>Document Field - Name of the document field to look up. Use the dot notation to define a field in an embedded document as follows:</p><pre><code>&#x3C;embedded document>.&#x3C;field1>.&#x3C;field2>
    </code></pre></li><li>SDC Field - Name of the field in the record that contains the lookup value.</li></ul><p>Using simple or bulk edit mode, click the Add icon to create additional field mappings.</p> |
    | Result Field                  | Name of the new list-map field in the record that receives all values from the returned document.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Multiple Values Behavior      | <p>Action to take upon finding multiple matching documents:</p><ul><li>First value only - Generates a single record for the return values of the first matching document.</li><li>Split into Multiple Records - Generates a separate record for the return values of every matching document.</li></ul>                                                                                                                                                                                                                                                           |
    | Missing Values Behavior       | <p>Action to take upon finding no document to return:</p><ul><li>Send to error - Sends the record to error.</li><li>Pass the record along the pipeline unchanged - Passes the record without a lookup return value.</li></ul>                                                                                                                                                                                                                                                                                                                                     |
    | Enable Local Caching          | Specifies whether to locally cache the returned values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Maximum Entries to Cache      | <p>Maximum number of values to cache. When the maximum number is reached, the processor evicts the oldest values from the cache.</p><p>Default is -1, which means unlimited.</p>                                                                                                                                                                                                                                                                                                                                                                                  |
    | Eviction Policy Type          | <p>Policy used to evict values from the local cache when the expiration time has passed:</p><ul><li>Expire After Last Access - Measures the expiration time since the value was last accessed by a read or a write.</li><li>Expire After Last Write - Measures the expiration time since the value was created, or since the value was last replaced.</li></ul>                                                                                                                                                                                                   |
    | Expiration Time               | <p>Amount of time that a value can remain in the local cache without being accessed or written to.</p><p>Default is 1 second.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Time Unit                     | <p>Unit of time for the expiration time.</p><p>Default is seconds.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
4.  To enter credentials separately from the MongoDB connection string, click the Credentials tab and configure the following properties:

    | Credentials           | Description                                                                                                                       |
    | --------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
    | Authentication Type   | Authentication used by the MongoDB server: Username/Password or LDAP.                                                             |
    | Username              | MongoDB or LDAP user name.                                                                                                        |
    | Password              | MongoDB or LDAP password.                                                                                                         |
    | Authentication Source | <p>An optional alternate database name to perform delegated authentication.</p><p>Available for the Username/Password option.</p> |
5.  Optionally, click the Advanced tab to configure how the processor connects to MongoDB.The defaults for these properties should work in most cases:

    | Advanced Property                                  | Description                                                                                                                                                                                                                                                                                             |
    | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Connections Per Host                               | <p>Maximum number of connections for each host.</p><p>Default is 100.</p>                                                                                                                                                                                                                               |
    | Min Connections Per Host                           | <p>Minimum number of connections for each host.</p><p>Default is 0.</p>                                                                                                                                                                                                                                 |
    | Connection Timeout                                 | <p>Maximum time in milliseconds to wait for a connection.</p><p>Default is 10,000.</p>                                                                                                                                                                                                                  |
    | Max Connection Idle Time                           | <p>Maximum time in milliseconds that a pooled connection can remain idle. When a pooled connection exceeds the idle time, the connection is closed. Use 0 to opt out of this property.</p><p>Default is 0.</p>                                                                                          |
    | Max Connection Lifetime                            | <p>Maximum time in milliseconds that a pooled connection can be active. When a pooled connection exceeds the lifetime, the connection is closed. Use 0 to opt out of this property.</p><p>Default is 0.</p>                                                                                             |
    | Max Wait Time                                      | <p>Maximum time in milliseconds that a thread can wait for a connection to become available. Use 0 to opt out of this property. Use a negative value to wait indefinitely.</p><p>Default is 120,000.</p>                                                                                                |
    | Server Selection Timeout                           | <p>Maximum time in milliseconds that Data Pipelines waits for a server selection before throwing an exception. If you use 0, an exception is thrown immediately if no server is available. Use a negative value to wait indefinitely.</p><p>Default is 30,000.</p>                                      |
    | Threads Allowed to Block for Connection Multiplier | <p>Multiplier that determines the maximum number of threads that can wait for a connection to become available from the pool. This number multiplied by the Connections Per Host value determines the maximum number of threads.</p><p>Default is 5.</p>                                                |
    | Heartbeat Frequency                                | <p>The frequency in milliseconds at which Data Pipelines attempts to determine the current state of each server in the cluster.</p><p>Default is 10,000.</p>                                                                                                                                            |
    | Min Heartbeat Frequency                            | <p>Minimum heartbeat frequency in milliseconds. Data Pipelines waits at least this long before checking the state of each server.</p><p>Default is 500.</p>                                                                                                                                             |
    | Heartbeat Connection Timeout                       | <p>Maximum time in milliseconds to wait for a connection used for the cluster heartbeat.</p><p>Default is 20,000.</p>                                                                                                                                                                                   |
    | Heartbeat Socket Timeout                           | <p>Maximum time in milliseconds for a socket timeout for connections used for the cluster heartbeat.</p><p>Default is 20,000.</p>                                                                                                                                                                       |
    | Local Threshold                                    | <p>Local threshold in milliseconds. Requests are sent to a server whose ping time is less than or equal to the server with the fastest ping time plus the local threshold value.</p><p>Default is 15.</p>                                                                                               |
    | Required Replica Set Name                          | Required replica set name to use for the cluster.                                                                                                                                                                                                                                                       |
    | Cursor Finalizer Enabled                           | Specifies whether to enable cursor finalizers.                                                                                                                                                                                                                                                          |
    | Socket Keep Alive                                  | Specifies whether to enable socket keep alive.                                                                                                                                                                                                                                                          |
    | Socket Timeout                                     | <p>Maximum time in milliseconds for the socket timeout. Use 0 to opt out of this property.</p><p>Default is 0.</p>                                                                                                                                                                                      |
    | SSL Enabled                                        | <p>Enables SSL/TLS between Data Pipelines and MongoDB.</p><p>If the MongoDB certificate is signed by a private CA or not trusted by the default Java truststore, you also must define the truststore file and password in the SDC_JAVA_OPTS environment variable, as described in Enabling SSL/TLS.</p> |
    | SSL Invalid Host Name Allowed                      | Specifies whether invalid host names are allowed in SSL/TLS certificates.                                                                                                                                                                                                                               |
