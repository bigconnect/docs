---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Expose a SOAP Service as a REST API

MWARE ESB supports the management of an existing SOAP and WSDL based services exposing as REST APIs. The organizations that have SOAP/WSDL based services can easily bridge their existing services to REST without the cost of a major migration. WSO2 API Manager supports two types of services. One service performs a pass-through of the SOAP message to the backend, and the other service generates [a RESTful API from the backend SOAP service](https://apim.docs.wso2.com/en/4.2.0/design/create-api/generate-rest-api-from-soap-backend/).

Follow the instructions below to create a SOAP service as a RESTful API using **Pass Through**

1.  Sign in to the API Publisher and click **CREATE API**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/create-soap-api.jpg" alt=""><figcaption></figcaption></figure>
2. Select the **Pass Through** option and thereafter, select one of the following options:
   * WSDL URL - If you select this option, you need to provide an endpoint URL.
   * WSDL Archive/File - If you select this option, click Browse and upload either an individual WSDL file or a WSDL archive, which is a WSDL project that has multiple WSDLs.

{% hint style="info" %}
When uploading a WSDL archive, all the dependent WSDLS/XSDS that are referred to in the parent WSDL file should reside inside the WSDL archive itself. If not, the validation will fail at the point of API creation.
{% endhint %}

This example uses the WSDL [Phone Verify](http://ws.cdyne.com/phoneverify/phoneverify.asmx?wsdl) from CDYNE as the endpoint here, but you can use any SOAP backend of your choice.&#x20;

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/generate-rest-api-from-soap-backend.jpg" alt=""><figcaption></figcaption></figure>

3. Click **NEXT** to proceed to the next phase, provide the information in the table below, and click **CREATE**.

<table><thead><tr><th width="189">Field</th><th>Sample Value</th></tr></thead><tbody><tr><td>Name</td><td>PhoneVerification</td></tr><tr><td>Context</td><td>/phoneverify</td></tr><tr><td>Version</td><td>1.0</td></tr><tr><td>Endpoint</td><td>http://ws.cdyne.com/phoneverify/phoneverify.asmx</td></tr></tbody></table>

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/create-soap-api-form.jpg" alt=""><figcaption></figcaption></figure>

The created API appears in the Publisher as follows.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/created-soap-api.jpg" alt=""><figcaption></figcaption></figure>

4. Click **API Definition** to view the API definition of the created schema.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-definition-of-soap-api-created-by-passthrough-mode.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you wish to add scopes to the resources that were created, click **Resources**. Thereafter, create new scopes and specify them under operation scope. If you specify a scope, you need to use the same scope when generating access tokens for the subscribed application to invoke the API. For more information on working with the scopes, see [OAuth scopes](https://apim.docs.wso2.com/en/4.2.0/design/api-security/oauth2/oauth2-scopes/fine-grained-access-control-with-oauth-scopes/).
{% endhint %}

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-scope-for-passthrough-soap-api.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that when creating this API, **API Level** was selected as the default option for the **Rate limiting level**. For more information on setting advanced throttling policies, see [Enforce Throttling and Resource Access Policies](https://apim.docs.wso2.com/en/4.2.0/design/rate-limiting/setting-throttling-limits/).
{% endhint %}

Now, the SOAP service is created and configured successfully as a RESTful API.

For more information on API publishing, see [Publish API](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/publish-on-dev-portal/publish-an-api/).

Once you create and publish the API, you can also [deploy the API you created](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/deploy-api/deploy-an-api/).

To learn more, see the tutorial on [Creating and Publishing a SOAP service as a RESTful API](https://apim.docs.wso2.com/en/4.2.0/tutorials/expose-a-soap-service-as-a-rest-api/).
