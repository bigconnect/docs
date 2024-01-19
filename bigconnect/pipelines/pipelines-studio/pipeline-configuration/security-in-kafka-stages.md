# Security in Kafka Stages

You can configure Kafka stages – Kafka Consumer, Kafka Multitopic Consumer, and Kafka Producer – to use one of the following options to connect securely to Kafka:

* SSL/TLS encryption
* SSL/TLS encryption and authentication
* SASL authentication
* SASL authentication on SSL/TLS

For SASL authentication, either alone or with SSL/TLS, you can use the PLAIN (username/password) or GSSAPI (Kerberos) SASL mechanism.

Enabling security requires completing several prerequisite tasks in addition to configuring security properties in the stage.

## Prerequisite Tasks <a href="#ariaid-title2" id="ariaid-title2"></a>

Before enabling security for a Kafka stage, complete the following prerequisite tasks for the security method that you want to use:

SSL/TLSComplete the following prerequisite tasks before using SSL/TLS to connect to Kafka:

* Before using SSL/TLS to connect to Kafka, make sure Kafka is configured for SSL/TLS as described in the [Kafka documentation](http://kafka.apache.org/documentation.html#security\_ssl).
* If configuring a Kafka YARN cluster pipeline, store the SSL truststore and keystore files in the same location on the BigConnect Pipelines machine and on each node in the YARN cluster.

SASL with the PLAIN (username/password) mechanismComplete the following prerequisite tasks before using SASL with the PLAIN mechanism to connect to Kafka:

* Make sure Kafka is configured for SASL authentication with the PLAIN mechanism as described in the [Kafka documentation](http://kafka.apache.org/documentation.html#security\_sasl).
* Define the username and password credentials in a JAAS configuration file, as described in Providing PLAIN Credentials.
* If configuring a Kafka YARN cluster pipeline, store the JAAS configuration file in the same locations on the BigConnect Pipelines machine and on each node in the YARN cluster.

SASL with the GSSAPI (Kerberos) mechanism

Complete the following prerequisite tasks before using SASL with the GSSAPI (Kerberos) mechanism to connect to Kafka:

* Make sure Kafka is configured for SASL authentication with the GSSAPI (Kerberos) mechanism as described in the [Kafka documentation](http://kafka.apache.org/documentation.html#security\_sasl).
* Make sure that Kerberos authentication is enabled for BigConnect Pipelines, as described in Kerberos Authentication.
* Determine how to provide the Kerberos credentials and complete the required tasks as described in Providing Kerberos Credentials.
* If configuring a Kafka YARN cluster pipeline, store the JAAS configuration and Kafka keytab files in the same locations on the BigConnect Pipelines machine and on each node in the YARN cluster.

## SASL Authentication Credentials

When using SASL authentication to connect to Kafka, the method that you use to provide credentials depends on whether you use the PLAIN (username/password) or GSSAPI (Kerberos) SASL mechanism.

### Providing PLAIN Credentials <a href="#ariaid-title4" id="ariaid-title4"></a>

To connect to Kafka using SASL authentication with the PLAIN mechanism, provide the credentials in a Java Authentication and Authorization Service (JAAS) file.

Create a JAAS configuration file on the BigConnect Pipelines machine. You can define a single JAAS file for BigConnect Pipelines. As a result, every Kafka connection in every pipeline that uses SASL authentication with the PLAIN mechanism uses the same credentials.

Add the configuration properties required for Kafka clients based on your installation and authentication type:RPM, tarball, or Cloudera Manager installation without LDAP authenticationIf BigConnect Pipelines does not use LDAP authentication, create a separate JAAS configuration file on the BigConnect Pipelines machine. Add the following `KafkaClient` login section to the file:

```
KafkaClient {
    org.apache.kafka.common.security.plain.PlainLoginModule required 
    username="<username>" 
    password="<password>";
};
```

Then modify the SDC\_JAVA\_OPTS environment variable to include the following option that defines the path to the JAAS configuration file:

```
-Djava.security.auth.login.config=<JAAS config path>/kafka_client_jaas.conf
```

Modify environment variables using the method required by your installation type.

RPM or tarball installation with LDAP authenticationIf LDAP authentication is enabled in an RPM or tarball installation, add the properties to the JAAS configuration file used by BigConnect Pipelines - the `$SDC_CONF/ldap-login.conf` file. Add the following `KafkaClient` login section to the end of the `ldap-login.conf` file:

```
KafkaClient {
    org.apache.kafka.common.security.plain.PlainLoginModule required 
    username="<username>" 
    password="<password>";
};
```

Cloudera Manager installation with LDAP authenticationIf LDAP authentication is enabled in a Cloudera Manager installation, enable the LDAP Config File Substitutions (ldap.login.file.allow.substitutions) property for the StreamSets service in Cloudera Manager.

If the Use Safety Valve to Edit LDAP Information (use.ldap.login.file) property is enabled and LDAP authentication is configured in the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for ldap-login.conf field, then add the JAAS configuration properties to the same ldap-login.conf safety valve.

If LDAP authentication is configured through the LDAP properties rather than the ldap-login.conf safety value, add the JAAS configuration properties to the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for generated-ldap-login-append.conf field.

Add the following `KafkaClient` login section to the appropriate field as follows:

```
KafkaClient {
    org.apache.kafka.common.security.plain.PlainLoginModule required 
    username="<username>" 
    password="<password>";
};
```

### Providing Kerberos Credentials <a href="#ariaid-title5" id="ariaid-title5"></a>

To connect to Kafka using SASL authentication with the GSSAPI (Kerberos) mechanism, you must provide the Kerberos credentials to use.

You can provide Kerberos credentials in either of the following ways. You can also use both methods, as needed:

JAAS fileDefine Kerberos credentials in a Java Authentication and Authorization Service (JAAS) file when you want to use the same keytab and principal for every Kafka connection in every pipeline that you create. When configured, credentials defined in connection properties override JAAS file credentials.You might use this method to provide a default keytab and principal. Then, use connection properties to specify different credentials, as needed.

Add the configuration properties required for Kafka clients based on your installation and authentication type:

*   RPM, tarball, or Cloudera Manager installation without LDAP authentication - If BigConnect Pipelines does not use LDAP authentication, create a separate JAAS configuration file on the BigConnect Pipelines machine. Add the following `KafkaClient` login section to the file:

    ```
    KafkaClient {
        com.sun.security.auth.module.Krb5LoginModule required
        useKeyTab=true
        keyTab="<keytab path>"
        principal="<principal name>/<host name>@<realm>";
    };
    ```

    For example:

    ```
    KafkaClient {
        com.sun.security.auth.module.Krb5LoginModule required
        useKeyTab=true
        keyTab="/etc/security/keytabs/kafka_client.keytab"
        principal="kafka/node-01.cluster@EXAMPLE.COM";
    };
    ```

    Then modify the SDC\_JAVA\_OPTS environment variable to include the following option that defines the path to the JAAS configuration file:

    ```
    -Djava.security.auth.login.config=<JAAS config path>/kafka_client_jaas.conf
    ```

    Modify environment variables using the method required by your installation type.
*   RPM or tarball installation with LDAP authentication - If LDAP authentication is enabled in an RPM or tarball installation, add the properties to the JAAS configuration file used by BigConnect Pipelines - the `$SDC_CONF/ldap-login.conf` file. Add the following `KafkaClient` login section to the end of the `ldap-login.conf` file:

    ```
    KafkaClient {
        com.sun.security.auth.module.Krb5LoginModule required
        useKeyTab=true
        keyTab="<keytab path>"
        principal="<principal name>/<host name>@<realm>";
    };
    ```

    For example:

    ```
    KafkaClient {
        com.sun.security.auth.module.Krb5LoginModule required
        useKeyTab=true
        keyTab="/etc/security/keytabs/kafka_client.keytab"
        principal="kafka/node-01.cluster@EXAMPLE.COM";
    };
    ```
*   Cloudera Manager installation with LDAP authentication - If LDAP authentication is enabled in a Cloudera Manager installation, enable the LDAP Config File Substitutions (ldap.login.file.allow.substitutions) property for the StreamSets service in Cloudera Manager.

    If the Use Safety Valve to Edit LDAP Information (use.ldap.login.file) property is enabled and LDAP authentication is configured in the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for ldap-login.conf field, then add the JAAS configuration properties to the same ldap-login.conf safety valve.

    If LDAP authentication is configured through the LDAP properties rather than the ldap-login.conf safety value, add the JAAS configuration properties to the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for generated-ldap-login-append.conf field.

    Add the following `KafkaClient` login section to the appropriate field as follows:

    ```
    KafkaClient {
         com.sun.security.auth.module.Krb5LoginModule required
         useKeyTab=true
         keyTab="_KEYTAB_PATH"
         principal="<principal name>/_HOST@<realm>";
    };
    ```

    For example:

    ```
    KafkaClient {
         com.sun.security.auth.module.Krb5LoginModule required
         useKeyTab=true
         keyTab="_KEYTAB_PATH"
         principal="sdc/_HOST@EXAMPLE.COM";
    };
    ```

    Cloudera Manager generates the appropriate keytab path and host name.

Stage propertiesYou can define Kerberos credentials in stage properties when the Kafka stage uses a stage library for Kafka 0.11.0.0 or higher. Define Kerberos credentials in stage properties when you want to use different credentials in different Kafka stages.If you also configure a JAAS file to provide Kerberos credentials, the credentials that you enter in stage properties overrides those in the JAAS file.To provide Kerberos credentials in stage properties, you select the Provide Keytab property on the Security tab of the stage. You specify the principal in plain text, then you use one of the following methods to specify the keytab:

*   Enter a Base64-encoded keytab in the Runtime Keytab property.

    Encode the keytab before entering it in the stage property. Be sure to remove unnecessary characters, such as newline characters, before encoding the keytab.
*   Use a credential function to access a Base64-encoded keytab defined in a credential store.

    For more information, see Using a Credential Store.

Note: Configuring Kerberos credentials in stage properties is not supported in cluster pipelines at this time.

For details on enabling Kafka connections to use SASL authentication with the GSSAPI (Kerberos) mechanism, see Enabling SASL Authentication or Enabling SASL Authentication on SSL/TLS.

#### **Using a Credential Store**

You can define Kerberos keytabs in a credential store, then call the appropriate keytab from a Kafka stage.

Defining Kerberos keytabs in a credential store allows you to store multiple keytabs for use by Kafka stages. It also provides flexibility in how you use the keytabs. For example, you might create two separate keytabs, one for Kafka origins and one for Kafka destinations. Or, you might provide separate keytabs for every Kafka stage that you define.

Using a credential store makes it easy to update keytabs without having to edit the stages that use them. This can simplify tasks such as recycling keytabs or migrating pipelines to production.

Make sure that BigConnect Pipelines is configured to use a supported credential store. For a list of supported credential stores and instructions on enabling each credential store, see Credential Stores.

For an additional layer of security, you can require group access to credential store secrets.

## Enabling SSL/TLS Encryption <a href="#ariaid-title7" id="ariaid-title7"></a>

When the Kafka cluster uses the Kafka SSL security protocol, enable the Kafka stage to use SSL/TLS encryption.

Before you enable Kafka stages to use SSL/TLS, make sure that you have performed all necessary prerequisite tasks. Then, perform the following steps to enable the Kafka stages to use SSL/TLS to connect to Kafka.

1.  On the General tab of the stage, set the Stage Library property to the appropriate Kafka version.

    If configuring a Kafka Consumer origin for a Kafka YARN cluster pipeline, set the property to Kafka version 0.10.0.0 or later.
2.  On the Kafka tab of the stage, configure each Kafka broker URI to use the SSL/TLS port.

    The default SSL/TLS port number is 9093.
3.  On the Security tab, configure the following properties:\


    | Security Property   | Description                                                                                                                                                                                             |
    | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Security Option     | Set to SSL/TLS Encryption (Security Protocol=SSL).                                                                                                                                                      |
    | Truststore Type     | <p>Type of truststore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                      |
    | Truststore File     | Path to the truststore file. Enter an absolute path to the file or a path relative to the BigConnect Pipelines resources directory: `$SDC_RESOURCES`.                                                   |
    | Truststore Password | Password to the truststore file.Tip: To secure sensitive information such as passwords, you can use runtime resources or credential stores.                                                             |
    | Enabled Protocols   | Comma-separated list of protocols used to connect to the Kafka brokers. Ensure that at least one of these protocols is enabled in the Kafka brokers.Note: Older protocols are not as secure as TLSv1.2. |



## Enabling SSL/TLS Encryption and Authentication <a href="#ariaid-title8" id="ariaid-title8"></a>

When the Kafka cluster uses the Kafka SSL security protocol and requires client authentication, enable the Kafka stage to use SSL/TLS encryption and authentication.

Before you enable a Kafka stage to use SSL/TLS encryption and authentication, make sure that you have performed all necessary prerequisite tasks. Then, perform the following steps to enable the stage to use SSL/TLS encryption and authentication to connect to Kafka.

1.  On the General tab of the stage, set the Stage Library property to the appropriate Kafka version.

    If configuring a Kafka Consumer origin for a Kafka YARN cluster pipeline, set the property to Kafka version 0.10.0.0 or later.
2.  On the Kafka tab of the stage, configure each Kafka broker URI to use the SSL/TLS port.

    The default SSL/TLS port number is 9093.
3.  On the Security tab, configure the following properties:\


    | Security Property   | Description                                                                                                                                                                                             |
    | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Security Option     | Set to SSL/TLS Encryption and Authentication (Security Protocol=SSL).                                                                                                                                   |
    | Truststore Type     | <p>Type of truststore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                      |
    | Truststore File     | Path to the truststore file. Enter an absolute path to the file or a path relative to the BigConnect Pipelines resources directory: `$SDC_RESOURCES`.                                                   |
    | Truststore Password | Password to the truststore file.Tip: To secure sensitive information such as passwords, you can use runtime resources or credential stores.                                                             |
    | Keystore Type       | <p>Type of keystore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                        |
    | Keystore File       | Path to the keystore file. Enter an absolute path to the file or a path relative to the BigConnect Pipelines resources directory: `$SDC_RESOURCES`.                                                     |
    | Keystore Password   | Password to the keystore file.                                                                                                                                                                          |
    | Key Password        | Password for the key in the keystore file.Tip: To secure sensitive information such as passwords, you can use runtime resources or credential stores.                                                   |
    | Enabled Protocols   | Comma-separated list of protocols used to connect to the Kafka brokers. Ensure that at least one of these protocols is enabled in the Kafka brokers.Note: Older protocols are not as secure as TLSv1.2. |

## Enabling SASL Authentication <a href="#ariaid-title9" id="ariaid-title9"></a>

When the Kafka cluster uses the Kafka SASL\_PLAINTEXT security protocol, enable the Kafka stage to use SASL authentication.

Before you enable Kafka stages to use SASL authentication, make sure that you have performed all necessary prerequisite tasks.

Note: The following steps provide details on providing Kerberos credentials using a JAAS file or stage properties. You can use either method or both. Skip the steps that are not relevant to your desired implementation.

1.  To use a Java Authentication and Authorization Service (JAAS) file to provide plain or Kerberos credentials, create a JAAS configuration file on the BigConnect Pipelines machine.

    The contents of the JAAS configuration file depend on whether you use the PLAIN or GSSAPI (Kerberos) SASL mechanism and depend on your BigConnect Pipelines installation and authentication type. For details, see Providing PLAIN Credentials or Providing Kerberos Credentials.
2.  If using the GSSAPI (Kerberos) SASL mechanism and a credential store to call keytabs from stage properties, add the Base64-encoded keytabs that you want to use to the credential store.Note: Be sure to remove unnecessary characters, such as newline characters, before encoding the keytab.

    If you configured BigConnect Pipelines to require group secrets, for each keytab secret that you define, create a group secret and specify a comma-separated list of groups allowed to access the keytab secret.

    Name the group secret based on the keytab secret name, as follows: `<keytab secret name>-groups`.

    For details on defining secrets, see your credential store documentation.
3.  On the General tab of the Kafka stage, set the Stage Library property to the appropriate Kafka version.

    If configuring a Kafka Consumer origin for a Kafka YARN cluster pipeline, select a stage library for Kafka version 0.10.0.0 or later.

    If using stage properties to define Kafka credentials, select a stage library for Kafka version 0.11.0.0 or later.
4.  On the Security tab of the stage, configure the following properties:\


    | Security Property         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Security Option           | Set to Kerberos Authentication (Security Protocol=SASL\_PLAINTEXT).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | SASL Mechanism            | <p>SASL mechanism to use:</p><ul><li>PLAIN (username/password)</li><li>GSSAPI (Kerberos)</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Kerberos Service Name     | <p>Kerberos service principal name that the Kafka brokers run as.</p><p>Available when using the GSSAPI (Kerberos) mechanism.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Provide Keytab at Runtime | <p>Enables providing Kerberos credentials in the connection properties.Important: Configuring Kerberos credentials in connection properties is not supported in Transformer pipelines nor in BigConnect Pipelines cluster pipelines at this time.</p><p>Available when using the GSSAPI (Kerberos) mechanism.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Runtime Keytab            | <p>Kerberos keytab to use for the connection, specified in one of the following ways:</p><ul><li><p>Enter a Base64-encoded keytab.</p><p>Be sure to remove unnecessary characters, such as newline characters, before encoding the keytab.</p></li><li>If using a credential store, use the <code>credential:get()</code> or <code>credential:getWithOptions()</code> credential function to retrieve a Base64-encoded keytab.Note: The user who starts the pipeline must be in the BigConnect Pipelines group specified in the credential function. When BigConnect Pipelines requires a group secret, the user must also be in a group associated with the keytab.</li></ul><p>For more information about using keytabs in a credential store, see Using a Credential Store.</p><p>Available when using the GSSAPI (Kerberos) mechanism.</p> |
    | Runtime Principal         | <p>Kerberos principal to use for the connection, specified in the following format: <code>&#x3C;principal name>/&#x3C;host name>@&#x3C;realm></code>.</p><p>Available when using the GSSAPI (Kerberos) mechanism.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

## Enabling SASL Authentication on SSL/TLS <a href="#ariaid-title10" id="ariaid-title10"></a>

When the Kafka cluster uses the SASL\_SSL security protocol, enable the Kafka stage to use SASL authentication on SSL/TLS.

Before you enable Kafka stages to use SASL authentication on SSL/TLS, make sure that you have performed all necessary prerequisite tasks.

Note: The following steps provide details on providing Kerberos credentials using a JAAS file or stage properties. You can use either method or both. Skip the steps that are not relevant to your desired implementation.

1.  To use a Java Authentication and Authorization Service (JAAS) file to provide plain or Kerberos credentials, create a JAAS configuration file on the BigConnect Pipelines machine.

    The contents of the JAAS configuration file depend on whether you use the PLAIN or GSSAPI (Kerberos) SASL mechanism and depend on your BigConnect Pipelines installation and authentication type. For details, see Providing PLAIN Credentials or Providing Kerberos Credentials.
2.  If using the GSSAPI (Kerberos) SASL mechanism and a credential store to call keytabs from stage properties, add the Base64-encoded keytabs that you want to use to the credential store.Note: Be sure to remove unnecessary characters, such as newline characters, before encoding the keytab.

    If you configured BigConnect Pipelines to require group secrets, for each keytab secret that you define, create a group secret and specify a comma-separated list of groups allowed to access the keytab secret.

    Name the group secret based on the keytab secret name, as follows: `<keytab secret name>-groups`.

    For details on defining secrets, see your credential store documentation.
3.  On the General tab of the Kafka stage, set the Stage Library property to the appropriate Kafka version.

    If configuring a Kafka Consumer origin for a Kafka YARN cluster pipeline, select a stage library for Kafka version 0.10.0.0 or later.

    If using stage properties to define Kafka credentials, select a stage library for Kafka version 0.11.0.0 or later.
4.  On the Security tab of the stage, configure the following properties:

    | Security Property         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Security Option           | Set to Kerberos Authentication on SSL/TLS (Security Protocol=SASL\_SSL).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | SASL Mechanism            | <p>SASL mechanism to use:</p><ul><li>PLAIN (username/password)</li><li>GSSAPI (Kerberos)</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Kerberos Service Name     | <p>Kerberos service principal name that the Kafka brokers run as.</p><p>Available when using the GSSAPI (Kerberos) mechanism.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Provide Keytab at Runtime | <p>Enables providing Kerberos credentials in the connection properties.Important: Configuring Kerberos credentials in connection properties is not supported in Transformer pipelines nor in BigConnect Pipelines cluster pipelines at this time.</p><p>Available when using the GSSAPI (Kerberos) mechanism.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Runtime Keytab            | <p>Kerberos keytab to use for the connection, specified in one of the following ways:</p><ul><li><p>Enter a Base64-encoded keytab.</p><p>Be sure to remove unnecessary characters, such as newline characters, before encoding the keytab.</p></li><li>If using a credential store, use the <code>credential:get()</code> or <code>credential:getWithOptions()</code> credential function to retrieve a Base64-encoded keytab.Note: The user who starts the pipeline must be in the BigConnect Pipelines group specified in the credential function. When BigConnect Pipelinesrequires a group secret, the user must also be in a group associated with the keytab.</li></ul><p>For more information about using keytabs in a credential store, see Using a Credential Store.</p><p>Available when using the GSSAPI (Kerberos) mechanism.</p> |
    | Runtime Principal         | <p>Kerberos principal to use for the connection, specified in the following format: <code>&#x3C;principal name>/&#x3C;host name>@&#x3C;realm></code>.</p><p>Available when using the GSSAPI (Kerberos) mechanism.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Truststore File           | Path to the truststore file. Enter an absolute path to the file or a path relative to the BigConnect Pipelines resources directory: `$SDC_RESOURCES`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Truststore Password       | Password to the truststore file.Tip: To secure sensitive information such as passwords, you can use runtime resources or credential stores.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Enabled Protocols         | Comma-separated list of protocols used to connect to the Kafka brokers. Ensure that at least one of these protocols is enabled in the Kafka brokers.Note: Older protocols are not as secure as TLSv1.2.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Truststore Type           | <p>Type of truststore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

\
