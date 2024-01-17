# Syslog

The Syslog destination writes syslog messages to a Syslog server.

For example, you might have network devices that write log messages to flat files. You need to send these flat files to a centralized Syslog server, but the messages first must be converted to syslog messages. You design a pipeline that reads the flat files, performs some processing on the data, and then uses a Syslog destination that formats the data into syslog messages and writes the messages to the Syslog server.

When you configure the Syslog destination, you define the host name and port number of the Syslog server. You also define whether to use the UDP or TCP protocol to send messages to the server. If using the TCP protocol, you can optionally enable SSL/TLS for the destination.

You construct the syslog message content by specifying expressions that evaluate to the message values - such as the timestamp, facility code, severity level, message ID, and the log message itself.

You'll typically want to write the log message using the text data format. However, if needed, you can configure the Syslog destination to use a different data format for the log message.

### Protocol <a href="#ariaid-title2" id="ariaid-title2"></a>

You configure the Syslog destination to use one of the following protocols to send messages to the Syslog server:UDP protocolWhen the destination uses the UDP protocol, you select the syslog message format that the destination uses. Choose one of the following formats:

* [RFC 5424](https://tools.ietf.org/html/rfc5424)
* [RFC 3164](https://tools.ietf.org/html/rfc3164)

TCP protocolWhen the destination uses the TCP protocol, the destination uses the [RFC 6587](https://tools.ietf.org/html/rfc6587) message format.When using the TCP protocol, you can also enable SSL/TLS for the destination.

### Enabling SSL/TLS <a href="#ariaid-title3" id="ariaid-title3"></a>

If you configure the Syslog destination to use the TCP protocol, you can enable the destination to use SSL/TLS to connect to the Syslog server.

1. In the Syslog Connection tab, select the Enable SSL property.
2.  If the Syslog server certificate is signed by a private CA or not trusted by the default Java truststore, create a custom truststore file or modify a copy of the default Java truststore file to add the CA to the file. Then configure Data Pipelines to use the modified truststore file.

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
        keytool -import -file <Syslog certificate> -trustcacerts -noprompt -alias <Syslog alias> -storepass <password> -keystore "${SDC_CONF}/truststore.jks"
        ```

        For example:

        ```
        keytool -import -file  mySyslogServer.pem -trustcacerts -noprompt -alias MySyslogServer -storepass changeit -keystore "${SDC_CONF}/truststore.jks"
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
    6. Restart Data Pipelines to enable the changes to the environment variable.

### Message Content <a href="#ariaid-title4" id="ariaid-title4"></a>

A syslog message includes fields such as a timestamp, facility code, severity level, message ID, and the log message itself. You construct the syslog message content by specifying the values for message fields on the Message tab. The content of the log message depends on how you configure the Data Format tab.

Use the Message tab to construct the syslog message properties by entering values or expressions for the message properties. In most cases, you'll want to define expressions to call fields in the record that contain the message values.

For example, the following image shows the timestamp and host name message properties defined with expressions and the severity level and syslog facility set to the default values of 6 and 1:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/SyslogMsgContentText.png)

The content of the log message itself depends on the data format selected on the Data Format tab. For the Text data format, the destination writes data from a single text field in the record to the log message field. For the Binary data format, the destination writes data from a single field in the record to the log message field. For the remaining data formats, the destination writes data from all fields in the record to the log message field.

### Data Formats <a href="#ariaid-title5" id="ariaid-title5"></a>

The Syslog destination writes data to the log message based on the data format that you select.

The destination can use the following data formats:

Avro

The stage writes records based on the Avro schema. You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. The Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in the Confluent Schema Registry by the schema ID or subject.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the stage to register the Avro schema with the Confluent Schema Registry. You can also optionally include the schema definition in the message. Omitting the schema definition can improve performance, but requires the appropriate schema management to avoid losing track of the schema associated with the data.

You can include the Avro schema in the output.You can also compress data with an Avro-supported compression codec. When using Avro compression, avoid configuring any other compression properties in the stage.

Binary

The stage writes binary data to a single field in the record.

Delimited

The destination writes records as delimited data. When you use this data format, the root field must be list or list-map.You can use the following delimited format types:

* Default CSV - File that includes comma-separated values. Ignores empty lines in the file.
* RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.
* MS Excel CSV - Microsoft Excel comma-separated file.
* MySQL CSV - MySQL comma-separated file.
* Tab-Separated Values - File that includes tab-separated values.
* PostgreSQL CSV - PostgreSQL comma-separated file.
* PostgreSQL Text - PostgreSQL text file.
* Custom - File that uses user-defined delimiter, escape, and quote characters.
* Multi Character Delimited - File that uses multiple user-defined characters to delimit fields and lines, and single user-defined escape and quote characters.

JSON

The destination writes records as JSON data. You can use one of the following formats:

* Array - Each file includes a single array. In the array, each element is a JSON representation of each record.
* Multiple objects - Each file includes multiple JSON objects. Each object is a JSON representation of a record.

Protobuf

Writes one record in a message. Uses the user-defined message type and the definition of the message type in the descriptor file to generate the message.

Record

The destination writes records in the Record data format.

Text

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.

When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

XML

The destination creates a valid XML document for each record. The destination requires the record to have a single root field that contains the rest of the record data.&#x20;

The destination can include indentation to produce human-readable documents. It can also validate that the generated XML conforms to the specified schema definition. Records with invalid schemas are handled based on the error handling configured for the destination.
