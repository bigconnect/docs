# Disable Security

WSO2 API Manager enables OAuth 2.0 security for APIs by default. Therefore, all APIs must be invoked with an access token. You can get an access token in the following ways.

* [Get an internal test key from Publisher](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/test-a-rest-api/)
* [Get an access token for a subscription from Developer Portal](https://apim.docs.wso2.com/en/4.2.0/consume/consume-api-overview/)

For testing purposes, you can eliminate the need of an access token by disabling security for the entire API or a specific resource.

### Disabling security for a single API resource <a href="#disabling-security-for-a-single-api-resource" id="disabling-security-for-a-single-api-resource"></a>

1.  Click **Resources** listed under **API Configurations** in the left menu to navigate to the Resources page in the Publisher.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/prototype-api/create-prototype-api-resource-page.png" alt=""><figcaption></figcaption></figure>
2.  Expand any method and switch the security slidebar off to disable security for that specific resource.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/prototype-api/create-prototype-api-resource-disable-sec.png" alt=""><figcaption></figcaption></figure>

### Disabling security for all API resources <a href="#disabling-security-for-all-api-resources" id="disabling-security-for-all-api-resources"></a>

1. Click **Resources** to navigate to the Resources page in the Publisher.
2.  Click on the lock icon to disable security for all the resources that correspond to a specific API.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/prototype-api/create-prototype-api-resource-disable-all-sec.png" alt=""><figcaption></figcaption></figure>
