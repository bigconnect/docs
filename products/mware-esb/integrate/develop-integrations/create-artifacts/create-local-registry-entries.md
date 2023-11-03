# Create Local Registry Entries

The **local registry** acts as a memory registry where you can store static content as a key-value pair. This could be a static text specified as **inline text**, static XML specified as an **inline XML** fragment, or a URL (using the `src` attribute).

Inline text

```xml
<localEntry key="version">0.1</localEntry>
```

Inline XMLSource URL

This is useful for the type of static content often found in XSLT files, WSDL files, URLs, etc. Local entries can be referenced from mediators in the Micro Integrator mediation flows and resolved at runtime. These entries are top-level entries and are globally visible within the entire system. Values of these entries can be retrieved via the extension XPath function `synapse:get-property(prop-name)`, and the keys of these entries could be specified wherever a registry key is expected within the configuration. A local entry shadows any entry with the same name from a remote Registry.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/registry/creating-local-registry-entries/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the local entry[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/registry/creating-local-registry-entries/#creating-the-local-entry) <a href="#creating-the-local-entry" id="creating-the-local-entry"></a>

Follow these steps to create a new local entry.

1.  Right-click the [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) and go to **New → Local Entry** to open the **New Local Entry** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_local\_entry/select-local-entry.jpg)
2.  Select **Create a New Local Entry** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_local\_entry/new-local-entry-wizard-1.png)
3.  Enter a unique name for the local entry, specify one of the following types of local entries and specify the details.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_local\_entry/new-local-entry-wizard-2.png)

    * **In-Line Text Entry**: Type the text you want to store
    * **In-Line XML Entry**: Type the XML code you want to store
    * **Source URL Entry**: Type or browse to the URL you want to store
4. In the **Save in** field, specify the project to save the artifact:
   * To save the local entry in an existing ESB Config project in your workspace, click **Browse** and select that project.
   * To save the local entry in a new ESB Config project, click **Create new Project** and create the new project.
5. Click **Finish**.&#x20;

The local entry is created in the `src/main/synapse-config/local-entries` folder under the ESB Config project you specified, and the local entry appears in the editor.

#### Updating the properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/registry/creating-local-registry-entries/#updating-the-properties) <a href="#updating-the-properties" id="updating-the-properties"></a>

Open the new local entry artifact from the project explorer. You can use the **Form** view or the **Source** view to update message processor properties.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_local\_entry/new-local-entry-form-view.jpg)

#### Using a local entry[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/registry/creating-local-registry-entries/#using-a-local-entry) <a href="#using-a-local-entry" id="using-a-local-entry"></a>

After you create a local entry, you can reference it from a mediator in your mediation workflow. For example, if you created a local entry with XSLT code, you can add an XSLT mediator to the workflow and then reference the local entry as follows:

1. Open to the **Design View** of your [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties).
2.  Drag and drop an [XSLT Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/xslt-mediator) to the mediation flow as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_local\_entry/local-entry-graphical-editor-1.png)
3.  Double-click the XSLT mediator icon to open the **Properties** tab.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_local\_entry/xslt-mediator-properties.png)
4.  Click the **XSLT Static Schema Key** property to get the **Resource Key** wizard.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_local\_entry/xslt-mediator-resource-key.png)
5.  Click the **Workspace** link, and then navigate to and select the local entry that contains the XSLT code.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_local\_entry/xslt-mediator-resource-key-workspace.png)
6. Click **OK**.
