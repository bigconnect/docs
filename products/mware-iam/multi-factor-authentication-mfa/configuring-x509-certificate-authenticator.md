# Configuring X509 Certificate Authenticator

### Working with certificates <a href="#working-with-certificates" id="working-with-certificates"></a>

X509 authentication requires the client to possess a Public Key Certificate (PKC).

If **X509 authentication** is specified, the IAM Server will authenticate the client using the client’s public key certificate. To issue the digital certificate, a Certificate Authority (CA) is required. A CA issues digital certificates that contain identity credentials to help websites, people and devices represent their authentic, CA-verified, online identity.

To create a sample certificate and create your own Certificate Authority to sign the certificates, follow the following steps:

1.  The first step is to create the private RSA key:

    ```
    openssl genrsa -out rootCA.key 2048
    ```

    Here, the specified key size is 2048 bit. You can specify the key size for your private key.
2.  Based on this key you can now generate an actual certificate which is valid for 10 years using the following command:

    ```
    openssl req -new -x509 -days 3650 -key rootCA.key -out rootCA.crt
    ```
3. You are prompted to provide the following details, and the details you provide are incorporated into the certificate request. An example is shown below. Make sure you use the values that fit your use case.
   * Country Name (2 letter code) \[AU]: SL
   * State or Province Name (full name) \[Some-State]: Western
   * Locality Name (eg, city) \[ ]: Colombo
   * Organization Name (eg, company) \[Internet Widgits Pty Ltd]: WSO2
   * Organizational Unit Name (eg, section) \[ ]: QA
   * Common Name (e.g. serverFQDN or YOUR name) \[ ]: wso2is.com
   * Email Address \[ ]: kim@wso2.com
4.  An OpenSSL CA requires new files and supporting directories. Therefore, create a new directory. Create the directory structure according to your `openssl.conf` format.

    ```
    mkdir -p demoCA/newcerts
    ```
5.  You also need some initial files inside your CA directory structure.

    ```
    touch demoCA/index.txt
    echo '01' > demoCA/serial
    ```
6.  For the JVM to trust your newly created certificate import your certificate into your JVM trust store by executing the following command:

    ```
    keytool -import -noprompt -trustcacerts -alias rootCA -file rootCA.crt -keystore ${JAVA_HOME}/jre/lib/security/cacerts -storepass changeit
    ```
7. Once you have done the above steps, you have the keystore (`localcrt.jks`), truststore (`cacerts.jks`), and pkcs12 (`localhost.p12`) files that you need to use later on in this guide.
8. Now you have created the CA to sign the certificate. To create the server certificate follow the steps given below:
   1.  Create the keystore that includes the private key by executing the following command:

       ```
       keytool -genkey -v -alias localcrt -keyalg RSA -validity 3650 -keystore localcrt.jks -storepass localpwd -keypass localpwd
       ```
   2.  Execute the following command to generate the certificate signing request(CSR) using the generated keystore file.

       ```
       keytool -certreq -alias localcrt -file localcrt.csr -keystore localcrt.jks -storepass localpwd
       ```
   3. To enable CRL or OCSP based certificate revocation validation, configure the necessary openSSL extension configurations.
      1. Open either of the following files.
         * `validation.cnf`
         * `/usr/lib/ssl/openssl.cnf`
      2.  Set the following properties under `x509_extensions`:

          ```java
          crlDistributionPoints = URI:http://pki.google.com/GIAG2.crl
          authorityInfoAccess = OCSP;URI: http://clients1.google.com/ocsp
          ```
   4.  Once it is done, sign the CSR, which requires the CA root key.

       ```xml
       openssl ca -batch -startdate 150813080000Z -enddate 250813090000Z -keyfile rootCA.key -cert rootCA.crt -policy policy_anything -out localcrt.crt -infiles localcrt.csr
       ```

       This creates a signed certificate called `localcrt.crt` that is valid for a specified period that is denoted by the `startdate` and `enddate`.
   5.  The next step is to import the CA and signed certificate into the keystore.

       ```xml
       keytool -importcert -alias rootCA -file rootCA.crt -keystore localcrt.jks -storepass localpwd -noprompt

       keytool -importcert -alias localcrt -file demoCA/newcerts/01.pem -keystore localcrt.jks -storepass localpwd -noprompt
       ```
   6.  Now, get the `pkcs12` out of `.crt` file using the command given below as it is been used to import certificates to the browser.

       ```xml
       keytool -importkeystore -srckeystore localcrt.jks -destkeystore localhost.p12 -srcstoretype JKS -deststoretype PKCS12 -srcstorepass localpwd -deststorepass browserpwd -srcalias localcrt -destalias browserKey -srckeypass localpwd -destkeypass browserpwd -noprompt
       ```

       Make sure to use the same password you used when creating the keystore for the `srcstorepass` in the above step. Now you have the `localhost.p12` file that you can import into your browser as explained in the import certificate section.
   7.  Next, create a new trust store and import the server certificate into the trust store using the following commands:

       ```xml
       keytool -import -keystore cacerts.jks -storepass cacertspassword -alias rootCA -file rootCA.crt -noprompt
       keytool -importcert -alias localcrt -file localcrt.crt -keystore cacerts.jks -storepass cacertspassword -noprompt

       ```

Once you have done the above steps, you have the keystore (`localcrt.jks`), truststore (`cacerts.jks`), and pkcs12 (`localhost.p12`) files that you need to use later on in this guide.

