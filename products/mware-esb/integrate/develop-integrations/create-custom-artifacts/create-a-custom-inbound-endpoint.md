# Create a Custom Inbound Endpoint

WSO2 Micro Integrator supports several inbound endpoints. However, there can be scenarios that require functionality not provided by the existing inbound endpoints. For example, you might need an inbound endpoint to connect to a certain back-end server or vendor specific protocol.

To support such scenarios, you can write your own custom inbound endpoint by extending the behavior for **Listening**, **Polling**, and **Event-Based** inbound endpoints.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-custom-inbound-endpoint/#instructions) <a href="#instructions" id="instructions"></a>

#### Step 1: Developing the custom Inbound Endpoint[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-custom-inbound-endpoint/#step-1-developing-the-custom-inbound-endpoint) <a href="#step-1-developing-the-custom-inbound-endpoint" id="step-1-developing-the-custom-inbound-endpoint"></a>

* To create a **custom listening inbound endpoint**, download the maven artifact used in the [sample custom listening inbound endpoint configuration](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom\_inbound\_listening) configuration.
* To create a **custom polling inbound endpoint**, download the maven artifact used in the [sample custom polling inbound endpoint configuration](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom\_inbound).
* To create a **custom event-based inbound endpoint**, download the maven artifact used in the [sample custom event-based inbound endpoint configuration](https://github.com/wso2-docs/ESB/tree/master/ESB-Artifacts/inbound/custom\_inbound\_waiting).

#### Step 2: Deploying the Custom Inbound Endpoint[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-custom-inbound-endpoint/#step-2-deploying-the-custom-inbound-endpoint) <a href="#step-2-deploying-the-custom-inbound-endpoint" id="step-2-deploying-the-custom-inbound-endpoint"></a>

You need to copy the built jar file to the `MI_HOME/lib` directory and restart the Micro Integrator to load the class. To copy the jar file to the Embedded Micro Integrator, open the Embedded Micro Integrator Server Configuration Wizard by clicking on the (![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/common/server-config-64x64.png)) icon on the upper menu and add the jar to select libraries section.

#### Step 3: Adding the custom Inbound Endpoint[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-custom-inbound-endpoint/#step-3-adding-the-custom-inbound-endpoint) <a href="#step-3-adding-the-custom-inbound-endpoint" id="step-3-adding-the-custom-inbound-endpoint"></a>

1. If you have already created an [Integration Project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project), right-click the [ESB Config module](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#types-of-projects) and go to **New → Inbound Endpoint** to open the **New Inbound Endpoint Artifact**.
2. Select **Create a New Inbound Endpoint** and click **Next**.
3. Type a unique name for the inbound endpoint, and then select **Custom** as the **Inbound Endpoint Creation Type**.
4. Specify the location where the artifact should be saved: Select an existing ESB Config project in your workspace, or click **Create new Project** and create a new project.
5. Click **Finish**. The inbound endpoint is created in the `src/main/synapse-config/inbound-endpoint` folder under the ESB Config project you specified.
6.  Open the new artifact from the project explorer, go to the **Source View**, and update the following properties:

    | Property Name    | Description                                                                                                                                                                                                |
    | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | class            | Name of the custom class you implemented in [step 1](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-custom-inbound-endpoint/#step-1-developing-a-custom-inbound-endpoint). |
    | sequence         | Name of the sequence message that should be injected. Specify a valid sequence name.                                                                                                                       |
    | onError          | Name of the fault sequence that should be invoked in case of failure. Specify a valid sequence name.                                                                                                       |
    | inbound.behavior | Specify whether your custom endpoint is `listening`, `polling`, or `event-based`.                                                                                                                          |
