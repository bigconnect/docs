# MongoDB

The MongoDB destination writes data to MongoDB.&#x20;

To write data, the MongoDB destination requires records to include a CRUD operation record header attribute. The CRUD operation header attribute indicates the operation to perform for each record. You can also enable upserts for Update and Replace records.&#x20;

When you configure the MongoDB destination, you define connection information, such as the connection string and MongoDB credentials. You also configure the database, and collection, and write concern to use.

To replace and update records, you must specify a unique key field and can optionally enable an upsert flag. When you do not specify a unique key field, replace and update records are sent to the stage for error handling.

You can optionally configure advanced options that determine how the destination connects to MongoDB, including enabling SSL/TLS for the destination.

### Credentials <a href="#ariaid-title2" id="ariaid-title2"></a>

Based on the authentication used by the MongoDB server, configure the origin to use no authentication, username/password authentication, or LDAP authentication. When using username/password authentication, you can also use delegated authentication.

By default, the destination uses no authentication.

To use username/password or LDAP authentication, enter the required credentials in one of the following ways:

Connection string in the MongoDB tab

Enter credentials in the connection string in the MongoDB tab.

To enter credentials for username/password authentication, enter the username and password before the host name. Use the following format:

<pre><code><strong>mongodb://username:password@host[:port][/[database][?options]]
</strong></code></pre>

To enter credentials for LDAP authentication, enter the username and password before the host name, and set the authMechanism option to PLAIN. Use the following format:

<pre><code><strong>mongodb://username:password@host[:port][/[database]?authMechanism=PLAIN
</strong></code></pre>

Credentials tab

Select either the Username/Password or LDAP authentication type in the Credentials tab. Then enter the username and password for the authentication type.

If you enter credentials in both the connection string and in the Credentials tab, the Credentials tab takes precedence.

### Define the CRUD Operation <a href="#ariaid-title3" id="ariaid-title3"></a>

To write to MongoDB, ensure that the CRUD operation record header attribute is defined for each record earlier in the pipeline. Records without an operation record header attribute are sent to error.

To update and replace records, you must specify a unique key field. You can also enable upserts for update and replace records.

Note that when performing a DELETE operation, the destination deletes a maximum of one matching document in MongoDB. It does not delete all matching documents, as is sometimes possible with MongoDB.

To write records to MongoDB, make sure records include the following CRUD operation record header attribute:sdc.operation.typeWhen defined, the MongoDB destination uses the CRUD operation in the sdc.operation.type record header attribute when writing to MongoDB. The MongoDB destination supports the following values for the sdc.operation.type attribute:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE
* 7 for REPLACE

If your pipeline includes a CRUD-enabled origin that processes changed data, the destination simply reads the operation type from the `sdc.operation.type` header attribute that the origin generates. If your pipeline uses a non-CDC origin, you can use the Expression Evaluator or a scripting processor to define the record header attribute.

#### Performing Upserts <a href="#ariaid-title4" id="ariaid-title4"></a>

You can configure the destination to perform upserts. When you enable upserts, the destination inserts records for Update and Replace records when it does not find an existing record to update or replace.

By default, the destination does not perform upserts. If the destination does not find an existing record for a record flagged for update or replace, it does not write the record to MongoDB.

For more information about MongoDB operations and the upsert flag, see the MongoDB documentation.

### Enabling SSL/TLS <a href="#ariaid-title5" id="ariaid-title5"></a>

You can enable the MongoDB destination to use SSL/TLS to connect to MongoDB.

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

