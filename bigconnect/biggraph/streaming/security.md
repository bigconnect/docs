# Security

## 7.1 Security Overview

In release 0.9.0.0, the Kafka community added a number of features that, used either separately or together, increases security in a Kafka cluster. The following security measures are currently supported:

1. Authentication of connections to brokers from clients (producers and consumers), other brokers and tools, using either SSL or SASL. Kafka supports the following SASL mechanisms:
   * SASL/GSSAPI (Kerberos) - starting at version 0.9.0.0
   * SASL/PLAIN - starting at version 0.10.0.0
   * SASL/SCRAM-SHA-256 and SASL/SCRAM-SHA-512 - starting at version 0.10.2.0
   * SASL/OAUTHBEARER - starting at version 2.0
2. Authentication of connections from brokers to ZooKeeper
3. Encryption of data transferred between brokers and clients, between brokers, or between brokers and tools using SSL (Note that there is a performance degradation when SSL is enabled, the magnitude of which depends on the CPU type and the JVM implementation.)
4. Authorization of read / write operations by clients
5. Authorization is pluggable and integration with external authorization services is supported

It's worth noting that security is optional - non-secured clusters are supported, as well as a mix of authenticated, unauthenticated, encrypted and non-encrypted clients. The guides below explain how to configure and use the security features in both clients and brokers.

## Listener Configuration <a href="#listener_configuration" id="listener_configuration"></a>

In order to secure a Kafka cluster, it is necessary to secure the channels that are used to communicate with the servers. Each server must define the set of listeners that are used to receive requests from clients as well as other servers. Each listener may be configured to authenticate clients using various mechanisms and to ensure traffic between the server and the client is encrypted. This section provides a primer for the configuration of listeners.

Kafka servers support listening for connections on multiple ports. This is configured through the `listeners` property in the server configuration, which accepts a comma-separated list of the listeners to enable. At least one listener must be defined on each server. The format of each listener defined in `listeners` is given below:

```
{LISTENER_NAME}://{hostname}:{port}
```

The `LISTENER_NAME` is usually a descriptive name which defines the purpose of the listener. For example, many configurations use a separate listener for client traffic, so they might refer to the corresponding listener as `CLIENT` in the configuration:

```
listeners=CLIENT://localhost:9092
```

The security protocol of each listener is defined in a separate configuration: `listener.security.protocol.map`. The value is a comma-separated list of each listener mapped to its security protocol. For example, the follow value configuration specifies that the `CLIENT` listener will use SSL while the `BROKER` listener will use plaintext.

```
listener.security.protocol.map=CLIENT:SSL,BROKER:PLAINTEXT
```

Possible options (case-insensitive) for the security protocol are given below:

1. PLAINTEXT
2. SSL
3. SASL\_PLAINTEXT
4. SASL\_SSL

The plaintext protocol provides no security and does not require any additional configuration. In the following sections, this document covers how to configure the remaining protocols.

If each required listener uses a separate security protocol, it is also possible to use the security protocol name as the listener name in `listeners`. Using the example above, we could skip the definition of the `CLIENT` and `BROKER` listeners using the following definition:

```
listeners=SSL://localhost:9092,PLAINTEXT://localhost:9093
```

However, we recommend users to provide explicit names for the listeners since it makes the intended usage of each listener clearer.

Among the listeners in this list, it is possible to declare the listener to be used for inter-broker communication by setting the `inter.broker.listener.name` configuration to the name of the listener. The primary purpose of the inter-broker listener is partition replication. If not defined, then the inter-broker listener is determined by the security protocol defined by `security.inter.broker.protocol`, which defaults to `PLAINTEXT`.

For legacy clusters which rely on Zookeeper to store cluster metadata, it is possible to declare a separate listener to be used for metadata propagation from the active controller to the brokers. This is defined by `control.plane.listener.name`. The active controller will use this listener when it needs to push metadata updates to the brokers in the cluster. The benefit of using a control plane listener is that it uses a separate processing thread, which makes it less likely for application traffic to impede timely propagation of metadata changes (such as partition leader and ISR updates). Note that the default value is null, which means that the controller will use the same listener defined by `inter.broker.listener`

In a KRaft cluster, a broker is any server which has the `broker` role enabled in `process.roles` and a controller is any server which has the `controller` role enabled. Listener configuration depends on the role. The listener defined by `inter.broker.listener.name` is used exclusively for requests between brokers. Controllers, on the other hand, must use separate listener which is defined by the `controller.listener.names` configuration. This cannot be set to the same value as the inter-broker listener.

Controllers receive requests both from other controllers and from brokers. For this reason, even if a server does not have the `controller` role enabled (i.e. it is just a broker), it must still define the controller listener along with any security properties that are needed to configure it. For example, we might use the following configuration on a standalone broker:

```
process.roles=broker
listeners=BROKER://localhost:9092
inter.broker.listener.name=BROKER
controller.quorum.voters=0@localhost:9093
controller.listener.names=CONTROLLER
listener.security.protocol.map=BROKER:SASL_SSL,CONTROLLER:SASL_SSL
```

The controller listener is still configured in this example to use the `SASL_SSL` security protocol, but it is not included in `listeners` since the broker does not expose the controller listener itself. The port that will be used in this case comes from the `controller.quorum.voters` configuration, which defines the complete list of controllers.

For KRaft servers which have both the broker and controller role enabled, the configuration is similar. The only difference is that the controller listener must be included in `listeners`:

```
process.roles=broker,controller
listeners=BROKER://localhost:9092,CONTROLLER://localhost:9093
inter.broker.listener.name=BROKER
controller.quorum.voters=0@localhost:9093
controller.listener.names=CONTROLLER
listener.security.protocol.map=BROKER:SASL_SSL,CONTROLLER:SASL_SSL
```

It is a requirement for the port defined in `controller.quorum.voters` to exactly match one of the exposed controller listeners. For example, here the `CONTROLLER` listener is bound to port 9093. The connection string defined by `controller.quorum.voters` must then also use port 9093, as it does here.

The controller will accept requests on all listeners defined by `controller.listener.names`. Typically there would be just one controller listener, but it is possible to have more. For example, this provides a way to change the active listener from one port or security protocol to another through a roll of the cluster (one roll to expose the new listener, and one roll to remove the old listener). When multiple controller listeners are defined, the first one in the list will be used for outbound requests.

It is conventional in Kafka to use a separate listener for clients. This allows the inter-cluster listeners to be isolated at the network level. In the case of the controller listener in KRaft, the listener should be isolated since clients do not work with it anyway. Clients are expected to connect to any other listener configured on a broker. Any requests that are bound for the controller will be forwarded as described below

In the following section, this document covers how to enable SSL on a listener for encryption as well as authentication. The subsequent section will then cover additional authentication mechanisms using SASL.

## Encryption and Authentication using SSL <a href="#security_ssl" id="security_ssl"></a>

Apache Kafka allows clients to use SSL for encryption of traffic as well as authentication. By default, SSL is disabled but can be turned on if needed. The following paragraphs explain in detail how to set up your own PKI infrastructure, use it to create certificates and configure Kafka to use these.

