---
description: Mapping the Parameters of your Backend URLs with the API Publisher URLs
---

# URL Mapping

This tutorial explains how to map your backend URLs to the pattern that you want in the API Publisher.

Let's do the following API resource to backend mapping in this tutorial as an example.

```
<gateway-url>/business/{id}/details  --->  <backend-url>/api/business/{id}/info
```

1.  Log in to the API Publisher, **Create** a new API with the following information.

    | Field    |   | Sample value                 |
    | -------- | - | ---------------------------- |
    | Name     |   | TestAPI                      |
    | Context  |   | /test                        |
    | Version  |   | 1.0.0                        |
    | Endpoint |   | https://httpbin.org/anything |

    Info

    * [httpbin.org](https://httpbin.org/) is a simple HTTP Request & Response Service.
    * [httpbin.org/anything](https://httpbin.org/anything) returns the request back.
    * We use [httpbin.org/anything](https://httpbin.org/anything) as the backend endpoint to inspect the requests we make.
2.  Go to **Resources** tab under the API Configurations and add a resource with the following parameters.

    | Field       | Value                    |
    | ----------- | ------------------------ |
    | HTTP Verb   | `/business/{id}/details` |
    | URL pattern | GET                      |
3. Delete the auto generated wildcard `/*` resources. Otherwise all the requests will go to them.
4.  Then **Save and deploy**. Final API will look as follows.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-gateway/message-mediation/test-api.png" alt=""><figcaption></figcaption></figure>
5.  Copy the the following to a text editor and save the file in XML format.\
    (e.g., `TestSequence.xml` ).

    ```java
        <property name="NEW_URL" value="/business/id/info" />
        <rewrite inProperty="NEW_URL" outProperty="NEW_URL">
            <rewriterule>
                <action type="replace" regex="id" xpath="get-property('uri.var.id')" fragment="path" />
            </rewriterule>
        </rewrite>
        <property name="REST_URL_POSTFIX" expression="get-property('NEW_URL')" scope="axis2" />
    ```

    Info

    * This sequence does the required mapping at synapse runtime.
    * The sequence is self explanetory and you can customize it for your own requirements.
6. Go to **Policies** tab and create a custom policy with any name using the above sequence. ( We use **Custom Policy** as the name here )
7.  Add the **Custom Policy** to the **Request Flow** of **/business/{id}/details** resource. Then **Save and deploy**.

    Now parameter mapping should work as intended and let's test this out.
8.  Click the **Try Out** tab of and try **/business/{id}/details**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-gateway/message-mediation/test-api-try-out.png" alt=""><figcaption></figcaption></figure>

In this tutorial, you mapped the URL pattern of the APIs in the Publisher with the endpoint URL pattern of a sample backend.
