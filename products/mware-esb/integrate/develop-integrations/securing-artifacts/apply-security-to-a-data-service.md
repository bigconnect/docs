# Apply Security to a Data Service

WSO2 supports WS-Security, WS-Policy, and WS-Security Policy specifications. These specifications define a behavioral model for Web services. To enable a security policy for a data service, you need to first create a security policy file, and then add it to the data service.

### Prerequisites[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/securing-data-services/#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

Be sure to [configure a user store](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/mi-setup/user\_stores/setting\_up\_a\_userstore/) for the Micro Integrator and add the required users and roles.

### Step 1: Creating a registry resource module[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/securing-data-services/#step-1-creating-a-registry-resource-module) <a href="#step-1-creating-a-registry-resource-module" id="step-1-creating-a-registry-resource-module"></a>

Registry artifacts (such as security policy files) should be stored in a **Registry Resource** module. Follow the steps given below to create a module:

1.  Right click on the [Integration project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project) and go to **New → Registry Resource**.

    !!! Tip Alternatively, you can go to **File → New → Others** and select **Registry Resources** from the opening wizard.
2. Enter a name for the module and click **Next** .
3. Enter the Maven information about the module and click **Finish** .
4. The new module will be listed in the project explorer.

### Step 2: Creating a security policy as a registry resource[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/securing-data-services/#step-2-creating-a-security-policy-as-a-registry-resource) <a href="#step-2-creating-a-security-policy-as-a-registry-resource" id="step-2-creating-a-security-policy-as-a-registry-resource"></a>

1. Right-click the registry resource module in the left navigation panel, click **New**, and then click **Registry Resource**. This will open the **New Registry Resource** window.
2. Select the **From existing template** option as shown below and click **Next** .\
   ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/119130577/119130583.png)
3.  Enter the following details:

    | Property      | Value          |
    | ------------- | -------------- |
    | Resource Name | Sample\_Policy |
    | Artifact Name | Sample\_Policy |
    | Template      | WS-Policy      |
    | Registry      | gov            |
    | Registry path | ws-policy/     |
4. Click **Finish** and the policy file will be listed in the navigator.
   1.  Let's use the **Design View** to enable the required security scenario. For example, enable the **Sign and Encrypt** security scenario.

       Tip

       Click the icon next to the scenario to get details of the scenario.

       ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/119130577/119130596.png)
   2.  You can also provide encryption properties, signature properties, and advanced rampart configurations.

       Info

       **Using role-based permissions?**

       For certain scenarios, you can specify user roles. After you select the scenario, scroll to the right to see the **User Roles** button. Either define the user roles inline or retrieve the user roles from the server.

       Info

       Switch to source view of the policy file and make sure the tokenStoreClass in the policy file is 'org.wso2.micro.integrator.security.extensions.SecurityTokenStore'. In addition, replace the ServerCrypto class with 'org.wso2.micro.integrator.security.util.ServerCrypto' if present.
5. Save the policy file.

### Step 2: Adding the security policy to the data service[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/securing-data-services/#step-2-adding-the-security-policy-to-the-data-service) <a href="#step-2-adding-the-security-policy-to-the-data-service" id="step-2-adding-the-security-policy-to-the-data-service"></a>

Once you have configured the policy file, you can add the security policy to the data service as explained below.

1.  If you have already created a data service using WSO2 Integration Studio, select the file from the Project Explorer.

    Tip

    Be sure to update your database credentials in the dataservice file.
2. Once you have opened the data service file, switch to the **Source View** to see the source of the data service.
3.  Add the following elements inside the `<data>` element and save the file.

    ```xml
     <policy key="gov:ws-policy/Sample_Policy.xml" />
     <enableSec />
    ```

### Step 3: Package the artifacts[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/securing-data-services/#step-3-package-the-artifacts) <a href="#step-3-package-the-artifacts" id="step-3-package-the-artifacts"></a>

See the instructions on [packaging the artifacts](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/packaging-artifacts) into a composite exporter.

### Step 4: Build and run the artifacts[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/securing-data-services/#step-4-build-and-run-the-artifacts) <a href="#step-4-build-and-run-the-artifacts" id="step-4-build-and-run-the-artifacts"></a>

See the instructions [deploying the artifacts](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/deploy-artifacts).

### Step 5: Testing the service[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/securing-data-services/#step-5-testing-the-service) <a href="#step-5-testing-the-service" id="step-5-testing-the-service"></a>

Create a Soap UI project with the relevant security settings and then send the request to the hosted service.

For guidelines on using SoapUI, see [general guidelines on testing with SOAP UI](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/advanced-development/applying-security-to-a-proxy-service/#general-guidelines-on-testing-with-soap-ui).

### Using an encrypted datasource password[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/securing-data-services/#using-an-encrypted-datasource-password) <a href="#using-an-encrypted-datasource-password" id="using-an-encrypted-datasource-password"></a>

When you create a data service for an RDBMS datasource, you have the option of encrypting the datasource connection password. This ensures that the password is encrypted in the configuration file (.dbs file) of the data service.

See the instructions on [encrypting plain-text passwords](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/mi-setup/security/encrypting\_plain\_text)

Once you have encrypted the datasource password, you can update the data service as explained below.

1. Open the data service and click **Data Sources** to expand the section. ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/data\_source\_expanded.png)
2. Click on the **Edit** icon of the respective Datasource to open **Edit Datasource** page. ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/tutorials/data\_services/edit\_datasource.png)
3. Make sure to check **Use as a Secret Alias**.
4. Update the Secret Alias and click on **Save**.
