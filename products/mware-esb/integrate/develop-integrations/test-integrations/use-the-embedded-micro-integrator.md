# Use the Embedded Micro Integrator

WSO2 Integration Studio contains an embedded Micro Integrator instance, which you can use for testing your integration solutions during the development process.

### Deploy and run artifacts in the (embedded) server[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#deploy-and-run-artifacts-in-the-embedded-server) <a href="#deploy-and-run-artifacts-in-the-embedded-server" id="deploy-and-run-artifacts-in-the-embedded-server"></a>

Once you have the [integration artifacts packaged](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/packaging-artifacts) in a composite application, you can deploy and run them in the embedded Micro Integrator using a single click.

1. Select the composite application in the project explorer.
2. Click the ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/common/play-head-icon.jpg) icon in the menu palette to open the Run As dialog box.
3.  Select Run on Micro Integrator and click OK.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/run-as-micro-integrator.png)
4. Select the artifacts from the composite application that you want to deploy.\
   ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/testing\_artifact\_selection.png)
5. Click **Finish**. The artifacts will be deployed in the WSO2 Micro Integrator and the server will start. See the startup log in the **Console** tab:\
   ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/testing\_log.png)
6. If you find errors in your mediation sequence, use the [debugging features](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/debugging-mediation) to troubleshoot.

### View deployed endpoints in the (embedded) server[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#view-deployed-endpoints-in-the-embedded-server) <a href="#view-deployed-endpoints-in-the-embedded-server" id="view-deployed-endpoints-in-the-embedded-server"></a>

Use the Runtime Services tab in WSO2 Integration Studio to view the endpoint URLs of the artifacts deployed in the embedded Micro Integrator.

When you [deploy the artifacts and start](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#deploy-and-run-artifacts-in-the-embedded-server) the embedded Micro Integrator, the Console tab prints the server startup logs and the Runtime Services tab will open as shown below.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/deployed-endpoints.jpg)

If you have closed the tab and you want to open it again, go to Window -> Show View -> Other and select Runtime Services.

[![Runtime\_services](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/show-deployed-endpoints.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/show-deployed-endpoints.png))

### Update (embedded) server configurations and libraries[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#update-embedded-server-configurations-and-libraries) <a href="#update-embedded-server-configurations-and-libraries" id="update-embedded-server-configurations-and-libraries"></a>

For some integrations, it is necessary to update the server configurations. For example, if you are integrating with an external broker, you need to update broker connection details and also add the broker's connection JARs to the server's `/lib` folder.

Click the ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/server-configs-panel-icon.png) icon to open the Embedded Micro Integrator Configuration dialog box shown below.

Note

You can also paramterize configurations as [environment variables](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/mi-setup/dynamic\_server\_configurations) and later [inject environment variables to the embedded Micro Integrator](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#injecting-environment-variables-to-embedded-micro-integrator).

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/server-configs-panel.png)

In the upper section, update the server configuration file (`deployment.toml` file). In the lower section, add any required third-party libraries to the `/lib` folder of the server.

### Encrypt static (embedded) server secrets[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#encrypt-static-embedded-server-secrets) <a href="#encrypt-static-embedded-server-secrets" id="encrypt-static-embedded-server-secrets"></a>

If you have secrets in the `deployment.toml` file, you can encrypt them using the Cipher Tool.

1. Open the [Embedded Micro Integrator Configuration](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#update-embedded-server-configs-and-libraries) dialog box.
2. Update the static secrets in the `deployment.toml` file as explained in [encrypting server secrets](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/mi-setup/security/encrypting\_plain\_text).
3.  Click Encrypt Secrets.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/encrypt-secrets.png)

This will run the Cipher Tool internally and encrypt the secrets. The plain-text values you entered are now replaced with the encrypted values.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/encrypt-secrets-executed.png)

### Redeploy integration artifacts[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#redeploy-integration-artifacts) <a href="#redeploy-integration-artifacts" id="redeploy-integration-artifacts"></a>

Hot deployment is enabled in the Micro Integrator by default. This allows you to redeploy artifacts without restarting the server. However, if you have applied changes to the server configurations and libraries, the server will restart.

1. Select the composite application that contains your artifacts.
2. Click the ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/common/play-head-icon.jpg) icon in the menu palette.

### Injecting environment variables to embedded Micro Integrator[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#injecting-environment-variables-to-embedded-micro-integrator) <a href="#injecting-environment-variables-to-embedded-micro-integrator" id="injecting-environment-variables-to-embedded-micro-integrator"></a>

WSO2 Micro Integrator supports environment variables for server configurations as well as synapse configurations (integration artifacts).

Note

To be able to dynamically inject parameters to the embedded Micro Integrator, you must first define the relevant configurations as environment variables. See the following topics for instructions:

* [Environment variables for server Configurations](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/mi-setup/dynamic\_server\_configurations)
* [Environment variables for synapse configurations](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/injecting-parameters)

Follow the steps given below.

1.  [Deploy and run](https://apim.docs.wso2.com/en/latest/integrate/develop/using-embedded-micro-integrator/#deploy-and-run-artifacts-in-the-embedded-server) the artifacts in the embedded Micro Integrator.

    Tip

    Note that you need to run the embedded Micro Integrator at least once before proceeding to specify environment variables.
2.  You can now go to **Run** -> **Run Configurations** in the upper menu bar of your computer:

    ![run configurations menu](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/run-configs-menu.png)
3.  In the **Run Configurations** dialog box that opens, select **Micro Integrator Server 1.2.0** that is listed under **Generic Server** in the navigator:

    ![run configurations dialog box](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/run-configs-dialog-box.png)
4. In the **Server** tab, select Micro Integrator 1.2.0 from the list if it is not already selected.
5.  Go to the **Environment** tab and click **New** to add an environment variable:

    ![run configurations environments](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/run-configs-env.png)
6.  Enter the variable name and value as a key-value pair and click **OK**. In this example, let's use the server offset:

    Tip

    The offset parameter in the `deployment.toml` file of the embedded Micro Integrator should be specified as follows:

    ```toml
    [server]
    offset="$env{offset}"
    ```

    ![run configurations environments](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/run-configs-env-popup.png)
7.  Click **Apply** to apply the new environment variable.

    ![run configurations environments](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/run-configs-env-apply.png)
