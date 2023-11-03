# Create an Inbound Endpoint

Follow the instructions given below to create a new [Inbound Endpoint](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/about-inbound-endpoints) artifact in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-an-inbound-endpoint/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the Inbound Endpoint artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-an-inbound-endpoint/#creating-the-inbound-endpoint-artifact) <a href="#creating-the-inbound-endpoint-artifact" id="creating-the-inbound-endpoint-artifact"></a>

1.  If you have already created an [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project), right-click the project and go to **New → Inbound Endpoint** to open the **New Inbound Endpoint Artifact**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/select-inbound-endpoint.png)
2.  Select **Create a New Inbound Endpoint** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/new-inbound-endpoint-wizard-1.png)
3.  Enter a unique name for the inbound endpoint, and select an **Inbound Endpoint Creation Type** from the list.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/new-inbound-endpoint-wizard-2-1.png)
4.  Specify values for the required parameter for the selected inbound endpoint type.

    Note

    For certain protocols (HL7, KAFKA, Custom, MQTT, RabbitMq, WSO2\_MB, WS, and  WSS) the **main sequence** and **error sequence** are mandatory fields. ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/new-inbound-endpoint-wizard-2-2.png)

    You can select sequences that already exist in the workspace and add them to the **Sequence** and **Error sequence** fields. If you don't have any sequences in the workspace, click **Generate Sequence and Error Sequence** to generate new sequences for the inbound endpoint.
5. Do one of the following:
   * To save the endpoint in an existing ESB Config project in your workspace, click **Browse** and select that project.
   * To save the endpoint in a new ESB Config project, click **Create new Project** and create the new project.
6. Click **Finish**.&#x20;

The inbound endpoint is created in the `src/main/synapse-config/inbound-endpoint` folder under the ESB Config project you specified.

#### Designing the integration[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-an-inbound-endpoint/#designing-the-integration) <a href="#designing-the-integration" id="designing-the-integration"></a>

When you open the inbound endpoint from the **Config** project in the project explorer, you will see the default **Design** view.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/inbound-endpoint-design-view.png)

The integration flow for an inbound endpoint is defined within [named sequences](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties/#named-sequences). You can drag and drop **sequences** from the **Palette** to the canvas as shown below.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/inbound-graphical-editor.png)

Double-click the **Sequence** artifact to open the canvas for the sequence. You can now drag and drop the mediation artifacts from the palette and design the integration flow.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/sequence-graphical-editor.png)

#### Updating the properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-an-inbound-endpoint/#updating-the-properties) <a href="#updating-the-properties" id="updating-the-properties"></a>

To update properties from the **Design** view:

1.  Double-click the **Inbound Endpoint** icon to open the Properties tab.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/inbound-endpoint-properties.png)
2. See the following links for the list of parameters for each inbound endpoint type:
   * [HTTP Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/listening-inbound-endpoints/http-inbound-endpoint-properties)
   * [CXF WS RM Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/listening-inbound-endpoints/cxf-ws-rm-inbound-endpoint-properties)
   * [HL7 Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/listening-inbound-endpoints/hl7-inbound-endpoint-properties)
   * [WebSocket Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/listening-inbound-endpoints/websocket-inbound-endpoint-properties)
   * [File Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/polling-inbound-endpoints/file-inbound-endpoint-properties)
   * [JMS Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/polling-inbound-endpoints/jms-inbound-endpoint-properties)
   * [Kafka Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/polling-inbound-endpoints/kafka-inbound-endpoint-properties)
   * [RabbitMQ Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/event-based-inbound-endpoints/rabbitmq-inbound-endpoint-properties)
   * [MQTT Inbound Parameters](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/inbound-endpoints/event-based-inbound-endpoints/mqtt-inbound-endpoint-properties)

Note

**Redeployment of listening inbound endpoints fail?**

A **listening inbound endpoint** opens the port for itself during deployment. Therefore, if you are **redeploying** a listening inbound endpoint artifact, the redeployment will not be successful until the port that was previously opened for the inbound endpoint is closed.

By default, the system will wait for 10 seconds for the previously opened port to close down. If you want to increase this waiting time beyond 10 seconds, be sure to add the following system property in the `deployment.toml` file, which is stored in the `MI_HOME/conf/` directory and restart the server before redeploying the artifacts.

```toml
[system.parameter]
'synapse.transport.portCloseVerifyTimeout' = 20
```

Note that `synapse.transport.portCloseVerifyTimeout` should be wrapped by single quotes since it contain dots. The TOML format detects the dot as an object separator. Also note that this setting may be required in Windows environments as the process of closing a port can sometimes take longer than 10 seconds.

#### Using the Source View[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-an-inbound-endpoint/#using-the-source-view) <a href="#using-the-source-view" id="using-the-source-view"></a>

Click the **Source** tab to view the XML-based synapse configuration (source code) of the inbound endpoint. You can update the service using this view.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_inbound\_endpoint/inbound-source-view.png)