1.  **Generate SSL key and certificate for each Kafka broker**

    The first step of deploying one or more brokers with SSL support is to generate a public/private keypair for every server. Since Kafka expects all keys and certificates to be stored in keystores we will use Java's keytool command for this task. The tool supports two different keystore formats, the Java specific jks format which has been deprecated by now, as well as PKCS12. PKCS12 is the default format as of Java version 9, to ensure this format is being used regardless of the Java version in use all following commands explicitly specify the PKCS12 format.

    ```bash
    > keytool -keystore {keystorefile} -alias localhost -validity {validity} -genkey -keyalg RSA -storetype pkcs12
    ```

    You need to specify two parameters in the above command:

    1. keystorefile: the keystore file that stores the keys (and later the certificate) for this broker. The keystore file contains the private and public keys of this broker, therefore it needs to be kept safe. Ideally this step is run on the Kafka broker that the key will be used on, as this key should never be transmitted/leave the server that it is intended for.
    2. validity: the valid time of the key in days. Please note that this differs from the validity period for the certificate, which will be determined in Signing the certificate. You can use the same key to request multiple certificates: if your key has a validity of 10 years, but your CA will only sign certificates that are valid for one year, you can use the same key with 10 certificates over time.

    \
    To obtain a certificate that can be used with the private key that was just created a certificate signing request needs to be created. This signing request, when signed by a trusted CA results in the actual certificate which can then be installed in the keystore and used for authentication purposes.\
    To generate certificate signing requests run the following command for all server keystores created so far.

    ```bash
    > keytool -keystore server.keystore.jks -alias localhost -validity {validity} -genkey -keyalg RSA -destkeystoretype pkcs12 -ext SAN=DNS:{FQDN},IP:{IPADDRESS1}
    ```

    This command assumes that you want to add hostname information to the certificate, if this is not the case, you can omit the extension parameter `-ext SAN=DNS:{FQDN},IP:{IPADDRESS1}`. Please see below for more information on this.

    **Host Name Verification**

    Host name verification, when enabled, is the process of checking attributes from the certificate that is presented by the server you are connecting to against the actual hostname or ip address of that server to ensure that you are indeed connecting to the correct server.\
    The main reason for this check is to prevent man-in-the-middle attacks. For Kafka, this check has been disabled by default for a long time, but as of Kafka 2.0.0 host name verification of servers is enabled by default for client connections as well as inter-broker connections.\
    Server host name verification may be disabled by setting `ssl.endpoint.identification.algorithm` to an empty string.\
    For dynamically configured broker listeners, hostname verification may be disabled using `kafka-configs.sh`:\


    ```bash
    > bin/kafka-configs.sh --bootstrap-server localhost:9093 --entity-type brokers --entity-name 0 --alter --add-config "listener.name.internal.ssl.endpoint.identification.algorithm="
    ```

    Note:

    Normally there is no good reason to disable hostname verification apart from being the quickest way to "just get it to work" followed by the promise to "fix it later when there is more time"!\
    Getting hostname verification right is not that hard when done at the right time, but gets much harder once the cluster is up and running - do yourself a favor and do it now!

    If host name verification is enabled, clients will verify the server's fully qualified domain name (FQDN) or ip address against one of the following two fields:

    1. Common Name (CN)
    2. [Subject Alternative Name (SAN)](https://tools.ietf.org/html/rfc5280#section-4.2.1.6)

    \
    While Kafka checks both fields, usage of the common name field for hostname verification has been [deprecated](https://tools.ietf.org/html/rfc2818#section-3.1) since 2000 and should be avoided if possible. In addition the SAN field is much more flexible, allowing for multiple DNS and IP entries to be declared in a certificate.\
    Another advantage is that if the SAN field is used for hostname verification the common name can be set to a more meaningful value for authorization purposes. Since we need the SAN field to be contained in the signed certificate, it will be specified when generating the signing request. It can also be specified when generating the keypair, but this will not automatically be copied into the signing request.\
    To add a SAN field append the following argument `-ext SAN=DNS:{FQDN},IP:{IPADDRESS}` to the keytool command:

    ```bash
    > keytool -keystore server.keystore.jks -alias localhost -validity {validity} -genkey -keyalg RSA -destkeystoretype pkcs12 -ext SAN=DNS:{FQDN},IP:{IPADDRESS1}
    ```
2.  **Creating your own CA**

    After this step each machine in the cluster has a public/private key pair which can already be used to encrypt traffic and a certificate signing request, which is the basis for creating a certificate. To add authentication capabilities this signing request needs to be signed by a trusted authority, which will be created in this step.

    A certificate authority (CA) is responsible for signing certificates. CAs works likes a government that issues passports - the government stamps (signs) each passport so that the passport becomes difficult to forge. Other governments verify the stamps to ensure the passport is authentic. Similarly, the CA signs the certificates, and the cryptography guarantees that a signed certificate is computationally difficult to forge. Thus, as long as the CA is a genuine and trusted authority, the clients have a strong assurance that they are connecting to the authentic machines.

    For this guide we will be our own Certificate Authority. When setting up a production cluster in a corporate environment these certificates would usually be signed by a corporate CA that is trusted throughout the company. Please see Common Pitfalls in Production for some things to consider for this case.

    Due to a [bug](https://www.openssl.org/docs/man1.1.1/man1/x509.html#BUGS) in OpenSSL, the x509 module will not copy requested extension fields from CSRs into the final certificate. Since we want the SAN extension to be present in our certificate to enable hostname verification, we'll use the _ca_ module instead. This requires some additional configuration to be in place before we generate our CA keypair.\
    Save the following listing into a file called openssl-ca.cnf and adjust the values for validity and common attributes as necessary.

    ```bash
    HOME            = .
    RANDFILE        = $ENV::HOME/.rnd

    ####################################################################
    [ ca ]
    default_ca    = CA_default      # The default ca section

    [ CA_default ]

    base_dir      = .
    certificate   = $base_dir/cacert.pem   # The CA certificate
    private_key   = $base_dir/cakey.pem    # The CA private key
    new_certs_dir = $base_dir              # Location for new certs after signing
    database      = $base_dir/index.txt    # Database index file
    serial        = $base_dir/serial.txt   # The current serial number

    default_days     = 1000         # How long to certify for
    default_crl_days = 30           # How long before next CRL
    default_md       = sha256       # Use public key default MD
    preserve         = no           # Keep passed DN ordering

    x509_extensions = ca_extensions # The extensions to add to the cert

    email_in_dn     = no            # Don't concat the email in the DN
    copy_extensions = copy          # Required to copy SANs from CSR to cert

    ####################################################################
    [ req ]
    default_bits       = 4096
    default_keyfile    = cakey.pem
    distinguished_name = ca_distinguished_name
    x509_extensions    = ca_extensions
    string_mask        = utf8only

    ####################################################################
    [ ca_distinguished_name ]
    countryName         = Country Name (2 letter code)
    countryName_default = DE

    stateOrProvinceName         = State or Province Name (full name)
    stateOrProvinceName_default = Test Province

    localityName                = Locality Name (eg, city)
    localityName_default        = Test Town

    organizationName            = Organization Name (eg, company)
    organizationName_default    = Test Company

    organizationalUnitName         = Organizational Unit (eg, division)
    organizationalUnitName_default = Test Unit

    commonName         = Common Name (e.g. server FQDN or YOUR name)
    commonName_default = Test Name

    emailAddress         = Email Address
    emailAddress_default = test@test.com

    ####################################################################
    [ ca_extensions ]

    subjectKeyIdentifier   = hash
    authorityKeyIdentifier = keyid:always, issuer
    basicConstraints       = critical, CA:true
    keyUsage               = keyCertSign, cRLSign

    ####################################################################
    [ signing_policy ]
    countryName            = optional
    stateOrProvinceName    = optional
    localityName           = optional
    organizationName       = optional
    organizationalUnitName = optional
    commonName             = supplied
    emailAddress           = optional

    ####################################################################
    [ signing_req ]
    subjectKeyIdentifier   = hash
    authorityKeyIdentifier = keyid,issuer
    basicConstraints       = CA:FALSE
    keyUsage               = digitalSignature, keyEncipherment
    ```

    Then create a database and serial number file, these will be used to keep track of which certificates were signed with this CA. Both of these are simply text files that reside in the same directory as your CA keys.

    ```bash
    > echo 01 > serial.txt
    > touch index.txt
    ```

    With these steps done you are now ready to generate your CA that will be used to sign certificates later.

    ```bash
    > openssl req -x509 -config openssl-ca.cnf -newkey rsa:4096 -sha256 -nodes -out cacert.pem -outform PEM
    ```

    The CA is simply a public/private key pair and certificate that is signed by itself, and is only intended to sign other certificates.\
    This keypair should be kept very safe, if someone gains access to it, they can create and sign certificates that will be trusted by your infrastructure, which means they will be able to impersonate anybody when connecting to any service that trusts this CA.\
    The next step is to add the generated CA to the \*\*clients' truststore\*\* so that the clients can trust this CA:

    ```bash
    > keytool -keystore client.truststore.jks -alias CARoot -import -file ca-cert
    ```

    Note: If you configure the Kafka brokers to require client authentication by setting ssl.client.auth to be "requested" or "required" in the [Kafka brokers config](https://kafka.apache.org/documentation/#brokerconfigs) then you must provide a truststore for the Kafka brokers as well and it should have all the CA certificates that clients' keys were signed by.

    ```bash
    > keytool -keystore server.truststore.jks -alias CARoot -import -file ca-cert
    ```

    In contrast to the keystore in step 1 that stores each machine's own identity, the truststore of a client stores all the certificates that the client should trust. Importing a certificate into one's truststore also means trusting all certificates that are signed by that certificate. As the analogy above, trusting the government (CA) also means trusting all passports (certificates) that it has issued. This attribute is called the chain of trust, and it is particularly useful when deploying SSL on a large Kafka cluster. You can sign all certificates in the cluster with a single CA, and have all machines share the same truststore that trusts the CA. That way all machines can authenticate all other machines.
3.  **Signing the certificate**

    Then sign it with the CA:

    ```bash
    > openssl ca -config openssl-ca.cnf -policy signing_policy -extensions signing_req -out {server certificate} -infiles {certificate signing request}
    ```

    Finally, you need to import both the certificate of the CA and the signed certificate into the keystore:

    ```bash
    > keytool -keystore {keystore} -alias CARoot -import -file {CA certificate}
    > keytool -keystore {keystore} -alias localhost -import -file cert-signed
    ```

    The definitions of the parameters are the following:

    1. keystore: the location of the keystore
    2. CA certificate: the certificate of the CA
    3. certificate signing request: the csr created with the server key
    4. server certificate: the file to write the signed certificate of the server to

    This will leave you with one truststore called _truststore.jks_ - this can be the same for all clients and brokers and does not contain any sensitive information, so there is no need to secure this.\
    Additionally you will have one _server.keystore.jks_ file per node which contains that nodes keys, certificate and your CAs certificate, please refer to Configuring Kafka Brokers and Configuring Kafka Clients for information on how to use these files.

    For some tooling assistance on this topic, please check out the [easyRSA](https://github.com/OpenVPN/easy-rsa) project which has extensive scripting in place to help with these steps.

    **SSL key and certificates in PEM format**

    From 2.7.0 onwards, SSL key and trust stores can be configured for Kafka brokers and clients directly in the configuration in PEM format. This avoids the need to store separate files on the file system and benefits from password protection features of Kafka configuration. PEM may also be used as the store type for file-based key and trust stores in addition to JKS and PKCS12. To configure PEM key store directly in the broker or client configuration, private key in PEM format should be provided in `ssl.keystore.key` and the certificate chain in PEM format should be provided in `ssl.keystore.certificate.chain`. To configure trust store, trust certificates, e.g. public certificate of CA, should be provided in `ssl.truststore.certificates`. Since PEM is typically stored as multi-line base-64 strings, the configuration value can be included in Kafka configuration as multi-line strings with lines terminating in backslash ('\\') for line continuation.

    Store password configs `ssl.keystore.password` and `ssl.truststore.password` are not used for PEM. If private key is encrypted using a password, the key password must be provided in `ssl.key.password`. Private keys may be provided in unencrypted form without a password. In production deployments, configs should be encrypted or externalized using password protection feature in Kafka in this case. Note that the default SSL engine factory has limited capabilities for decryption of encrypted private keys when external tools like OpenSSL are used for encryption. Third party libraries like BouncyCastle may be integrated with a custom `SslEngineFactory` to support a wider range of encrypted private keys.
4.  **Common Pitfalls in Production**

    The above paragraphs show the process to create your own CA and use it to sign certificates for your cluster. While very useful for sandbox, dev, test, and similar systems, this is usually not the correct process to create certificates for a production cluster in a corporate environment. Enterprises will normally operate their own CA and users can send in CSRs to be signed with this CA, which has the benefit of users not being responsible to keep the CA secure as well as a central authority that everybody can trust. However it also takes away a lot of control over the process of signing certificates from the user. Quite often the persons operating corporate CAs will apply tight restrictions on certificates that can cause issues when trying to use these certificates with Kafka.

    1.  Extended Key Usage\
        Certificates may contain an extension field that controls the purpose for which the certificate can be used. If this field is empty, there are no restrictions on the usage, but if any usage is specified in here, valid SSL implementations have to enforce these usages.\
        Relevant usages for Kafka are:

        * Client authentication
        * Server authentication

        Kafka brokers need both these usages to be allowed, as for intra-cluster communication every broker will behave as both the client and the server towards other brokers. It is not uncommon for corporate CAs to have a signing profile for webservers and use this for Kafka as well, which will only contain the _serverAuth_ usage value and cause the SSL handshake to fail.
    2. Intermediate Certificates\
       Corporate Root CAs are often kept offline for security reasons. To enable day-to-day usage, so called intermediate CAs are created, which are then used to sign the final certificates. When importing a certificate into the keystore that was signed by an intermediate CA it is necessary to provide the entire chain of trust up to the root CA. This can be done by simply _cat_ing the certificate files into one combined certificate file and then importing this with keytool.
    3.  Failure to copy extension fields\
        CA operators are often hesitant to copy and requested extension fields from CSRs and prefer to specify these themselves as this makes it harder for a malicious party to obtain certificates with potentially misleading or fraudulent values. It is advisable to double check signed certificates, whether these contain all requested SAN fields to enable proper hostname verification. The following command can be used to print certificate details to the console, which should be compared with what was originally requested:

        ```bash
        > openssl x509 -in certificate.crt -text -noout
        ```
5.  **Configuring Kafka Brokers**

    If SSL is not enabled for inter-broker communication (see below for how to enable it), both PLAINTEXT and SSL ports will be necessary.

    ```
    listeners=PLAINTEXT://host.name:port,SSL://host.name:port
    ```

    Following SSL configs are needed on the broker side

    ```
    ssl.keystore.location=/var/private/ssl/server.keystore.jks
    ssl.keystore.password=test1234
    ssl.key.password=test1234
    ssl.truststore.location=/var/private/ssl/server.truststore.jks
    ssl.truststore.password=test1234
    ```

    Note: ssl.truststore.password is technically optional but highly recommended. If a password is not set access to the truststore is still available, but integrity checking is disabled. Optional settings that are worth considering:

    1. ssl.client.auth=none ("required" => client authentication is required, "requested" => client authentication is requested and client without certs can still connect. The usage of "requested" is discouraged as it provides a false sense of security and misconfigured clients will still connect successfully.)
    2. ssl.cipher.suites (Optional). A cipher suite is a named combination of authentication, encryption, MAC and key exchange algorithm used to negotiate the security settings for a network connection using TLS or SSL network protocol. (Default is an empty list)
    3. ssl.enabled.protocols=TLSv1.2,TLSv1.1,TLSv1 (list out the SSL protocols that you are going to accept from clients. Do note that SSL is deprecated in favor of TLS and using SSL in production is not recommended)
    4. ssl.keystore.type=JKS
    5. ssl.truststore.type=JKS
    6. ssl.secure.random.implementation=SHA1PRNG

    If you want to enable SSL for inter-broker communication, add the following to the server.properties file (it defaults to PLAINTEXT)

    ```
    security.inter.broker.protocol=SSL
    ```

    Due to import regulations in some countries, the Oracle implementation limits the strength of cryptographic algorithms available by default. If stronger algorithms are needed (for example, AES with 256-bit keys), the [JCE Unlimited Strength Jurisdiction Policy Files](https://www.oracle.com/technetwork/java/javase/downloads/index.html) must be obtained and installed in the JDK/JRE. See the [JCA Providers Documentation](https://docs.oracle.com/javase/8/docs/technotes/guides/security/SunProviders.html) for more information.

    The JRE/JDK will have a default pseudo-random number generator (PRNG) that is used for cryptography operations, so it is not required to configure the implementation used with the `ssl.secure.random.implementation`. However, there are performance issues with some implementations (notably, the default chosen on Linux systems, `NativePRNG`, utilizes a global lock). In cases where performance of SSL connections becomes an issue, consider explicitly setting the implementation to be used. The `SHA1PRNG` implementation is non-blocking, and has shown very good performance characteristics under heavy load (50 MB/sec of produced messages, plus replication traffic, per-broker).

    Once you start the broker you should be able to see in the server.log

    ```
    with addresses: PLAINTEXT -> EndPoint(192.168.64.1,9092,PLAINTEXT),SSL -> EndPoint(192.168.64.1,9093,SSL)
    ```

    To check quickly if the server keystore and truststore are setup properly you can run the following command

    ```bash
    > openssl s_client -debug -connect localhost:9093 -tls1
    ```

    (Note: TLSv1 should be listed under ssl.enabled.protocols)\
    In the output of this command you should see server's certificate:

    ```
    -----BEGIN CERTIFICATE-----
    {variable sized random bytes}
    -----END CERTIFICATE-----
    subject=/C=US/ST=CA/L=Santa Clara/O=org/OU=org/CN=Sriharsha Chintalapani
    issuer=/C=US/ST=CA/L=Santa Clara/O=org/OU=org/CN=kafka/emailAddress=test@test.com
    ```

    If the certificate does not show up or if there are any other error messages then your keystore is not setup properly.
6.  **Configuring Kafka Clients**

    SSL is supported only for the new Kafka Producer and Consumer, the older API is not supported. The configs for SSL will be the same for both producer and consumer.\
    If client authentication is not required in the broker, then the following is a minimal configuration example:

    ```
    security.protocol=SSL
    ssl.truststore.location=/var/private/ssl/client.truststore.jks
    ssl.truststore.password=test1234
    ```

    Note: ssl.truststore.password is technically optional but highly recommended. If a password is not set access to the truststore is still available, but integrity checking is disabled. If client authentication is required, then a keystore must be created like in step 1 and the following must also be configured:

    ```
    ssl.keystore.location=/var/private/ssl/client.keystore.jks
    ssl.keystore.password=test1234
    ssl.key.password=test1234
    ```

    Other configuration settings that may also be needed depending on our requirements and the broker configuration:

    1. ssl.provider (Optional). The name of the security provider used for SSL connections. Default value is the default security provider of the JVM.
    2. ssl.cipher.suites (Optional). A cipher suite is a named combination of authentication, encryption, MAC and key exchange algorithm used to negotiate the security settings for a network connection using TLS or SSL network protocol.
    3. ssl.enabled.protocols=TLSv1.2,TLSv1.1,TLSv1. It should list at least one of the protocols configured on the broker side
    4. ssl.truststore.type=JKS
    5. ssl.keystore.type=JKS

    \
    Examples using console-producer and console-consumer:

    ```bash
    > kafka-console-producer.sh --bootstrap-server localhost:9093 --topic test --producer.config client-ssl.properties
    > kafka-console-consumer.sh --bootstrap-server localhost:9093 --topic test --consumer.config client-ssl.properties
    ```

## Authentication using SASL <a href="#security_sasl" id="security_sasl"></a>

1.  **JAAS configuration**

    Kafka uses the Java Authentication and Authorization Service ([JAAS](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jaas/JAASRefGuide.html)) for SASL configuration.

    1.  **JAAS configuration for Kafka brokers**

        `KafkaServer` is the section name in the JAAS file used by each KafkaServer/Broker. This section provides SASL configuration options for the broker including any SASL client connections made by the broker for inter-broker communication. If multiple listeners are configured to use SASL, the section name may be prefixed with the listener name in lower-case followed by a period, e.g. `sasl_ssl.KafkaServer`.

        `Client` section is used to authenticate a SASL connection with zookeeper. It also allows the brokers to set SASL ACL on zookeeper nodes which locks these nodes down so that only the brokers can modify it. It is necessary to have the same principal name across all brokers. If you want to use a section name other than Client, set the system property `zookeeper.sasl.clientconfig` to the appropriate name (_e.g._, `-Dzookeeper.sasl.clientconfig=ZkClient`).

        ZooKeeper uses "zookeeper" as the service name by default. If you want to change this, set the system property `zookeeper.sasl.client.username` to the appropriate name (_e.g._, `-Dzookeeper.sasl.client.username=zk`).

        Brokers may also configure JAAS using the broker configuration property `sasl.jaas.config`. The property name must be prefixed with the listener prefix including the SASL mechanism, i.e. `listener.name.{listenerName}.{saslMechanism}.sasl.jaas.config`. Only one login module may be specified in the config value. If multiple mechanisms are configured on a listener, configs must be provided for each mechanism using the listener and mechanism prefix. For example,

        ```
        listener.name.sasl_ssl.scram-sha-256.sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required \
            username="admin" \
            password="admin-secret";
        listener.name.sasl_ssl.plain.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
            username="admin" \
            password="admin-secret" \
            user_admin="admin-secret" \
            user_alice="alice-secret";
        ```

        If JAAS configuration is defined at different levels, the order of precedence used is:

        * Broker configuration property `listener.name.{listenerName}.{saslMechanism}.sasl.jaas.config`
        * `{listenerName}.KafkaServer` section of static JAAS configuration
        * `KafkaServer` section of static JAAS configuration

        Note that ZooKeeper JAAS config may only be configured using static JAAS configuration.

        See [GSSAPI (Kerberos)](https://kafka.apache.org/documentation/#security\_sasl\_kerberos\_brokerconfig), [PLAIN](https://kafka.apache.org/documentation/#security\_sasl\_plain\_brokerconfig), [SCRAM](https://kafka.apache.org/documentation/#security\_sasl\_scram\_brokerconfig) or [OAUTHBEARER](https://kafka.apache.org/documentation/#security\_sasl\_oauthbearer\_brokerconfig) for example broker configurations.
    2.  **JAAS configuration for Kafka clients**

        Clients may configure JAAS using the client configuration property [sasl.jaas.config](https://kafka.apache.org/documentation/#security\_client\_dynamicjaas) or using the [static JAAS config file](https://kafka.apache.org/documentation/#security\_client\_staticjaas) similar to brokers.

        1.  **JAAS configuration using client configuration property**

            Clients may specify JAAS configuration as a producer or consumer property without creating a physical configuration file. This mode also enables different producers and consumers within the same JVM to use different credentials by specifying different properties for each client. If both static JAAS configuration system property `java.security.auth.login.config` and client property `sasl.jaas.config` are specified, the client property will be used.

            See [GSSAPI (Kerberos)](https://kafka.apache.org/documentation/#security\_sasl\_kerberos\_clientconfig), [PLAIN](https://kafka.apache.org/documentation/#security\_sasl\_plain\_clientconfig), [SCRAM](https://kafka.apache.org/documentation/#security\_sasl\_scram\_clientconfig) or [OAUTHBEARER](https://kafka.apache.org/documentation/#security\_sasl\_oauthbearer\_clientconfig) for example configurations.
        2.  **JAAS configuration using static config file**

            To configure SASL authentication on the clients using static JAAS config file:

            1.  Add a JAAS config file with a client login section named `KafkaClient`. Configure a login module in `KafkaClient` for the selected mechanism as described in the examples for setting up [GSSAPI (Kerberos)](https://kafka.apache.org/documentation/#security\_sasl\_kerberos\_clientconfig), [PLAIN](https://kafka.apache.org/documentation/#security\_sasl\_plain\_clientconfig), [SCRAM](https://kafka.apache.org/documentation/#security\_sasl\_scram\_clientconfig) or [OAUTHBEARER](https://kafka.apache.org/documentation/#security\_sasl\_oauthbearer\_clientconfig). For example, [GSSAPI](https://kafka.apache.org/documentation/#security\_sasl\_gssapi\_clientconfig) credentials may be configured as:

                ```
                KafkaClient {
                    com.sun.security.auth.module.Krb5LoginModule required
                    useKeyTab=true
                    storeKey=true
                    keyTab="/etc/security/keytabs/kafka_client.keytab"
                    principal="kafka-client-1@EXAMPLE.COM";
                };
                ```
            2.  Pass the JAAS config file location as JVM parameter to each client JVM. For example:

                ```bash
                -Djava.security.auth.login.config=/etc/kafka/kafka_client_jaas.conf
                ```
2.  **SASL configuration**

    SASL may be used with PLAINTEXT or SSL as the transport layer using the security protocol SASL\_PLAINTEXT or SASL\_SSL respectively. If SASL\_SSL is used, then SSL must also be configured.

    1.  **SASL mechanisms**

        Kafka supports the following SASL mechanisms:

        * GSSAPI (Kerberos)
        * PLAIN
        * SCRAM-SHA-256
        * SCRAM-SHA-512
        * OAUTHBEARER
    2. **SASL configuration for Kafka brokers**
       1.  Configure a SASL port in server.properties, by adding at least one of SASL\_PLAINTEXT or SASL\_SSL to the _listeners_ parameter, which contains one or more comma-separated values:

           ```
           listeners=SASL_PLAINTEXT://host.name:port
           ```

           If you are only configuring a SASL port (or if you want the Kafka brokers to authenticate each other using SASL) then make sure you set the same SASL protocol for inter-broker communication:

           ```
           security.inter.broker.protocol=SASL_PLAINTEXT (or SASL_SSL)
           ```
       2. Select one or more supported mechanisms to enable in the broker and follow the steps to configure SASL for the mechanism.&#x20;
    3.  **SASL configuration for Kafka clients**

        SASL authentication is only supported for the new Java Kafka producer and consumer, the older API is not supported.

        To configure SASL authentication on the clients, select a SASL mechanism that is enabled in the broker for client authentication and follow the steps to configure SASL for the selected mechanism.

        Note: When establishing connections to brokers via SASL, clients may perform a reverse DNS lookup of the broker address. Due to how the JRE implements reverse DNS lookups, clients may observe slow SASL handshakes if fully qualified domain names are not used, for both the client's `bootstrap.servers` and a broker's [`advertised.listeners`](https://kafka.apache.org/documentation/#brokerconfigs\_advertised.listeners).
3. **Authentication using SASL/Kerberos**
   1. **Prerequisites**
      1. Kerberos\
         If your organization is already using a Kerberos server (for example, by using Active Directory), there is no need to install a new server just for Kafka. Otherwise you will need to install one, your Linux vendor likely has packages for Kerberos and a short guide on how to install and configure it ([Ubuntu](https://help.ubuntu.com/community/Kerberos), [Redhat](https://access.redhat.com/documentation/en-US/Red\_Hat\_Enterprise\_Linux/6/html/Managing\_Smart\_Cards/installing-kerberos.html)). Note that if you are using Oracle Java, you will need to download JCE policy files for your Java version and copy them to $JAVA\_HOME/jre/lib/security.
      2.  Create Kerberos Principals\
          If you are using the organization's Kerberos or Active Directory server, ask your Kerberos administrator for a principal for each Kafka broker in your cluster and for every operating system user that will access Kafka with Kerberos authentication (via clients and tools).\
          If you have installed your own Kerberos, you will need to create these principals yourself using the following commands:

          ```bash
          > sudo /usr/sbin/kadmin.local -q 'addprinc -randkey kafka/{hostname}@{REALM}'
          > sudo /usr/sbin/kadmin.local -q "ktadd -k /etc/security/keytabs/{keytabname}.keytab kafka/{hostname}@{REALM}"
          ```
      3. Make sure all hosts can be reachable using hostnames - it is a Kerberos requirement that all your hosts can be resolved with their FQDNs.
   2. **Configuring Kafka Brokers**
      1.  Add a suitably modified JAAS file similar to the one below to each Kafka broker's config directory, let's call it kafka\_server\_jaas.conf for this example (note that each broker should have its own keytab):

          ```
          KafkaServer {
              com.sun.security.auth.module.Krb5LoginModule required
              useKeyTab=true
              storeKey=true
              keyTab="/etc/security/keytabs/kafka_server.keytab"
              principal="kafka/kafka1.hostname.com@EXAMPLE.COM";
          };

          // Zookeeper client authentication
          Client {
              com.sun.security.auth.module.Krb5LoginModule required
              useKeyTab=true
              storeKey=true
              keyTab="/etc/security/keytabs/kafka_server.keytab"
              principal="kafka/kafka1.hostname.com@EXAMPLE.COM";
          };
          ```

          `KafkaServer` section in the JAAS file tells the broker which principal to use and the location of the keytab where this principal is stored. It allows the broker to login using the keytab specified in this section. See [notes](https://kafka.apache.org/documentation/#security\_jaas\_broker) for more details on Zookeeper SASL configuration.
      2.  Pass the JAAS and optionally the krb5 file locations as JVM parameters to each Kafka broker (see [here](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jgss/tutorials/KerberosReq.html) for more details):

          ```bash
          -Djava.security.krb5.conf=/etc/kafka/krb5.conf
          -Djava.security.auth.login.config=/etc/kafka/kafka_server_jaas.conf
          ```
      3. Make sure the keytabs configured in the JAAS file are readable by the operating system user who is starting kafka broker.
      4.  Configure SASL port and SASL mechanisms in server.properties as described [here](https://kafka.apache.org/documentation/#security\_sasl\_brokerconfig). For example:

          ```
          listeners=SASL_PLAINTEXT://host.name:port
          security.inter.broker.protocol=SASL_PLAINTEXT
          sasl.mechanism.inter.broker.protocol=GSSAPI
          sasl.enabled.mechanisms=GSSAPI
          ```

          We must also configure the service name in server.properties, which should match the principal name of the kafka brokers. In the above example, principal is "kafka/kafka1.hostname.com@EXAMPLE.com", so:

          ```
          sasl.kerberos.service.name=kafka
          ```
   3.  **Configuring Kafka Clients**

       To configure SASL authentication on the clients:

       1.  Clients (producers, consumers, connect workers, etc) will authenticate to the cluster with their own principal (usually with the same name as the user running the client), so obtain or create these principals as needed. Then configure the JAAS configuration property for each client. Different clients within a JVM may run as different users by specifying different principals. The property `sasl.jaas.config` in producer.properties or consumer.properties describes how clients like producer and consumer can connect to the Kafka Broker. The following is an example configuration for a client using a keytab (recommended for long-running processes):

           ```
           sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required \
               useKeyTab=true \
               storeKey=true  \
               keyTab="/etc/security/keytabs/kafka_client.keytab" \
               principal="kafka-client-1@EXAMPLE.COM";
           ```

           For command-line utilities like kafka-console-consumer or kafka-console-producer, kinit can be used along with "useTicketCache=true" as in:

           ```
           sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required \
               useTicketCache=true;
           ```

           JAAS configuration for clients may alternatively be specified as a JVM parameter similar to brokers as described [here](https://kafka.apache.org/documentation/#security\_client\_staticjaas). Clients use the login section named `KafkaClient`. This option allows only one user for all client connections from a JVM.
       2. Make sure the keytabs configured in the JAAS configuration are readable by the operating system user who is starting kafka client.
       3.  Optionally pass the krb5 file locations as JVM parameters to each client JVM (see [here](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jgss/tutorials/KerberosReq.html) for more details):

           ```bash
           -Djava.security.krb5.conf=/etc/kafka/krb5.conf
           ```
       4.  Configure the following properties in producer.properties or consumer.properties:

           ```
           security.protocol=SASL_PLAINTEXT (or SASL_SSL)
           sasl.mechanism=GSSAPI
           sasl.kerberos.service.name=kafka
           ```
4.  **Authentication using SASL/PLAIN**

    SASL/PLAIN is a simple username/password authentication mechanism that is typically used with TLS for encryption to implement secure authentication. Kafka supports a default implementation for SASL/PLAIN which can be extended for production use as described [here](https://kafka.apache.org/documentation/#security\_sasl\_plain\_production).

    Under the default implementation of `principal.builder.class`, the username is used as the authenticated `Principal` for configuration of ACLs etc.

    1. **Configuring Kafka Brokers**
       1.  Add a suitably modified JAAS file similar to the one below to each Kafka broker's config directory, let's call it kafka\_server\_jaas.conf for this example:

           ```
           KafkaServer {
               org.apache.kafka.common.security.plain.PlainLoginModule required
               username="admin"
               password="admin-secret"
               user_admin="admin-secret"
               user_alice="alice-secret";
           };
           ```

           This configuration defines two users (_admin_ and _alice_). The properties `username` and `password` in the `KafkaServer` section are used by the broker to initiate connections to other brokers. In this example, _admin_ is the user for inter-broker communication. The set of properties `user_`_`userName`_ defines the passwords for all users that connect to the broker and the broker validates all client connections including those from other brokers using these properties.
       2.  Pass the JAAS config file location as JVM parameter to each Kafka broker:

           ```bash
           -Djava.security.auth.login.config=/etc/kafka/kafka_server_jaas.conf
           ```
       3.  Configure SASL port and SASL mechanisms in server.properties as described [here](https://kafka.apache.org/documentation/#security\_sasl\_brokerconfig). For example:

           ```
           listeners=SASL_SSL://host.name:port
           security.inter.broker.protocol=SASL_SSL
           sasl.mechanism.inter.broker.protocol=PLAIN
           sasl.enabled.mechanisms=PLAIN
           ```
    2.  **Configuring Kafka Clients**

        To configure SASL authentication on the clients:

        1.  Configure the JAAS configuration property for each client in producer.properties or consumer.properties. The login module describes how the clients like producer and consumer can connect to the Kafka Broker. The following is an example configuration for a client for the PLAIN mechanism:

            ```
            sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
                username="alice" \
                password="alice-secret";
            ```

            The options `username` and `password` are used by clients to configure the user for client connections. In this example, clients connect to the broker as user _alice_. Different clients within a JVM may connect as different users by specifying different user names and passwords in `sasl.jaas.config`.

            JAAS configuration for clients may alternatively be specified as a JVM parameter similar to brokers as described [here](https://kafka.apache.org/documentation/#security\_client\_staticjaas). Clients use the login section named `KafkaClient`. This option allows only one user for all client connections from a JVM.
        2.  Configure the following properties in producer.properties or consumer.properties:

            ```
            security.protocol=SASL_SSL
            sasl.mechanism=PLAIN
            ```
    3. **Use of SASL/PLAIN in production**
       * SASL/PLAIN should be used only with SSL as transport layer to ensure that clear passwords are not transmitted on the wire without encryption.
       * The default implementation of SASL/PLAIN in Kafka specifies usernames and passwords in the JAAS configuration file as shown [here](https://kafka.apache.org/documentation/#security\_sasl\_plain\_brokerconfig). From Kafka version 2.0 onwards, you can avoid storing clear passwords on disk by configuring your own callback handlers that obtain username and password from an external source using the configuration options `sasl.server.callback.handler.class` and `sasl.client.callback.handler.class`.
       * In production systems, external authentication servers may implement password authentication. From Kafka version 2.0 onwards, you can plug in your own callback handlers that use external authentication servers for password verification by configuring `sasl.server.callback.handler.class`.
5.  **Authentication using SASL/SCRAM**

    Salted Challenge Response Authentication Mechanism (SCRAM) is a family of SASL mechanisms that addresses the security concerns with traditional mechanisms that perform username/password authentication like PLAIN and DIGEST-MD5. The mechanism is defined in [RFC 5802](https://tools.ietf.org/html/rfc5802). Kafka supports [SCRAM-SHA-256](https://tools.ietf.org/html/rfc7677) and SCRAM-SHA-512 which can be used with TLS to perform secure authentication. Under the default implementation of `principal.builder.class`, the username is used as the authenticated `Principal` for configuration of ACLs etc. The default SCRAM implementation in Kafka stores SCRAM credentials in Zookeeper and is suitable for use in Kafka installations where Zookeeper is on a private network. Refer to Security Considerations for more details.

    1.  **Creating SCRAM Credentials**

        The SCRAM implementation in Kafka uses Zookeeper as credential store. Credentials can be created in Zookeeper using `kafka-configs.sh`. For each SCRAM mechanism enabled, credentials must be created by adding a config with the mechanism name. Credentials for inter-broker communication must be created before Kafka brokers are started. Client credentials may be created and updated dynamically and updated credentials will be used to authenticate new connections.

        Create SCRAM credentials for user _alice_ with password _alice-secret_:

        ```bash
        > bin/kafka-configs.sh --zookeeper localhost:2182 --zk-tls-config-file zk_tls_config.properties --alter --add-config 'SCRAM-SHA-256=[iterations=8192,password=alice-secret],SCRAM-SHA-512=[password=alice-secret]' --entity-type users --entity-name alice
        ```

        The default iteration count of 4096 is used if iterations are not specified. A random salt is created and the SCRAM identity consisting of salt, iterations, StoredKey and ServerKey are stored in Zookeeper. See [RFC 5802](https://tools.ietf.org/html/rfc5802) for details on SCRAM identity and the individual fields.

        The following examples also require a user _admin_ for inter-broker communication which can be created using:

        ```bash
        > bin/kafka-configs.sh --zookeeper localhost:2182 --zk-tls-config-file zk_tls_config.properties --alter --add-config 'SCRAM-SHA-256=[password=admin-secret],SCRAM-SHA-512=[password=admin-secret]' --entity-type users --entity-name admin
        ```

        Existing credentials may be listed using the _--describe_ option:

        ```bash
        > bin/kafka-configs.sh --zookeeper localhost:2182 --zk-tls-config-file zk_tls_config.properties --describe --entity-type users --entity-name alice
        ```

        Credentials may be deleted for one or more SCRAM mechanisms using the _--alter --delete-config_ option:

        ```bash
        > bin/kafka-configs.sh --zookeeper localhost:2182 --zk-tls-config-file zk_tls_config.properties --alter --delete-config 'SCRAM-SHA-512' --entity-type users --entity-name alice
        ```
    2. **Configuring Kafka Brokers**
       1.  Add a suitably modified JAAS file similar to the one below to each Kafka broker's config directory, let's call it kafka\_server\_jaas.conf for this example:

           ```
           KafkaServer {
               org.apache.kafka.common.security.scram.ScramLoginModule required
               username="admin"
               password="admin-secret";
           };
           ```

           The properties `username` and `password` in the `KafkaServer` section are used by the broker to initiate connections to other brokers. In this example, _admin_ is the user for inter-broker communication.
       2.  Pass the JAAS config file location as JVM parameter to each Kafka broker:

           ```bash
           -Djava.security.auth.login.config=/etc/kafka/kafka_server_jaas.conf
           ```
       3.  Configure SASL port and SASL mechanisms in server.properties as described [here](https://kafka.apache.org/documentation/#security\_sasl\_brokerconfig). For example:

           ```
           listeners=SASL_SSL://host.name:port
           security.inter.broker.protocol=SASL_SSL
           sasl.mechanism.inter.broker.protocol=SCRAM-SHA-256 (or SCRAM-SHA-512)
           sasl.enabled.mechanisms=SCRAM-SHA-256 (or SCRAM-SHA-512)
           ```
    3.  **Configuring Kafka Clients**

        To configure SASL authentication on the clients:

        1.  Configure the JAAS configuration property for each client in producer.properties or consumer.properties. The login module describes how the clients like producer and consumer can connect to the Kafka Broker. The following is an example configuration for a client for the SCRAM mechanisms:

            ```
            sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required \
                username="alice" \
                password="alice-secret";
            ```

            The options `username` and `password` are used by clients to configure the user for client connections. In this example, clients connect to the broker as user _alice_. Different clients within a JVM may connect as different users by specifying different user names and passwords in `sasl.jaas.config`.

            JAAS configuration for clients may alternatively be specified as a JVM parameter similar to brokers as described [here](https://kafka.apache.org/documentation/#security\_client\_staticjaas). Clients use the login section named `KafkaClient`. This option allows only one user for all client connections from a JVM.
        2.  Configure the following properties in producer.properties or consumer.properties:

            ```
            security.protocol=SASL_SSL
            sasl.mechanism=SCRAM-SHA-256 (or SCRAM-SHA-512)
            ```
    4. **Security Considerations for SASL/SCRAM**
       * The default implementation of SASL/SCRAM in Kafka stores SCRAM credentials in Zookeeper. This is suitable for production use in installations where Zookeeper is secure and on a private network.
       * Kafka supports only the strong hash functions SHA-256 and SHA-512 with a minimum iteration count of 4096. Strong hash functions combined with strong passwords and high iteration counts protect against brute force attacks if Zookeeper security is compromised.
       * SCRAM should be used only with TLS-encryption to prevent interception of SCRAM exchanges. This protects against dictionary or brute force attacks and against impersonation if Zookeeper is compromised.
       * From Kafka version 2.0 onwards, the default SASL/SCRAM credential store may be overridden using custom callback handlers by configuring `sasl.server.callback.handler.class` in installations where Zookeeper is not secure.
       * For more details on security considerations, refer to [RFC 5802](https://tools.ietf.org/html/rfc5802#section-9).
6.  **Authentication using SASL/OAUTHBEARER**

    The [OAuth 2 Authorization Framework](https://tools.ietf.org/html/rfc6749) "enables a third-party application to obtain limited access to an HTTP service, either on behalf of a resource owner by orchestrating an approval interaction between the resource owner and the HTTP service, or by allowing the third-party application to obtain access on its own behalf." The SASL OAUTHBEARER mechanism enables the use of the framework in a SASL (i.e. a non-HTTP) context; it is defined in [RFC 7628](https://tools.ietf.org/html/rfc7628). The default OAUTHBEARER implementation in Kafka creates and validates [Unsecured JSON Web Tokens](https://tools.ietf.org/html/rfc7515#appendix-A.5) and is only suitable for use in non-production Kafka installations. Refer to Security Considerations for more details.

    Under the default implementation of `principal.builder.class`, the principalName of OAuthBearerToken is used as the authenticated `Principal` for configuration of ACLs etc.

    1. **Configuring Kafka Brokers**
       1.  Add a suitably modified JAAS file similar to the one below to each Kafka broker's config directory, let's call it kafka\_server\_jaas.conf for this example:

           ```
           KafkaServer {
               org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required
               unsecuredLoginStringClaim_sub="admin";
           };
           ```

           The property `unsecuredLoginStringClaim_sub` in the `KafkaServer` section is used by the broker when it initiates connections to other brokers. In this example, _admin_ will appear in the subject (`sub`) claim and will be the user for inter-broker communication.
       2.  Pass the JAAS config file location as JVM parameter to each Kafka broker:

           ```bash
           -Djava.security.auth.login.config=/etc/kafka/kafka_server_jaas.conf
           ```
       3.  Configure SASL port and SASL mechanisms in server.properties as described [here](https://kafka.apache.org/documentation/#security\_sasl\_brokerconfig). For example:

           ```
           listeners=SASL_SSL://host.name:port (or SASL_PLAINTEXT if non-production)
           security.inter.broker.protocol=SASL_SSL (or SASL_PLAINTEXT if non-production)
           sasl.mechanism.inter.broker.protocol=OAUTHBEARER
           sasl.enabled.mechanisms=OAUTHBEARER
           ```
    2.  **Configuring Kafka Clients**

        To configure SASL authentication on the clients:

        1.  Configure the JAAS configuration property for each client in producer.properties or consumer.properties. The login module describes how the clients like producer and consumer can connect to the Kafka Broker. The following is an example configuration for a client for the OAUTHBEARER mechanisms:

            ```
            sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required \
                unsecuredLoginStringClaim_sub="alice";
            ```

            The option `unsecuredLoginStringClaim_sub` is used by clients to configure the subject (`sub`) claim, which determines the user for client connections. In this example, clients connect to the broker as user _alice_. Different clients within a JVM may connect as different users by specifying different subject (`sub`) claims in `sasl.jaas.config`.

            JAAS configuration for clients may alternatively be specified as a JVM parameter similar to brokers as described [here](https://kafka.apache.org/documentation/#security\_client\_staticjaas). Clients use the login section named `KafkaClient`. This option allows only one user for all client connections from a JVM.
        2.  Configure the following properties in producer.properties or consumer.properties:

            ```
            security.protocol=SASL_SSL (or SASL_PLAINTEXT if non-production)
            sasl.mechanism=OAUTHBEARER
            ```
        3. The default implementation of SASL/OAUTHBEARER depends on the jackson-databind library. Since it's an optional dependency, users have to configure it as a dependency via their build tool.
    3. **Unsecured Token Creation Options for SASL/OAUTHBEARER**
       * The default implementation of SASL/OAUTHBEARER in Kafka creates and validates [Unsecured JSON Web Tokens](https://tools.ietf.org/html/rfc7515#appendix-A.5). While suitable only for non-production use, it does provide the flexibility to create arbitrary tokens in a DEV or TEST environment.
       *   Here are the various supported JAAS module options on the client side (and on the broker side if OAUTHBEARER is the inter-broker protocol):

           | JAAS Module Option for Unsecured Token Creation   | Documentation                                                                                                                                                                                                                                                                                                                                |
           | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
           | `unsecuredLoginStringClaim_<claimname>="value"`   | Creates a `String` claim with the given name and value. Any valid claim name can be specified except '`iat`' and '`exp`' (these are automatically generated).                                                                                                                                                                                |
           | `unsecuredLoginNumberClaim_<claimname>="value"`   | Creates a `Number` claim with the given name and value. Any valid claim name can be specified except '`iat`' and '`exp`' (these are automatically generated).                                                                                                                                                                                |
           | `unsecuredLoginListClaim_<claimname>="value"`     | Creates a `String List` claim with the given name and values parsed from the given value where the first character is taken as the delimiter. For example: `unsecuredLoginListClaim_fubar="\|value1\|value2"`. Any valid claim name can be specified except '`iat`' and '`exp`' (these are automatically generated).                         |
           | `unsecuredLoginExtension_<extensionname>="value"` | Creates a `String` extension with the given name and value. For example: `unsecuredLoginExtension_traceId="123"`. A valid extension name is any sequence of lowercase or uppercase alphabet characters. In addition, the "auth" extension name is reserved. A valid extension value is any combination of characters with ASCII codes 1-127. |
           | `unsecuredLoginPrincipalClaimName`                | Set to a custom claim name if you wish the name of the `String` claim holding the principal name to be something other than '`sub`'.                                                                                                                                                                                                         |
           | `unsecuredLoginLifetimeSeconds`                   | Set to an integer value if the token expiration is to be set to something other than the default value of 3600 seconds (which is 1 hour). The '`exp`' claim will be set to reflect the expiration time.                                                                                                                                      |
           | `unsecuredLoginScopeClaimName`                    | Set to a custom claim name if you wish the name of the `String` or `String List` claim holding any token scope to be something other than '`scope`'.                                                                                                                                                                                         |
    4. **Unsecured Token Validation Options for SASL/OAUTHBEARER**
       *   Here are the various supported JAAS module options on the broker side for [Unsecured JSON Web Token](https://tools.ietf.org/html/rfc7515#appendix-A.5) validation:

           | JAAS Module Option for Unsecured Token Validation | Documentation                                                                                                                                                                          |
           | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
           | `unsecuredValidatorPrincipalClaimName="value"`    | Set to a non-empty value if you wish a particular `String` claim holding a principal name to be checked for existence; the default is to check for the existence of the '`sub`' claim. |
           | `unsecuredValidatorScopeClaimName="value"`        | Set to a custom claim name if you wish the name of the `String` or `String List` claim holding any token scope to be something other than '`scope`'.                                   |
           | `unsecuredValidatorRequiredScope="value"`         | Set to a space-delimited list of scope values if you wish the `String/String List` claim holding the token scope to be checked to make sure it contains certain values.                |
           | `unsecuredValidatorAllowableClockSkewMs="value"`  | Set to a positive integer value if you wish to allow up to some number of positive milliseconds of clock skew (the default is 0).                                                      |
       * The default unsecured SASL/OAUTHBEARER implementation may be overridden (and must be overridden in production environments) using custom login and SASL Server callback handlers.
       * For more details on security considerations, refer to [RFC 6749, Section 10](https://tools.ietf.org/html/rfc6749#section-10).
    5.  **Token Refresh for SASL/OAUTHBEARER**

        Kafka periodically refreshes any token before it expires so that the client can continue to make connections to brokers. The parameters that impact how the refresh algorithm operates are specified as part of the producer/consumer/broker configuration and are as follows. See the documentation for these properties elsewhere for details. The default values are usually reasonable, in which case these configuration parameters would not need to be explicitly set.

        | Producer/Consumer/Broker Configuration Property |
        | ----------------------------------------------- |
        | `sasl.login.refresh.window.factor`              |
        | `sasl.login.refresh.window.jitter`              |
        | `sasl.login.refresh.min.period.seconds`         |
        | `sasl.login.refresh.min.buffer.seconds`         |
    6.  **Secure/Production Use of SASL/OAUTHBEARER**

        Production use cases will require writing an implementation of `org.apache.kafka.common.security.auth.AuthenticateCallbackHandler` that can handle an instance of `org.apache.kafka.common.security.oauthbearer.OAuthBearerTokenCallback` and declaring it via either the `sasl.login.callback.handler.class` configuration option for a non-broker client or via the `listener.name.sasl_ssl.oauthbearer.sasl.login.callback.handler.class` configuration option for brokers (when SASL/OAUTHBEARER is the inter-broker protocol).

        Production use cases will also require writing an implementation of `org.apache.kafka.common.security.auth.AuthenticateCallbackHandler` that can handle an instance of `org.apache.kafka.common.security.oauthbearer.OAuthBearerValidatorCallback` and declaring it via the `listener.name.sasl_ssl.oauthbearer.sasl.server.callback.handler.class` broker configuration option.
    7. **Security Considerations for SASL/OAUTHBEARER**
       * The default implementation of SASL/OAUTHBEARER in Kafka creates and validates [Unsecured JSON Web Tokens](https://tools.ietf.org/html/rfc7515#appendix-A.5). This is suitable only for non-production use.
       * OAUTHBEARER should be used in production enviromnments only with TLS-encryption to prevent interception of tokens.
       * The default unsecured SASL/OAUTHBEARER implementation may be overridden (and must be overridden in production environments) using custom login and SASL Server callback handlers as described above.
       * For more details on OAuth 2 security considerations in general, refer to [RFC 6749, Section 10](https://tools.ietf.org/html/rfc6749#section-10).
7. **Enabling multiple SASL mechanisms in a broker**
   1.  Specify configuration for the login modules of all enabled mechanisms in the `KafkaServer` section of the JAAS config file. For example:

       ```
       KafkaServer {
           com.sun.security.auth.module.Krb5LoginModule required
           useKeyTab=true
           storeKey=true
           keyTab="/etc/security/keytabs/kafka_server.keytab"
           principal="kafka/kafka1.hostname.com@EXAMPLE.COM";

           org.apache.kafka.common.security.plain.PlainLoginModule required
           username="admin"
           password="admin-secret"
           user_admin="admin-secret"
           user_alice="alice-secret";
       };
       ```
   2.  Enable the SASL mechanisms in server.properties:

       ```
       sasl.enabled.mechanisms=GSSAPI,PLAIN,SCRAM-SHA-256,SCRAM-SHA-512,OAUTHBEARER
       ```
   3.  Specify the SASL security protocol and mechanism for inter-broker communication in server.properties if required:

       ```
       security.inter.broker.protocol=SASL_PLAINTEXT (or SASL_SSL)
       sasl.mechanism.inter.broker.protocol=GSSAPI (or one of the other enabled mechanisms)
       ```
   4. Follow the mechanism-specific steps in [GSSAPI (Kerberos)](https://kafka.apache.org/documentation/#security\_sasl\_kerberos\_brokerconfig), [PLAIN](https://kafka.apache.org/documentation/#security\_sasl\_plain\_brokerconfig), [SCRAM](https://kafka.apache.org/documentation/#security\_sasl\_scram\_brokerconfig) and [OAUTHBEARER](https://kafka.apache.org/documentation/#security\_sasl\_oauthbearer\_brokerconfig) to configure SASL for the enabled mechanisms.
8.  **Modifying SASL mechanism in a Running Cluster**

    SASL mechanism can be modified in a running cluster using the following sequence:

    1. Enable new SASL mechanism by adding the mechanism to `sasl.enabled.mechanisms` in server.properties for each broker. Update JAAS config file to include both mechanisms. Incrementally bounce the cluster nodes.
    2. Restart clients using the new mechanism.
    3. To change the mechanism of inter-broker communication (if this is required), set `sasl.mechanism.inter.broker.protocol` in server.properties to the new mechanism and incrementally bounce the cluster again.
    4. To remove old mechanism (if this is required), remove the old mechanism from `sasl.enabled.mechanisms` in server.properties and remove the entries for the old mechanism from JAAS config file. Incrementally bounce the cluster again.
9.  **Authentication using Delegation Tokens**

    Delegation token based authentication is a lightweight authentication mechanism to complement existing SASL/SSL methods. Delegation tokens are shared secrets between kafka brokers and clients. Delegation tokens will help processing frameworks to distribute the workload to available workers in a secure environment without the added cost of distributing Kerberos TGT/keytabs or keystores when 2-way SSL is used. See [KIP-48](https://cwiki.apache.org/confluence/display/KAFKA/KIP-48+Delegation+token+support+for+Kafka) for more details.

    Under the default implementation of `principal.builder.class`, the owner of delegation token is used as the authenticated `Principal` for configuration of ACLs etc.

    Typical steps for delegation token usage are:

    1. User authenticates with the Kafka cluster via SASL or SSL, and obtains a delegation token. This can be done using Admin APIs or using `kafka-delegation-tokens.sh` script.
    2. User securely passes the delegation token to Kafka clients for authenticating with the Kafka cluster.
    3. Token owner/renewer can renew/expire the delegation tokens.
    4.  **Token Management**

        A secret is used to generate and verify delegation tokens. This is supplied using config option `delegation.token.secret.key`. The same secret key must be configured across all the brokers. If using Kafka with KRaft the controllers must also be configured with the secret using the same config option. If the secret is not set or set to empty string, delegation token authentication and API operations will fail.

        When using Kafka with Zookeeper, the token details are stored in Zookeeper and delegation tokens are suitable for use in Kafka installations where Zookeeper is on a private network. When using Kafka with KRaft, the token details are stored with the other metadata on the controller nodes and delegation tokens are suitable for use when the controllers are on a private network or when all commnications between brokers and controllers is encrypted. Currently, this secret is stored as plain text in the server.properties config file. We intend to make these configurable in a future Kafka release.

        A token has a current life, and a maximum renewable life. By default, tokens must be renewed once every 24 hours for up to 7 days. These can be configured using `delegation.token.expiry.time.ms` and `delegation.token.max.lifetime.ms` config options.

        Tokens can also be cancelled explicitly. If a token is not renewed by the token’s expiration time or if token is beyond the max life time, it will be deleted from all broker caches as well as from zookeeper.
    5.  **Creating Delegation Tokens**

        Tokens can be created by using Admin APIs or using `kafka-delegation-tokens.sh` script. Delegation token requests (create/renew/expire/describe) should be issued only on SASL or SSL authenticated channels. Tokens can not be requests if the initial authentication is done through delegation token. A token can be created by the user for that user or others as well by specifying the `--owner-principal` parameter. Owner/Renewers can renew or expire tokens. Owner/renewers can always describe their own tokens. To describe other tokens, a DESCRIBE\_TOKEN permission needs to be added on the User resource representing the owner of the token. `kafka-delegation-tokens.sh` script examples are given below.

        Create a delegation token:

        ```bash
        > bin/kafka-delegation-tokens.sh --bootstrap-server localhost:9092 --create   --max-life-time-period -1 --command-config client.properties --renewer-principal User:user1
        ```

        Create a delegation token for a different owner:

        ```bash
        > bin/kafka-delegation-tokens.sh --bootstrap-server localhost:9092 --create   --max-life-time-period -1 --command-config client.properties --renewer-principal User:user1 --owner-principal User:owner1
        ```

        Renew a delegation token:

        ```bash
        > bin/kafka-delegation-tokens.sh --bootstrap-server localhost:9092 --renew    --renew-time-period -1 --command-config client.properties --hmac ABCDEFGHIJK
        ```

        Expire a delegation token:

        ```bash
        > bin/kafka-delegation-tokens.sh --bootstrap-server localhost:9092 --expire   --expiry-time-period -1   --command-config client.properties  --hmac ABCDEFGHIJK
        ```

        Existing tokens can be described using the --describe option:

        ```bash
        > bin/kafka-delegation-tokens.sh --bootstrap-server localhost:9092 --describe --command-config client.properties  --owner-principal User:user1
        ```
    6.  **Token Authentication**

        Delegation token authentication piggybacks on the current SASL/SCRAM authentication mechanism. We must enable SASL/SCRAM mechanism on Kafka cluster as described in [here](https://kafka.apache.org/documentation/#security\_sasl\_scram).

        Configuring Kafka Clients:

        1.  Configure the JAAS configuration property for each client in producer.properties or consumer.properties. The login module describes how the clients like producer and consumer can connect to the Kafka Broker. The following is an example configuration for a client for the token authentication:

            ```
            sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required \
                username="tokenID123" \
                password="lAYYSFmLs4bTjf+lTZ1LCHR/ZZFNA==" \
                tokenauth="true";
            ```

            The options `username` and `password` are used by clients to configure the token id and token HMAC. And the option `tokenauth` is used to indicate the server about token authentication. In this example, clients connect to the broker using token id: _tokenID123_. Different clients within a JVM may connect using different tokens by specifying different token details in `sasl.jaas.config`. Clients use the login section named `KafkaClient`. This option allows only one user for all client connections from a JVM.
    7.  **Procedure to manually rotate the secret:**

        We require a re-deployment when the secret needs to be rotated. During this process, already connected clients will continue to work. But any new connection requests and renew/expire requests with old tokens can fail. Steps are given below.

        1. Expire all existing tokens.
        2. Rotate the secret by rolling upgrade, and
        3. Generate new tokens

        We intend to automate this in a future Kafka release.

## Authorization and ACLs <a href="#security_authz" id="security_authz"></a>

Kafka ships with a pluggable authorization framework, which is configured with the `authorizer.class.name` property in the server configuration. Configured implementations must extend `org.apache.kafka.server.authorizer.Authorizer`. Kafka provides default implementations which store ACLs in the cluster metadata (either Zookeeper or the KRaft metadata log). For Zookeeper-based clusters, the provided implementation is configured as follows:

```
authorizer.class.name=kafka.security.authorizer.AclAuthorizer
```

For KRaft clusters, use the following configuration on all nodes (brokers, controllers, or combined broker/controller nodes):

```
authorizer.class.name=org.apache.kafka.metadata.authorizer.StandardAuthorizer
```

Kafka ACLs are defined in the general format of "Principal {P} is \[Allowed|Denied] Operation {O} From Host {H} on any Resource {R} matching ResourcePattern {RP}". You can read more about the ACL structure in [KIP-11](https://cwiki.apache.org/confluence/display/KAFKA/KIP-11+-+Authorization+Interface) and resource patterns in [KIP-290](https://cwiki.apache.org/confluence/display/KAFKA/KIP-290%3A+Support+for+Prefixed+ACLs). In order to add, remove, or list ACLs, you can use the Kafka ACL CLI `kafka-acls.sh`. By default, if no ResourcePatterns match a specific Resource R, then R has no associated ACLs, and therefore no one other than super users is allowed to access R. If you want to change that behavior, you can include the following in server.properties.

```
allow.everyone.if.no.acl.found=true
```

One can also add super users in server.properties like the following (note that the delimiter is semicolon since SSL user names may contain comma). Default PrincipalType string "User" is case sensitive.

```
super.users=User:Bob;User:Alice
```

### **KRaft Principal Forwarding**

In KRaft clusters, admin requests such as `CreateTopics` and `DeleteTopics` are sent to the broker listeners by the client. The broker then forwards the request to the active controller through the first listener configured in `controller.listener.names`. Authorization of these requests is done on the controller node. This is achieved by way of an `Envelope` request which packages both the underlying request from the client as well as the client principal. When the controller receives the forwarded `Envelope` request from the broker, it first authorizes the `Envelope` request using the authenticated broker principal. Then it authorizes the underlying request using the forwarded principal.\
All of this implies that Kafka must understand how to serialize and deserialize the client principal. The authentication framework allows for customized principals by overriding the `principal.builder.class` configuration. In order for customized principals to work with KRaft, the configured class must implement `org.apache.kafka.common.security.auth.KafkaPrincipalSerde` so that Kafka knows how to serialize and deserialize the principals. The default implementation `org.apache.kafka.common.security.authenticator.DefaultKafkaPrincipalBuilder` uses the Kafka RPC format defined in the source code: `clients/src/main/resources/common/message/DefaultPrincipalData.json`. For more detail about request forwarding in KRaft, see [KIP-590](https://cwiki.apache.org/confluence/display/KAFKA/KIP-590%3A+Redirect+Zookeeper+Mutation+Protocols+to+The+Controller)

### **Customizing SSL User Name**

By default, the SSL user name will be of the form "CN=writeuser,OU=Unknown,O=Unknown,L=Unknown,ST=Unknown,C=Unknown". One can change that by setting `ssl.principal.mapping.rules` to a customized rule in server.properties. This config allows a list of rules for mapping X.500 distinguished name to short name. The rules are evaluated in order and the first rule that matches a distinguished name is used to map it to a short name. Any later rules in the list are ignored.\
The format of `ssl.principal.mapping.rules` is a list where each rule starts with "RULE:" and contains an expression as the following formats. Default rule will return string representation of the X.500 certificate distinguished name. If the distinguished name matches the pattern, then the replacement command will be run over the name. This also supports lowercase/uppercase options, to force the translated result to be all lower/uppercase case. This is done by adding a "/L" or "/U' to the end of the rule.

```
RULE:pattern/replacement/
RULE:pattern/replacement/[LU]
```

Example `ssl.principal.mapping.rules` values are:

```
RULE:^CN=(.*?),OU=ServiceUsers.*$/$1/,
RULE:^CN=(.*?),OU=(.*?),O=(.*?),L=(.*?),ST=(.*?),C=(.*?)$/$1@$2/L,
RULE:^.*[Cc][Nn]=([a-zA-Z0-9.]*).*$/$1/L,
DEFAULT
```

Above rules translate distinguished name "CN=serviceuser,OU=ServiceUsers,O=Unknown,L=Unknown,ST=Unknown,C=Unknown" to "serviceuser" and "CN=adminUser,OU=Admin,O=Unknown,L=Unknown,ST=Unknown,C=Unknown" to "adminuser@admin".\
For advanced use cases, one can customize the name by setting a customized PrincipalBuilder in server.properties like the following.

```
principal.builder.class=CustomizedPrincipalBuilderClass
```

### **Customizing SASL User Name**

By default, the SASL user name will be the primary part of the Kerberos principal. One can change that by setting `sasl.kerberos.principal.to.local.rules` to a customized rule in server.properties. The format of `sasl.kerberos.principal.to.local.rules` is a list where each rule works in the same way as the auth\_to\_local in [Kerberos configuration file (krb5.conf)](https://web.mit.edu/Kerberos/krb5-latest/doc/admin/conf\_files/krb5\_conf.html). This also support additional lowercase/uppercase rule, to force the translated result to be all lowercase/uppercase. This is done by adding a "/L" or "/U" to the end of the rule. check below formats for syntax. Each rules starts with RULE: and contains an expression as the following formats. See the kerberos documentation for more details.

```
RULE:[n:string](regexp)s/pattern/replacement/
RULE:[n:string](regexp)s/pattern/replacement/g
RULE:[n:string](regexp)s/pattern/replacement//L
RULE:[n:string](regexp)s/pattern/replacement/g/L
RULE:[n:string](regexp)s/pattern/replacement//U
RULE:[n:string](regexp)s/pattern/replacement/g/U
```

An example of adding a rule to properly translate user@MYDOMAIN.COM to user while also keeping the default rule in place is:

```
sasl.kerberos.principal.to.local.rules=RULE:[1:$1@$0](.*@MYDOMAIN.COM)s/@.*//,DEFAULT
```

### **Command Line Interface**

Kafka Authorization management CLI can be found under bin directory with all the other CLIs. The CLI script is called kafka-acls.sh. Following lists all the options that the script supports:

| OPTION                                  | DESCRIPTION                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | DEFAULT                                                                          | OPTION TYPE     |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | --------------- |
| --add                                   | Indicates to the script that user is trying to add an acl.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |                                                                                  | Action          |
| --remove                                | Indicates to the script that user is trying to remove an acl.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                  | Action          |
| --list                                  | Indicates to the script that user is trying to list acls.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                  | Action          |
| --bootstrap-server                      | A list of host/port pairs to use for establishing the connection to the Kafka cluster. Only one of --bootstrap-server or --authorizer option must be specified.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                  | Configuration   |
| --command-config                        | A property file containing configs to be passed to Admin Client. This option can only be used with --bootstrap-server option.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                  | Configuration   |
| --cluster                               | Indicates to the script that the user is trying to interact with acls on the singular cluster resource.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                  | ResourcePattern |
| --topic \[topic-name]                   | Indicates to the script that the user is trying to interact with acls on topic resource pattern(s).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                  | ResourcePattern |
| --group \[group-name]                   | Indicates to the script that the user is trying to interact with acls on consumer-group resource pattern(s)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                  | ResourcePattern |
| --transactional-id \[transactional-id]  | The transactionalId to which ACLs should be added or removed. A value of \* indicates the ACLs should apply to all transactionalIds.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                  | ResourcePattern |
| --delegation-token \[delegation-token]  | Delegation token to which ACLs should be added or removed. A value of \* indicates ACL should apply to all tokens.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                                                  | ResourcePattern |
| --user-principal \[user-principal]      | A user resource to which ACLs should be added or removed. This is currently supported in relation with delegation tokens. A value of \* indicates ACL should apply to all users.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                                                                                  | ResourcePattern |
| --resource-pattern-type \[pattern-type] | <p>Indicates to the script the type of resource pattern, (for --add), or resource pattern filter, (for --list and --remove), the user wishes to use.<br>When adding acls, this should be a specific pattern type, e.g. 'literal' or 'prefixed'.<br>When listing or removing acls, a specific pattern type filter can be used to list or remove acls from a specific type of resource pattern, or the filter values of 'any' or 'match' can be used, where 'any' will match any pattern type, but will match the resource name exactly, and 'match' will perform pattern matching to list or remove all acls that affect the supplied resource(s).<br>WARNING: 'match', when used in combination with the '--remove' switch, should be used with care.</p> | literal                                                                          | Configuration   |
| --allow-principal                       | <p>Principal is in PrincipalType:name format that will be added to ACL with Allow permission. Default PrincipalType string "User" is case sensitive.<br>You can specify multiple --allow-principal in a single command.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                  | Principal       |
| --deny-principal                        | <p>Principal is in PrincipalType:name format that will be added to ACL with Deny permission. Default PrincipalType string "User" is case sensitive.<br>You can specify multiple --deny-principal in a single command.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                  | Principal       |
| --principal                             | <p>Principal is in PrincipalType:name format that will be used along with --list option. Default PrincipalType string "User" is case sensitive. This will list the ACLs for the specified principal.<br>You can specify multiple --principal in a single command.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                                                                  | Principal       |
| --allow-host                            | IP address from which principals listed in --allow-principal will have access.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | if --allow-principal is specified defaults to \* which translates to "all hosts" | Host            |
| --deny-host                             | IP address from which principals listed in --deny-principal will be denied access.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | if --deny-principal is specified defaults to \* which translates to "all hosts"  | Host            |
| --operation                             | <p>Operation that will be allowed or denied.<br>Valid values are:</p><ul><li>Read</li><li>Write</li><li>Create</li><li>Delete</li><li>Alter</li><li>Describe</li><li>ClusterAction</li><li>DescribeConfigs</li><li>AlterConfigs</li><li>IdempotentWrite</li><li>CreateTokens</li><li>DescribeTokens</li><li>All</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                 | All                                                                              | Operation       |
| --producer                              | Convenience option to add/remove acls for producer role. This will generate acls that allows WRITE, DESCRIBE and CREATE on topic.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                  | Convenience     |
| --consumer                              | Convenience option to add/remove acls for consumer role. This will generate acls that allows READ, DESCRIBE on topic and READ on consumer-group.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                                                                                  | Convenience     |
| --idempotent                            | <p>Enable idempotence for the producer. This should be used in combination with the --producer option.<br>Note that idempotence is enabled automatically if the producer is authorized to a particular transactional-id.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                  | Convenience     |
| --force                                 | Convenience option to assume yes to all queries and do not prompt.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                                                  | Convenience     |
| --authorizer                            | (DEPRECATED: not supported in KRaft) Fully qualified class name of the authorizer.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | kafka.security.authorizer.AclAuthorizer                                          | Configuration   |
| --authorizer-properties                 | (DEPRECATED: not supported in KRaft) key=val pairs that will be passed to authorizer for initialization. For the default authorizer in ZK clsuters, the example values are: zookeeper.connect=localhost:2181                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                  | Configuration   |
| --zk-tls-config-file                    | (DEPRECATED: not supported in KRaft) Identifies the file where ZooKeeper client TLS connectivity properties for the authorizer are defined. Any properties other than the following (with or without an "authorizer." prefix) are ignored: zookeeper.clientCnxnSocket, zookeeper.ssl.cipher.suites, zookeeper.ssl.client.enable, zookeeper.ssl.crl.enable, zookeeper.ssl.enabled.protocols, zookeeper.ssl.endpoint.identification.algorithm, zookeeper.ssl.keystore.location, zookeeper.ssl.keystore.password, zookeeper.ssl.keystore.type, zookeeper.ssl.ocsp.enable, zookeeper.ssl.protocol, zookeeper.ssl.truststore.location, zookeeper.ssl.truststore.password, zookeeper.ssl.truststore.type                                                        |                                                                                  | Configuration   |

**Examples**

*   Adding Acls\
    Suppose you want to add an acl "Principals User:Bob and User:Alice are allowed to perform Operation Read and Write on Topic Test-Topic from IP 198.51.100.0 and IP 198.51.100.1". You can do that by executing the CLI with following options:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:Bob --allow-principal User:Alice --allow-host 198.51.100.0 --allow-host 198.51.100.1 --operation Read --operation Write --topic Test-topic
    ```

    By default, all principals that don't have an explicit acl that allows access for an operation to a resource are denied. In rare cases where an allow acl is defined that allows access to all but some principal we will have to use the --deny-principal and --deny-host option. For example, if we want to allow all users to Read from Test-topic but only deny User:BadBob from IP 198.51.100.3 we can do so using following commands:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:'*' --allow-host '*' --deny-principal User:BadBob --deny-host 198.51.100.3 --operation Read --topic Test-topic
    ```

    Note that `--allow-host` and `--deny-host` only support IP addresses (hostnames are not supported). Above examples add acls to a topic by specifying --topic \[topic-name] as the resource pattern option. Similarly user can add acls to cluster by specifying --cluster and to a consumer group by specifying --group \[group-name]. You can add acls on any resource of a certain type, e.g. suppose you wanted to add an acl "Principal User:Peter is allowed to produce to any Topic from IP 198.51.200.0" You can do that by using the wildcard resource '\*', e.g. by executing the CLI with following options:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:Peter --allow-host 198.51.200.1 --producer --topic '*'
    ```

    You can add acls on prefixed resource patterns, e.g. suppose you want to add an acl "Principal User:Jane is allowed to produce to any Topic whose name starts with 'Test-' from any host". You can do that by executing the CLI with following options:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:Jane --producer --topic Test- --resource-pattern-type prefixed
    ```

    Note, --resource-pattern-type defaults to 'literal', which only affects resources with the exact same name or, in the case of the wildcard resource name '\*', a resource with any name.
*   Removing Acls\
    Removing acls is pretty much the same. The only difference is instead of --add option users will have to specify --remove option. To remove the acls added by the first example above we can execute the CLI with following options:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --remove --allow-principal User:Bob --allow-principal User:Alice --allow-host 198.51.100.0 --allow-host 198.51.100.1 --operation Read --operation Write --topic Test-topic 
    ```

    If you want to remove the acl added to the prefixed resource pattern above we can execute the CLI with following options:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --remove --allow-principal User:Jane --producer --topic Test- --resource-pattern-type Prefixed
    ```
*   List Acls\
    We can list acls for any resource by specifying the --list option with the resource. To list all acls on the literal resource pattern Test-topic, we can execute the CLI with following options:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --list --topic Test-topic
    ```

    However, this will only return the acls that have been added to this exact resource pattern. Other acls can exist that affect access to the topic, e.g. any acls on the topic wildcard '\*', or any acls on prefixed resource patterns. Acls on the wildcard resource pattern can be queried explicitly:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --list --topic '*'
    ```

    However, it is not necessarily possible to explicitly query for acls on prefixed resource patterns that match Test-topic as the name of such patterns may not be known. We can list _all_ acls affecting Test-topic by using '--resource-pattern-type match', e.g.

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --list --topic Test-topic --resource-pattern-type match
    ```

    This will list acls on all matching literal, wildcard and prefixed resource patterns.
*   Adding or removing a principal as producer or consumer\
    The most common use case for acl management are adding/removing a principal as producer or consumer so we added convenience options to handle these cases. In order to add User:Bob as a producer of Test-topic we can execute the following command:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:Bob --producer --topic Test-topic
    ```

    Similarly to add Alice as a consumer of Test-topic with consumer group Group-1 we just have to pass --consumer option:

    ```bash
    > bin/kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:Bob --consumer --topic Test-topic --group Group-1 
    ```

    Note that for consumer option we must also specify the consumer group. In order to remove a principal from producer or consumer role we just need to pass --remove option.
*   Admin API based acl management\
    Users having Alter permission on ClusterResource can use Admin API for ACL management. kafka-acls.sh script supports AdminClient API to manage ACLs without interacting with zookeeper/authorizer directly. All the above examples can be executed by using --bootstrap-server option. For example:

    ```bash
    bin/kafka-acls.sh --bootstrap-server localhost:9092 --command-config /tmp/adminclient-configs.conf --add --allow-principal User:Bob --producer --topic Test-topic
    bin/kafka-acls.sh --bootstrap-server localhost:9092 --command-config /tmp/adminclient-configs.conf --add --allow-principal User:Bob --consumer --topic Test-topic --group Group-1
    bin/kafka-acls.sh --bootstrap-server localhost:9092 --command-config /tmp/adminclient-configs.conf --list --topic Test-topic
    bin/kafka-acls.sh --bootstrap-server localhost:9092 --command-config /tmp/adminclient-configs.conf --add --allow-principal User:tokenRequester --operation CreateTokens --user-principal "owner1"
    ```

### **Authorization Primitives**

Protocol calls are usually performing some operations on certain resources in Kafka. It is required to know the operations and resources to set up effective protection. In this section we'll list these operations and resources, then list the combination of these with the protocols to see the valid scenarios.

### **Operations in Kafka**

There are a few operation primitives that can be used to build up privileges. These can be matched up with certain resources to allow specific protocol calls for a given user. These are:

* Read
* Write
* Create
* Delete
* Alter
* Describe
* ClusterAction
* DescribeConfigs
* AlterConfigs
* IdempotentWrite
* CreateTokens
* DescribeTokens
* All

### **Resources in Kafka**

The operations above can be applied on certain resources which are described below.

* Topic: this simply represents a Topic. All protocol calls that are acting on topics (such as reading, writing them) require the corresponding privilege to be added. If there is an authorization error with a topic resource, then a TOPIC\_AUTHORIZATION\_FAILED (error code: 29) will be returned.
* Group: this represents the consumer groups in the brokers. All protocol calls that are working with consumer groups, like joining a group must have privileges with the group in subject. If the privilege is not given then a GROUP\_AUTHORIZATION\_FAILED (error code: 30) will be returned in the protocol response.
* Cluster: this resource represents the cluster. Operations that are affecting the whole cluster, like controlled shutdown are protected by privileges on the Cluster resource. If there is an authorization problem on a cluster resource, then a CLUSTER\_AUTHORIZATION\_FAILED (error code: 31) will be returned.
* TransactionalId: this resource represents actions related to transactions, such as committing. If any error occurs, then a TRANSACTIONAL\_ID\_AUTHORIZATION\_FAILED (error code: 53) will be returned by brokers.
* DelegationToken: this represents the delegation tokens in the cluster. Actions, such as describing delegation tokens could be protected by a privilege on the DelegationToken resource.&#x20;
* User: CreateToken and DescribeToken operations can be granted to User resources to allow creating and describing tokens for other users.

### **Operations and Resources on Protocols**

In the below table we'll list the valid operations on resources that are executed by the Kafka API protocols.

| PROTOCOL (API KEY)                      | OPERATION       | RESOURCE        | NOTE                                                                                                                                                                                                                                                     |
| --------------------------------------- | --------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PRODUCE (0)                             | Write           | TransactionalId | An transactional producer which has its transactional.id set requires this privilege.                                                                                                                                                                    |
| PRODUCE (0)                             | IdempotentWrite | Cluster         | An idempotent produce action requires this privilege.                                                                                                                                                                                                    |
| PRODUCE (0)                             | Write           | Topic           | This applies to a normal produce action.                                                                                                                                                                                                                 |
| FETCH (1)                               | ClusterAction   | Cluster         | A follower must have ClusterAction on the Cluster resource in order to fetch partition data.                                                                                                                                                             |
| FETCH (1)                               | Read            | Topic           | Regular Kafka consumers need READ permission on each partition they are fetching.                                                                                                                                                                        |
| LIST\_OFFSETS (2)                       | Describe        | Topic           |                                                                                                                                                                                                                                                          |
| METADATA (3)                            | Describe        | Topic           |                                                                                                                                                                                                                                                          |
| METADATA (3)                            | Create          | Cluster         | If topic auto-creation is enabled, then the broker-side API will check for the existence of a Cluster level privilege. If it's found then it'll allow creating the topic, otherwise it'll iterate through the Topic level privileges (see the next one). |
| METADATA (3)                            | Create          | Topic           | This authorizes auto topic creation if enabled but the given user doesn't have a cluster level permission (above).                                                                                                                                       |
| LEADER\_AND\_ISR (4)                    | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| STOP\_REPLICA (5)                       | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| UPDATE\_METADATA (6)                    | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| CONTROLLED\_SHUTDOWN (7)                | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| OFFSET\_COMMIT (8)                      | Read            | Group           | An offset can only be committed if it's authorized to the given group and the topic too (see below). Group access is checked first, then Topic access.                                                                                                   |
| OFFSET\_COMMIT (8)                      | Read            | Topic           | Since offset commit is part of the consuming process, it needs privileges for the read action.                                                                                                                                                           |
| OFFSET\_FETCH (9)                       | Describe        | Group           | Similarly to OFFSET\_COMMIT, the application must have privileges on group and topic level too to be able to fetch. However in this case it requires describe access instead of read. Group access is checked first, then Topic access.                  |
| OFFSET\_FETCH (9)                       | Describe        | Topic           |                                                                                                                                                                                                                                                          |
| FIND\_COORDINATOR (10)                  | Describe        | Group           | The FIND\_COORDINATOR request can be of "Group" type in which case it is looking for consumergroup coordinators. This privilege would represent the Group mode.                                                                                          |
| FIND\_COORDINATOR (10)                  | Describe        | TransactionalId | This applies only on transactional producers and checked when a producer tries to find the transaction coordinator.                                                                                                                                      |
| JOIN\_GROUP (11)                        | Read            | Group           |                                                                                                                                                                                                                                                          |
| HEARTBEAT (12)                          | Read            | Group           |                                                                                                                                                                                                                                                          |
| LEAVE\_GROUP (13)                       | Read            | Group           |                                                                                                                                                                                                                                                          |
| SYNC\_GROUP (14)                        | Read            | Group           |                                                                                                                                                                                                                                                          |
| DESCRIBE\_GROUPS (15)                   | Describe        | Group           |                                                                                                                                                                                                                                                          |
| LIST\_GROUPS (16)                       | Describe        | Cluster         | When the broker checks to authorize a list\_groups request it first checks for this cluster level authorization. If none found then it proceeds to check the groups individually. This operation doesn't return CLUSTER\_AUTHORIZATION\_FAILED.          |
| LIST\_GROUPS (16)                       | Describe        | Group           | If none of the groups are authorized, then just an empty response will be sent back instead of an error. This operation doesn't return CLUSTER\_AUTHORIZATION\_FAILED. This is applicable from the 2.1 release.                                          |
| SASL\_HANDSHAKE (17)                    |                 |                 | The SASL handshake is part of the authentication process and therefore it's not possible to apply any kind of authorization here.                                                                                                                        |
| API\_VERSIONS (18)                      |                 |                 | The API\_VERSIONS request is part of the Kafka protocol handshake and happens on connection and before any authentication. Therefore it's not possible to control this with authorization.                                                               |
| CREATE\_TOPICS (19)                     | Create          | Cluster         | If there is no cluster level authorization then it won't return CLUSTER\_AUTHORIZATION\_FAILED but fall back to use topic level, which is just below. That'll throw error if there is a problem.                                                         |
| CREATE\_TOPICS (19)                     | Create          | Topic           | This is applicable from the 2.0 release.                                                                                                                                                                                                                 |
| DELETE\_TOPICS (20)                     | Delete          | Topic           |                                                                                                                                                                                                                                                          |
| DELETE\_RECORDS (21)                    | Delete          | Topic           |                                                                                                                                                                                                                                                          |
| INIT\_PRODUCER\_ID (22)                 | Write           | TransactionalId |                                                                                                                                                                                                                                                          |
| INIT\_PRODUCER\_ID (22)                 | IdempotentWrite | Cluster         |                                                                                                                                                                                                                                                          |
| OFFSET\_FOR\_LEADER\_EPOCH (23)         | ClusterAction   | Cluster         | If there is no cluster level privilege for this operation, then it'll check for topic level one.                                                                                                                                                         |
| OFFSET\_FOR\_LEADER\_EPOCH (23)         | Describe        | Topic           | This is applicable from the 2.1 release.                                                                                                                                                                                                                 |
| ADD\_PARTITIONS\_TO\_TXN (24)           | Write           | TransactionalId | This API is only applicable to transactional requests. It first checks for the Write action on the TransactionalId resource, then it checks the Topic in subject (below).                                                                                |
| ADD\_PARTITIONS\_TO\_TXN (24)           | Write           | Topic           |                                                                                                                                                                                                                                                          |
| ADD\_OFFSETS\_TO\_TXN (25)              | Write           | TransactionalId | Similarly to ADD\_PARTITIONS\_TO\_TXN this is only applicable to transactional request. It first checks for Write action on the TransactionalId resource, then it checks whether it can Read on the given group (below).                                 |
| ADD\_OFFSETS\_TO\_TXN (25)              | Read            | Group           |                                                                                                                                                                                                                                                          |
| END\_TXN (26)                           | Write           | TransactionalId |                                                                                                                                                                                                                                                          |
| WRITE\_TXN\_MARKERS (27)                | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| TXN\_OFFSET\_COMMIT (28)                | Write           | TransactionalId |                                                                                                                                                                                                                                                          |
| TXN\_OFFSET\_COMMIT (28)                | Read            | Group           |                                                                                                                                                                                                                                                          |
| TXN\_OFFSET\_COMMIT (28)                | Read            | Topic           |                                                                                                                                                                                                                                                          |
| DESCRIBE\_ACLS (29)                     | Describe        | Cluster         |                                                                                                                                                                                                                                                          |
| CREATE\_ACLS (30)                       | Alter           | Cluster         |                                                                                                                                                                                                                                                          |
| DELETE\_ACLS (31)                       | Alter           | Cluster         |                                                                                                                                                                                                                                                          |
| DESCRIBE\_CONFIGS (32)                  | DescribeConfigs | Cluster         | If broker configs are requested, then the broker will check cluster level privileges.                                                                                                                                                                    |
| DESCRIBE\_CONFIGS (32)                  | DescribeConfigs | Topic           | If topic configs are requested, then the broker will check topic level privileges.                                                                                                                                                                       |
| ALTER\_CONFIGS (33)                     | AlterConfigs    | Cluster         | If broker configs are altered, then the broker will check cluster level privileges.                                                                                                                                                                      |
| ALTER\_CONFIGS (33)                     | AlterConfigs    | Topic           | If topic configs are altered, then the broker will check topic level privileges.                                                                                                                                                                         |
| ALTER\_REPLICA\_LOG\_DIRS (34)          | Alter           | Cluster         |                                                                                                                                                                                                                                                          |
| DESCRIBE\_LOG\_DIRS (35)                | Describe        | Cluster         | An empty response will be returned on authorization failure.                                                                                                                                                                                             |
| SASL\_AUTHENTICATE (36)                 |                 |                 | SASL\_AUTHENTICATE is part of the authentication process and therefore it's not possible to apply any kind of authorization here.                                                                                                                        |
| CREATE\_PARTITIONS (37)                 | Alter           | Topic           |                                                                                                                                                                                                                                                          |
| CREATE\_DELEGATION\_TOKEN (38)          |                 |                 | Creating delegation tokens has special rules, for this please see the Authentication using Delegation Tokens section.                                                                                                                                    |
| CREATE\_DELEGATION\_TOKEN (38)          | CreateTokens    | User            | Allows creating delegation tokens for the User resource.                                                                                                                                                                                                 |
| RENEW\_DELEGATION\_TOKEN (39)           |                 |                 | Renewing delegation tokens has special rules, for this please see the Authentication using Delegation Tokens section.                                                                                                                                    |
| EXPIRE\_DELEGATION\_TOKEN (40)          |                 |                 | Expiring delegation tokens has special rules, for this please see the Authentication using Delegation Tokens section.                                                                                                                                    |
| DESCRIBE\_DELEGATION\_TOKEN (41)        | Describe        | DelegationToken | Describing delegation tokens has special rules, for this please see the Authentication using Delegation Tokens section.                                                                                                                                  |
| DESCRIBE\_DELEGATION\_TOKEN (41)        | DescribeTokens  | User            | Allows describing delegation tokens of the User resource.                                                                                                                                                                                                |
| DELETE\_GROUPS (42)                     | Delete          | Group           |                                                                                                                                                                                                                                                          |
| ELECT\_PREFERRED\_LEADERS (43)          | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| INCREMENTAL\_ALTER\_CONFIGS (44)        | AlterConfigs    | Cluster         | If broker configs are altered, then the broker will check cluster level privileges.                                                                                                                                                                      |
| INCREMENTAL\_ALTER\_CONFIGS (44)        | AlterConfigs    | Topic           | If topic configs are altered, then the broker will check topic level privileges.                                                                                                                                                                         |
| ALTER\_PARTITION\_REASSIGNMENTS (45)    | Alter           | Cluster         |                                                                                                                                                                                                                                                          |
| LIST\_PARTITION\_REASSIGNMENTS (46)     | Describe        | Cluster         |                                                                                                                                                                                                                                                          |
| OFFSET\_DELETE (47)                     | Delete          | Group           |                                                                                                                                                                                                                                                          |
| OFFSET\_DELETE (47)                     | Read            | Topic           |                                                                                                                                                                                                                                                          |
| DESCRIBE\_CLIENT\_QUOTAS (48)           | DescribeConfigs | Cluster         |                                                                                                                                                                                                                                                          |
| ALTER\_CLIENT\_QUOTAS (49)              | AlterConfigs    | Cluster         |                                                                                                                                                                                                                                                          |
| DESCRIBE\_USER\_SCRAM\_CREDENTIALS (50) | Describe        | Cluster         |                                                                                                                                                                                                                                                          |
| ALTER\_USER\_SCRAM\_CREDENTIALS (51)    | Alter           | Cluster         |                                                                                                                                                                                                                                                          |
| VOTE (52)                               | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| BEGIN\_QUORUM\_EPOCH (53)               | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| END\_QUORUM\_EPOCH (54)                 | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| DESCRIBE\_QUORUM (55)                   | Describe        | Cluster         |                                                                                                                                                                                                                                                          |
| ALTER\_PARTITION (56)                   | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| UPDATE\_FEATURES (57)                   | Alter           | Cluster         |                                                                                                                                                                                                                                                          |
| ENVELOPE (58)                           | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| FETCH\_SNAPSHOT (59)                    | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| DESCRIBE\_CLUSTER (60)                  | Describe        | Cluster         |                                                                                                                                                                                                                                                          |
| DESCRIBE\_PRODUCERS (61)                | Read            | Topic           |                                                                                                                                                                                                                                                          |
| BROKER\_REGISTRATION (62)               | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| BROKER\_HEARTBEAT (63)                  | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| UNREGISTER\_BROKER (64)                 | Alter           | Cluster         |                                                                                                                                                                                                                                                          |
| DESCRIBE\_TRANSACTIONS (65)             | Describe        | TransactionalId |                                                                                                                                                                                                                                                          |
| LIST\_TRANSACTIONS (66)                 | Describe        | TransactionalId |                                                                                                                                                                                                                                                          |
| ALLOCATE\_PRODUCER\_IDS (67)            | ClusterAction   | Cluster         |                                                                                                                                                                                                                                                          |
| CONSUMER\_GROUP\_HEARTBEAT (68)         | Read            | Group           |                                                                                                                                                                                                                                                          |

## Incorporating Security Features in a Running Cluster <a href="#security_rolling_upgrade" id="security_rolling_upgrade"></a>

You can secure a running cluster via one or more of the supported protocols discussed previously. This is done in phases:

* Incrementally bounce the cluster nodes to open additional secured port(s).
* Restart clients using the secured rather than PLAINTEXT port (assuming you are securing the client-broker connection).
* Incrementally bounce the cluster again to enable broker-to-broker security (if this is required)
* A final incremental bounce to close the PLAINTEXT port.

The specific steps for configuring SSL and SASL are described in sections above. Follow these steps to enable security for your desired protocol(s).The security implementation lets you configure different protocols for both broker-client and broker-broker communication. These must be enabled in separate bounces. A PLAINTEXT port must be left open throughout so brokers and/or clients can continue to communicate.When performing an incremental bounce stop the brokers cleanly via a SIGTERM. It's also good practice to wait for restarted replicas to return to the ISR list before moving onto the next node.As an example, say we wish to encrypt both broker-client and broker-broker communication with SSL. In the first incremental bounce, an SSL port is opened on each node:

```
listeners=PLAINTEXT://broker1:9091,SSL://broker1:9092
```

We then restart the clients, changing their config to point at the newly opened, secured port:

```
bootstrap.servers = [broker1:9092,...]
security.protocol = SSL
...etc
```

In the second incremental server bounce we instruct Kafka to use SSL as the broker-broker protocol (which will use the same SSL port):

```
listeners=PLAINTEXT://broker1:9091,SSL://broker1:9092
security.inter.broker.protocol=SSL
```

In the final bounce we secure the cluster by closing the PLAINTEXT port:

```
listeners=SSL://broker1:9092
security.inter.broker.protocol=SSL
```

Alternatively we might choose to open multiple ports so that different protocols can be used for broker-broker and broker-client communication. Say we wished to use SSL encryption throughout (i.e. for broker-broker and broker-client communication) but we'd like to add SASL authentication to the broker-client connection also. We would achieve this by opening two additional ports during the first bounce:

```
listeners=PLAINTEXT://broker1:9091,SSL://broker1:9092,SASL_SSL://broker1:9093
```

We would then restart the clients, changing their config to point at the newly opened, SASL & SSL secured port:

```
bootstrap.servers = [broker1:9093,...]
security.protocol = SASL_SSL
...etc
```

The second server bounce would switch the cluster to use encrypted broker-broker communication via the SSL port we previously opened on port 9092:

```
listeners=PLAINTEXT://broker1:9091,SSL://broker1:9092,SASL_SSL://broker1:9093
security.inter.broker.protocol=SSL
```

The final bounce secures the cluster by closing the PLAINTEXT port.

```
listeners=SSL://broker1:9092,SASL_SSL://broker1:9093
security.inter.broker.protocol=SSL
```

ZooKeeper can be secured independently of the Kafka cluster.&#x20;

## ZooKeeper Authentication <a href="#zk_authz" id="zk_authz"></a>

ZooKeeper supports mutual TLS (mTLS) authentication beginning with the 3.5.x versions. Kafka supports authenticating to ZooKeeper with SASL and mTLS -- either individually or both together -- beginning with version 2.5.&#x20;

When using mTLS alone, every broker and any CLI tools should identify itself with the same Distinguished Name (DN) because it is the DN that is ACL'ed. This can be changed as described below, but it involves writing and deploying a custom ZooKeeper authentication provider. Generally each certificate should have the same DN but a different Subject Alternative Name (SAN) so that hostname verification of the brokers and any CLI tools by ZooKeeper will succeed.

When using SASL authentication to ZooKeeper together with mTLS, both the SASL identity and either the DN that created the znode (i.e. the creating broker's certificate) or the DN of the Security Migration Tool (if migration was performed after the znode was created) will be ACL'ed, and all brokers and CLI tools will be authorized even if they all use different DNs because they will all use the same ACL'ed SASL identity. It is only when using mTLS authentication alone that all the DNs must match (and SANs become critical -- again, in the absence of writing and deploying a custom ZooKeeper authentication provider as described below).

Use the broker properties file to set TLS configs for brokers as described below.

Use the `--zk-tls-config-file <file>` option to set TLS configs in the Zookeeper Security Migration Tool. The `kafka-acls.sh` and `kafka-configs.sh` CLI tools also support the `--zk-tls-config-file <file>` option.

Use the `-zk-tls-config-file <file>` option (note the single-dash rather than double-dash) to set TLS configs for the `zookeeper-shell.sh` CLI tool.

### **New clusters**

#### **ZooKeeper SASL Authentication**

To enable ZooKeeper SASL authentication on brokers, there are two necessary steps:

1. Create a JAAS login file and set the appropriate system property to point to it as described above
2. Set the configuration property `zookeeper.set.acl` in each broker to true

The metadata stored in ZooKeeper for the Kafka cluster is world-readable, but can only be modified by the brokers. The rationale behind this decision is that the data stored in ZooKeeper is not sensitive, but inappropriate manipulation of that data can cause cluster disruption. We also recommend limiting the access to ZooKeeper via network segmentation (only brokers and some admin tools need access to ZooKeeper).

#### **ZooKeeper Mutual TLS Authentication**

ZooKeeper mTLS authentication can be enabled with or without SASL authentication. As mentioned above, when using mTLS alone, every broker and any CLI tools must generally identify itself with the same Distinguished Name (DN) because it is the DN that is ACL'ed, which means each certificate should have an appropriate Subject Alternative Name (SAN) so that hostname verification of the brokers and any CLI tool by ZooKeeper will succeed.

It is possible to use something other than the DN for the identity of mTLS clients by writing a class that extends `org.apache.zookeeper.server.auth.X509AuthenticationProvider` and overrides the method `protected String getClientId(X509Certificate clientCert)`. Choose a scheme name and set `authProvider.[scheme]` in ZooKeeper to be the fully-qualified class name of the custom implementation; then set `ssl.authProvider=[scheme]` to use it.

Here is a sample (partial) ZooKeeper configuration for enabling TLS authentication. These configurations are described in the [ZooKeeper Admin Guide](https://zookeeper.apache.org/doc/r3.5.7/zookeeperAdmin.html#sc\_authOptions).

```
secureClientPort=2182
serverCnxnFactory=org.apache.zookeeper.server.NettyServerCnxnFactory
authProvider.x509=org.apache.zookeeper.server.auth.X509AuthenticationProvider
ssl.keyStore.location=/path/to/zk/keystore.jks
ssl.keyStore.password=zk-ks-passwd
ssl.trustStore.location=/path/to/zk/truststore.jks
ssl.trustStore.password=zk-ts-passwd
```

**IMPORTANT**: ZooKeeper does not support setting the key password in the ZooKeeper server keystore to a value different from the keystore password itself. Be sure to set the key password to be the same as the keystore password.

Here is a sample (partial) Kafka Broker configuration for connecting to ZooKeeper with mTLS authentication. These configurations are described above in [Broker Configs](https://kafka.apache.org/documentation/#brokerconfigs).

```
# connect to the ZooKeeper port configured for TLS
zookeeper.connect=zk1:2182,zk2:2182,zk3:2182
# required to use TLS to ZooKeeper (default is false)
zookeeper.ssl.client.enable=true
# required to use TLS to ZooKeeper
zookeeper.clientCnxnSocket=org.apache.zookeeper.ClientCnxnSocketNetty
# define key/trust stores to use TLS to ZooKeeper; ignored unless zookeeper.ssl.client.enable=true
zookeeper.ssl.keystore.location=/path/to/kafka/keystore.jks
zookeeper.ssl.keystore.password=kafka-ks-passwd
zookeeper.ssl.truststore.location=/path/to/kafka/truststore.jks
zookeeper.ssl.truststore.password=kafka-ts-passwd
# tell broker to create ACLs on znodes
zookeeper.set.acl=true
```

**IMPORTANT**: ZooKeeper does not support setting the key password in the ZooKeeper client (i.e. broker) keystore to a value different from the keystore password itself. Be sure to set the key password to be the same as the keystore password.

### **Migrating clusters**

If you are running a version of Kafka that does not support security or simply with security disabled, and you want to make the cluster secure, then you need to execute the following steps to enable ZooKeeper authentication with minimal disruption to your operations:

1.  Enable SASL and/or mTLS authentication on ZooKeeper. If enabling mTLS, you would now have both a non-TLS port and a TLS port, like this:

    ```
    clientPort=2181
    secureClientPort=2182
    serverCnxnFactory=org.apache.zookeeper.server.NettyServerCnxnFactory
    authProvider.x509=org.apache.zookeeper.server.auth.X509AuthenticationProvider
    ssl.keyStore.location=/path/to/zk/keystore.jks
    ssl.keyStore.password=zk-ks-passwd
    ssl.trustStore.location=/path/to/zk/truststore.jks
    ssl.trustStore.password=zk-ts-passwd
    ```
2. Perform a rolling restart of brokers setting the JAAS login file and/or defining ZooKeeper mutual TLS configurations (including connecting to the TLS-enabled ZooKeeper port) as required, which enables brokers to authenticate to ZooKeeper. At the end of the rolling restart, brokers are able to manipulate znodes with strict ACLs, but they will not create znodes with those ACLs
3. If you enabled mTLS, disable the non-TLS port in ZooKeeper
4. Perform a second rolling restart of brokers, this time setting the configuration parameter `zookeeper.set.acl` to true, which enables the use of secure ACLs when creating znodes
5. Execute the ZkSecurityMigrator tool. To execute the tool, there is this script: `bin/zookeeper-security-migration.sh` with `zookeeper.acl` set to secure. This tool traverses the corresponding sub-trees changing the ACLs of the znodes. Use the `--zk-tls-config-file <file>` option if you enable mTLS.

It is also possible to turn off authentication in a secure cluster. To do it, follow these steps:

1. Perform a rolling restart of brokers setting the JAAS login file and/or defining ZooKeeper mutual TLS configurations, which enables brokers to authenticate, but setting `zookeeper.set.acl` to false. At the end of the rolling restart, brokers stop creating znodes with secure ACLs, but are still able to authenticate and manipulate all znodes
2. Execute the ZkSecurityMigrator tool. To execute the tool, run this script `bin/zookeeper-security-migration.sh` with `zookeeper.acl` set to unsecure. This tool traverses the corresponding sub-trees changing the ACLs of the znodes. Use the `--zk-tls-config-file <file>` option if you need to set TLS configuration.
3. If you are disabling mTLS, enable the non-TLS port in ZooKeeper
4. Perform a second rolling restart of brokers, this time omitting the system property that sets the JAAS login file and/or removing ZooKeeper mutual TLS configuration (including connecting to the non-TLS-enabled ZooKeeper port) as required
5. If you are disabling mTLS, disable the TLS port in ZooKeeper

Here is an example of how to run the migration tool:

```bash
> bin/zookeeper-security-migration.sh --zookeeper.acl=secure --zookeeper.connect=localhost:2181
```

Run this to see the full list of parameters:

```bash
> bin/zookeeper-security-migration.sh --help
```

### **Migrating the ZooKeeper ensemble**

It is also necessary to enable SASL and/or mTLS authentication on the ZooKeeper ensemble. To do it, we need to perform a rolling restart of the server and set a few properties. See above for mTLS information. Please refer to the ZooKeeper documentation for more detail:

1. [Apache ZooKeeper documentation](https://zookeeper.apache.org/doc/r3.5.7/zookeeperProgrammers.html#sc\_ZooKeeperAccessControl)
2. [Apache ZooKeeper wiki](https://cwiki.apache.org/confluence/display/ZOOKEEPER/Zookeeper+and+SASL)

### **ZooKeeper Quorum Mutual TLS Authentication**

It is possible to enable mTLS authentication between the ZooKeeper servers themselves. Please refer to the [ZooKeeper documentation](https://zookeeper.apache.org/doc/r3.5.7/zookeeperAdmin.html#Quorum+TLS) for more detail.

## ZooKeeper Encryption <a href="#zk_encryption" id="zk_encryption"></a>

ZooKeeper connections that use mutual TLS are encrypted. Beginning with ZooKeeper version 3.5.7 (the version shipped with Kafka version 2.5) ZooKeeper supports a sever-side config `ssl.clientAuth` (case-insensitively: `want`/`need`/`none` are the valid options, the default is `need`), and setting this value to `none` in ZooKeeper allows clients to connect via a TLS-encrypted connection without presenting their own certificate. Here is a sample (partial) Kafka Broker configuration for connecting to ZooKeeper with just TLS encryption.&#x20;

```
# connect to the ZooKeeper port configured for TLS
zookeeper.connect=zk1:2182,zk2:2182,zk3:2182
# required to use TLS to ZooKeeper (default is false)
zookeeper.ssl.client.enable=true
# required to use TLS to ZooKeeper
zookeeper.clientCnxnSocket=org.apache.zookeeper.ClientCnxnSocketNetty
# define trust stores to use TLS to ZooKeeper; ignored unless zookeeper.ssl.client.enable=true
# no need to set keystore information assuming ssl.clientAuth=none on ZooKeeper
zookeeper.ssl.truststore.location=/path/to/kafka/truststore.jks
zookeeper.ssl.truststore.password=kafka-ts-passwd
# tell broker to create ACLs on znodes (if using SASL authentication, otherwise do not set this)
zookeeper.set.acl=true
```

## Message Encryption

Although Kafka provides multiple authentication methods and encrypted communication over [TLS](https://en.wikipedia.org/wiki/Transport\_Layer\_Security), it does not encrypt data at rest. Yet Kafka is increasingly used as a store of data, not just as a means of transferring data from one location to another. This means that Kafka must conform to the same security and compliance requirements as conventional data storage systems such as relational databases. To compensate, Kafka service providers often use disk or file system encryption. This approach provides a degree of risk mitigation but also has well-known shortcomings, most notably the ability of anyone with appropriate file system permissions, such as system administrators, to read data. Security best practices and compliance standards therefore explicitly call for application- or database-level encryption to protect sensitive information against such exposures. Further, there should be a separation of duty between key management and system administration.

BigConnect Core aims to deliver topic encryption by intercepting the message stream between client and broker in a proxy.

The architecture has the advantage that neither broker nor client need to be modified in order to provide encryption at rest. Incoming data messages ([Produce requests](https://cwiki.apache.org/confluence/display/KAFKA/A%2BGuide%2BTo%2BThe%2BKafka%2BProtocol#AGuideToTheKafkaProtocol-ProduceRequest)) are inspected to determine whether their payload should be encrypted according to a policy. If so, the data portions are encrypted and the modified message is forwarded to the broker. The topic data is then stored by the broker in encrypted form. In the reverse direction, encrypted responses (i.e., [responses to Fetch requests](https://cwiki.apache.org/confluence/display/KAFKA/A%2BGuide%2BTo%2BThe%2BKafka%2BProtocol#AGuideToTheKafkaProtocol-FetchResponse)) are decrypted prior to being sent to clients.

The diagram below depicts the main components, illustrating clients sending and receiving plaintext messages while the proxy exchanges ciphertext messages with the broker:

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

One core component, the _Encryption Module_, provides the encryption functionality. A second core component, the _Proxy_, intercepts Kafka connections and delegates message processing to the Encryption Module.

Topics can be encrypted by different keys, allowing brokers to store a mix of encrypted and unencrypted data, with data owners managing the keys to their topics. Keys will be stored in an external key management system with access policies and logging.

### Configure the Kafka broker's listeners

The address advertised by Kafka must be that of the proxy, not the broker itself.

Modify the `advertised.listeners` property in `$KAFKA_HOME/config/server.properties` to point to the proxy host and port, as shown in the snippet below:

```
# The address the socket server listens on. It will get the value returned from 
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
listeners=PLAINTEXT://:9092

# Hostname and port the broker will advertise to producers and consumers. If not set, 
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
advertised.listeners=PLAINTEXT://127.0.0.1:1234
```

Stop the Kafka broker and start it after the proxy is running.

### Configure the proxy

Create a configuration file, `config.json` and add the following JSON contents:

```
{
  "listening_port" : 1234,
  "kafka_broker"   : "localhost:9092",
  "policy_repo"    : "test"
}
```

### Start Kafka broker

Start the Kafka broker, for example:

```
$KAFKA_HOME/bin/kafka-server-start.sh config/server.properties 
```

### Run Kafka clients

Start the Kafka console producer (note the proxy address in the broker list):

```
$KAFKA_HOME/bin/kafka-console-producer.sh  --broker-list localhost:1234 --topic enctest --producer.config config/producer.properties 
```

Start the Kafka console consumer, like the producer, specifying the proxy host and port:

```
$KAFKA_HOME/bin/kafka-console-consumer.sh --bootstrap-server localhost:1234 --consumer.config config/consumer.properties --topic enctest  --from-beginning
```

Enter arbitry data in the producer and verify that it appears in consumer.

Inspect the topic segment files and verify they indeed are encrypted.

```
$KAFKA_HOME/kafka-dump-log.sh --files /tmp/kafka-logs/enctest-0/00000000000000000000.log --value-decoder-class kafka.serializer.StringDecoder
```