### Configuring the X509 certificate for the app <a href="#configuring-the-x509-certificate-for-the-app" id="configuring-the-x509-certificate-for-the-app"></a>

Replace your keystore file path, keystore password, trust store file path and trust store password (you can use the keystore and truststore in the following configuration and add it to the `<IAM_HOME>/repository/conf/deployment.toml` file.

```toml
[custom_trasport.x509.properties]
protocols="HTTP/1.1"
port="8443"
maxThreads="200"
scheme="https"
secure=true
SSLEnabled=true
keystoreFile="/path/to/keystore.jks"
keystorePass="keystorepwd"
truststoreFile="/path/to/truststore.jks"
truststorePass="truststorespassword"
bindOnInit=false
clientAuth="want"
ssl_protocol = "TLS"
```

### Configuring the Authentication Endpoint <a href="#configuring-the-authentication-endpoint" id="configuring-the-authentication-endpoint"></a>

1. Open the `deployment.toml` file in the `<IAM_HOME>/repository/conf/` directory.
2.  Add the following configuration to the file.

    1. `authentication_endpoint` : This is the URL with the port that is secured with the certificate, e.g., `https://localhost:8443/x509-certificate-servlet` . Update this based on your host name.
    2. `username` : This attribute value will be taken as the authenticated user subject identifier. Update this with any of the certificate attributes, e.g., CN and Email.
    3. `name` : This attribute identifies the authenticator that is configured as the second authentication step.
    4. `enable`: This attribute, when set to true makes the authenticator capable of being involved in the authentication process.

    ```toml
    [authentication.authenticator.x509_certificate.parameters]
    name ="x509CertificateAuthenticator"
    enable=true
    AuthenticationEndpoint="https://localhost:8443/x509-certificate-servlet"
    username= "CN"
    ```
3.  If you are using the identity claim dialect URI to store X509 certificate, add the following parameter.

    ```toml
    [authentication.authenticator.x509_certificate.parameters]
    setClaimURI = "http://wso2.org/claims/identity/userCertificate"
    ```
4.  To enable storing the X509 certificate as a user claim, add the following parameter.

    ```toml
    [authentication.authenticator.x509_certificate.parameters]
    EnforceSelfRegistration = true
    ```

### Adding a claim mapping for the certificate <a href="#adding-a-claim-mapping-for-the-certificate" id="adding-a-claim-mapping-for-the-certificate"></a>

If storing the certificate as a user claim is enabled, the X509 certificate will be stored as a user claim and verified with the retrieved certificate from the request.

1.  Sign in to the Management Console with one of the following URLs using `admin` as the **username** and **password**.

    ```java
    For HTTP  --> http://<HTTP_HOST>:9776/carbon
    For HTTPS --> https://<HTTPS_HOST>:9443/carbon
    ```
2. On the **Main** tab, click **Claims > Add**.\
   ![add-claim-x509](https://is.docs.wso2.com/en/latest/assets/img/learn/add-claim-x509.png)
3. Click **Add Local Claim**.\
   ![add-local-claim](https://is.docs.wso2.com/en/latest/assets/img/learn/add-local-claim.png)
4. Add a new claim for the **certificate** by giving the details as below, e.g., select a mapped attribute for the claim that is supported by the underlying database type. ![claim-for-certificate](https://is.docs.wso2.com/en/latest/assets/img/learn/claim-for-certificate.png)
5. Click **Add**.

### Configuring the service provider <a href="#configuring-the-service-provider" id="configuring-the-service-provider"></a>

The next step is to configure the service provider.

1. Return to the management console.
2. In the **Service Providers** section under the **Main** tab, click **Add**.
3. Since you are using Travelocity as the sample, enter travelocity.com in the **Service Provider Name** text box and click **Register**.
4. In the **Inbound Authentication Configuration** section, click **Configure** under the **SAML2 Web SSO Configuration** section.
5. Now set the configuration as follows:
   1. **Issuer** : travelocity.com
   2. **Assertion Consumer URL** : http://localhost:8080/travelocity.com/home.jsp
6.  Select the following check-boxes:

    1. **Enable Response Signing**.
    2. **Enable Single Logout**.
    3. **Enable Attribute Profile**.
    4. **Include Attributes in the Response Always**.

    <img src="https://is.docs.wso2.com/en/latest/assets/img/learn/configure-sp.png" alt="configure-sp" data-size="original">
7. Click **Update** to save the changes. Now you will be sent back to the **Service Providers** page.
8. Go to the **Local and Outbound Authentication Configuration** section.
9. You have two options here. You can add X509 certificate authenticator as the first factor and also as the second factor.
   1. Second factor
      1. Select the **Advanced** configuration radio button option.
      2. Add the **basic** authentication as a first step and **X509Certificate** authentication as the second step.\
         ![first-step-authentication](https://is.docs.wso2.com/en/latest/assets/img/learn/first-step-authentication.png)
   2. First factor
      * Select **Local Authentication** as the **Authentication Type** and select **X509Certificate** from the drop-down list.\
        ![first-factor-x509](https://is.docs.wso2.com/en/latest/assets/img/learn/first-factor-x509.png)
      * When using X509 as first step authentication, you need to create a user in IS management console with the Email provided while creating the browser certificate.\
        Example:\
        ![browser-certificate](https://is.docs.wso2.com/en/latest/assets/img/learn/browser-certificate.png)
10. Finally, click on **Update** to finish the service provider configurations.

You have now added and configured the service provider.
