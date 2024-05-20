# Security

## Authentication

You can enable Ignite Authentication by setting the `authenticationEnabled` property to `true` in the node’s configuration. This type of authentication requires persistent storage be enabled for at least one data region.

The first node that you start must have authentication enabled. Upon start-up, Ignite creates a user account with the name "ignite" and password "ignite". This account is meant to be used to create other user accounts for your needs. Then simply delete the "ignite" account.

You can manage users using the following SQL commands:

* CREATE USER
* ALTER USER
* DROP USER

## SSL/TLS <a href="#ssl-tls" id="ssl-tls"></a>

To ensure a sufficient level of security, we recommend that each node (server or client) has its own unique certificate in the node’s keystore (including the private key). This certificate must be trusted by all other server nodes.

### SSL/TLS for Nodes

To enable SSL/TLS for cluster nodes, configure an `SSLContext` factory in the node configuration. You can use the `org.apache.ignite.ssl.SslContextFactory`, which is the default factory that uses a configurable keystore to initialize the SSL context.

The keystore must contain the node’s certificate, including its private key. The trust store must contain the trusted certificates for all other cluster nodes.

You can define other properties, such as key algorithm, key store type, or trust manager.&#x20;

After starting the node, you should see the following messages in the logs:

<pre><code><strong>Security status [authentication=off, tls/ssl=on]
</strong></code></pre>

### SSL/TLS for Thin Clients and JDBC/ODBC

Ignite uses the same SSL/TLS properties for all clients, including thin clients and JDBC/ODBC connections. The properties are configured within the client connector configuration. The client connector configuration is defined via the `IgniteConfiguration.clientConnectorConfiguration` property.

To enable SSL/TLS for client connections, set the `sslEnabled` property to `true` and provide an `SslContextFactory` in the client connector configuration. You can re-use the SSLContextFactory configured for nodes, or you can configure an SSLContext factory that will be used for client connections only.

Then, configure SSL on the client side in the same way. Refer to the specific client documentation for details.

### Disabling Certificate Validation

In some cases, it is useful to disable certificate validation, for example when connecting to a server with a self-signed certificate. This can be achieved by using a disabled trust manager, which can be obtained by calling the `SslContextFactory.getDisabledTrustManager()` method.

### Upgrading Certificates

If your SSL certificates are about to expire or have been compromised, you can install new certificates without shutting down the whole cluster.

The following is a procedure for updating certificate.

1.  First of all, make sure the new certificates are trusted by all cluster nodes. This step may not be necessary if your trusted stores contain the root certificate and the new certificates are signed by the same CA.

    Repeat the following procedure for the nodes where the certificate is not trusted:

    1. Import the new certificate to the trusted store of the node.
    2. Gracefully restart the node.
    3. Repeat these steps for all server nodes.

    Now all nodes trust the new certificates.
2. Import the new certificate (including the private key) to the key store of the corresponding node and remove the old certificate. Then gracefully restart the node. Repeat this procedure for all certificates you want to update.

