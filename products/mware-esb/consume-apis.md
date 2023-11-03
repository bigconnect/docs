# Consume APIs

An API Consumer is typically an application developer who may be internal or external to your organization. Consuming APIs is the process by which the application developer accesses the various APIs that are exposed by you (the API provider) and then uses those APIs to develop one’s own software applications and products.

API consumers discover and access APIs from the **Developer Portal** of your WSO2 API Manager as shown below.

![developer portal of wso2 api manager](https://apim.docs.wso2.com/en/4.2.0/assets/img/get\_started/architecture/developer-portal-overview.png)

### API Consumer Tasks[¶](https://apim.docs.wso2.com/en/latest/consume/consume-api-overview/#api-consumer-tasks) <a href="#api-consumer-tasks" id="api-consumer-tasks"></a>

The process of consuming an API from WSO2 API Manager involves the following steps:

![main tasks for an api consumer](https://apim.docs.wso2.com/en/4.2.0/assets/img/consume/api-consumer-workflow.png)

#### Discover APIs[¶](https://apim.docs.wso2.com/en/latest/consume/consume-api-overview/#discover-apis) <a href="#discover-apis" id="discover-apis"></a>

When APIs are created and published through the **Publisher**, they become available through the **Developer Portal**. Developers can use the search option to find APIs of interest.

* [Searching for APIs](https://apim.docs.wso2.com/en/4.2.0/consume/discover-apis/search)

#### Subscribe to APIs[¶](https://apim.docs.wso2.com/en/latest/consume/consume-api-overview/#subscribe-to-apis) <a href="#subscribe-to-apis" id="subscribe-to-apis"></a>

Before using an API, the developer must first subscribe to the APIs and obtain the required authentication keys through an **application**.

**Applications**

An application is a logical representation of a physical application such as a mobile app, webapp, device, etc. An API subscription is created, authenticated, and managed through an application. Find out more about [applications](https://apim.docs.wso2.com/en/4.2.0/consume/manage-application/create-application).

**Authentication**

The subscription process is authenticated using OAuth2 by default. The authentication keys are generated for each application per gateway environment (Production or Sandbox). When the subscribing developer invokes the API through an application, the access token for the relevant gateway environment should be used.

**Business Plans**

Developers need to select a business plan for each API subscription. The business plan determines the number of requests that are allowed to be sent to the API per minute. Therefore, this is also the [throttling policy](https://apim.docs.wso2.com/en/4.2.0/design/rate-limiting/introducing-throttling-use-cases) that applies to a subscription.

#### Test APIs[¶](https://apim.docs.wso2.com/en/latest/consume/consume-api-overview/#test-apis) <a href="#test-apis" id="test-apis"></a>

Before using an API for development, the API consumer may want to test it’s capabilities. The following options are available in the Developer Portal for testing:

* [Test APIs using the Integrated API Console](https://apim.docs.wso2.com/en/4.2.0/consume/invoke-apis/invoke-apis-using-tools/invoke-an-api-using-the-integrated-api-console)
  * [Include Additional Headers in the API Console](https://apim.docs.wso2.com/en/4.2.0/consume/invoke-apis/invoke-apis-using-tools/include-additional-headers-in-the-api-console)
* [Test GraphQL APIs Using the Integrated GraphQL Console](https://apim.docs.wso2.com/en/4.2.0/consume/invoke-apis/invoke-apis-using-tools/invoke-an-graphql-api-using-the-integrated-graphql-console)
* [Test an API Using a SOAP Client](https://apim.docs.wso2.com/en/4.2.0/consume/invoke-apis/invoke-apis-using-tools/invoke-an-api-using-a-soap-client)
* [Test a REST API Using Postman](https://apim.docs.wso2.com/en/4.2.0/consume/invoke-apis/invoke-apis-using-tools/try-out-using-postman)

### Rate and Support APIs[¶](https://apim.docs.wso2.com/en/latest/consume/consume-api-overview/#rate-and-support-apis) <a href="#rate-and-support-apis" id="rate-and-support-apis"></a>

As members of the API consumer community, developers can rate and support the APIs through the Developer Portal. This is also a forum where API consumers can give feedback to the API providers.
