# API Authentication

Choreo Connect provides the ability to secure APIs in API level and resource levels. The following section would concisely guide you through the available security options in Choreo Connect. API security can be divided into two main categories; namely, Application-level security and Transport-level security.

By default, APIs are secured with application security. However, if you want to disable security for the API or only for a specific resource, see [Disabling Security](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/disabling-security/).

#### Application Security[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-authentication/#application-security) <a href="#application-security" id="application-security"></a>

Application security can be provided in the API and resource levels. The following are the API authentication types granted by Choreo Connect.

* [OAuth 2.0 Authentication](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/oauth2-access-tokens/)
* [Publisher Internal Key Authentication](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/internal-key-authentication/)
* [API Key Authentication](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/api-key-authentication/)
* [Enforcer Test Key Authentication](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/generate-a-test-jwt/)

Once authenticated, end-user attributes can be passed to the backend by enabling [Backend JWT Generation](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/passing-enduser-attributes-to-the-backend-via-choreo-connect/).
