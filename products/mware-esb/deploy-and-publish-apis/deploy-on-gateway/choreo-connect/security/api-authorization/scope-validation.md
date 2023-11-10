# Scope Validation

Scopes are used to validating the rights to access the requested API resource. When an API/operation is protected with scopes, the invocation request should have a token with one of the required scopes to successfully access the resource. To provide scopes for authorization, we need to define the OAuth2 security scheme with relevant scopes for the operation in the API Definition.

WSO2 Choreo Connect provides scope validation for the following API security types:

* OAuth2 tokens (JWT only)

#### Define supported scopes in API definition[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authorization/scope-validation/#define-supported-scopes-in-api-definition) <a href="#define-supported-scopes-in-api-definition" id="define-supported-scopes-in-api-definition"></a>

First, define an OAuth2 scheme under components/securitySchemes with all supported scopes. If you define multiple scopes under a security scheme, they will be in an OR relationship (i.e., providing at least one scope of them, is enough to authorize the request successfully).

Note

The following example shows how to define OAuth2 security schemes in [OAS3](https://swagger.io/docs/specification/authentication/oauth2/) . If you are using an OAS2 API definition, please refer to [this](https://swagger.io/docs/specification/2-0/authentication/api-keys/) [Swagger document](https://swagger.io/docs/specification/2-0/authentication/) on defining authorization in OAS2.

```yml
components:
  securitySchemes:
    OAuth2:
      type: oauth2
      flows:
        authorizationCode:
          authorizationUrl: https://example.com/oauth/authorize
          tokenUrl: https://example.com/oauth/token
          scopes:
            read: Grants read access
            write: Grants write access
            admin: Grants access to admin operations
```

List the scopes required by each operation or API by listing OAuth2 security with relevant scopes. To do this, list the scopes required by each operation in the `security` section of that operation:

```yml
# Assign oauth2 security scheme with scopes to the operation
paths:
  "/pet/{petId}":
     get:
       security:
         - OAuth2:
           - read
           - write
```

Note

When an OAuth2 type security scheme is provided under an operation or API, it will secure the operation or API with OAuth2 security (JWT and Opaque security tokens).

#### Validating scopes through JWT authentication[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authorization/scope-validation/#validating-scopes-through-jwt-authentication) <a href="#validating-scopes-through-jwt-authentication" id="validating-scopes-through-jwt-authentication"></a>

If the operation or API is secured with an OAuth2 security scheme with scopes, you can authenticate the API request with a valid JWT token. However, the JWT token should have at least one required scope under the "scopes" claim to authorize the request.

```yml
"scope": "read write" # payload of the JWT should contain required scopes separated by space
```
