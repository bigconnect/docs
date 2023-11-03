# Create a Proxy Service

Follow the instructions given below to create a new [Proxy Service](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/proxy-service-properties) artifact in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-proxy-service/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the Proxy Service artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-proxy-service/#creating-the-proxy-service-artifact) <a href="#creating-the-proxy-service-artifact" id="creating-the-proxy-service-artifact"></a>

Follow the steps given below.

1. Right-click the project in the navigator and go to **New → Proxy Service** to open the **New Proxy Service** dialog box.\
   ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/select-new-proxy.png)
2.  Select **Create New Proxy Service** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/create-new-proxy-option.png)
3.  Enter a unique name for the proxy service and select a proxy service template from the list shown below. These templates will automatically generate the mediation flow that is required for each use case.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/new-proxy-artifact-dialog.png)

    | Template Type      | Description                                                                                                                                                                                                                                                                                                                                                                                                           |
    | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Pass-Through proxy | This template creates a proxy service that forwards messages to the endpoint without performing any processing.                                                                                                                                                                                                                                                                                                       |
    | Transformer proxy  | This template creates a proxy service that transforms all the incoming requests using XSLT and then forwards them to a given endpoint. If required it can also transform responses from the back-end service according to an XSLT that you specify.                                                                                                                                                                   |
    | Log Forward proxy  | This template creates a proxy service that first logs all the incoming requests and passes them to a given endpoint. It can also log responses from the backend service before routing them to the client. You can specify the log level for requests and responses.                                                                                                                                                  |
    | WSDL-Based proxy   | This template generates a proxy service from the remotely hosted WSDL of an existing web service. The endpoint information is extracted from the WSDL you specify. Alternatively, you can generate a proxy service from a WSDL definition as explained [below](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-proxy-service/#creating-a-proxy-service-using-a-wsdl-definition). |
    | Secure proxy       | This template creates a proxy service that uses WS-Security to process incoming requests and forward them to an unsecured backend service. You simply need to provide the policy file that should be used.                                                                                                                                                                                                            |
    | Custom proxy       | This template creates an empty proxy service file, where you can manually create the mediation flow by adding all the sequences, endpoints, transports, and other QoS settings.                                                                                                                                                                                                                                       |
4. Do one of the following to save the proxy service:
   * To save the proxy service in an existing ESB Config project in your workspace, click **Browse** and select that project.
   * To save the proxy service in a new ESB Config project, click **Create new Project** and create the new project.
5. Click **Finish**.&#x20;

The proxy service is created in the `src/main/synapse-config/proxy-services` folder under the project you specified.

#### Creating a Proxy Service using a WSDL definition[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-proxy-service/#creating-a-proxy-service-using-a-wsdl-definition) <a href="#creating-a-proxy-service-using-a-wsdl-definition" id="creating-a-proxy-service-using-a-wsdl-definition"></a>

Follow the steps given below after opening the **New Proxy Service** dialog box.

1.  Select **Generate Proxy Service using WSDL file** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/proxy-service-properties.png)
2.  Provide a URL or a file location as the source of the WSDL and click **Finish**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/create-proxy-from-wsdl.png)

You will now see the mediation logic generated from the WSDL as shown below. Note that the [Switch mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/switch-mediator) is added to the mediation logic and that the different operations given in the WSDL are represented as switch cases.

Tip

If your WSDL does not have `SOAPActions` specified for the operations, only the **default** switch case will be generated.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/skeleton-proxy-service-wsdl.png)

#### Designing the integration[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-proxy-service/#designing-the-integration) <a href="#designing-the-integration" id="designing-the-integration"></a>

When you open the proxy service from the **Config** project in the project explorer, you will see the default **Design** view as shown below.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/proxy-service-design-view.png)

Drag and drop the required integration artifacts from the **Palette** to the canvas and design the integration flow.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/proxy-service-graphical-editor.png)

#### Updating the properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-proxy-service/#updating-the-properties) <a href="#updating-the-properties" id="updating-the-properties"></a>

To add service-level properties to the proxy service from the **Design** view:

1.  Double-click the **Proxy Service** icon to open the Properties tab for the service.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/proxy-service-properties.png)
2. Expand each section and add the required parameters.

To add service-level transport parameters:

1.  Go to the **Properties** tab and expand the **Parameters** section as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/service-level-params.png)
2.  Click the **plus** ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/common/plus-icon.jpg) icon and add the parameter name and value as a key-value pair:

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/service-level-params-dialog.png)

See the following links for the list of transport parameters you can use:

* [VFS Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/transport-parameters/vfs-transport-parameters)
* [JMS Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/transport-parameters/jms-transport-parameters)
* [FIX Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/transport-parameters/fix-transport-parameters)
* [MailTo Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/transport-parameters/mailto-transport-parameters)
* [MQTT Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/transport-parameters/mqtt-transport-parameters)
* [RabbitMQ Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/transport-parameters/rabbitmq-transport-parameters)
* See the complete list of [service-level properties and parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/proxy-service-properties) that you can configure.

#### Using the Source View[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-proxy-service/#using-the-source-view) <a href="#using-the-source-view" id="using-the-source-view"></a>

Click the **Source** tab to view the XML-based synapse configuration (source code) of the proxy service. You can update the service using this view.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_proxy\_service/proxy-service-source-view.png)
