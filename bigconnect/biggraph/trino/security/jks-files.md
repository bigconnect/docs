# JKS files

This topic describes how to validate a Java keystore (JKS) file used to configure TLS and HTTPS.

The Java KeyStore (JKS) system is provided as part of your Java installation. Private keys and certificates for your server are stored in a _keystore_ file. The JKS system supports both PKCS #12 `.p12` files as well as legacy keystore `.jks` files.

The keystore file itself is always password-protected. The keystore file can have more than one key in the the same file, each addressed by its **alias** name.

If you receive a keystore file from your site’s network admin group, verify that it shows the correct information for your Trino cluster, as described next.

### Inspect and validate keystore[#](https://trino.io/docs/current/security/inspect-jks.html#inspect-and-validate-keystore) <a href="#inspect-and-validate-keystore" id="inspect-and-validate-keystore"></a>

Inspect the keystore file to make sure it contains the correct information for your Trino server. Use the `keytool` command, which is installed as part of your Java installation, to retrieve information from your keystore file:

```
keytool -list -v -keystore yourKeystore.jks
```

Keystores always require a password. If not provided on the `keytool` command line, `keytool` prompts for the password.

Independent of the keystore’s password, it is possible that an individual key has its own password. It is easiest to make sure these passwords are the same. If the JKS key inside the keystore has a different password, you are prompted twice.

In the output of the `keytool -list` command, look for:

* The keystore may contain either a private key (`Entry type: PrivateKeyEntry`) or certificate (`Entry type: trustedCertEntry`) or both.
* Modern browsers now enforce 398 days as the maximum validity period for a certificate. Look for the `Valid from ... until` entry, and make sure the time span does not exceed 398 days.
*   Modern browsers and clients require the **SubjectAlternativeName** (SAN) field. Make sure this shows the DNS name of your server, such as `DNS:cluster.example.com`. Certificates without SANs are not supported.

    Example:

```
SubjectAlternativeName [
    DNSName:  cluster.example.com
]
```

If your keystore shows valid information for your cluster, proceed to configure the Trino server, as described in Place the certificate file and Configure the coordinator.

The rest of this page describes additional steps that may apply in certain circumstances.

### Extra: add PEM to keystore[#](https://trino.io/docs/current/security/inspect-jks.html#extra-add-pem-to-keystore) <a href="#extra-add-pem-to-keystore" id="extra-add-pem-to-keystore"></a>

Your site may have standardized on using JKS semantics for all servers. If a vendor sends you a PEM-encoded certificate file for your Trino server, you can import it into a keystore with a command like the following. Consult `keytool` references for different options.

```
keytool -trustcacerts -import -alias cluster -file localhost.pem -keystore localkeys.jks
```

If the specified keystore file exists, `keytool` prompts for its password. If you are creating a new keystore, `keytool` prompts for a new password, then prompts you to confirm the same password. `keytool` shows you the contents of the key being added, similar to the `keytool -list` format, then prompts:

```
Trust this certificate? [no]:
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Type `yes` to add the PEM certificate to the keystore.

The `alias` name is an arbitrary string used as a handle for the certificate you are adding. A keystore can contain multiple keys and certs, so `keytool` uses the alias to address individual entries.

### Extra: Java truststores[#](https://trino.io/docs/current/security/inspect-jks.html#extra-java-truststores) <a href="#extra-java-truststores" id="extra-java-truststores"></a>

Note

Remember that there may be no need to identify a local truststore when directly using a signed PEM-encoded certificate, independent of a keystore. PEM certs can contain the server’s private key and the certificate chain all the way back to a recognzied CA.

Truststore files contain a list of Certificate Authorities trusted by Java to validate the private keys of servers, plus a list of the certificates of trusted TLS servers. The standard Java-provided truststore file, `cacerts`, is part of your Java installation in a standard location.

Keystores normally rely on the default location of the system truststore, which therefore does not need to be configured.

However, there are cases in which you need to use an alternate truststore. For example, if your site relies on the JKS system, your network managers may have appended site-specific, local CAs to the standard list, to validate locally signed keys.

If your server must use a custom truststore, identify its location in the server’s config properties file. For example:

```
http-server.https.truststore.path=/mnt/shared/certs/localcacerts
http-server.https.truststore.key=<truststore-password>
```

If connecting clients such as browsers or the Trino CLI must be separately configured, contact your site’s network administrators for assistance.[\
](https://trino.io/docs/current/security/authentication-types.html)
