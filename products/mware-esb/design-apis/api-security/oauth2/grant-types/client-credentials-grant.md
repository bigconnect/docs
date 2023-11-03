# Client Credentials Grant

Client credentials can be used when the authorization scope is limited to the protected resources belonging to the client. Client credentials are used as an authorization grant when the client requests access to protected resources based on an authorization previously arranged with the authorization server. The client application requests an access token from the authorization server, authenticating the request with its client key and client secret. If the client is successfully authenticated, an access token is returned.

### Flow[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#flow) <a href="#flow" id="flow"></a>

The client can request an access token using only its client credentials with this grant type. It is similar to the [resource owner password credentials grant](https://apim.docs.wso2.com/en/4.2.0/design/api-security/oauth2/grant-types/password-grant) type, except in this case, only the client’s credentials are used to authenticate a request for an access token.

[![OAuth Client Credentials](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/oauth-client-credentials-diagram.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/oauth-client-credentials-diagram.png)

### Invoking the Token API to generate the tokens[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#invoking-the-token-api-to-generate-the-tokens) <a href="#invoking-the-token-api-to-generate-the-tokens" id="invoking-the-token-api-to-generate-the-tokens"></a>

#### Step 1 - Access the WSO2 Developer Portal[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#step-1-access-the-wso2-developer-portal) <a href="#step-1-access-the-wso2-developer-portal" id="step-1-access-the-wso2-developer-portal"></a>

Sign in to the WSO2 Developer Portal (`https://<hostname>:9443/devportal`).

#### Step 2 - Enable the Client Credentials grant type[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#step-2-enable-the-client-credentials-grant-type) <a href="#step-2-enable-the-client-credentials-grant-type" id="step-2-enable-the-client-credentials-grant-type"></a>

Follow the instructions below to make sure that the Client Credentials grant type is enabled:

1. Click **Applications** and click on the respective application name.
2.  Click **Production Keys** or **Sandbox Keys** based on the environment that you wish to work with.

    Let's click **Production Keys**.
3. Click **OAuth2 Tokens** to navigate to the **OAuth2 Keys** page.
4. Navigate to the **Grant Types** section, which is under the **Key Configurations** section.
5.  Select **Client Credentials**.

    [![Client Credentials](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/client-credentials.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/client-credentials.png)

#### Step 3 - Get a valid consumer key and consumer secret pair[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#step-3-get-a-valid-consumer-key-and-consumer-secret-pair) <a href="#step-3-get-a-valid-consumer-key-and-consumer-secret-pair" id="step-3-get-a-valid-consumer-key-and-consumer-secret-pair"></a>

1. Navigate back to the **OAuth2 Keys** page (in case you have navigated away from it).
2. Click **GENERATE KEYS** to generate the Production keys.

#### Step 4 - Encode the consumer key and consumer secret pair[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#step-4-encode-the-consumer-key-and-consumer-secret-pair) <a href="#step-4-encode-the-consumer-key-and-consumer-secret-pair" id="step-4-encode-the-consumer-key-and-consumer-secret-pair"></a>

Combine the consumer key and consumer secret key in the format `consumer-key:consumer-secret` and encode the combined string using [base64](http://base64encode.org/).

#### Step 5 - Obtain the access token[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#step-5-obtain-the-access-token) <a href="#step-5-obtain-the-access-token" id="step-5-obtain-the-access-token"></a>

Use either one of the following sample cURL commands for this purpose.

**Obtain the access token without specifying the scope**[**¶**](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#obtain-the-access-token-without-specifying-the-scope)

Format

```java
curl -k -d "grant_type=client_credentials" -H "Authorization: Basic <Base64-encoded-client_key:client_secret>" -H "Content-Type: application/x-www-form-urlencoded" https://localhost:<https-port>/token -v
```

Example

**Response**

```
> POST /token HTTP/1.1
> Host: localhost:8243
> User-Agent: curl/7.54.0
> Accept: */*
> Authorization: Basic cEJ6dUlaaEdwaGZRbWRjVVgwbG5lRmlpdXh3YTo0U0pnV19qTU56aGpIU284OGJuZVhtTnFNMjRh
> Content-Type: application/x-www-form-urlencoded
> Content-Length: 29
< HTTP/1.1 200 OK
< X-Frame-Options: DENY
< Cache-Control: no-store
< X-Content-Type-Options: nosniff
< Pragma: no-cache
< X-XSS-Protection: 1; mode=block
< Content-Type: application/json
< Date: Thu, 18 Jan 2018 12:54:32 GMT
< Transfer-Encoding: chunked
{"access_token":"4c27f899-6f9c-3217-b974-3ceb5a409ac3","scope":"am_application_scope default","token_type":"Bearer","expires_in":723}
```

**Obtain the access token by specifying a scope**[**¶**](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#obtain-the-access-token-by-specifying-a-scope)

Format

```java
curl -k -d "grant_type=client_credentials&scope=test" -H "Authorization: Basic <ConsumerKey:ConsumerSecret>" -H "Content-Type: application/x-www-form-urlencoded" https://localhost:9443/oauth2/token
```

Example

### Additional information[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#additional-information) <a href="#additional-information" id="additional-information"></a>

#### Disabling the Client Credentials grant type[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#disabling-the-client-credentials-grant-type) <a href="#disabling-the-client-credentials-grant-type" id="disabling-the-client-credentials-grant-type"></a>

If you want to **disable the Client Credentials grant type** in the API-M instance, add the following entry to the `deployment.toml` file in the `<API-M_HOME>/repository/conf/` folder.

```toml
[oauth.grant_type.client_credentials]
enable = false
```

#### Refresh Token grant type - supported or not[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/grant-types/client-credentials-grant/#refresh-token-grant-type-supported-or-not) <a href="#refresh-token-grant-type-supported-or-not" id="refresh-token-grant-type-supported-or-not"></a>

The Client Credentials grant type **does not support** the Refresh Token grant type. This grant type does not issue a refresh token which can be used to obtain new access tokens using the [refresh token grant](https://apim.docs.wso2.com/en/4.2.0/learn/api-security/oauth2/grant-types/refresh-token-grant/).
