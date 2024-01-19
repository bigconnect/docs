# JMS Producer

The JMS Producer destination writes data to a Java Messaging Service (JMS). Before you use JMS Producer, install the drivers required for your JMS implementation.&#x20;

When you configure a JMS Producer, you configure how the destination connects to the JMS server and the JMS destination name. You can optionally specify the JMS destination type, JMS credentials, and configure any additional JMS configuration properties that you require.

### Installing JMS Drivers <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you use the JMS Producer, install the JMS drivers for the implementation that you are using.

For example, to use the Oracle version of JMS, you need to install the Oracle JMS drivers: `wljmsclient.jar` and `wlclient.jar`. Or to use the ActiveMQ version, install all the ActiveMQ jars: activemq-all-\*.jar.

### Additional JMS Properties <a href="#ariaid-title3" id="ariaid-title3"></a>

You can configure additional JMS or JNDI configuration properties for the destination to use in the following locations:

* Additional JMS Configuration Properties on the JMS tab
* Additional Security Properties on the Credentials tab

You can define any additional JMS or JNDI property in either location. However, only additional security properties defined on the Credentials tab support using credential functions to retrieve sensitive information from supported credential stores.

If you define the same property in both locations, additional security properties defined on the Credentials tab take precedence.

When you add a configuration property to either location, enter the exact property name and the value. The JMS Consumer does not validate the property names or values.

### Working with TIBCO EMS <a href="#ariaid-title4" id="ariaid-title4"></a>

To communicate with TIBCO Enterprise Message Service (EMS), add the following additional JMS properties to the JMS Producer:

* java.naming.security.principal = \<username>
* java.naming.security.credentials = \<password>

#### Configuring for TIBCO and SSL <a href="#ariaid-title5" id="ariaid-title5"></a>

If you are configuring SSL on the TIBCO EMS server, add the following additional JMS properties. Your connection fails if you do not specify these properties.

* com.tibco.tibjms.naming.security\_protocol = ssl
* com.tibco.tibjms.naming.ssl\_enable\_verify\_host = false
* com.tibco.tibjms.naming.ssl\_expected\_hostname = \<tibco \_server\_hostname>
* com.tibco.tibjms.naming.ssl\_identity = \<path\_to>/server-cert.p12
* com.tibco.tibjms.naming.ssl\_password = \<password>
* com.tibco.tibjms.naming.ssl\_trusted\_certs = \<path\_to>/cert\_chain.pem
* com.tibco.tibjms.naming.ssl\_vendor = j2se-default

For example:

* com.tibco.tibjms.naming.security\_protocol = ssl
* com.tibco.tibjms.naming.ssl\_enable\_verify\_host = false
* com.tibco.tibjms.naming.ssl\_expected\_hostname = server1
* com.tibco.tibjms.naming.ssl\_identity = /tibco/ems/certs/server-cert.p12
* com.tibco.tibjms.naming.ssl\_password = password
* com.tibco.tibjms.naming.ssl\_trusted\_certs = /tibco/ems/certs/cert\_chain.pem
* com.tibco.tibjms.naming.ssl\_vendor = j2se-default

### Data Formats <a href="#ariaid-title6" id="ariaid-title6"></a>

The JMS Producer destination writes data to JMS based on the data format that you select. You can use the following data formats:

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

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.

When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.

When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

XML

The destination creates a valid XML document for each record. The destination requires the record to have a single root field that contains the rest of the record data.&#x20;

The destination can include indentation to produce human-readable documents. It can also validate that the generated XML conforms to the specified schema definition. Records with invalid schemas are handled based on the error handling configured for the destination.

### &#x20;<a href="#ariaid-title7" id="ariaid-title7"></a>

\
