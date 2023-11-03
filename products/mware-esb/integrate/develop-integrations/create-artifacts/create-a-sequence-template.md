# Create a Sequence Template

Follow the instructions given below to create a new **Sequence Template** in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-sequence-templates/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the Sequence Template artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-sequence-templates/#creating-the-sequence-template-artifact) <a href="#creating-the-sequence-template-artifact" id="creating-the-sequence-template-artifact"></a>

1.  Right-click the [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) and go to **New → Template** to open the **New Template Artifact** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/select-template.png)
2.  Select **Create a New Template** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/new-template-wizard-1.png)
3.  Enter a unique name for the template and select **Sequence Template** from the list.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/new-template-wizard-2-2.png)

    Specify values for the [required parameter](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/template-properties/#endpoint-template-properties) for the selected endpoint type.
4. Do one of the following to save the artifact:
   * To save the template in an existing ESB Config project in your workspace, click **Browse** and select that project.
   * To save the template in a new ESB Config project, click **Create new Project** and create the new project.
5.  Click **Finish**.&#x20;

    The template is created in the `src/main/synapse-config/templates` folder under the ESB Config project you specified.
6. To use the sequence template, [update the properties](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-sequence-templates/#updating-properties).

#### Updating properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-sequence-templates/#updating-properties) <a href="#updating-properties" id="updating-properties"></a>

1.  Open the **Design View** of the sequence template you created.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/sequence-template-design-view-1.png)
2. Drag-and-drop the required mediators from the **Palette**.
3.  Specify parameter values as an XPATH.

    In the following example, the `GREETING_MESSAGE` property of the **Log** mediator is specified using the `$func:message` expression.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/sequence-template-design-view-2.png)

#### Designing the integration[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-sequence-templates/#designing-the-integration) <a href="#designing-the-integration" id="designing-the-integration"></a>

When you have a Sequence template defined, you can use a [Call Template Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/call-template-mediator) in your [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties).

1. Open to the **Design View** of your [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties).
2.  Drag the [Call Template Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/call-template-mediator) from the **Palette** and drop it to the relevant position in the [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties).

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/sequence-temp-graphical-editor-1.png)
3. Double-click the [Call Template Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/call-template-mediator) icon to open the **Properties** tab.
4.  Select your sequence template from the list in the **Available Templates** field and then add values using the template parameters.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_template/sequence-temp-graphical-editor-2.png)
