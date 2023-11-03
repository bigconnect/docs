# Call Interceptor Service

You can use interceptors in Choreo Connect to carry out transformations and mediation on the requests and responses. Learn more about [Message Transformation](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/message-transformation/message-transformation-overview/) in Choreo Connect.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/call-interceptor.png" alt=""><figcaption></figcaption></figure>

Note

You can also define call interceptor configurations in the Open API specification. If both the Open API specification and the "Call Interceptor Service" API Policy is attached, the "Call Interceptor Service" API Policy overrides the call interceptor configurations defined in the Open API specification.

The policy attribute “Includes to Payload” in the Call Interceptor Service supports the following values in the request flow.

* request\_headers
* request\_body
* request\_trailers
* invocation\_context

For more information, see [Request flow interceptor](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/message-transformation/defining-interceptors-in-an-open-api-definition/#request-flow-interceptor).

The following values are available in the response flow.

* request\_headers
* request\_body
* request\_trailers
* response\_headers
* response\_body
* response\_trailers
* invocation\_context

\
