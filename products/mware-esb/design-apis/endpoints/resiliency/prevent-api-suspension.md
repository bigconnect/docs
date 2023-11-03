# Prevent API Suspension

WSO2 API Manager suspends your API if the endpoint of your API cannot be reached. The default suspension time is 30 seconds. Any requests to your API will not be able to reach your endpoint for 30 seconds and will return an error message, as shown below.

```java
<am:fault xmlns:am="http://wso2.org/apimanager">
<am:code>303001</am:code>
<am:type>Status report</am:type>
<am:message>Runtime Error</am:message>
<am:description>Currently , Address endpoint : [ Name : somename-AT-sometenant--test_me_APIproductionEndpoint_0 ] [ State : SUSPENDED ]</am:description>
</am:fault>
```

Follow the instructions below to prevent or turn off API suspension:

1.  Sign in to the API Publisher Portal.

    `https://<hostname>:9443/publisher`

    Example: `https://localhost:9443/publisher`

    Use your username and password to sign in.
2. Click **Endpoints**.
3.  Click on the cogwheel, which is inline with the endpoint that you need to re-configure, and update the endpoint related configurations as required.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/prevent-endpoint-suspension.png" alt=""><figcaption></figcaption></figure>
4.  Configure the **Endpoint Suspension State**.

    Set the value for **Initial Duration** and **Max Duration** to zero to turn off suspension.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/initial-duration-and-max-duration.png" alt=""><figcaption></figcaption></figure>
5.  Click **Save** and re-publish the API.

    For more details on creating and publishing an API, see [Create an API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api) and [Publish an API](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/publish-on-dev-portal/publish-an-api).
