# SSL/TLS Configuration

Some stages allow you use SSL/TLS to securely connect to the external system.

When you enable TLS, you can generally configure properties on the TLS tab of the stage. The properties that are available can depend on the stage that you are configuring. The TLS tab can include the following properties:

* Keystore properties
* Truststore properties
* TLS protocols
* Cipher suites

You can enable SSL/TLS type properties in the following stages and locations:

* Cassandra destination
* Couchbase Lookup processor and Couchbase destination
* Databricks executor
* gRPC Client origin
* HTTP Client origin, processor, and destination
* HTTP Server origin
* HTTP to Kafka origin
* Kafka Consumer origin, Kafka Multitopic Consumer origin, and Kafka Producer destination - These stages require configuring additional Kafka properties. For more information, see "Enabling Security" in the stage documentation.
* MongoDB origin and destination, MongoDB Oplog origin, and MongoDB Lookup processor - These stages require configuring the SDC\_JAVA\_OPTS environment variable. For more information, see "Enabling SSL/TLS" in the stage documentation.
* MQTT Subscriber origin and MQTT Publisher destination
* OPC UA Client origin
* Pulsar Consumer origin and Pulsar Producer destination - These stages require certificate files rather than keystore and truststore files. For more information, see "Enabling Security" in the stage documentation.
* REST Service origin
* Salesforce origin, lookup, and destination, and the Einstein Analytics destination
* SDC RPC origin and destination
* SDC RPC to Kafka origin
* SFTP/FTP/FTPS Client origin and destination
* Splunk destination
* Syslog destination - This destination requires configuring the SDC\_JAVA\_OPTS environment variable.
* TCP Server origin
* UDP to Kafka origin
* WebSocket Client origin and destination
* WebSocket Server origin
* Pipeline error handling, when writing error records to another pipeline

## Keystore and Truststore Configuration

When SSL/TLS is enabled in a stage, you can also enable the use of a keystore and a truststore.

Though similar in many ways, a keystore contains a private key and public certificates that are used to verify the identity of the client upon a request from an SSL/TLS server. In contrast, a truststore generally contains certificates from trusted certificate authorities that an SSL/TLS client uses to verify the identity of an SSL/TLS server.

{% hint style="warning" %}
Before enabling SSL/TLS in a stage, store the keystore and truststore files on the Data Pipelines  machine.
{% endhint %}

When you configure a keystore or truststore, you can configure the following properties:

**keystore/truststore type**

You can use the following types of keystores and truststores:

* Java Keystore File (JKS)
* PKCS #12 (p12 file)

**file and location**

When specifying the file and location of the keystore or truststore file, you can either use an absolute path to the file or a path relative to the Data Pipelines resources directory.

**password**

A password is optional for keystore and truststore files, but highly recommended.

**algorithm**

Data Pipelines uses the SunX509 key exchange algorithm by default. You can use any algorithm compatible with your keystore/truststore file that is supported by your JVM.

## Transport Protocols

When SSL/TLS is enabled in a stage, you can configure the transport protocol to use.

Data Pipelines uses TLSv1.2 by default. You can specify one or more other protocols, but versions prior to TLSv1.2 are not as secure.

## Cipher Suites

When SSL/TLS is enabled in a stage, you can configure the cipher suites to use to perform the SSL/TLS handshake.

By default, a stage can use any of the following cipher suites:

| Supported Cipher Suite        | Java Secure Socket Extension (JSSE) Name       |
| ----------------------------- | ---------------------------------------------- |
| ECDHE-ECDSA-AES256-GCM-SHA384 | TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_CBC\_SHA384 |
| ECDHE-RSA-AES256-GCM-SHA384   | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384   |
| ECDHE-ECDSA-AES128-GCM-SHA256 | TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256 |
| ECDHE-RSA-AES128-GCM-SHA256   | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256   |
| ECDHE-ECDSA-AES256-SHA384     | TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_CBC\_SHA384 |
| ECDHE-RSA-AES256-SHA384       | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384   |
| ECDHE-ECDSA-AES128-SHA256     | TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_CBC\_SHA256 |
| ECDHE-RSA-AES128-SHA256       | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256   |
