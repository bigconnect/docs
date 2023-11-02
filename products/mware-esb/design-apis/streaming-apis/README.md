# Streaming APIs

Event Driven Architecture (EDA) allows changes to data and state, and interesting business events to be pushed to applications immediately without requiring the applications to poll for the updates. Organizations try to embrace EDA to modernize their applications with rich, interactive, and immediate user experience (e.g., immediately seeing the changes in the stock prices). In addition to EDA, you also need to manage streaming APIs in scenarios when an organization needs to expose their event-streams in a managed way, to generate money based on streaming data, or to only allow authorized clients to access certain APIs. However, organizations face multiple challenges, as listed below, when attempting to implement an event driven architecture or introduce eventing.

* Integrating existing applications with the event backbone.
* Exposing event streams to web applications and external users.
* New applications need to be modified to deal with the transport level concerns.
* Integrating persistent data stores in a streaming manner.
* Processing event streams.

To overcome the latter mentioned challenges MWARE ESB provides the capability of working with Streaming APIs.

## How does it work? <a href="#how-does-it-work" id="how-does-it-work"></a>

A **Streaming API** is a logical collection of related topics through which clients can publish and receive events in a well-defined format. The topics in the Streaming APIs represent the different channels. The topics in Streaming APIs can be compared to the resources in REST APIs.

The actions a **topic** allows you to do are "Subscribe" and/or "Publish".

* **Subscribe** - A topic of this type allows traffic from the server (backend) to the client.
* **Publish** - A topic of this type allows traffic from the client to the server.

The [Streaming Integrator component](https://apim.docs.wso2.com/en/latest/streaming/streaming-overview/) in WSO2 API Manager (WSO2 API-M) supports Streaming APIs via the following main protocols, which are compatible with HTTP.

* [**WebSocket**](https://apim.docs.wso2.com/en/latest/use-cases/streaming-usecase/create-streaming-api/create-a-websocket-streaming-api)
* [**WebSub (WebHook)**](https://apim.docs.wso2.com/en/latest/use-cases/streaming-usecase/create-streaming-api/create-a-websub-streaming-api)
* [**Server Sent Events (SSE)**](https://apim.docs.wso2.com/en/latest/use-cases/streaming-usecase/create-streaming-api/create-a-sse-streaming-api)

The WebSocket and SSE Streaming APIs require a persistent connection between the client and the API Gateway. This enables the server to stream events continually, with idle times in the middle if required, through a single connection, thereby eliminating the overhead of creating new connections. Thereby, the WebSocket and SSE protocols allow publishers to maintain persistent communication channels with receivers. However, WebSub Streaming APIs do not require to have a persistent connection. Instead, the WebSub APIs make an HTTP call when required.

When you create a WebSocket API, WebSub API/ WebHook API, or SSE API, it links an existing streaming backend API implementation to the [API Publisher,](https://apim.docs.wso2.com/en/latest/get-started/architecture/#api-publisher) so that you can manage and monitor your [API's lifecycle](https://apim.docs.wso2.com/en/latest/design/lifecycle-management/api-lifecycle), documentation, security, community, and subscriptions.

You can define Streaming APIs using [AsyncAPI](https://www.asyncapi.com/) definitions. Therefore, alternatively, you can also [**create a Streaming API, which supports any one of the above protocols, using an AsyncAPI definition.**](https://apim.docs.wso2.com/en/latest/use-cases/streaming-usecase/create-streaming-api/create-a-streaming-api-from-an-asyncapi-definition)

## Authorization for Streaming API Topics <a href="#authorization-for-streaming-api-topics" id="authorization-for-streaming-api-topics"></a>

A scope acts as a limiting factor on what Streaming API topics can be accessed using an access token, and it defines the authorization aspect for Streaming API events.

*   **Authorization for a single topic**

    When a query has a single topic, the access token should include the scope, which is attached to that specific topic, to be able to invoke the API.
*   **Authorization for multiple topics**

    When a query has multiple topics, the access token should include all the scopes, which are attached to the topics that correspond to that specific API, to be able to invoke the API.

