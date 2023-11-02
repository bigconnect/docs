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

# Test a REST API

Testing APIs refers to the process of trying out the APIs in the Publisher itself to make sure that the APIs meet the required functionalities and behavior.

WSO2 API Manager's Publisher offers an integrated API Console that allows API creators and API publishers to visualize the API contract and interact with API's resources.

Follow the instructions below to use the Publisher test console to test an API:

{% hint style="info" %}
Create the `PizzaShack` REST API or an API of your choice before following the instructions below. For more information, see [Create a REST API.](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api)
{% endhint %}

1.  Sign in to the Publisher.

    `https://<hostname>:9443/publisher`

    For testing purposes, you can use `https://localhost:9443/publisher` and `admin` as the username and password.

    The list of APIs that are available in the Publisher appears.
2.  Click on the API that you wish to test.

    Let's use the sample `PizzaShack` REST API.
3. Go to **Test** and click **Try Out**.

{% hint style="info" %}
The API can be tested in any lifecycle state, and can be used in any state except for the "retired" state. Even in the "block" state you can still try it out, but you will get a 503 (service unavailable) response.
{% endhint %}

<div align="left">

<figure><img src="../../../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

</div>

4. You can generate the internal key by clicking on the Generate Key button to invoke and test the PizzaShack API.

{% hint style="info" %}
The UI generates an Internal token to authenticate the API call. If the API Publishers have a token or key, then they can paste it in the UI and invoke the API using that key.
{% endhint %}

<figure><img src="../../../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

5. Expand the **GET** method, click **Try it out**, and click **Execute**.

Note the successful response for the API invocation.

<div data-full-width="true">

<figure><img src="../../../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

</div>
