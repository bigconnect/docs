# Create a Registry Resource

Initially, your Registry resources project will contain only a `pom` file. You can create any number of registry resources inside that project.

### Step 1: Creating the resource artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#step-1-creating-the-resource-artifact) <a href="#step-1-creating-the-resource-artifact" id="step-1-creating-the-resource-artifact"></a>

Right-click on the `Registry Resource project` and click **New** -> **Registry Resource**.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/select-resource.png)

This will open the **New Registry Resource** window.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-1-1.png)

Select one of the following options and click **Next**.

* [From existing template](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#from-existing-template)
* [Import from file system](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#import-from-file-system)
* [Import Registry dump file from file system](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#import-registry-dump-file-from-file-system)
* [Check-out from Registry](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#check-out-from-registry)

#### From existing template[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#from-existing-template) <a href="#from-existing-template" id="from-existing-template"></a>

Use the **From existing template** option if you want to generate a registry resource from a template.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-1-1.png)

Click **Next** and specify values for the following parameters:

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-2.png)

Enter a unique name for the resource and select a resource template for the **Template** field. In this example, a **WSDL File** template is used.

#### Import from file system[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#import-from-file-system) <a href="#import-from-file-system" id="import-from-file-system"></a>

Use the **Import from file system** option to import a file or a folder containing registry resources.

Tip

This helps you import a resource and collection from the same registry instance or a different registry instance that you have added. Similarly, you can export a resource or collection to the same registry instance or a different registry instance.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-1-2.png)

Click **Next** and specify values for the following parameters:

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-3.png)

| Browse file/Browse folder | Browse to find the relevant file or folder.                                                                                                                                                               |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Copy content only         | If you selected Browse Folder, the Copy content only check box will be enabled. Select the check box if you want to copy only the content of the folder (and not the folder itself) to the save location. |

#### Import Registry dump file from file system[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#import-registry-dump-file-from-file-system) <a href="#import-registry-dump-file-from-file-system" id="import-registry-dump-file-from-file-system"></a>

Use this option to browse for a registry dump file, which you can use to sync a registry.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-1-3.png)

Click **Next** and then click **Browse** to find the relevant file.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-4.png)

#### Check-out from registry[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#check-out-from-registry) <a href="#check-out-from-registry" id="check-out-from-registry"></a>

Use this option to check out files from the registry.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-1-4.png)

Click **Next** and specify the artifact name and the registry path from which you want to check out the files.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-5.png)

### Step 2: Saving the resource artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#step-2-saving-the-resource-artifact) <a href="#step-2-saving-the-resource-artifact" id="step-2-saving-the-resource-artifact"></a>

Specify the location to save the registry resource and click **Finish**.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_resource/new-resource-wizard-save.png)

| Registry path to deploy | Specify where the registry resource should be saved to at the time of deployment.                                                                                                                                                                |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Save Resource in        | Select an existing [registry resource project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#registry-resource-project) to save the resource. Alternatively, you can create a new registry resource project. |

### Editing a Registry Resource[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-registry-resources/#editing-a-registry-resource) <a href="#editing-a-registry-resource" id="editing-a-registry-resource"></a>

You may need to change the details you entered for a registry resource, for example, the registry path. You can edit such information using the **Registry Resource Editor**. To open this editor, right-click the [Registry Resource project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#registry-resource-project) and click **Registry Resource Editor**.

This editor lists all the registry resources that you have defined in that project and it will list the **Registry Path to Deploy** information per resource.
