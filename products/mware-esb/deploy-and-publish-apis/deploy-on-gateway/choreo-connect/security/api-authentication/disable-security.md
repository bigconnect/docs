# Disable Security

### Via WSO2 API Manager Publisher Portal[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/disabling-security/#via-wso2-api-manager-publisher-portal) <a href="#via-wso2-api-manager-publisher-portal" id="via-wso2-api-manager-publisher-portal"></a>

Follow the steps given in [Disabling Security for APIs](https://apim.docs.wso2.com/en/4.2.0/design/api-security/api-authentication/disable-security/).

### By updating the OpenAPI definition[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/disabling-security/#by-updating-the-openapi-definition) <a href="#by-updating-the-openapi-definition" id="by-updating-the-openapi-definition"></a>

APIs can be exposed without requiring any authentication (i.e. disable transport security and application security) using the OpenAPI extension `x-wso2-disable-security` . This extension is supported at API, resource, and operation levels. The following is an example of how you can disable security for an API.

API Level

```yml
openapi: 3.0.0
info:
  version: 1.0.0
  title: Petstore
x-wso2-disable-security: true
  paths:
    "/pet/findByStatus":
      get:
```
