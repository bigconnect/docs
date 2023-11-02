# Create a Server Sent Events API

## Overview <a href="#overview" id="overview"></a>

A Server-Sent Events (SSE) API is a streaming API in WSO2 API Manager (WSO2 API-M) that is implemented based on the [SSE](https://html.spec.whatwg.org/multipage/server-sent-events.html#server-sent-events) specification. SSE is an HTTP-based protocol that allows one-way communication similar to WebHooks, from the server to the client. The SSE server transfers events over an already established connection without creating new connections. Therefore, the SSE protocol has a lower delivery latency when compared to typical HTTP. WSO2 API Manager allows API Developers to integrate an SSE backend with an API and to receive events from the backend.

You can create an SSE API from scratch in WSO2 API-M and export the SSE APIs that are created within WSO2 API-M as AsyncAPI definitions. Alternatively, you can also import [existing AsyncAPI definitions to create SSE APIs in WSO2 API-M](https://apim.docs.wso2.com/en/latest/use-cases/streaming-usecase/create-streaming-api/create-a-streaming-api-from-an-asyncapi-definition).

This section guides you through the process of creating an API from scratch, which will expose a SSE backend via WSO2 API Manager. After an SSE API is created, you will be able to add different topics and each topic can be mapped to different paths of the backend and can be used to manage them.

## How it works <a href="#how-it-works" id="how-it-works"></a>

SSE APIs use regular HTTP requests for a persistent connection. In addition, it gets multiplexing over HTTP/2 out-of-the-box. If the connection drops, the EventSource fires an error event and automatically tries to reconnect. The server can also control the timeout before the client tries to reconnect. Clients can send a unique ID with messages. When a client tries to reconnect after a dropped connection, it will send the last known ID. Then the server can see that the client missed "n" number of messages and send the backlog of missed messages on reconnection.

## Example usage <a href="#example-usage" id="example-usage"></a>

For example, stock market applications use SSE APIs to send messages in a uni-directional manner from the server to the client. Therefore, if users need the latest stock market prices, they will subscribe to the respective channel that publishers stock market stats. Thereafter, the server will keep sending the user the latest stock market prices as and when it gets updated in order to provide an immediate user experience.

## Basic flow <a href="#basic-flow" id="basic-flow"></a>

Follow the instructions below to create the API using the basic flow.

### Step 1 - Design a SSE API <a href="#step-1-design-a-sse-api" id="step-1-design-a-sse-api"></a>

1.  Sign in to the Publisher.

    `https://<hostname>:9443/publisher`

    For testing purposes, you can use `https://localhost:9443/publisher` and `admin` as the username and password.
2.  Click **CREATE API**, go to **Streaming API**, and Click **SSE API**.





    <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/design-new-streaming-api.png" alt=""><figcaption></figcaption></figure>
3.  Enter API details.

    <table><thead><tr><th width="170">Field</th><th>Sample value</th></tr></thead><tbody><tr><td>Name</td><td>ServerSentEvents</td></tr><tr><td>Context</td><td><p><code>/events</code></p><p>The API context is used by the Gateway to identify the API. Therefore, the API context must be unique. This context is the API's root context when invoking the API through the Gateway.</p><p>You can define the API's version as a parameter of its context by adding the <code>{version}</code> into the context. For example, <code>{version}/event</code>. The API Manager assigns the actual version of the API to the <code>{version}</code> parameter internally. For example, <code>https://localhost:8243/1.0.0/event</code>. Note that the version appears before the context, allowing you to group your APIs based on the versions.</p></td></tr><tr><td>Version</td><td>1.0.0</td></tr><tr><td>Protocol</td><td>SSE</td></tr><tr><td>Endpoint</td><td><p><code>http://localhost:8080</code></p><p>You need to have a Server Sent Events server running for this purpose locally</p></td></tr></tbody></table>



    <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/create-sse-api-form.png" alt=""><figcaption></figcaption></figure>
4.  Click **CREATE** to create the API.

    The overview page of the newly created API appears.



    <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/sse-api-overview-page.png" alt=""><figcaption></figcaption></figure>

### Step 2 - Configure the Topics <a href="#step-2-configure-the-topics" id="step-2-configure-the-topics"></a>

Topics of an SSE API are always **Subscribe only**, where the flow of events will be from the server (backend) to the client. By default, an SSE API will have a topic with the name `/*`.

1. Click **Topics** under **API Configurations** to navigate to the **Topics** page.
2. Modify the topics as follows and click **Save** to update them.
   1.  Optionally, click delete, as shown below, to delete an existing topic.



       <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/sse-api-delete-default-topic.png" alt=""><figcaption></figcaption></figure>
   2.  Select **sub** under **Types**, enter the **Topic Name**, and click **+** as shown below, to add a new topic.



       The newly added topic is displayed as follows.

       [![SSE API Newly Added Topic](https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/sse-api-newly-added-topic.png)](https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/sse-api-newly-added-topic.png)

       <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/sse-api-create-new-topic.png" alt=""><figcaption></figcaption></figure>

### Step 3 - View the AsyncAPI Definition <a href="#step-3-view-the-asyncapi-definition" id="step-3-view-the-asyncapi-definition"></a>

Click **AsyncAPI Definition** under **API Configurations**.

The AsyncAPI definition of the streaming API, which you just created, appears.

<figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/sse-api-asyncapi.png" alt=""><figcaption></figcaption></figure>

### Step 4 - Configure the Runtime Configurations <a href="#step-4-configure-the-runtime-configurations" id="step-4-configure-the-runtime-configurations"></a>

1.  Click **Runtime** under **API Configurations**.

    Transport Level Security defines the transport protocol on which the API is exposed.



    <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/sse-api-runtime-configurations-page.png" alt=""><figcaption></figcaption></figure>
2.  If you wish to limit the API availability to only one transport (e.g., HTTPS), uncheck the appropriate checkbox under **Transport Level Security**.

    Both HTTP and HTTPS transports are selected by default.

Now, you have successfully created and configured a Streaming API. Next, let's [Publish your API](https://apim.docs.wso2.com/en/latest/deploy-and-publish/publish-on-dev-portal/publish-an-api).
