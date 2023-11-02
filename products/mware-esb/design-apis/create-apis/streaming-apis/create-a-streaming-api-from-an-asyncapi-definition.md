# Create a Streaming API from an AsyncAPI Definition

[AsyncAPI specification](https://www.asyncapi.com/) is a format that describes streaming APIs. An API Creator can import an existing AsyncAPI definition to WSO2 API Manager to create a streaming API using any one of the following protocols.

* [WebSocket](https://apim.docs.wso2.com/en/latest/use-cases/streaming-usecase/create-streaming-api/create-a-websocket-streaming-api/#overview)
* [WebSub (WebHooks)](https://apim.docs.wso2.com/en/latest/use-cases/streaming-usecase/create-streaming-api/create-a-websub-streaming-api/#overview)
* [Server Sent Events (SSE)](https://apim.docs.wso2.com/en/latest/use-cases/streaming-usecase/create-streaming-api/create-a-sse-streaming-api/#overview)

The API Creator can import the existing AsyncAPI definition by either uploading a file or by providing an Async API URL.

Follow the instructions below to create a Streaming API using an AsyncAPI definition for an existing API:

## Step 1 - Design a Streaming API <a href="#step-1-design-a-streaming-api" id="step-1-design-a-streaming-api"></a>

1.  Sign in to the Publisher.\


    `https://<hostname>:9443/publisher`\


    For testing purposes, you can use `https://localhost:9443/publisher` and `admin` as the username and password.
2.  Click **CREATE API** and then click **Import AsyncAPI Definition**.



    The following two options to import the AsynAPI definition appears.

    * **AsyncAPI URL** - If you select this option, you need to provide a URL.
    * **AsyncAPI File** - If you select this option, click **Browse File to Upload** and upload a file, which contains an AsyncAPI definition.

    <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/design-new-streaming-api.png" alt=""><figcaption></figcaption></figure>
3.  Select the way in which you are going to import the AsynAPI definition, and click **Next**.\


    For this example, let's select **AsyncAPI URL**, provide the following URL, and click **Next**.\


    ```sh
    https://raw.githubusercontent.com/asyncapi/spec/v2.0.0/examples/2.0.0/streetlights.yml
    ```



    <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/websocket-streaming-api-from-asyncapi-url-import.png" alt=""><figcaption></figcaption></figure>
4.  Edit the Streaming API information and click **Create**.



    For this example, let's design a WebSocket API using the following information.

    <table data-header-hidden><thead><tr><th width="220"></th><th></th></tr></thead><tbody><tr><td><strong>Field</strong></td><td><strong>Sample value</strong></td></tr><tr><td>Name</td><td>StreetlightsAPI</td></tr><tr><td>Context</td><td>/streetlights</td></tr><tr><td>Version</td><td>1.0.0</td></tr><tr><td>Protocol</td><td>WebSocket (or any other type of Streaming API)</td></tr><tr><td>Endpoint</td><td>ws://localhost:8080</td></tr></tbody></table>



    Now, the **StreetlightsAPI API** overview page will appear.



    <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/websocket-streaming-api-from-asyncapi-configure-values.png" alt=""><figcaption></figcaption></figure>

    <figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/websocket-streaming-api-from-asyncapi-overview.png" alt=""><figcaption></figcaption></figure>

## Step 2 - Configure Topics <a href="#step-2-configure-topics" id="step-2-configure-topics"></a>

Click **Topics** to navigate to the topics page.

<figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/websocket-streaming-api-from-asyncapi-topics.png" alt=""><figcaption></figcaption></figure>

You will notice that the topics have been created automatically from the AsyncAPI definition specified in the provided URL.

## Step 3 - View the AsyncAPI Definition <a href="#step-3-view-the-asyncapi-definition" id="step-3-view-the-asyncapi-definition"></a>

Click **AsyncAPI Definition** under **API Configurations**.

The AsyncAPI definition of the streaming API, which you just created, appears.

<figure><img src="https://apim.docs.wso2.com/en/latest/assets/img/design/create-api/streaming-api/websocket-view-asyncapi.png" alt=""><figcaption></figcaption></figure>

Now, you have successfully created a Streaming API from an Async API Definition.

Next, publish the API, for more information, see [Publish an API](https://apim.docs.wso2.com/en/latest/deploy-and-publish/publish-on-dev-portal/publish-an-api).
