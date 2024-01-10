# Prerequisites

Before you enable HTTPS for BigConnect Pipelines and cluster pipelines, complete the following requirements:

**Obtain access to OpenSSL and Java keytool**\
\
If you do not have keystore files that include SSL/TLS certificates signed by a certificate authority (CA), you can request certificates and create the keystore files using the following tools:

* OpenSSL - Use OpenSSL to create a Certificate Signing Request (CSR) that you send to the CA of your choice, as well as to create the keystore and truststore files. For more information, see the OpenSSL documentation.
* Java keytool - You can also use Java keytool to create a CSR and to create keystore and truststore files. Java keytool is part of the Java Development Kit (JDK). For more information, see the keytool documentation.

**Generate SSL/TLS certificate and private key pairs signed by a certificate authority (CA)**\
\
To enable HTTPS for BigConnect Pipelines, generate a single private key and public certificate pair for BigConnect Pipelines. BigConnect Pipelines provides a self-signed certificate that you can use. However, web browsers generally issue a warning for self-signed certificates. BigConnect Pipelines strongly recommends that you generate a key and certificate pair signed by a CA.\
\
To enable HTTPS for cluster pipelines, each worker node requires a certificate. You can generate multiple private key and public certificate pairs - one for each worker node. Or you can generate a single Subject Alternative Names (SAN) certificate valid for all of the worker nodes. BigConnect Pipelines runs on the gateway node in the cluster, so the gateway node uses the same certificate that you generate for BigConnect Pipelines.\
\
**Important**: Each signed certificate must include the fully qualified domain name (FQDN) for the BigConnect Pipelines machine or worker node.\
\
To obtain a certificate from a trusted CA, you must provide proof that you are the owner of the domain name for which you are requesting the certificate. Use OpenSSL or keytool to generate a key pair and then submit a Certificate Signing Request (CSR) to the CA. The exact procedure depends on the CA that you choose to use - see the documentation provided by the CA.
