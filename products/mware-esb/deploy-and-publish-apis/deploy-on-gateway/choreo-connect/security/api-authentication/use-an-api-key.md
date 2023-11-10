# Use an API Key

API Key Authentication in Choreo Connect is a simple authentication scheme that accepts a valid self-contained JWT token issued for accessing APIs. Since self-contained tokens can have all authorization information, the token processing time is fast and more efficient. The common practice is to have a short expiration time for self-contained access tokens.

### Update the OpenAPI definition of the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#update-the-openapi-definition-of-the-api) <a href="#update-the-openapi-definition-of-the-api" id="update-the-openapi-definition-of-the-api"></a>

When deploying via WSO2 API Manager, the already existing API Key security schemes defined in the OpenAPI definition will be overridden as you configure API security from the Publisher Portal. Therefore, making changes via the API Manager Publisher UI is sufficient. Use the following steps to enable API Key authentication when deploying an API using apictl, in other words, during standalone mode.

#### Step 1 - Define the API Key security scheme[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#step-1-define-the-api-key-security-scheme) <a href="#step-1-define-the-api-key-security-scheme" id="step-1-define-the-api-key-security-scheme"></a>

Security schemes must be defined in the OpenAPI definition under `securitySchemes`. One or more API Key security schemes can be used simultaneously, similar to a logical OR operator. Define the API Key security scheme as follows:

| Field Name | Description                              |
| ---------- | ---------------------------------------- |
| `name`     | Header name or query key for the APIKey. |
| `in`       | This can be either `header` or `query`.  |
| `type`     | Specify `apiKey` as the type.            |

The following example defines an API Key security scheme named `apiKeyAuth`, which expects `X-API-Key` as a request header. Here, `apiKeyAuth` is the name provided for the security scheme and `X-API-Key` is the name for the header.

Note

