# Use an Internal Key

This authentication can be used to test APIs that are created in the API Manager Publisher right after deploying the API and even before publishing. An Internal Key can be generated from the API Manager Publisher Try Out page and can be used for both of the modes given below.

* Choreo Connect is run with API Manager as the Control Plane
* Choreo Connect is run as a standalone Gateway and **APIM Publisher** is set as a [token service](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/configuring-an-external-key-manager/)

#### Step 1 - Create and deploy an API to Choreo Connect via API Manager[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/internal-key-authentication/#step-1-create-and-deploy-an-api-to-choreo-connect-via-api-manager) <a href="#step-1-create-and-deploy-an-api-to-choreo-connect-via-api-manager" id="step-1-create-and-deploy-an-api-to-choreo-connect-via-api-manager"></a>

Follow the instructions in the guide [Deploying a REST API in Choreo Connect](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect).

#### Step 2 - Generate an Internal Key from API-M Publisher[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/internal-key-authentication/#step-2-generate-an-internal-key-from-api-m-publisher) <a href="#step-2-generate-an-internal-key-from-api-m-publisher" id="step-2-generate-an-internal-key-from-api-m-publisher"></a>

1.  Navigate to **Try Out** from the left menu bar in API-M Publisher.

    [![Try out menu option in the left panel](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/create-api/test-api/publisher-testconsole-leftpane.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/create-api/test-api/publisher-testconsole-leftpane.png)
2.  In the Try Out page, you will find an Internal Key that has already been generated for you. You can click the button **Generate Key** whenever you need a new token.

    [![Generate key](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/create-api/test-api/publisher-testconsole-generatekey.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/create-api/test-api/publisher-testconsole-generatekey.png)

    Important

    This token must be included in the header `Internal-Key` when invoking the API. The header name used for Internal Key can be updated using the following config.

    ```
    [enforcer.security.authHeader]
      testConsoleHeaderName = "Internal-Key-New"
    ```

    Info

    The Internal key is also a JWT and has the `token_type` as `InternalKey` in its payload. This value is refered during validation in Choreo Connect to identify Internal Keys.

#### Step 3 - Invoke the API using the Internal Key[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/internal-key-authentication/#step-3-invoke-the-api-using-the-internal-key) <a href="#step-3-invoke-the-api-using-the-internal-key" id="step-3-invoke-the-api-using-the-internal-key"></a>

Use the cURL command below to invoke the API via the gateway.

Format

```bash
curl -k -X GET "<API_URL>" -H  "accept: application/json" -H  "Internal-Key: <Internal-Key>"
```

Example

Important

If the APIM publisher uses certificate with different alias name, add the alias name for certificateAlias Configuration in below configuration define at config.toml.

```
[[security.enforcer.tokenService]]
name="APIM Publisher"
issuer = "https://localhost:9443/publisher"
certificateAlias = ""
certificateFilePath = "/home/wso2/security/truststore/wso2carbon.pem"
```
