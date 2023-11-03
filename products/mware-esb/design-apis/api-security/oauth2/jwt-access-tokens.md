# JWT Access Tokens

JSON Web Token (JWT) is an open standard of transmitting information securely between two parties. As the tokens are digitally signed, the information is secured. The authentication and authorization process uses JWT access tokens. It is ideal to use JWT access tokens as API credentials because JWT access tokens can carry claims (data) that are used in order to authenticate and authorize requests.

WSO2 API Manager supports the use of self-contained and signed JWT formatted OAuth2.0 access tokens as API credentials. Therefore, you can use JWT formatted OAuth2.0 access tokens to authenticate any API that is secured using the OAuth2 security scheme. The App Developer can create an application via the Developer Portal, in WSO2 API Manager, in order to subscribe to an API. The applications in WSO2 API Manager use self-contained signed JWT formatted access tokens.

The gateway itself validates the API requests that are authenticated using the JWT access tokens.

Note

By default, JWT access tokens are not stored or retained after generation, leading to the generation of a new token for each access token request.

### Prerequisites for JWT based tokens[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#prerequisites-for-jwt-based-tokens) <a href="#prerequisites-for-jwt-based-tokens" id="prerequisites-for-jwt-based-tokens"></a>

The following prerequisites have to be satisfied for JWT based tokens to work.

* Only signed JWT access tokens are allowed.
*   The expected token format is as follows:

    `base64(header).base64(payload).base64(signature)`
