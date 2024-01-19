# Step 2. Create a Truststore File

A truststore file contains certificates from trusted CAs that an SSL/TLS client uses to verify the identity of an SSL/TLS server.

BigConnect Pipelines requires a truststore file to verify the identity of the following SSL/TLS servers:

* Secure LDAP server when BigConnect Pipelines is configured for secure LDAP authentication.
* Control Hub on-premises installation enabled for HTTPS when BigConnect Pipelines is registered with Control Hub on-premises.
* Worker node when BigConnect Pipelines runs cluster pipelines enabled for HTTPS.

If you've enabled HTTPS for cluster pipelines, worker nodes require a truststore file to verify the identity of the gateway node where BigConnect Pipelines is installed.

By default, BigConnect Pipelines and worker nodes use the default Java truststore file located in `$JAVA_HOME/jre/lib/security/cacerts.` If your certificates are signed by a trusted CA that is included in the default Java truststore file, you do not need to create a truststore file for BigConnect Pipelinesor worker nodes and can skip this step.

If your certificates are signed by a private CA or not trusted by the default Java truststore, you must create a custom truststore file or modify a copy of the default Java truststore file to add the root and intermediate CA certificates to the BigConnect Pipelines and worker node truststore file. For example, if your organization generates its own certificates, you must add the root and intermediate certificates for your organization to the truststore file.

You can create a single truststore file used by both BigConnect Pipelines and worker nodes. Or you can create separate truststore files.

In these steps, we show how to modify a copy of the default truststore file to add an additional CA to the list of trusted CAs. We assume that the same CA signed our certificates used by BigConnect Pipelines and by each worker node in the cluster. If multiple CAs signed your certificates, you'll need to add each CA to the truststore file.

If you prefer to create a custom truststore file, see the keytool documentation.

You can create the following types of truststores for BigConnect Pipelines and worker nodes:

* Java keystore file (JKS)
* PKCS #12 (p12 file)

1.  Use the following command to set the JAVA\_HOME environment variable:

    ```
    export JAVA_HOME=<Java home directory>
    ```
2.  Use the following command to set the SDC\_CONF environment variable:

    ```
    export SDC_CONF=<Data Collector configuration directory>
    ```

    For example, for an RPM installation use:

    ```
    export SDC_CONF=/etc/sdc
    ```
3.  Use the following command to copy the default Java truststore file to the BigConnect Pipelines configuration directory:

    ```
    cp "${JAVA_HOME}/jre/lib/security/cacerts" "${SDC_CONF}/truststore.jks"
    ```
4.  Use the following keytool command to import the CA certificate into the truststore file:

    ```
    keytool -import -file <CA certificate> -trustcacerts -noprompt -alias <CA alias> -storepass <password> -keystore "${SDC_CONF}/truststore.jks"
    ```

    For example:

    ```
    keytool -import -file  sdc_company_com.pem -trustcacerts -noprompt -alias MyCorporateCA -storepass changeit -keystore "${SDC_CONF}/truststore.jks"
    ```
5.  If you are enabling HTTPS for cluster pipelines, copy the modified truststore file to the same absolute location on each worker node in the cluster.\
    \
    For example, we'd store our truststore.jks file in the following directory on each worker node:

    ```
    /opt/security/truststore.jks
    ```

    Then store the truststore password in a password text file in the same absolute location on each worker node in the cluster.\


    For our example, we'd store the password in a file named truststore-password.txt in the following directory on each worker node:

    ```
    /opt/security/truststore-password.txt
    ```
