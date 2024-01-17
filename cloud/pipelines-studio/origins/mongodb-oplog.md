# MongoDB Oplog

The MongoDB Oplog origin reads entries from MongoDB Oplog.&#x20;

MongoDB stores information about changes to the database in a local capped collection called an Oplog. The Oplog contains information about changes in data as well as changes in the database. The MongoDB Oplog origin can read any operation written to the Oplog.

Use the MongoDB Oplog origin to capture changes in data or the database. To process MongoDB data written to a standard capped or uncapped collection, use the MongoDB origin.

The MongoDB Oplog origin includes the CRUD operation type in a record header attribute so generated records can be easily processed by CRUD-enabled destinations

When you configure the MongoDB Oplog origin, you configure connection information, such as the connection string and MongoDB credentials. You also define an optional timestamp and ordinal to specify where to start the read, the operations that you want to process, and the read preference.

You can optionally configure advanced options that determine how the origin connects to MongoDB, including enabling SSL/TLS.

### Credentials <a href="#ariaid-title2" id="ariaid-title2"></a>

Based on the authentication used by the MongoDB server, configure the origin to use no authentication, username/password authentication, or LDAP authentication. When using username/password authentication, you can also use delegated authentication.

By default, the origin uses no authentication.

To use username/password or LDAP authentication, enter the required credentials in one of the following ways:Connection string in the MongoDB tabEnter credentials in the connection string in the MongoDB tab.To enter credentials for username/password authentication, enter the username and password before the host name. Use the following format:

<pre><code><strong>mongodb://username:password@host[:port][/[database][?options]]
</strong></code></pre>

To enter credentials for LDAP authentication, enter the username and password before the host name, and set the authMechanism option to PLAIN. Use the following format:

<pre><code><strong>mongodb://username:password@host[:port][/[database]?authMechanism=PLAIN
</strong></code></pre>

Credentials tabSelect either the Username/Password or LDAP authentication type in the Credentials tab. Then enter the username and password for the authentication type.

If you enter credentials in both the connection string and in the Credentials tab, the Credentials tab takes precedence.

### Oplog Timestamp and Ordinal <a href="#ariaid-title3" id="ariaid-title3"></a>

When you start the pipeline, the MongoDB Oplog origin starts reading from the beginning of the Oplog by default. You can configure a timestamp and ordinal to specify where you want to start the processing.

MongoDB Oplog entries include a timestamp field named "ts" that consists of a timestamp and ordinal as follows:

```
"ts": Timestamp(<timestamp>, <ordinal>)
```

The timestamp format is the seconds since the Unix epoch, such as 1412180887.

The ordinal is an integer counter used to differentiate between entries that occur in the same second.

You can use a timestamp and ordinal to specify where to begin reading from the Oplog. When you use a timestamp, you must also define an ordinal.

For more information about the Oplog timestamp field, see the MongoDB documentation.

### Read Preference <a href="#ariaid-title4" id="ariaid-title4"></a>

You can configure the read preference that the MongoDB Oplog origin uses.

The read preference determines how the origin reads data from different members of the MongoDB replica set.

You can use the following MongoDB read preferences:

* Primary - Requires reading from the primary member.
* Primary Preferred - Prefers reading from the primary, but allows reads from a secondary member.
* Secondary - Requires reading from a secondary member.
* Secondary Preferred - Prefers reading from a secondary, but allows reads from a primary when necessary.
* Nearest - Reads from the member with the least network latency.

By default, the origin uses Secondary Preferred to avoid making unnecessary requests to the primary member.

### Generated Records <a href="#ariaid-title5" id="ariaid-title5"></a>

The MongoDB Oplog origin generates records based on data from the MongoDB Oplog and adds CRUD and CDC related record header attributes.

The structure of Oplog records is unique, so when necessary, you might use some processors in the pipeline to convert record structure.

For example, for insert records, record data resides in a map field named "o". But for an update record, the \_id field is part of the o2 map field. To merge the record data, you can use a Field Flattener to flatten the map fields and a Field Remover to remove any unnecessary fields.

For more information about the Oplog record structure, see the MongoDB documentation. The following site is also a good resource: [https://www.compose.com/articles/the-mongodb-oplog-and-node-js/](https://www.compose.com/articles/the-mongodb-oplog-and-node-js/).

#### CRUD Operation and CDC Header Attributes <a href="#ariaid-title6" id="ariaid-title6"></a>

The MongoDB Oplog origin includes the CRUD operation type in the sdc.operation.type record header attribute.

If you use a CRUD-enabled destination in the pipeline such as JDBC Producer or Elasticsearch, the destination can use the operation type when writing to destination systems. When necessary, you can use an Expression Evaluator or scripting processors to manipulate the value in the `sdc.operation.type` header attribute.&#x20;

The MongoDB Oplog origin uses the following values in the sdc.operation.type record header attribute to represent the operation type:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE
* 5 for unsupported operations, such as CMD, NOOP, or DB, which are available MongoDB operation types but not applicable to record data.

The MongoDB Oplog origin can also include the following header attributes when the information is relevant:

* op - The CRUD operation using the following values:
  * i for INSERT
  * u for UPDATE
  * d for DELETE
* ns - The namespace, using the following format: `<database>:<tablename>`.

### Enabling SSL/TLS <a href="#ariaid-title7" id="ariaid-title7"></a>

You can enable the MongoDB Oplog origin to use SSL/TLS to connect to MongoDB.

1. In the Advanced tab for the stage, select the SSL Enabled property.
2.  If the MongoDB certificate is signed by a private CA or not trusted by the default Java truststore, create a custom truststore file or modify a copy of the default Java truststore file to add the CA to the file. Then configure Data Pipelines to use the modified truststore file.

    By default, Data Pipelines uses the Java truststore file located in $JAVA\_HOME/jre/lib/security/cacerts. If your certificate is signed by a CA that is included in the default Java truststore file, you do not need to create a truststore file and can skip this step.

    In these steps, we show how to modify the default truststore file to add an additional CA to the list of trusted CAs. If you prefer to create a custom truststore file, see the [keytool documentation](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html).
3. **Note**: If Data Pipelines is already configured to use a custom truststore file to enable HTTPS or for secure connections to an LDAP server, then simply add this additional CA to the same modified truststore file.
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
