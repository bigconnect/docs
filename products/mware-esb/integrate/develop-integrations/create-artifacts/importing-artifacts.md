# Importing Artifacts

Follow the instructions given below to import an integration artifact into WSO2 Integration Studio.

1. [Create an ESB project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project).
2.  Right-click the ESB project, click **New**, and select the type of artifact you want to import. For example, let's import a REST API artifact.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new-artifact.png)
3.  Select the **Import Artifact** option and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/select-import-artifact-option.png)
4.  Browse for the configuration file of your artifact, specify the location to save the artifact.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/select-artifact-file.png)
5. Click **Finish**.&#x20;

The artifacts are created in the `src/main/synapse-config/<artifact_type>` folder under the ESB project you specified.
