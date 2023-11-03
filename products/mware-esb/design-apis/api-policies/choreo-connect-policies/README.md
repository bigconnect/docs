# Choreo Connect Policies

Choreo Connect supports API policies at the HTTP method level for request and response flows. Follow the steps given below to add policies to an API.

1. Open the API Manager Publisher Portal.
2. Select the API.
3. Click **Policies** under **API Configurations** in the left menu.
4. Select **Choreo Connect** as the Gateway type.
5. Then, drag and drop the policies to the relevant flow in the required order.
6. Scroll to the bottom of the page, click the arrow next to **Save**, and click **Save and deploy**.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/choreo-connect-policy.png" alt=""><figcaption></figcaption></figure>

Following are the policies supported by Choreo Connect.

### Request flow policies <a href="#request-flow-policies" id="request-flow-policies"></a>

* Add header
* Remove header
* Remove Query Parameter
* Rewrite Resource Path
* Change HTTP Method
* Validate Request with OPA
* Call Interceptor Service

### Response flow policies <a href="#response-flow-policies" id="response-flow-policies"></a>

* Add header
* Remove header
* Call Interceptor Service
