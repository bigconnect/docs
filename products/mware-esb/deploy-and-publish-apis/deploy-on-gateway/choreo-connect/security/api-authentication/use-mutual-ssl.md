# Use Mutual SSL

Certificate-based authentication on the Choreo Connect is authenticating a request based on a digital certificate, before granting access to the backend. By way of certificate-based authentication, the Choreo Connect supports mutual SSL. In mutual SSL, both parties the client and the server identifies themselves in order to create a successful SSL connection. Mutual SSL allows a client to make a request without a username and password, provided that the server is aware of the client's certificate.

### Create an API secured with Mutual SSL[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/mutual-ssl-authentication/#create-an-api-secured-with-mutual-ssl) <a href="#create-an-api-secured-with-mutual-ssl" id="create-an-api-secured-with-mutual-ssl"></a>

1. [Create an API](https://apim.docs.wso2.com/en/4.1.0/design/create-api/create-rest-api/create-a-rest-api/).
2. Click **Develop -> API Configurations -> Runtime**.
3.  Select **Mutual SSL**.

    [![Enable mutual SSL](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/enable-mutual-ssl.png)](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/enable-mutual-ssl.png)

    Note

    HTTP transport will be disabled for an API if it has Mutual SSL enabled.
4.  Click **Add Certificate** to upload a new client certificate.

    Note

    This feature currently supports only the `.crt` format for certificates.

    If you need to use a certificate in any other format, you can convert it using a standard tool before uploading it.
5.  Provide an alias and public certificate. Select the tier that should be used to throttle out the calls using this particular client certificate and click **Upload**.

    [![Upload Certificate](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/upload-certificate.png)](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/upload-certificate.png)
6. **Save and Deploy** the API.

Important

To invoke mTLS enabled APIs that deployed in Choreo Connect, the CA certificates of the client's public certificates should be added as trusted certificates to the router.

* [Add a Certificate to Choreo Connect Router as a Trusted Certificate](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/tls/backend-certificates/#add-a-certificate-to-choreo-connect-router-as-a-trusted-certificate)

If you need to change the location of the volume mount, the `trustedCertPath` value under the `[router.downstream.tls]` in config.toml should also be changed.

#### Invoke an API secured with Mutual SSL using Postman[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/mutual-ssl-authentication/#invoke-an-api-secured-with-mutual-ssl-using-postman) <a href="#invoke-an-api-secured-with-mutual-ssl-using-postman" id="invoke-an-api-secured-with-mutual-ssl-using-postman"></a>

Import the certificate and private key to Postman.

1.  Navigate to the certificates tab in Postman settings.

    [![Add the certificate to Postman](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/add-certificate-to-postman.png)](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/add-certificate-to-postman.png)
2.  Add the certificate and private key.

    [![Provide certificate and private key](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/provide-crt-and-private-key.png)](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/provide-crt-and-private-key.png)
3. Invoke the API from Postman.

#### Invoke an API secured with Mutual SSL using cURL[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/mutual-ssl-authentication/#invoke-an-api-secured-with-mutual-ssl-using-curl) <a href="#invoke-an-api-secured-with-mutual-ssl-using-curl" id="invoke-an-api-secured-with-mutual-ssl-using-curl"></a>

*   Format

    ```
     curl -k --location -X GET "<API_URL>" -H  "accept: application/json" -H  "Authorization: Bearer <access-token>" --key <client_private_key> --cert <client_public_certificate>
    ```

    Example

Note

`enableClientValidation` configuration should be `true` for sending the client certificates directly to the Choroe Connect. No need to add this configuration to the `config.toml` file since this is the default configuration. If you have added this and changed this to `false`, then the client certificates should be sent within a header.

```
 [enforcer.security.mutualSSL]
     enableClientValidation = true
```

#### Limitations[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/mutual-ssl-authentication/#limitations) <a href="#limitations" id="limitations"></a>

Listed below are the known limitations for this feature.

* Application subscription is not permitted for APIs that are only protected with Mutual SSL. Therefore, subscription or application-level throttling is not applicable to these types of APIs.
* Resource-level throttling is not applicable to the APIs that are only protected with Mutual SSL.
* Resource-level security will not be applicable to the APIs that are only protected with Mutual SSL.
* Scope-level security will not be applicable to the APIs that are only protected with Mutual SSL.

#### Handling MTLS when SSL is terminated by the Load Balancer or Reverse Proxy[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/mutual-ssl-authentication/#handling-mtls-when-ssl-is-terminated-by-the-load-balancer-or-reverse-proxy) <a href="#handling-mtls-when-ssl-is-terminated-by-the-load-balancer-or-reverse-proxy" id="handling-mtls-when-ssl-is-terminated-by-the-load-balancer-or-reverse-proxy"></a>

When SSL termination of API requests takes place at the Load Balancer or Reverse Proxy, the following prerequisites need to be met by the Load Balancer.

* Terminate the mutual SSL connection from the client.
*   Pass the client SSL certificate to the Gateway in an HTTP Header.

    For more information, see the [Nginx documentation](https://nginx.org/en/docs/http/ngx\_http\_ssl\_module.html#ssl\_client\_certificate).

The following diagram illustrates how Mutual SSL works in such an environment.

[![MTLS Load Balancer](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/mtls-loadbalancer.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/mtls-loadbalancer.png)

#### Using mTLS Header to invoke APIs secured with Mutual SSL[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/mutual-ssl-authentication/#using-mtls-header-to-invoke-apis-secured-with-mutual-ssl) <a href="#using-mtls-header-to-invoke-apis-secured-with-mutual-ssl" id="using-mtls-header-to-invoke-apis-secured-with-mutual-ssl"></a>

By default, the Choreo Connect retrieves the client certificate from the **X-WSO2-CLIENT-CERTIFICATE** HTTP header.

Follow the instructions below to enable the mTLS Header and some properties:

1. Navigate to the `<CHOREO-CONNECT_HOME>/docker-compose/choreo-connect(-with-apim)/conf/config.toml` file.
2.  Configure the `enableClientValidation` to `false` under the `[enforcer.security.mutualSSL]` configuration to enable the mTLS Header.

    Format

    ```
     [enforcer.security.mutualSSL]
        certificateHeader = "<Header Name>"      # This will give a custom header name for the mTLS header
        enableClientValidation = false           # This should be false to check the header value for the client certificate
        clientCertificateEncode = false          # This should be true if the client certificate in the header is encoded
        enableOutboundCertificateHeader = false  # This should be true if the client certificate is needed to be sent to the backend
    ```

    Example
3. Start the Server.
4.  Invoke the API with the custom header.

    Format

    ```bash
    curl -k --location -X GET "<API_URL>" -H  "accept: application/json" -H  "Authorization: Bearer <access-token>" -H "<MTSL_Header_name>:<Certificate_Key>"
    ```

    Example

\
