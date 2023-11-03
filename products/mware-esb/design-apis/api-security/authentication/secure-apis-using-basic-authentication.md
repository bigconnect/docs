# Secure APIs using Basic Authentication

Basic authentication is a simple HTTP authentication scheme in which the request will contain an authorization header with a valid base64 encoded username and password. The WSO2 API Manager is able to authenticate requests using Basic and OAuth2 authentication schemes. In addition to using these schemes individually, it is also possible to use the OAuth2 and Basic schemes at the same time.

The subsequent sections explain how you can work with Basic Authentication on the WSO2 API Gateway.

### Enabling Basic Authentication for an API <a href="#enabling-basic-authentication-for-an-api" id="enabling-basic-authentication-for-an-api"></a>

Basic Authentication is an API level configuration. Please sign in to the API Publisher and click on the API that you need to configure the Basic Authentication. Under the Application Level Security section in the Runtime Configuration of the API Details page allow users to configure the Basic Authentication as follow.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/basic_authentication.png" alt=""><figcaption></figcaption></figure>

### Understanding multiple authentication schemes <a href="#understanding-multiple-authentication-schemes" id="understanding-multiple-authentication-schemes"></a>

The WSO2 API Gateway is able to authenticate requests using Mutual SSL, Basic, OAuth2 and API Key authentication schemes. In addition to using these schemes individually, it is also possible to use the multiple schemes at the same time.

If someone enable multiple schemes, the priority will be given in the order of Mutual SSL, OAuth2, Basic Auth and API Key. Between OAuth2/Basic Auth and API Key, it will authenticate with only one authentication type based on the above order.

Mutual SSL treats as a transport level authentication scheme and it's separate out from the Application security schemes.

Additionally, the user will required to specify either one of Mutual SSL or OAuth2/Basic Auth/API Key mandatory as without anyone of mandatory will skip the authentication.

Note : If OAuth2/Basic Auth is set as mandatory, the request needs to be authenticated using only one of them. If OAuth2 failed only, the Basic Authentication will be applied.

### Invoking an API using Basic Authentication <a href="#invoking-an-api-using-basic-authentication" id="invoking-an-api-using-basic-authentication"></a>

Use the cURL command below to invoke the API via the gateway.

Format

```bash
curl -k -X GET "<API_URL>" -H  "accept: application/json" -H  "Authorization: Basic base64(username:password)"
```

Example

### Basic Authentication with Scopes <a href="#basic-authentication-with-scopes" id="basic-authentication-with-scopes"></a>

WSO2 API Manager allow users to configure [Scopes](https://apim.docs.wso2.com/en/4.2.0/design/api-security/oauth2/oauth2-scopes/fine-grained-access-control-with-oauth-scopes/) with role bindings which can associate with API Resources. Basic authentication uses credentials of the user to authenticate with the Basic Authentication protected API.

If a user associate API resources with scopes which protected with Basic Authentication scheme, API Manager will perform the role validation of the defined scopes in the resources against the authenticated user roles.
