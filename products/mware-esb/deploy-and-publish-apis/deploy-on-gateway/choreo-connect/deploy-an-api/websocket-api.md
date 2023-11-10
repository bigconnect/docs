# WebSocket API

### Via the API Manager Publisher Portal[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#via-wso2-api-manager-publisher-portal) <a href="#via-wso2-api-manager-publisher-portal" id="via-wso2-api-manager-publisher-portal"></a>

Follow the instructions below to deploy a WebSocket type Streaming API to Choreo Connect via the Publisher Portal in WSO2 API Manager. The Publisher Portal provides the capability to conveniently add and update the following.

* The AsyncAPI definition
* Rate limiting policies
* Security scopes
* URL mappings

Before you begin

This guide assumes that you already have a Choreo Connect instance configured to run with API Manager. If not, checkout the [Quick Start Guide](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/getting-started/quick-start-guide-docker-with-apim/) on how to install and run Choreo Connect. To learn more about Choreo Connect, have a look at the [Overview of Choreo Connect](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/getting-started/choreo-connect-overview).

#### Step 1 - Create a WebSocket API in API Manager[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-1-create-a-websocket-api-in-api-manager) <a href="#step-1-create-a-websocket-api-in-api-manager" id="step-1-create-a-websocket-api-in-api-manager"></a>

1. Create a WebSocket API by following the steps in [Create a WebSocket API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-streaming-api/create-a-websocket-streaming-api/).
2.  By default, a WebSocket API is protected by OAuth2 in Choreo Connect. Make sure to switch on security for the API via Publisher and **Save**.

    [![Security switched on](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/create-api/streaming-api/streaming-api-security-on.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/create-api/streaming-api/streaming-api-security-on.png)

#### Step 2 - Deploy and publish the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-2-deploy-and-publish-the-api) <a href="#step-2-deploy-and-publish-the-api" id="step-2-deploy-and-publish-the-api"></a>

1. Deploy the API in Choreo Connect by navigating to the **Deployments** page from the left menu. For more information, see [Deploy API](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/deploy-api/deploy-an-api/).
2. Publish the API from the **Lifecycle** page.

#### Step 3 - Generate an Access Token to invoke the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-3-generate-an-access-token-to-invoke-the-api) <a href="#step-3-generate-an-access-token-to-invoke-the-api" id="step-3-generate-an-access-token-to-invoke-the-api"></a>

Tip

To generate a temporary test key to invoke the API, follow the steps [here](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/security/generate-a-test-jwt/)

1. Click **View in Dev Portal** at the top right corner to open Developer Portal in another browser tab.
2. Open **Applications** from the top menu and select **DefaultApplication** from the list.
3. Open the **Subscriptions** tab and subscribe your API to the **DefaultApplication**.
4. Open **APIs** from the top menu and select your API.
5. Open the **Subscriptions** tab from the left menu bar, click on **PROD KEYS**, and generate keys.
6. Open the **Try Out** tab and click **GET TEST KEY**. This will include the access token in the cURL command you generate in the section below.

#### Step 4 - Invoke the API using a WebSocket client[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-4-invoke-the-api-using-a-websocket-client) <a href="#step-4-invoke-the-api-using-a-websocket-client" id="step-4-invoke-the-api-using-a-websocket-client"></a>

The WebSocket API exposed via Choreo Connect can be invoked using a WebSocket client.

1. Install wscat client.
2. From Developer Portal resource tab open the topic `/notifications` and generate the cURL command.
3. Open a command-line interface and run the cURL command.
4. Once connected, type anything you want. The websocket server you are connected to must echo back what you type.

Note

Choreo Connect handles websocket connections via the same ports used for HTTP (9090) and HTTPS (9095).

### Via apictl for Standalone Mode[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#via-apictl-for-standalone-mode) <a href="#via-apictl-for-standalone-mode" id="via-apictl-for-standalone-mode"></a>

The CLI tool **apictl** does not support initializing projects for Streaming APIs yet. However, you can download a WebSocket API from a WSO2 API Manager instance and deploy the downloaded project in Choreo Connect. You do not need any special configurations in API-M or Choreo Connect for this. If you do not have a running API Manager instance already, simply download the pack from the [official site](https://wso2.com/api-manager/) and start one locally.

The following are the steps to download and deploy the API using apictl.

#### Step 1 - Create an API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-1-create-an-api) <a href="#step-1-create-an-api" id="step-1-create-an-api"></a>

Create a WebSocket API following the exact steps in [Create a WebSocket API in API Manager](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-1-create-a-websocket-api-in-api-manager).

#### Step 2 - Download the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-2-download-the-api) <a href="#step-2-download-the-api" id="step-2-download-the-api"></a>

After creating the API and making the necessary changes, click **Download API** in the top right corner. The downloaded .zip file can be deployed via apictl using the next few commands.

#### Step 3 - Add a Choreo Connect Environment to apictl[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-3-add-a-choreo-connect-environment-to-apictl) <a href="#step-3-add-a-choreo-connect-environment-to-apictl" id="step-3-add-a-choreo-connect-environment-to-apictl"></a>

To use apictl for Choreo Connect, a Choreo Connect environment needs to be added to apictl. This environment will hold the adapter URL for further commands.

```bash
apictl mg add env dev --adapter https://localhost:9843
```

Info

Note `mg` in the above command. The apictl commands that starts as `apictl mg` are Choreo Connect specific. If a command does not have `mg` after `apictl` then the command could probably be common to both Choreo Connect and API Manager, but it could also be API Manager specific.

Tip

The apictl commands here onwards are executed with the -k flag to avoid SSL verification with the Choreo Connect.

To communicate via HTTPS without skipping SSL verification (without -k flag), add the cert in the Choreo Connect truststore into the `<USER_HOME>/.wso2apictl/certs` folder.

#### Step 4 - Log in to the Choreo Connect Environment in apictl[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-4-log-in-to-the-choreo-connect-environment-in-apictl) <a href="#step-4-log-in-to-the-choreo-connect-environment-in-apictl" id="step-4-log-in-to-the-choreo-connect-environment-in-apictl"></a>

You can use the following command to log in to the above Choreo Connect cluster (in other words log in to the Choreo Connect adapter). When you log in, an access token will be retrieved from Choreo Connect and saved in apictl.

```bash
apictl mg login dev -k
```

or

```bash
apictl mg login dev -u admin -p admin -k
```

#### Step 5 - Deploy the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-5-deploy-the-api) <a href="#step-5-deploy-the-api" id="step-5-deploy-the-api"></a>

Now let's deploy the API to Choreo Connect by executing the following command.

```bash
apictl mg deploy api -f <path_to_the_API_project_just_created>/petstore -e dev -k
```

#### Step 6 - Generate an access token[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-6-generate-an-access-token) <a href="#step-6-generate-an-access-token" id="step-6-generate-an-access-token"></a>

After the APIs are exposed via Choreo Connect, you can invoke an API with a valid access token.

Let's use the following command to generate a JWT to access the API, and set it to the variable `TOKEN`.

```
TOKEN=$(curl -X POST "https://localhost:9095/testkey" -d "scope=read:pets" -H "Authorization: Basic YWRtaW46YWRtaW4=" -k -v)
```

#### Step 7 - Invoke the API[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-websocket-api-in-choreo-connect/#step-7-invoke-the-api) <a href="#step-7-invoke-the-api" id="step-7-invoke-the-api"></a>

1. Install wscat client.
2.  Invoke the API using the following command.

    ```
    wscat -c 'ws://localhost:9090/websocket/1.0.0/notification -H "Authorization:Bearer $TOKEN"
    ```
