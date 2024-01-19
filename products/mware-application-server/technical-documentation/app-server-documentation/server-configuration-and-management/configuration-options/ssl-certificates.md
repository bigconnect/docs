# SSL Certificates

MWARE AS uses a dedicated key and certificate store when using SSL/TLS connections. The files are located in the configuration directory of the domain `<payara-home>/glassfish/domains/<domain-name>/config` and referenced through JVM options so that they are used for the entire JVM.

_cacerts.p12_: Java Keystore formatted file used as Trust Store file holding the X.509 certificates. _keystore.p12_: Java Keystore formatted file containing the Cryptographic keys, like the private keys associated with the certificates.

These keystores are protected with the Master Password of your domain.

### Adding SSL Certificate <a href="#add-certificate" id="add-certificate"></a>

To add your SSL Certificate to the MWARE AS configuration files, you need:

* The private key associated with your certificate.
* The Certificate you received from the Certificate Authority based on your Certificate Signing Request (CSR).

The private key can be stored in various formats. We assume that the key is in a format which can be read by the Java key tool program, such as PKCS#8 (file starts with `BEGIN ENCRYPTED PRIVATE KEY`)

#### Combine the Private Key with the Certificate.

In a first step, we combine the Private key and the Certificate we received into a file. This file can then be used to merge our data into the files used by the Payara installation. In this step, we also specify an alias name for our key/certificate. This alias name is important as we will use it later to refer to our specific SSL Certificate.

The command to perform this combination is the following with OpenSSL:

```shell
openssl pkcs12 -export -in mycert.crt -inkey mycert.key -out mycert.p12 -name mydomain_certificate
```

* `mycert.key` contains the private key
* `mycert.crt` contains the X.509 certificate
* `-name` indicate the alias we assign to our certificate

The resulting file, _mycert.p12_ is also encrypted with a password that is requested during the execution of the command.

#### Import into `keystore.p12`

The information needs to be imported into the `keystore.p12` file of your Payara Domain which will use it.

This is performed using the _keytool_ command of the JDK:

```shell
keytool -importkeystore -destkeystore keystore.p12 -srckeystore mycert.p12 -srcstoretype PKCS12 -alias mydomain_certificate
```

The parameters of the above command are self-explanatory. Important here is the correct alias value.

#### Import into `cacerts.p12`

A similar command is used to import the information into the Trust Store file so that our certificate is trusted by the JVM.

```shell
keytool -importcert -trustcacerts -destkeystore cacerts.p12 -file mycert.crt -alias mydomain_certificate
```

#### Update HTTP Listener

The last step we need to perform is the indication which 'certificate' needs to be used when Payara receives an SSL/TLS request. This can be done by specifying the alias name we have used when we integrated the required information into the key store files of the domain.

This information can be set either via the asadmin CLI like this:

```shell
asadmin set configs.config.server-config.network-config.protocols.protocol.http-listener-2.ssl.cert-nickname=mydomain_certificate
```

In a default installation, the _http-listener-2_ is responsible for handling the secure connections on port **8181**. The above command sets the alias name so that the system knows which certificate needs to be used.

The information can also be entered in the Web Administration console, by navigating to the page Configurations → server-config → HTTP Service → HTTP Listeners → `http-listener-2`.

### Loading Certificates From Multiple Keystores

To add additional keystores to MWARE AS you will need to have your keystore and truststore files in a local directory. You can then configure this using the admin console or asadmin commands. If you want to load more than one additional keystore, you will need to use a delimiter between the paths to the additional keystores. This is different depending on your OS, Windows uses ';' and Linux uses ':'.

The new JVM properties used to add additional keystores are:

```
-Dfish.payara.ssl.additionalKeyStores
-Dfish.payara.ssl.additionalTrustStores
```

#### Using the Admin Console

To configure the additional keystore locations in the admin console, head to the **Configurations → \<instance configuration> → JVM Settings** and on the **JVM Options** tab click **Add JVM Option**.

You can add the new JVM property and the relative paths to your keystores or truststores here.

![Configure additional keystores and truststores in Admin Console](https://docs.payara.fish/community/docs/\_images/ssl/add-additional-keystores-admin-console.png)

#### Using the Asadmin CLI

JVM options can be configured using the 'create-jvm-options' asadmin command, you can configure your additional keystore and truststore files using this command. If you are loading in multiple additional keystores via asadmin commands, you will need to prefix the appropriate delimiter for your OS with '\\' to avoid creating multiple JVM options.

**Additional Keystores**

```shell
asadmin create-jvm-options "-Dfish.payara.ssl.additionalKeyStores=/path/to/keystore.p12\:/path2/to/keystore2.p12"
```

**Additional Truststores**

```shell
asadmin create-jvm-options "-Dfish.payara.ssl.additionalTrustStores=/path/to/truststore.p12\:/path2/to/truststore2.p12"
```

### Certificate expiration

All the X.509 certificates have a validity period when they can be used. Once this validity period is passed, the users will see a warning or error message depending on the browser that the certificate is no longer valid.

Within the server log file, the expired certificates are listed when the system encounters one. Besides your custom certificates which are added as described in a previous chapter, the Trust Store also contains certificates from the Certificate Authorities. Also, they can expire and thus can be listed in the log.
