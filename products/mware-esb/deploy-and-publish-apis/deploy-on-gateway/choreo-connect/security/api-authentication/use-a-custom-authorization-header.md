# Use a Custom Authorization Header

By default Choreo Connect uses **Authorization** header to receive the authorization token to secure the APIs. However there can be scenarios where this header needs to be reserved for some other purposes. For example if the backend endpoint of an API requires Authorization header to be present in the request to do perform its own authentication step, you can configure it either in per API or globally at Choreo Connect in order to use a different header to receive the authorization token.

### Per API Configuration[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/use-a-custom-authorization-header/#per-api-configuration) <a href="#per-api-configuration" id="per-api-configuration"></a>

Below extension can be used to do above configuration per API. This extension is not supported in resource level.

Adding OpenAPI extension inside the definition

```java
x-wso2-auth-header: "XAuth"
```

### Global configuration[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/use-a-custom-authorization-header/#global-configuration) <a href="#global-configuration" id="global-configuration"></a>

This security configuration is to be added in to the config.toml file and it is global for all API in a specific runtime.

Adding configuration

```java
[security.adapter]
  authorizationHeader = "XAuth"
```
