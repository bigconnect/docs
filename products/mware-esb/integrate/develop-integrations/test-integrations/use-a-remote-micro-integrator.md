# Use a Remote Micro Integrator

The light-weight Micro Integrator is already included in your WSO2 Integration Studio package, which allows you to [deploy and run the artifacts instantly](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/using-embedded-micro-integrator).

The following instructions can be used to run your artifacts in a remote Micro Integrator instance.

### Deploy and run artifacts in a remote instance[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-remote-micro-integrator/#deploy-and-run-artifacts-in-a-remote-instance) <a href="#deploy-and-run-artifacts-in-a-remote-instance" id="deploy-and-run-artifacts-in-a-remote-instance"></a>

1. [Download and install](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/install/installing-the-product/installing-mi) the Micro Integrator server and on your computer.
2. [Package your Synapse artifacts](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/packaging-artifacts) from WSO2 Integration Studio.

However, when your solutions are ready to be moved to your production environments, it is recommended to use a **CICD pipeline**.

Note

As an alternative, you can skip the steps given below and manually copy the exported CAR file to the `<MI_HOME>/repository/deployment/server/carbonapps/` folder, where `<MI_HOME>` is the root folder of your Micro Integrator installation. For more information on how to export a CAR file, see [Exporting Artifacts](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/exporting-artifacts).

### Add a new remote instance[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-remote-micro-integrator/#add-a-new-remote-instance) <a href="#add-a-new-remote-instance" id="add-a-new-remote-instance"></a>

1.  Open the Getting Started view and click Add Server to open the New Server dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/is\_getting\_started\_page.png)
2.  In the New Server dialog box that opens, expand the WSO2 folder and select the version of your server.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/new-server-choose-type.png)
3. Click Next. In the CARBON\_HOME field, provide the path to your product's home directory and then click Next.
4.  Review the default port details for your server and click Next.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/new-server-mi-port-settings.png)

    Note

    * If you selected an Enterprise Integrator server in the previous step, enter the port details required for an Enterprise Integrator.
    * If you are already running another server on these ports, give unused ports. See [Default ports](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/deployment-best-practices/changing-the-default-ports-with-offset/) of the Micro Integrator for more information.

### Deploy and run artifacts in a remote instance[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-remote-micro-integrator/#deploy-and-run-artifacts-in-a-remote-instance\_1) <a href="#deploy-and-run-artifacts-in-a-remote-instance_1" id="deploy-and-run-artifacts-in-a-remote-instance_1"></a>

1. To deploy the C-App project to your server, select the composite application from the list, click Add to move it to the configured list, and then click Finish.\
   ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/new-server-add-capps.png)
2.  On the Servers tab, note that the server is currently stopped. Click the ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/common/play-head-icon.jpg) icon on the tool bar. If prompted to save changes to any of the artifact files you created earlier, click Yes.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/new-server-start.png)

### Deploy, redeploy, or remove artifacts in a remote instance[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-remote-micro-integrator/#deploy-redeploy-or-remove-artifacts-in-a-remote-instance) <a href="#deploy-redeploy-or-remove-artifacts-in-a-remote-instance" id="deploy-redeploy-or-remove-artifacts-in-a-remote-instance"></a>

*   To deploy/remove C-Apps, right-click the server, click Add and Remove, and follow the instructions on the wizard.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/testing-integrations/new-server-add-remove-capps.png)
* If you want to redeploy a C-App after modifying the included artifacts, select the already deployed C-App, right-click and click Redeploy.

Note

Hot deployment is enabled in the Micro Integrator by default. This allows you to redeploy artifacts without restarting the server. If you disabled hot deployment while adding the server, you need to restart the server as well.

### Disable graceful shutdown (Only for testing)[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/using-remote-micro-integrator/#disable-graceful-shutdown-only-for-testing) <a href="#disable-graceful-shutdown-only-for-testing" id="disable-graceful-shutdown-only-for-testing"></a>

By default, the graceful shutdown capability is enabled in the Micro Integrator distribution. This means that the server will not immediately shut down when there are incomplete HTTP messaging transactions that are still active. These are transactions that are processed by the HTTP/S PassThrough transport.

For example, consider a delay in receiving the response from the backend (which should be returned to the messaging client). Because graceful shutdown is enabled, the Micro Integrator will wait until the time specified by the following parameter in the server configuration file (`deployment.toml` file) is exceeded before shutting down.

```toml
[transport.http]
socket_timeout = 180000
```

You can disable this feature by using the following system property when you start the server:
