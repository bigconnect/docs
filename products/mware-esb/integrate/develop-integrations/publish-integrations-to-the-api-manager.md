# Publish Integrations to the API Manager

A REST API artifact you create from WSO2 Integration Studio is exposed to consumers when you run it on the Micro Integrator runtime. If you want to control and manage this API, and also expose it to an API marketplace where it becomes discoverable to a wider community of consumers, you need to publish this REST API to the API management layer (API-M runtime) of the product.

Follow the steps given below to publish REST APIs from the Micro Integrator to the API-M runtime.

### Prerequisites[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/working-with-service-catalog/#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

Develop a REST API artifact using WSO2 Integration Studio. This is your integration service with the mediation logic that will run on the Micro Integrator.

Tip

For instructions on creating a new integration service, use the following documentation:

* [Developing your First Integration Service](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/integration-development-kickstart).
* [Integration Tutorials](https://apim.docs.wso2.com/en/4.2.0/tutorials/tutorials-overview/#integration-tutorials).

### Step 1 - Update the service metadata[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/working-with-service-catalog/#step-1-update-the-service-metadata) <a href="#step-1-update-the-service-metadata" id="step-1-update-the-service-metadata"></a>

When you create a REST API artifact from WSO2 Integration Studio, a **resources** folder with metadata files is created as shown below. This metadata is used by the API management runtime to generate the API proxy for the service.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/service-catalog/metadata-folder-service-catalog.png)

Update the metadata for your service as explained below.

| Parameter   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| description | Explain the purpose of the API.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| serviceUrl  | <p>This is the URL of the API when it gets deployed in the Micro Integrator. You (as the integration developer) may not know this URL during development. Therefore, you can parameterize the URL to be resolved later using environment variables. By default, the <code>{MI_HOST}</code> and <code>{MI_PORT}</code> values are parameterized with placeholders.<br><br>You can configure the serviceUrl in the following ways:</p><ul><li>Add the complete URL without parameters. For example: <code>http://localhost:8290/healthcare</code>.<br></li><li>Parameterize using the host and port combination. For example: <code>http://{MI_HOST}:{MI_PORT}/healthcare</code>.</li><li>Parameterize using a preconfigured URL. For example: <code>http://{MI_URL}/healthcare</code>.</li></ul> |

Tip

See the [Service Catalog API documentation](https://apim.docs.wso2.com/en/4.2.0/reference/product-apis/service-catalog-apis/service-catalog-v1/service-catalog-v1/) for more information on the metadata in the YAML file.

### Step 2 - Configure the Micro Integrator server[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/working-with-service-catalog/#step-2-configure-the-micro-integrator-server) <a href="#step-2-configure-the-micro-integrator-server" id="step-2-configure-the-micro-integrator-server"></a>

The Micro Integrator contains a client for publishing integrations to the API-M runtime. To enable this client, update the following in the `deployment.toml` file of your Micro Integrator.

```toml
[[service_catalog]]
apim_host = "https://localhost:9443"
enable = true
username = "admin"
password = "admin"
```

See the descriptions of the [service catalog parameters](https://apim.docs.wso2.com/en/4.2.0/reference/config-catalog-mi/#service-catalog-client).

### Step 3 - Start the servers[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/working-with-service-catalog/#step-3-start-the-servers) <a href="#step-3-start-the-servers" id="step-3-start-the-servers"></a>

Once you have created the integration service and deployed it in the Micro Integrator, you only need to start the two servers (API-M server and the Micro Integrator server).

Note that the API-M server should be started before the Micro Integrator. The client in the Micro Integrator publishes the integration services to the API-M layer during server startup.

\