The following example shows how to define API Key security schemes in [OAS3](https://swagger.io/docs/specification/authentication/api-keys/). If you are using an OAS2 API definition, go to [this Swagger document](https://swagger.io/docs/specification/2-0/authentication/api-keys/) to get more information on defining API Key security in OAS2.

```yml
# step 1 - Define the API Key security scheme
components:
 securitySchemes:
   ApiKeyAuth:        # arbitrary name for the security scheme
     type: apiKey
     in: header       # can be "header" or "query" 
     name: X-API-KEY  # name of the header or query parameter
   appId:             # you can define several API Key security schemas
     type: apiKey
     in: header
     name: X-APP-ID
```

#### Step 2 - Apply the API Key Authentication security scheme[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#step-2-apply-the-api-key-authentication-security-scheme) <a href="#step-2-apply-the-api-key-authentication-security-scheme" id="step-2-apply-the-api-key-authentication-security-scheme"></a>

A security scheme can be specified at a resource level or to the whole API by using the **`security`** section as applicable. The following is an example of how to apply a security scheme to a resource. A resource-level security scheme will override an API-level security scheme. The `ApiKeyAuth` security scheme that has been defined has been referred to from the `/pet/{petId}:` resource by using the security section. For more information, refer to the [Swagger documentation on API Keys](https://swagger.io/docs/specification/authentication/api-keys/).

* **Operation-specific security**

The following code snippet is an example of how the API key can be applied to a specific resource.

```yml
 # step 2 - Specify the API Key security in operation level
 "/pet/{petId}":
   get:
     tags:
       - pet
     summary: Find pet by ID
     description: Returns a single pet
     operationId: getPetById
     parameters:
       - name: petId
         in: path
         description: ID of pet to return
         required: true
         schema:
           type: integer
           format: int64
     security:
       - ApiKeyAuth: []
```

*   **API Level security**

    The following code snippet is an example of how the API key can be applied globally to all operations.

    ```yml
    # step 2 - Specify the API Key security in api level
    security:
     - ApiKeyAuth: [] 
    ```

#### Step 3 - Generate an API Key[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#step-3-generate-an-api-key) <a href="#step-3-generate-an-api-key" id="step-3-generate-an-api-key"></a>

Currently, only API Keys issued by WSO2 API Manager are allowed. Hence, you need a valid API Key issued for the particular API Name and the version from WSO2 API Manager. Follow the steps mentioned on [generating the API Key documentation](https://apim.docs.wso2.com/en/4.2.0/design/api-security/api-authentication/secure-apis-using-api-keys/#using-api-keys-to-secure-an-api) to generate an API Key from API Manager.

#### Step 4 - Invoke the API using the API Key[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#step-4-invoke-the-api-using-the-api-key) <a href="#step-4-invoke-the-api-using-the-api-key" id="step-4-invoke-the-api-using-the-api-key"></a>

Use one of the following options to invoke APIs using an API key based on the method that you used to define API Key Authentication in [step 1](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#step-1-define-api-key-authentication-for-the-api).

*   **API key defined in the OpenAPI definition security schemes**

    Execute the following cURL command to invoke APIs when the API key is defined in the OpenAPI definition security schemes.

    Format

    ```bash
    curl -k -X GET "<API_URL>" -H  "accept: application/json" -H  "<Header name>: <API Key>"
    ```

    ExampleResponse

### Additional Information[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#additional-information) <a href="#additional-information" id="additional-information"></a>

Choreo Connect API Key Authentication supports API key restriction for IP address and HTTP referrer.

**1) IP address restriction**[**¶**](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#1-ip-address-restriction)

The IP address restriction allows only the clients with specific IP addresses to use the token. The IP addresses can be specified in the following formats.

* IPv4 (e.g., `192.168.1.2`)
* IPv6 (e.g., `2002:eb8::2`)
* IP range in CIDR notation (e.g. `152.12.0.0/13`, `1001:ab8::/14`)

**Generating an API key with an IP restriction**

1. Navigate to API key generation window of the specific application in the Developer Portal.
2. Select `IP Addresses`, add the IP addresses in the text input as shown below, and generate the key.

[![IP Restricted API key](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/ip-api-key.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/ip-api-key.png)

**2) HTTP referrer restriction**[**¶**](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#2-http-referrer-restriction)

When the HTTP referrer restriction has been enabled, only the specific HTTP referrers can use the token. Therefore, by using this restriction, when API clients run on web browsers, you can limit the access to an API through only specific web pages. The referrer can be specified in the following formats.

* A specific URL with an exact path: `www.example.com/path`
* Any URL in a single subdomain, using a wildcard asterisk (\*): `sub.example.com/*`
* Any subdomain or path URLs in a single domain, using wildcard asterisks (\*): `*.example.com/*`

**Generating an API key with the HTTP referrer restriction**

1. Navigate to API key generation window of that specific application in the Developer Portal.
2. Select `HTTP Referrers (Web Sites)` and add the referrers in the text input as shown below and generate the key.

[![HTTP Referer Restricted API key](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/http-referer-api-key.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/http-referer-api-key.png)

### Configurations related to API Key[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/#configurations-related-to-api-key) <a href="#configurations-related-to-api-key" id="configurations-related-to-api-key"></a>

You may use the following to reconfigure API Authentication related configs.

```
[[enforcer.security.tokenService]]
name = "APIM APIkey"
validateSubscription = true
certificateAlias = "apikey_certificate_alias"
certificateFilePath = "/home/wso2/security/truststore/wso2carbon.pem"
```

| Parameter            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name                 | Name of the token service and must not be changed                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| certificateAlias     | Alias name to be used by the internal Enforcer truststore for the public certificate of the JWT issuer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| certificateFilePath  | Certificate file path within the Enforcer container                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| validateSubscription | <p>Whether to validate subscriptions. **If disabled**, a single API key may be used against any API in the same Choreo Connect instance since only the JWT signature would be validated procedure. When set to true, subscription validation is done as given below depending on the Choreo Connect mode.<br><br>- For <a href="https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/concepts/apim-as-control-plane/">Choreo Connect with WSO2 API Manager as a Control Plane</a> - Check against the subscriptions received from the Control Plane in order to validate the mapping between the invoked API and application info in the API key JWT claims.<br><br>- For <a href="https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/concepts/as-a-standalone-gateway/">Choreo Connect as a Standalone Gateway</a> - Check the metadata of the invoked API against the API details included in the API key claims.</p> |

During the absence of the above config section, the following will be referred if present to support backward compatibility.

```
[[enforcer.security.tokenService]]
name = "APIM Publisher"
issuer = "https://localhost:9443/publisher"
validateSubscription = true
certificateAlias = ""
certificateFilePath = "/home/wso2/security/truststore/wso2carbon.pem"
```

