# Pass End User Attributes to the Backend

There can be scenarios where a backend service needs to make different decisions or respond with different data, depending on the application end-user that consumes an API. To achieve this the backend service needs to have access to the respective end-user's data at the time an API call takes place.

This can be facilitated by the Gateway by sending the end user attributes that are defined in the respective user store, in a JWT via an HTTP header, to the backend service when the API call is being forwarded.

### How does it work?[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#how-does-it-work) <a href="#how-does-it-work" id="how-does-it-work"></a>

The backend JSON Web Token (JWT) contains the claims that are transferred between two parties, such as the end-user and the backend. A claim is an attribute of the user that is mapped to the underlying user store. A set of claims is referred to as a dialect (e.g., http://wso2.org/claims).

If you enable backend JWT generation in the Gateway, each API request will carry a digitally signed JWT, which is in the following format to the backend service.

`{token header}.{claims list}.{signature}`

When the request goes through the Gateway, the backend JWT is appended as a transport header to the outgoing message. The backend service fetches the JWT and retrieves the required information about the user, application, or token.

### Sample backend JWT[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#sample-backend-jwt) <a href="#sample-backend-jwt" id="sample-backend-jwt"></a>

The following is an example of a backend JWT:

```json
{
    "typ": "JWT",
    "alg": "RS256",
    "x5t": "ODE3Y2ZhMTBjMDM4ZTBmMjAyYzliYTI2YjRhYTZlOGIyZmUxNWE3YQ==",
}
{
    "iss":"wso2.org/products/am",
    "exp":1345183492181,
    "http://wso2.org/claims/subscriber":"admin",
    "http://wso2.org/claims/applicationname":"app2",
    "http://wso2.org/claims/apicontext":"/placeFinder",
    "http://wso2.org/claims/version":"1.0.0",
    "http://wso2.org/claims/tier":"Silver",
    "http://wso2.org/claims/enduser":"sumedha"
}
```

The above JSON Web Token (JWT) contains the following information.

**JWT Header :**

* `"typ"` - Declares that the encoded object is a JWT access token
* `"alg"` - This defines the specific algorithm intended for use with the key
* `"x5t"` - Thumbprint of the x.509 cert (SHA-1 thumbprint)

**JWT Claims set :**

* `"iss"` - The issuer of the JWT
* `"exp"` - The token expiration time
* `"http://wso2.org/claims/subscriber"` - Subscriber to the API, usually the app developer
* `" http://wso2.org/claims/applicationname"` - Application through which API invocation is done
* `" http://wso2.org/claims/apicontext"` - Context of the API
* `" http://wso2.org/claims/version "` - API version
* `" http://wso2.org/claims/tier"` - Tier/price band for the subscription
* `" http://wso2.org/claims/enduser"` - End-user of the app who's action invoked the AP

### Enabling the default backend JWT generator[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#enabling-the-default-backend-jwt-generator) <a href="#enabling-the-default-backend-jwt-generator" id="enabling-the-default-backend-jwt-generator"></a>

Before passing end user attributes, you need to enable and configure the JWT implementation, as mentioned below in Choreo Connect.

1. Open the Choreo Connect configuration file according to the deployment type you are using.
2.  Enable and configure the backend JWT implementation.

    The following is the basic configuration that you need to have in place to enable backend JWT. For more information, on the other backend JWT configurations, see [JWT generation configuration details](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#backend-jwt-generator-configuration-details).

    ```toml
    [enforcer.jwtGenerator]
       enabled = true
    ```
3.  Start the server.

    For more information, see the [Quick Start Guide](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/getting-started/quick-start-guide-docker-with-apim/).

### Enabling a customized backend JWT generator[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#enabling-a-customized-backend-jwt-generator) <a href="#enabling-a-customized-backend-jwt-generator" id="enabling-a-customized-backend-jwt-generator"></a>

Note

WSO2 API Manager comes with the default JWT generator. This JWT generator will generate specific claims based on the specifications and the user demands at the time the product is released. When you update the products, you will need to add/change some of the claims based on the specification updates. This means that even with the given released version, standard claims that come from the API Manager are subjected to change. Therefore, if you have planned to use specific claims in the backend, it is always recommended to implement a custom JWT generator with mandatory claims you wish to consume at your backend

When generating the backend JWT, it retrieves the claims from the invoked JWT. If you need to change the way that JWT is generated in Choreo Connect, such as by adding additional claims or by completely changing the JWT, follow the instructions below to implement the customized Gateway JWT generation:

1.  Write your own JWTGenerator class extending the `org.wso2.carbon.apimgt.common.gateway.jwtgenerator.AbstractAPIMgtGatewayJWTGenerator` class.

    Info

    Choreo Connect uses the [AbstractAPIMgtGatewayJWTGenerator](https://github.com/wso2/carbon-apimgt/blob/master/components/apimgt/org.wso2.carbon.apimgt.common.gateway/src/main/java/org/wso2/carbon/apimgt/common/gateway/jwtgenerator/AbstractAPIMgtGatewayJWTGenerator.java) class to support JWT generation within Choreo Connect.

    ```java
    package org.wso2.carbon.test;

    import org.osgi.service.component.annotations.Component;
    import org.wso2.carbon.apimgt.common.gateway.dto.JWTInfoDto;
    import org.wso2.carbon.apimgt.common.gateway.jwtgenerator.APIMgtGatewayJWTGeneratorImpl;
    import org.wso2.carbon.apimgt.common.gateway.jwtgenerator.AbstractAPIMgtGatewayJWTGenerator;

    import java.util.Map;
    import java.util.UUID;

    @Component(
            enabled = true,
            service = AbstractAPIMgtGatewayJWTGenerator.class,
            name = "customgatewayJWTGenerator"
    )
    public class CustomGatewayJWTGenerator extends APIMgtGatewayJWTGeneratorImpl {

        @Override
        public Map<String, Object> populateStandardClaims(JWTInfoDto jwtInfoDto) {

            return super.populateStandardClaims(jwtInfoDto);
        }

        @Override
        public Map<String, Object> populateCustomClaims(JWTInfoDto jwtInfoDto) {

            Map<String, Object> claims = super.populateCustomClaims(jwtInfoDto);
            claims.put("uuid", UUID.randomUUID().toString());
            return claims;
        }
    }
    ```

    Click here for a sample [Custom Gateway JWTGenerator](https://github.com/wso2/samples-apim/tree/master/CustomGatewayJWTGenerator).
2.  Build your class and add the JAR file in the `<CHOREO-CONNECT_HOME>/resources/enforcer/dropins` directory.

    Note

    If you use Choreo Connect with Helm Charts, please refer to the documentation in [here](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/production-deployment-guideline/#mount-files-into-the-dropins-directory-optional) to add a JAR file into the dropins directory.
3. Enable and configure the JWT implementation.
   * For more information, see [JWT generation configuration details](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#backend-jwt-generator-configuration-details).
   *   Set `enforcer.jwtGenerator.gatewayGeneratorImpl` to your customized class name.

       ```toml
       [enforcer.jwtGenerator]
          gatewayGeneratorImpl = "org.wso2.carbon.test.CustomGatewayJWTGenerator"
       ```
4.  Start the server.

    For more information, see the [Quick Start Guide](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/getting-started/quick-start-guide-docker-with-apim/).

### Backend JWT generator configuration details[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#backend-jwt-generator-configuration-details) <a href="#backend-jwt-generator-configuration-details" id="backend-jwt-generator-configuration-details"></a>

The following is a sample configuration.

```toml
[enforcer.jwtGenerator]
   enabled = true
   encoding = "base64" # base64,base64url
   claimDialect = "http://wso2.org/claims"
   convertDialect = false
   header = "X-JWT-Assertion"
   signingAlgorithm = "SHA256withRSA"
   enableUserClaims = false
   gatewayGeneratorImpl = "org.wso2.carbon.test.CustomGatewayJWTGenerator"
   claimsExtractorImpl = "org.wso2.carbon.apimgt.impl.token.ExtendedDefaultClaimsRetriever"
   publicCertificatePath = "/home/wso2/security/truststore/mg.pem"
   privateKeyPath = "/home/wso2/security/keystore/mg.key"
```

The relevant elements in the JWT generation configuration are described below. If you do not configure these elements, they take their default values.

| Element                                                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Default Value                                                                                                                                     |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| <pre><code>enforcer.jwtGenerator.enable
</code></pre>                        | Uncomment this property and set this value to **`true`** to enable JWT.                                                                                                                                                                                                                                                                                                                                                                                             | false                                                                                                                                             |
| <pre><code>enforcer.jwtGenerator.header
</code></pre>                        | The name of the HTTP header to which the JWT is attached.                                                                                                                                                                                                                                                                                                                                                                                                           | X-JWT-Assertion                                                                                                                                   |
| <pre><code>enforcer.jwtGenerator.claimDialect
</code></pre>                  | The JWT access token contains all claims that are defined in the `enforcer.jwtGenerator.claimDialect` element. The default value of this element is `http://wso2.org/claims`. To get the list of a specific user's claims that need to be included in the JWT, uncomment this element after enabling the JWT. It will include all claims in `http://wso2.org/claims` to the JWT access token.                                                                       | http://wso2.org/claims                                                                                                                            |
| <pre><code>enforcer.jwtGenerator.convertDialect
</code></pre>                | Remap the OIDC claims into the configured dialect.                                                                                                                                                                                                                                                                                                                                                                                                                  | false                                                                                                                                             |
| <pre><code>enforcer.jwtGenerator.signingAlgorithm
</code></pre>              | <p>The signing algorithm is used to sign the JWT. The general format of the JWT is <code>{token header}.{claims list}.{signature}</code>. When `NONE` is specified as the algorithm, signing is turned off and the JWT looks as <code>{token header}.{claims list}</code> with two strings delimited by a period and a period at the end.</p><p>This element can have only two values - the default values are <code>SHA256withRSA</code> or <code>NONE</code>.</p> | SHA256withRSA                                                                                                                                     |
| <pre><code>enforcer.jwtGenerator.enableUserClaims
</code></pre>              | Enable/disable user claims in the token.                                                                                                                                                                                                                                                                                                                                                                                                                            | `false`                                                                                                                                           |
| <pre><code>enforcer.jwtGenerator.gatewayGeneratorImpl
</code></pre>          | Fully qualified custom JWT generator to used in JWT(Self Contained) Access Tokens.                                                                                                                                                                                                                                                                                                                                                                                  | `org.wso2.carbon.apimgt.common.gateway.jwtgenerator.APIMgtGatewayJWTGeneratorImpl`                                                                |
| <pre><code>enforcer.jwtGenerator.claimsExtractorImpl
</code></pre>           | Fully qualified custom Claim Retriever to add custom claims into JWT.                                                                                                                                                                                                                                                                                                                                                                                               | `org.wso2.carbon.apimgt.impl.token.ExtendedDefaultClaimsRetriever`                                                                                |
| <pre><code>enforcer.jwtGenerator.jwksRateLimitQuota
</code></pre>            | How many requests to the JWKS endpoint can be served in the time window.                                                                                                                                                                                                                                                                                                                                                                                            | `1000`                                                                                                                                            |
| <pre><code>enforcer.jwtGenerator.jwksRateLimitTimeWindow
</code></pre>       | Time window for the rate limit to reset.                                                                                                                                                                                                                                                                                                                                                                                                                            | `false`                                                                                                                                           |
| <pre><code>enforcer.jwtGenerator.jwksRateLimitQuota
</code></pre>            | How many requests to the JWKS endpoint can be served in the time window                                                                                                                                                                                                                                                                                                                                                                                             | `1000`                                                                                                                                            |
| <pre><code>enforcer.jwtGenerator.keypair
</code></pre>                       | An object containing the paths to public certificate and private key of an RSA keypair and specifying whether to use it for signing or not.                                                                                                                                                                                                                                                                                                                         | `{ privateKeyPath = "/home/wso2/security/keystore/mg.key" publicCertificatePath = "/home/wso2/security/truststore/mg.pem" useForSigning = true }` |
| <pre><code>enforcer.jwtGenerator.keypair.publicCertificatePath
</code></pre> | Path of the public certificate                                                                                                                                                                                                                                                                                                                                                                                                                                      | `/home/wso2/security/truststore/mg.pem`                                                                                                           |
| <pre><code>enforcer.jwtGenerator.keypair.privateKeyPath
</code></pre>        | Path of the private key                                                                                                                                                                                                                                                                                                                                                                                                                                             | `/home/wso2/security/keystore/mg.key`                                                                                                             |
| <pre><code>enforcer.jwtGenerator.keypair.useForSigning
</code></pre>         | Whether to use key for signing.                                                                                                                                                                                                                                                                                                                                                                                                                                     | `false`                                                                                                                                           |

### JWKS endpoint for backend JWTs[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#jwks-endpoint-for-backend-jwts) <a href="#jwks-endpoint-for-backend-jwts" id="jwks-endpoint-for-backend-jwts"></a>

Backend JWTs can be signed with RSA to ensure their validity when being sent between 2 parties. To verify the JWT on the backend we need the public certificate of the private key used to sign the JWT at the Gateway. The JWKS endpoint is a way to get this public certificate.

#### Usage[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#usage) <a href="#usage" id="usage"></a>

The JWKS endpoint is:

Format

```java
"https://<hostname>:<port>/.wellknown/jwks"
```

Example

The public keys can be received from this endpoint with a `GET` or `POST` request.

#### Sample JWKS response[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#sample-jwks-response) <a href="#sample-jwks-response" id="sample-jwks-response"></a>

```json
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "use": "sig",
            "kid": "ZjcwNmI2ZDJmNWQ0M2I5YzZiYzJmyNg",
            "alg": "RS256",
            "n": "8vjeHzRhvpfMystncPnLBWy_t5F3eCxbcLbdugWnzfnIgaV6TWnqPBUagJBKpzRZs4A9Qja_ZrSVJjYsbARzCS_qiWp0Cdwkqn6ZCXpmbpfjYnKORq8N8M-zWaSZYbNvWJ5oSO4kH-LKWzODaFebwTJBpsR1vChHH95doxFuUjiZaisVaQgUJ6drRdlDtImp9r9EAX36YROuYFPouD-dQ1sdJOa11P_yMs-glfQ"
        }
    ]
}
```

### JWKS Parameters[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#jwks-parameters) <a href="#jwks-parameters" id="jwks-parameters"></a>

| Parameter                     | Description                                                                    |
| ----------------------------- | ------------------------------------------------------------------------------ |
| <pre><code>kty
</code></pre>  | Key type identifies the cryptographic family this key belongs to.              |
| <pre><code>e
</code></pre>    | The exponent value of the public key.                                          |
| <pre><code>use
</code></pre>  | This defines the use of the key, whether it is used for signing or encryption. |
| <pre><code>kid
</code></pre>  | This is an ID parameter used to match a specific key(s).                       |
| <pre><code>alg
</code></pre>  | This defines the specific algorithm intended for use with the key.             |
| <pre><code>n
</code></pre>    | The modulus value of the public key.                                           |
| <pre><code>keys
</code></pre> | An array of the public keys available from the gateway.                        |

### Code Example in Ballerina[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/#code-example-in-ballerina) <a href="#code-example-in-ballerina" id="code-example-in-ballerina"></a>

```bal
import ballerina/http;
import ballerina/jwt;

service / on new http:Listener(8080) {

    resource function get hello(http:Request request) returns string|error {

        // JWT Validator config configured with the Issuer and the Signature config which points at the JWKS URL
        jwt:ValidatorConfig validatorConfig = {
        issuer: "wso2.org/products/am",
        clockSkew: 60,
        signatureConfig: {
            jwksConfig: {url: "https://gateway.e1-us-east-azure.preview-dv.choreoapis.dev/.wellknown/jwks", cacheConfig: {}}
        }
        };
        var jwt = request.getHeader("x-jwt-assertion");

        if !(jwt is string) {
            return error("JWT header not available");

        }
        // Validating the JWT based on its signature and expiration time
        jwt:Payload|jwt:Error result = check jwt:validate(jwt, validatorConfig);

        if result is jwt:Error {
            return error("Failed to authenticate " + result.message());
        }

        return result.toBalString();
    }
}
```

In this example we create a hello route on port 8080 and secure it with JWT. This hello function returns the JWT claims or the error that occurred during validation.

\