*   The public certificate of the private key that is used to sign the tokens should be added to the trust store under the `"gateway_certificate_alias"` alias. For more information, see [Import the public certificate into the client trust store.](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#import)

    Note

    The latter mentioned prerequisite is not applicable if you use the default certificates, which are the certificates that are shipped with the product itself.

### Mandatory attributes of a JWT access token[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#mandatory-attributes-of-a-jwt-access-token) <a href="#mandatory-attributes-of-a-jwt-access-token" id="mandatory-attributes-of-a-jwt-access-token"></a>

The following are the mandatory attributes that are required for a JWT access token.

*   `Header`

    | `alg` | The algorithm which signs the token(e.g., RS256). |
    | ----- | ------------------------------------------------- |
*   `Payload`

    | `sub` | The subject of the token, which identifies as to whom the token refers to. |
    | ----- | -------------------------------------------------------------------------- |
    | `iat` | Token issued time                                                          |
    | `exp` | The expiry time of the token.                                              |
    | `iss` | The claim identifies the principal that issued the JWT.                    |

### Using JWT access tokens[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#using-jwt-access-tokens) <a href="#using-jwt-access-tokens" id="using-jwt-access-tokens"></a>

Follow the instructions below to work with JWT Authentication in WSO2 API Manager.

#### Step 1 - Access the Developer Portal[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#step-1-access-the-developer-portal) <a href="#step-1-access-the-developer-portal" id="step-1-access-the-developer-portal"></a>

Sign in to the Developer Portal.

`https://<hostname>:9443/devportal`

#### Step 2 - Create a JWT access token based application[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#step-2-create-a-jwt-access-token-based-application) <a href="#step-2-create-a-jwt-access-token-based-application" id="step-2-create-a-jwt-access-token-based-application"></a>

1. Click **Applications**.
2. Click **ADD NEW APPLICATION**. [![Add new application option](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/add-new-application-option.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/add-new-application-option.png)
3.  Enter the application details. Let's create an application with the following details.

    | Field            | Value    |
    | ---------------- | -------- |
    | Application Name | TestApp  |
    | Per Token Quota  | 10PerMin |
    | Description      | Test App |

    [![Create new application](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/add-new-application.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/add-new-application.png)

#### Step 3 - Generate a JWT access token[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#step-3-generate-a-jwt-access-token) <a href="#step-3-generate-a-jwt-access-token" id="step-3-generate-a-jwt-access-token"></a>

1. Click **APIs** and click on the PizzaShackAPI.
2. Click **Subscriptions**.
3. Select the application that you created and select a throttling policy.
4.  Click **Subscribe**.

    [![Subscribe to the API](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/subscribe-to-api.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/subscribe-to-api.png)
5.  Click **PROD KEYS**, which corresponds to the JWT based application.

    [![View list of credentials](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/view-credentials-list.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/view-credentials-list.png)
6. Click **GENERATE KEYS** to generate consumer key and consumer secret for the production OAuth2 application.
7. Click **GENERATE ACCESS TOKEN**, click **Generate**, and copy the JWT.

#### Step 4 - Invoke the API using the JWT access token[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#step-4-invoke-the-api-using-the-jwt-access-token) <a href="#step-4-invoke-the-api-using-the-jwt-access-token" id="step-4-invoke-the-api-using-the-jwt-access-token"></a>

Invoking an API with a JWT formatted access token is similar to invoking the API with a regular opaque access token. Use the cURL command below to invoke the API via the API Gateway.

Format

```bash
curl -k -X GET  "<API_URL>" -H "accept: application/json" -H "Authorization: Bearer <JWT_TOKEN>"
```

ExampleResponse

### Additional Information[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#additional-information) <a href="#additional-information" id="additional-information"></a>

#### Importing the public certificate into the client trust store[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#importing-the-public-certificate-into-the-client-trust-store) <a href="#importing-the-public-certificate-into-the-client-trust-store" id="importing-the-public-certificate-into-the-client-trust-store"></a>

Follow the instructions below to import the public certificate into the client trust store.

1. Navigate to the `<API-M_HOME>/repository/resources/security/` directory.
2.  Run the following command to export the public certificate from WSO2 API Manager's key store (`wso2carbon.jks`).&#x20;

    `keytool -export -alias wso2carbon -file wso2.crt -keystore wso2carbon.jks`
3. Enter `wso2carbon` as the default password of the key store when prompted.
4.  Run the following command to import the public certificate into the trust store.&#x20;

    ```
    keytool -import -trustcacerts -keystore client-truststore.jks -alias gateway_certificate_alias -file wso2.crt
    ```
5. Enter `wso2carbon` as the default password of the trust store when prompted.

### Validating JWTs generated by external OAuth providers[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#validating-jwts-generated-by-external-oauth-providers) <a href="#validating-jwts-generated-by-external-oauth-providers" id="validating-jwts-generated-by-external-oauth-providers"></a>

JWTs generated by an external OAuth provider can be validated by the gateway as per the following diagram.

#### Signature validation[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#signature-validation) <a href="#signature-validation" id="signature-validation"></a>

[![External JWT Signature Validation](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/external-jwt-signature-validation.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-security/oauth2/access-token-types/external-jwt-signature-validation.png)

In order to do the signature validation of JWTs, you can use one of the following options.

*   Import the public certificate into the client-truststore.jks located at `<API-M_HOME>/repository/resources/security/` with the **kid** value of the certificate as the alias.

    If **kid** is not present in JWT, it will validate with the public certificate under `gateway_certificate_alias` alias from client-truststore.jks.
*   Use the JWKS endpoint of the OAuth provider. (**kid** value should be present in JWT)

    1. Navigate to the `deployment.toml` under the `<API-M_HOME>/repository/conf/` folder.
    2. Add the following configuration under the `[[apim.jwt.issuer]]` with the issuer details.

    Format

    ```
    [[apim.jwt.issuer]]
    name = "<issuer value of jwt>"
    [apim.jwt.issuer.jwks]
    url = "<jwks endpoint of oauth provider>"
    ```

    Example

Important

The JWTs generated by external OAuth providers can be validated only if the tokens are signed by an **asymmetric algorithm** such as `RS256`. Symmetric algorithms such as `HS256` are NOT supported in the validation flow.

#### JWT claim transformation[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#jwt-claim-transformation) <a href="#jwt-claim-transformation" id="jwt-claim-transformation"></a>

In order to convert the incoming JWT claims into desired claims, you can configure the claim mapping as below.

1. Navigate to the `deployment.toml` under the `<API-M_HOME>/repository/conf/` folder.
2.  Add the claim mapping as following under the `[[apim.jwt.issuer]]`.

    ```
    [[apim.jwt.issuer.claim_mapping]]
    remote_claim = "<remote claim value>"
    local_claim = "<local claim value to map>"
    ....
    ```

    By default, the claim mapping is done in a one-to-one manner. But, if the mapping needs to be done in an advanced manner, the users can write custom JWT transformers which can be attached to each token issuer.

#### Scope validation[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#scope-validation) <a href="#scope-validation" id="scope-validation"></a>

By default, the WSO2 API Manager validates the scopes coming in the `scope` claim of the JWT. If the JWT contains the scopes in a different claim, users can map that claim into the `scope` claim as per [JWT claim transformation](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/access-token-types/jwt-tokens/#jwt-claim-transformation) section.
