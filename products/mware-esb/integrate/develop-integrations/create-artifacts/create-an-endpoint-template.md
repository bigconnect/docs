# Create an Endpoint Template

Follow the instructions given below to create a new **Endpoint Template** in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoint-templates/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the Endpoint Template artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoint-templates/#creating-the-endpoint-template-artifact) <a href="#creating-the-endpoint-template-artifact" id="creating-the-endpoint-template-artifact"></a>

1.  Right-click the [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) and go to **New → Template** to open the **New Template Artifact** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/select-template.png)
2.  Select **Create a New Template** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/new-template-wizard-1.png)
3.  Enter a unique name for the template and select one of the following **Endpoint Template** types.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/new-template-wizard-2-1.png)

    * Address Endpoint Template
    * Default Endpoint Template
    * HTTP Endpoint Template
    * WSDL Endpoint Template

    Specify values for the [required parameter](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/template-properties/#endpoint-template-properties) for the selected endpoint type.
4. Do one of the following to save the artifact:
   * To save the template in an existing ESB Config project in your workspace, click **Browse** and select that project.
   * To save the template in a new ESB Config project, click **Create new Project** and create the new project.
5.  Click **Finish**.&#x20;

    The template is created in the `src/main/synapse-config/templates` folder under the ESB Config project you specified.
6. To use the endpoint template, [update the properties](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoint-templates/#updating-properties).

#### Updating properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoint-templates/#updating-properties) <a href="#updating-properties" id="updating-properties"></a>

1. Open the template artifact from the project explorer.
2.  First, update the endpoint parameter values with placeholders that are prefixed by `$`.

    For example:

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/endpoint-template-form-view-1.png)
3.  Then, click **Add Template Parameter** to open the **Parameter Configuration** dialog box and add the endpoint parameter placeholders (that you used above) as parameters:

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/endpoint-template-param-config-dialog.png)

#### Designing the integration[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-endpoint-templates/#designing-the-integration) <a href="#designing-the-integration" id="designing-the-integration"></a>

When you have an Endpoint template defined, you can use a **Template Endpoint** in your [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties) to call the parameters in the template.

1. Open to the **Design View** of your [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties).
2.  Drag the [Call Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/call-mediator) from the **Palette** and drop it to the relevant position in the [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties).

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/endpoint-temp-graphical-editor-1.png)

    Tip

    Similarly, you can use the [Send Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/send-mediator).
3.  Drag a [Template Endpoint](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/send-mediator) from the **Endpoints** section in the **Palette** and drop it to the empty box in the [Call Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/call-mediator).

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/endpoint-temp-graphical-editor-2.png)
4. Open the [Template Endpoint](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/send-mediator) from the project explorer and click **Add Parameters** to open the **Template Endpoint Parameter Configuration** dialog box.
5.  Specify the parameter values as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/template-endpoint-properties.png)
