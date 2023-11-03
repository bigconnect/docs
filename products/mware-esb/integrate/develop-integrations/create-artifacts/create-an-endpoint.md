# Create an Endpoint

Follow the instructions given below to create a new [Endpoint](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/endpoint-properties) artifact in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoints/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the Endpoint artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoints/#creating-the-endpoint-artifact) <a href="#creating-the-endpoint-artifact" id="creating-the-endpoint-artifact"></a>

1.  Right-click the [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) and go to **New → Endpoint** to open the **New Endpoint Artifact** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_endpoint/select-endpoint.png)
2.  Select **Create a New Endpoint** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_endpoint/new-endpoint-wizard-1.png)
3.  Enter a unique name for the endpoint, and then select the type of endpoint you are creating.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_endpoint/new-endpoint-wizard-2.png)
4. Specify values for the [required parameter](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/endpoint-properties) for the selected endpoint type.
5. Specify how you want to save the endpoint:
   * Select **Static Endpoint** to save the endpoint in the current workspace.
   * Select **Dynamic Endpoint** to save the endpoint as a registry resource.
6. Specify the location to save the endpoint:
   * To save in an existing ([ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) or [Registry Resource project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#registry-resource-project)) in your workspace, click **Browse** and select that project.
   * To save in a new project, click **Create new Project** and create the new project.
7. Click **Finish**.&#x20;

The endpoint is created in the `src/main/synapse-config/endpoints` folder under the ESB Config project or [registry resource project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#registry-resource-project) you specified.

#### Designing the integration[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoints/#designing-the-integration) <a href="#designing-the-integration" id="designing-the-integration"></a>

To add an endpoint artifact to the integration sequence, use the [Send Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/send-mediator) or the [Call Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/call-mediator).

1. Open to the **Design View** of your [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties).
2.  Drag the [Call Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/call-mediator) from the **Palette** and drop it to the relevant position in the [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties):

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_endpoint/endpoint-graphical-editor-1.png)

    Tip

    Similarly, you can use the [Send Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/send-mediator).
3.  Drag the new endpoint artifact from the **Defined Endpoints** section in the **Palette** and drop it to the empty box in the [Call Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/call-mediator):

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_endpoint/endpoint-graphical-editor-2.png)

The endpoint artifact is now linked to your integration sequence.

#### Updating the properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoints/#updating-the-properties) <a href="#updating-the-properties" id="updating-the-properties"></a>

Open the new endpoint artifact from the project explorer. You can use the **Form** view or the **Source** view to update endpoint properties.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_endpoint/endpoint-form-view.png)

See the descriptions of all [endpoint properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/endpoint-properties).
