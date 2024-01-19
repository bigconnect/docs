# Step 1. Create Keystore Files

Create a keystore file that includes each private key and public certificate pair signed by the CA. A keystore is used to verify the identity of the client upon a request from an SSL/TLS server.

To enable HTTPS for BigConnect Pipelines, create a single keystore file for BigConnect Pipelines.

To enable HTTPS for cluster pipelines, each worker node requires a keystore file. If you generated a unique certificate for each worker node, create a unique keystore file for each of those certificates. Or if you generated a SAN certificate valid for all of the worker nodes, create a single keystore file that all the worker nodes can use. BigConnect Pipelines runs on the gateway node in the cluster, so the gateway node uses the same keystore file that you create for BigConnect Pipelines.

BigConnect Pipelines recommends creating keystores in the `PKCS #12` (p12 file) format. In most cases, a CA issues certificates in PEM format. Use OpenSSL to directly import the certificate into a `PKCS #12` keystore.

1.  Use the following command to import the certificate and private key issued in PEM format to a PKCS #12 keystore for BigConnect Pipelines:\


    ```
    openssl pkcs12 -export -in <PEM certificate> -inkey <private key> -out <keystore filename> -name <keystore name> 
    ```

    \
    You will be prompted to create a password for the keystore file.\


    For example, the following command converts the certificate sdc\_company\_com.pem and private key sdc\_company\_com.key to the `PKCS #12` keystore file named sdc\_company\_com.p12:

    ```
    openssl pkcs12 -export -in sdc_company_com.pem -inkey sdc_company_com.key -out sdc_company_com.p12 -name sdc_company_com
    ```
2.  Store the BigConnect Pipelines keystore file in the BigConnect Pipelines configuration directory, `$SDC_CONF`.\
    \
    For example, for an RPM installation of BigConnect Pipelines, we'd store our sdc\_company\_com.p12 keystore file in the following directory:

    ```
    /etc/sdc/sdc_company_com.p12
    ```
3. Store the BigConnect Pipelines keystore password in the default password text file used by BigConnect Pipelines, `$SDC_CONF/keystore-password.txt`.
4. If enabling HTTPS for cluster pipelines, repeat step 1 for each worker node in the cluster.
5.  Store each worker keystore file in the same absolute location on each worker node in the cluster.\
    \
    For example, if we generated worker keystore files named sdc\_worker.p12, we'd store the files in the following directory on each worker node:

    ```
    /opt/security/sdc_worker.p12
    ```
6.  Store the worker keystore password in a password text file in the same absolute location on each worker node in the cluster.\
    \
    For example, we'd store the keystore-password.txt file in the following directory on each worker node:

    ```
    /opt/security/keystore-password.txt
    ```
