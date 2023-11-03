# Secure APIs using Mutual SSL

In contrast to the usual one-way SSL authentication where a client verifies the identity of the server, in mutual SSL the server validates the identity of the client so that both parties trust each other. This builds a system that has very tight security and avoids any requests made to the client with regard to providing its username/password, as long as the server is aware of the certificates that belong to the client.

The following section explains as to how the APIs in WSO2 API Manager can be secured using mutual SSL in addition to OAuth2.

### Create an API secured with Mutual SSL <a href="#create-an-api-secured-with-mutual-ssl" id="create-an-api-secured-with-mutual-ssl"></a>

1. [Create an API](https://apim.docs.wso2.com/en/4.1.0/design/create-api/create-rest-api/create-a-rest-api/).
2. Click **Develop -> API Configurations -> Runtime**.
3.  Select **Mutual SSL**.



    Note

    HTTP transport will be disabled for an API if it has Mutual SSL enabled.

    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/enable-mutual-ssl.png" alt=""><figcaption></figcaption></figure>
4.  Click **Add Certificate** to upload a new client certificate.

    Note

    This feature currently supports only the `.crt` format for certificates.

    If you need to use a certificate in any other format, you can convert it using a standard tool before uploading it.
5.  Provide an alias and public certificate. Select the tier that should be used to throttle out the calls using this particular client certificate and click **Upload**.

    [![Upload Certificate](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/upload-certificate.png)](https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/upload-certificate.png)
6. **Save and Deploy** the API.

Note

If you are on a Windows environment, the HTTPS listener would have started on a host address of 0:0:0:0:0:0:0:0. You can verify that from the Carbon logs. In that case, you need to define 0:0:0:0:0:0:0:0 as the bindAddress in `<APIM_HOME>/repository/resources/security/listenerprofiles.xml`

#### Invoke an API secured with Mutual SSL using Postman[¶](https://apim.docs.wso2.com/en/latest/design/api-security/api-authentication/secure-apis-using-mutual-ssl/#invoke-an-api-secured-with-mutual-ssl-using-postman) <a href="#invoke-an-api-secured-with-mutual-ssl-using-postman" id="invoke-an-api-secured-with-mutual-ssl-using-postman"></a>

Import the certificate and private key to Postman.

1.  Navigate to the certificates tab in Postman settings.



    <div align="left">

    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/add-certificate-to-postman.png" alt=""><figcaption></figcaption></figure>

    </div>
2.  Add the certificate and private key.



    <div align="left">

    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/provide-crt-and-private-key.png" alt=""><figcaption></figcaption></figure>

    </div>
3. Invoke the API from Postman.

#### Limitations <a href="#limitations" id="limitations"></a>

Listed below are the known limitations for this feature.

* Application subscription is not permitted for APIs that are only protected with Mutual SSL. Therefore, subscription or application-level throttling is not applicable to these types of APIs.
* Resource-level throttling is not applicable to the APIs that are only protected with Mutual SSL.
* Resource-level security will not be applicable to the APIs that are only protected with Mutual SSL.
* Scope-level security will not be applicable to the APIs that are only protected with Mutual SSL.

#### Handling MTLS when SSL is terminated by the Load Balancer or Reverse Proxy <a href="#handling-mtls-when-ssl-is-terminated-by-the-load-balancer-or-reverse-proxy" id="handling-mtls-when-ssl-is-terminated-by-the-load-balancer-or-reverse-proxy"></a>

When SSL termination of API requests takes place at the Load Balancer or Reverse Proxy, the following prerequisites need to be met by the Load Balancer.

* Terminate the mutual SSL connection from the client.
*   Pass the client SSL certificate to the Gateway in an HTTP Header.

    For more information, see the [Nginx documentation](https://nginx.org/en/docs/http/ngx\_http\_ssl\_module.html#ssl\_client\_certificate).

The following diagram illustrates how Mutual SSL works in such an environment.

[![MTLS Load Balancer](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/mtls-loadbalancer.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/mtls-loadbalancer.png)

#### Using MTLS Header to invoke APIs secured with Mutual SSL[¶](https://apim.docs.wso2.com/en/latest/design/api-security/api-authentication/secure-apis-using-mutual-ssl/#using-mtls-header-to-invoke-apis-secured-with-mutual-ssl) <a href="#using-mtls-header-to-invoke-apis-secured-with-mutual-ssl" id="using-mtls-header-to-invoke-apis-secured-with-mutual-ssl"></a>

By default, the WSO2 API Manager retrieves the client certificate from the **X-WSO2-CLIENT-CERTIFICATE** HTTP header.

Follow the instructions below to change the header:

1. Navigate to the `<API-M_HOME>/repository/conf/deployment.toml` file.
2.  Configure the `certificate_header` under the `[apimgt.mutual_ssl]` configuration.

    Format

    ```
     [apimgt.mutual_ssl]
     certificate_header = "<Header Name>"
     # This property needs to be true if the MutualSSL connection is established between the load balancer and the Gateway.
     enable_client_validation = false
     #This property needs to be true if the certificate should be decoded when it is passed from the load balancer to the Gateway.
     client_certificate_encode = false
    ```

    Example
3. Start the Server.
4.  Invoke the API with the custom header.

    Format

    ```bash
    Copied to clipboardcurl -k --location -X GET "<API_URL>" -H  "accept: application/json" -H  "Authorization: Bearer <access-token>" -H "<MTSL_Header_name>:<Certificate_Key>"
    ```

\
