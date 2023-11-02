# Create Prototype APIs

Prototyped APIs (Pre-Released APIs) give publishers the ability to make an API available in the Developer Portal while indicating that it still is a work in progress. Subscribers can try out these APIs and provide feedback for improvements. Additionally, Pre-Released APIs provide mocking capabilities that enables receiving direct responses from the gateway even without a backend.

**PRE-RELEASED** (previously **PROTOTYPED**) is a lifecycle state of the API that disallows adding monetizations to the API. After a period of time, the publishers can make changes requested by the users and publish the API by changing the lifecycle state of the API to _PUBLISHED_, to add monetizations as required.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

In the Developer Portal, these APIs will be the labelled as **PRE-RELEASED** and therefore can be clearly identified.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/prototype-api/prototype-api-devportal.png" alt=""><figcaption></figcaption></figure>

MWARE ESB allows prototyping an API at two different stages.

*   At an initial stage, before implementing the actual backend, the backend responses can be mocked by changing the **Endpoint Type** of the Prototype API to **Mock Implementation**. You can use the these APIs to get feedback before you start implementing the actual service.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/prototype-api/create-api-prototype-mock-impl-swagger-petstore.png" alt=""><figcaption></figcaption></figure>
* Once a backend is implemented, you can update the endpoint of the API with the actual backend URL (which could be of type HTTP/REST), and continue testing or use the API as an early promotion by keeping API in the state **PRE-RELEASED**.
