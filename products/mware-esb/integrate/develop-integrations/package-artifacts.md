# Package Artifacts

To package Synapse artifacts, you need to create a Composite Application Project. Use one of the following methods:

### Using an existing composite application[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/packaging-artifacts/#using-an-existing-composite-application) <a href="#using-an-existing-composite-application" id="using-an-existing-composite-application"></a>

If you have an already created composite application project, do the following to package the Synapse artifacts into the composite application:

1. Select the `pom.xml` file that is under the composite application project in the project explorer.\
   ![Create CAPP](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/capp\_proj\_explorer.png)
2.  In the **Dependencies** section, select the artifacts from each of the projects.

    Note

    If you have created a custom mediator artifact, it should be packaged in the same composite application along with the other artifacts that uses the mediator.

    ![Create CAPP](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/capp\_dependencies.png)
3. Save the artifacts.

### Creating a new composite application[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/packaging-artifacts/#creating-a-new-composite-application) <a href="#creating-a-new-composite-application" id="creating-a-new-composite-application"></a>

If you have not previously created a composite application project, do the following to package the artifacts in your ESB Config project.

1. Right click on the ESB project and go to **New** and then click **Composite Exporter**. ![Create new CAPP](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/create\_new\_capp.png)
2. In the **New Composite Application Project** dialog that opens, select the artifacts from the relevant ESB projects and click **Finish** .\
   ![Create new CAPP](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/create\_new\_capp\_dialog.png)

Alternatively,

1. Right-click the project explorer and click **New -> Project** .\
   ![Create new CAPP](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/create\_new\_project\_capp.png)
2. In the **New Project** dialog that opens, select **Composite Application Project** from the list and click **Next** .\
   ![Create new CAPP](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/create\_new\_project\_capp\_dialog.png)
3. Give a name for the **Composite Application** project and select the artifacts that you want to package.\
   ![Create new CAPP](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/create\_new\_project\_capp\_select\_dependencies.png)
4. In the **Composite Application Project POM Editor** that opens, under **Dependencies** , note the information for each of the projects you selected earlier.\
   ![Create new CAPP](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/create\_new\_project\_capp\_dependencies\_view.png)
