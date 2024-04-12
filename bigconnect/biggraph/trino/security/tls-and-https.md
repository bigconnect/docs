# TLS and HTTPS

Trino runs with no security by default. This allows you to connect to the server using URLs that specify the HTTP protocol when using the Trino CLI, the Web UI, or other clients.

This topic describes how to configure your Trino server to use TLS to require clients to use the HTTPS connection protocol. All authentication technologies supported by Trino require configuring TLS as the foundational layer.

Important

This page discusses only how to prepare the Trino server for secure client connections from outside of the Trino cluster to its coordinator.

See the Glossary to clarify unfamiliar terms.

### Supported standards# <a href="#supported-standards" id="supported-standards"></a>

When configured to use TLS, the Trino server responds to client connections using TLS 1.2 and TLS 1.3 certificates. The server rejects TLS 1.1, TLS 1.0, and all SSL format certificates.

The Trino server does not specify a set of supported ciphers, instead deferring to the defaults set by the JVM version in use. The documentation for Java 21 lists its supported cipher suites.

Run the following two-line code on the same JVM from the same vendor as configured on the coordinator to determine that JVM’s default cipher list.

```
echo "java.util.Arrays.asList(((javax.net.ssl.SSLServerSocketFactory) \
javax.net.ssl.SSLServerSocketFactory.getDefault()).getSupportedCipherSuites()).forEach(System.out::println)" | jshell -
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The default Trino server specifies a set of regular expressions that exclude older cipher suites that do not support forward secrecy (FS).

Use the `http-server.https.included-cipher` property to specify a comma-separated list of ciphers in preferred use order. If one of your preferred selections is a non-FS cipher, you must also set the `http-server.https.excluded-cipher` property to an empty list to override the default exclusions. For example:

```
http-server.https.included-cipher=TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA256
http-server.https.excluded-cipher=
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Specifying a different cipher suite is a complex issue that should only be considered in conjunction with your organization’s security managers. Using a different suite may require downloading and installing a different SunJCE implementation package. Some locales may have export restrictions on cipher suites. See the discussion in Java documentation that begins with Customizing the Encryption Algorithm Providers.

Note

If you manage the coordinator’s direct TLS implementatation, monitor the CPU usage on the Trino coordinator after enabling HTTPS. Java prefers the more CPU-intensive cipher suites, if you allow it to choose from a big list of ciphers. If the CPU usage is unacceptably high after enabling HTTPS, you can configure Java to use specific cipher suites as described in this section.

However, best practice is to instead use an external load balancer, as discussed next.

### Approaches <a href="#approaches" id="approaches"></a>

To configure Trino with TLS support, consider two alternative paths:

* Use the load balancer or proxy at your site or cloud environment to terminate TLS/HTTPS. This approach is the simplest and strongly preferred solution.
* Secure the Trino server directly. This requires you to obtain a valid certificate, and add it to the Trino coordinator’s configuration.

### Use a load balancer to terminate TLS/HTTPS# <a href="#use-a-load-balancer-to-terminate-tls-https" id="use-a-load-balancer-to-terminate-tls-https"></a>

Your site or cloud environment may already have a load balancer or proxy server configured and running with a valid, globally trusted TLS certificate. In this case, you can work with your network administrators to set up your Trino server behind the load balancer. The load balancer or proxy server accepts TLS connections and forwards them to the Trino coordinator, which typically runs with default HTTP configuration on the default port, 8080.

