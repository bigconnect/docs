# Create an API Using a Service

The service catalog in WSO2 API Manager contains services that correspond to either the Micro Integrator or the Streaming Integrator. The Micro Integrator services correspond to the REST API backend services, and the Streaming Integrator services correspond to the Streaming API (i.e., WebSocket, WebHook/WebSub or SSE) backend services that are managed by the respective integration layer (Micro Integrator or Streaming Integrator).

Therefore, you can use the respective services to either create REST APIs or Streaming APIs to expose integration services or streaming integration services as managed APIs.

## Step 1 - Create and publish your service to the service catalog <a href="#step-1-create-and-publish-your-service-to-the-service-catalog" id="step-1-create-and-publish-your-service-to-the-service-catalog"></a>

Create and publish your integration service or streaming integration service to the **Service Catalog** of the WSO2 API-M layer via the integration layer.

More information:

* For information on creating and publishing a REST API based on an integration service, see [Exposing an Integration Service as a Managed API](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/working-with-service-catalog).
* For information on creating and publishing a Streaming API based on a streaming integration service, see [Exposing a Stream as a Managed API](https://apim.docs.wso2.com/en/4.2.0/use-cases/streaming-usecase/exposing-stream-as-managed-api-in-service-catalog/).

## Step 2 - Discover the services <a href="#step-2-discover-the-services" id="step-2-discover-the-services"></a>

Follow the instructions below to discover the integration services:

1.  Sign in to the Publisher.\


    `https://<hostname>:9443/publisher`\


    For testing purposes, you can use `https://localhost:9443/publisher` and `admin` as the username and password.
2.  Click **Services**.



    The available integration services appear.



    <div align="left">

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/service-catalog/select-services.png" alt=""><figcaption></figcaption></figure>

    </div>

    <div align="left">

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/common/service-catalog/services-catalog-list.png" alt=""><figcaption></figcaption></figure>

    </div>

## Step 3 - Optionally, view the details of the service <a href="#step-3-optionally-view-the-details-of-the-service" id="step-3-optionally-view-the-details-of-the-service"></a>

1.  Click on the service that is in the service catalog to view the service details.

    You can view the service name, version, description, usages in APIs, and other important metadata.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/common/service-catalog/service-overview.png" alt=""><figcaption></figcaption></figure>
2.  Optionally, **download a service definition**.

    If required, click **Download** to download the service definition.
3.  Optionally, **view the service definition**.

    If you want to see the service definition before creating an API, click **View Definition**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/common/service-catalog/view-api-definition.png" alt=""><figcaption></figcaption></figure>

### Step 4 - Create an API from the service[¶](https://apim.docs.wso2.com/en/latest/design/create-api/create-an-api-using-a-service/#step-4-create-an-api-from-the-service) <a href="#step-4-create-an-api-from-the-service" id="step-4-create-an-api-from-the-service"></a>

1. Click on the service that is in the service catalog.
2.  Click **Create API**.

    This opens the **Create API** dialog box with the API details that are generated based on the service.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/common/service-catalog/create-api-from-service.png" alt=""><figcaption></figcaption></figure>
3.  Update the API name, context, and version if required, and click **Create API**.

    The overview page of the API that you just created appears.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/common/service-catalog/new-api-view.png" alt=""><figcaption></figcaption></figure>
4.  Update the portal configurations and API configurations as required.

    Now, you have successfully created an API using a service.

After you complete configuring the API, [Publish the API](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/publish-on-dev-portal/publish-an-api/).
