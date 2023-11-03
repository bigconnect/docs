# Authentication

API authentication is a way of protecting API access from unidentified or anonymous access. It ensures that the API is secured and accessible only by the consumers who proves their identity and whose identities are found within the API Management Platform.

WSO2 API Manager offers the following authentication mechanisms to secure your API from unauthenticated access.

* [Securing APIs using OAuth2 Access Tokens](https://apim.docs.wso2.com/en/4.2.0/design/api-security/api-authentication/secure-apis-using-oauth2-tokens)
  * [JWT (Self Contained) Access Tokens](https://apim.docs.wso2.com/en/4.2.0/design/api-security/oauth2/access-token-types/jwt-tokens)
* [Secure APIs Using API Keys](https://apim.docs.wso2.com/en/4.2.0/design/api-security/api-authentication/secure-apis-using-api-keys)
* [Secure APIs Using Mutual SSL](https://apim.docs.wso2.com/en/4.2.0/design/api-security/api-authentication/secure-apis-using-mutual-ssl)
* [Secure APIs Using Basic Authentication](https://apim.docs.wso2.com/en/4.2.0/design/api-security/api-authentication/secure-apis-using-basic-authentication)

WSO2 API Manager allows you to enable multiple Key Managers for authentication.

* The tenant admin can configure preferred Key Managers via the Admin Portal console. For more information, see [Configuring Key Managers](https://apim.docs.wso2.com/en/4.2.0/administer/key-managers/overview).
*   The enabled Key Managers can be disabled for a given API via the Publisher by navigating to **Develop -> API Configurations -> Runtime -> Application Level Security -> Key Manager Configuration**



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/multiple-km-publisher.png" alt=""><figcaption></figcaption></figure>
*   Application users are able to generate keys for an application using a preferred Key Manager as shown below.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/multiple-km-devportal.png" alt=""><figcaption></figcaption></figure>