When a load balancer accepts a TLS encrypted connection, it adds a [forwarded](https://developer.mozilla.org/docs/Web/HTTP/Proxy\_servers\_and\_tunneling#forwarding\_client\_information\_through\_proxies) HTTP header to the request, such as `X-Forwarded-Proto: https`.

This tells the Trino coordinator to process the connection as if a TLS connection has already been successfully negotiated for it. This is why you do not need to configure `http-server.https.enabled=true` for a coordinator behind a load balancer.

However, to enable processing of such forwarded headers, the server’s config properties file _must_ include the following:

```
http-server.process-forwarded=true
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

This completes any necessary configuration for using HTTPS with a load balancer. Client tools can access Trino with the URL exposed by the load balancer.

### Secure Trino directly[#](https://trino.io/docs/current/security/tls.html#secure-trino-directly) <a href="#secure-trino-directly" id="secure-trino-directly"></a>

Instead of the preferred mechanism of using an external load balancer, you can secure the Trino coordinator itself. This requires you to obtain and install a TLS certificate, and configure Trino to use it for client connections.

#### Add a TLS certificate[#](https://trino.io/docs/current/security/tls.html#add-a-tls-certificate) <a href="#add-a-tls-certificate" id="add-a-tls-certificate"></a>

Obtain a TLS certificate file for use with your Trino server. Consider the following types of certificates:

* **Globally trusted certificates** — A certificate that is automatically trusted by all browsers and clients. This is the easiest type to use because you do not need to configure clients. Obtain a certificate of this type from:
  * A commercial certificate vendor
  * Your cloud infrastructure provider
  * A domain name registrar, such as Verisign or GoDaddy
  * A free certificate generator, such as letsencrypt.org or sslforfree.com
* **Corporate trusted certificates** — A certificate trusted by browsers and clients in your organization. Typically, a site’s IT department runs a local certificate authority and preconfigures clients and servers to trust this CA.
* **Generated self-signed certificates** — A certificate generated just for Trino that is not automatically trusted by any client. Before using, make sure you understand the limitations of self-signed certificates.

The most convenient option and strongly recommended option is a globally trusted certificate. It may require a little more work up front, but it is worth it to not have to configure every single client.

#### Keys and certificates[#](https://trino.io/docs/current/security/tls.html#keys-and-certificates) <a href="#keys-and-certificates" id="keys-and-certificates"></a>

Trino can read certificates and private keys encoded in PEM encoded PKCS #1, PEM encoded PKCS #8, PKCS #12, and the legacy Java KeyStore (JKS) format. Certificates and private keys encoded in a binary format such as DER must be converted.

Make sure you obtain a certificate that is validated by a recognized certificate authority.

#### Inspect received certificates# <a href="#inspect-received-certificates" id="inspect-received-certificates"></a>

Before installing your certificate, inspect and validate the received key and certificate files to make sure they reference the correct information to access your Trino server. Much unnecessary debugging time is saved by taking the time to validate your certificates before proceeding to configure the server.

Inspect PEM-encoded files as described in Inspect PEM files.

Inspect PKCS # 12 and JKS keystores as described in Inspect JKS files.

#### Invalid certificates# <a href="#invalid-certificates" id="invalid-certificates"></a>

If your certificate does not pass validation, or does not show the expected information on inspection, contact the group or vendor who provided it for a replacement.

#### Place the certificate file[#](https://trino.io/docs/current/security/tls.html#place-the-certificate-file) <a href="#place-the-certificate-file" id="place-the-certificate-file"></a>

There are no location requirements for a certificate file as long as:

* The file can be read by the Trino coordinator server process.
* The location is secure from copying or tampering by malicious actors.

You can place your file in the Trino coordinator’s `etc` directory, which allows you to use a relative path reference in configuration files. However, this location can require you to keep track of the certificate file, and move it to a new `etc` directory when you upgrade your Trino version.

#### Configure the coordinator[#](https://trino.io/docs/current/security/tls.html#configure-the-coordinator) <a href="#configure-the-coordinator" id="configure-the-coordinator"></a>

On the coordinator, add the following lines to the config properties file to enable TLS/HTTPS support for the server.

Note

Legacy `keystore` and `truststore` wording is used in property names, even when directly using PEM-encoded certificates.

```
http-server.https.enabled=true
http-server.https.port=8443
http-server.https.keystore.path=etc/clustercoord.pem
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Possible alternatives for the third line include:

```
http-server.https.keystore.path=etc/clustercoord.jks
http-server.https.keystore.path=/usr/local/certs/clustercoord.p12
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Relative paths are relative to the Trino server’s root directory. In a `tar.gz` installation, the root directory is one level above `etc`.

JKS keystores always require a password, while PEM files with passwords are not supported by Trino. For JKS, add the following line to the configuration:

```
http-server.https.keystore.key=<keystore-password>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

It is possible for a key inside a keystore to have its own password, independent of the keystore’s password. In this case, specify the key’s password with the following property:

```
http-server.https.keymanager.password=<key-password>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

When your Trino coordinator has an authenticator enabled along with HTTPS enabled, HTTP access is automatically disabled for all clients, including the Web UI. Although not recommended, you can re-enable it by setting:

```
http-server.authentication.allow-insecure-over-http=true
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Verify configuration[#](https://trino.io/docs/current/security/tls.html#verify-configuration) <a href="#verify-configuration" id="verify-configuration"></a>

To verify TLS/HTTPS configuration, log in to the Web UI, and send a query with the Trino CLI.

* Connect to the Web UI from your browser using a URL that uses HTTPS, such as `https://trino.example.com:8443`. Enter any username into the `Username` text box, and log in to the UI. The `Password` box is disabled while [authentication](https://trino.io/docs/current/security/authentication-types.html) is not configured.
* Connect with the Trino CLI using a URL that uses HTTPS, such as `https://trino.example.com:8443`:

```
./trino --server https://trino.example.com:8443
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Send a query to test the connection:

```
trino> SELECT 'rocks' AS trino;

trino
-------
rocks
(1 row)

Query 20220919_113804_00017_54qfi, FINISHED, 1 node
Splits: 1 total, 1 done (100.00%)
0.12 [0 rows, 0B] [0 rows/s, 0B/s]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations of self-signed certificates[#](https://trino.io/docs/current/security/tls.html#limitations-of-self-signed-certificates) <a href="#limitations-of-self-signed-certificates" id="limitations-of-self-signed-certificates"></a>

It is possible to generate a self-signed certificate with the `openssl`, `keytool`, or on Linux, `certtool` commands. Self-signed certificates can be useful during development of a cluster for internal use only. We recommend never using a self-signed certificate for a production Trino server.

Self-signed certificates are not trusted by anyone. They are typically created by an administrator for expediency, because they do not require getting trust signoff from anyone.

To use a self-signed certificate while developing your cluster requires:

* distributing to every client a local truststore that validates the certificate
* configuring every client to use this certificate

However, even with this client configuration, modern browsers reject these certificates, which makes self-signed servers difficult to work with.

There is a difference between self-signed and unsigned certificates. Both types are created with the same tools, but unsigned certificates are meant to be forwarded to a CA with a Certificate Signing Request (CSR). The CA returns the certificate signed by the CA and now globally trusted.
