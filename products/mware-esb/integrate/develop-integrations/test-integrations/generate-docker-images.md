# Generate Docker Images

### Before you begin[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/generate-docker-image/#before-you-begin) <a href="#before-you-begin" id="before-you-begin"></a>

1. Install Docker from the [Docker Site](https://docs.docker.com/).
2. Create a Docker Account at [Docker Hub](https://hub.docker.com/) and log in.
3. Start the Docker server.

### Generate the Docker image[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/generate-docker-image/#generate-the-docker-image) <a href="#generate-the-docker-image" id="generate-the-docker-image"></a>

1.  Right-click the **Composite Application Project** in the project explorer and then click **Generate Docker Image**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/open-docker\_image\_generation\_wizard.png)
2.  In the **Generate Docker Image Wizard** that opens, select one from the following three options and proceed.

    ![Generate docker image dialog](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/generate-docker-image-options.png)

    *   **Create a new Docker Exporter Project**

        Select this option to create a new **Docker Exporter Project** and click **Proceed**. You can build a docker image using this Docker Exporter Project. You are now directed to the \[Docker Exporter Project wizard]\(create-docker-project.
    * **Generate Docker Image with the Embedded MI**
      1.  Select this option to generate a Docker image with the embedded Micro Integrator runtime of WSO2 Integration studio.

          Note

          This is recommended only for testing.
      2.  Click **Next** and enter the following details:

          ![Create docker image dialog](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/generate\_docker\_image\_dialog.png)

          | Parameter                | Description                                                                                                                                                                    |
          | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
          | Name of the Application  | The name of the composite application with the artifacts created for your ESB project. The name of the ESB project is displayed by default, but it can be changed if required. |
          | Application Version      | The version of the composite application.                                                                                                                                      |
          | Name of the Docker Image | Give a name for the Docker image.                                                                                                                                              |
          | Docker Image Tag         | A tag for the Docker image to be used for reference. This is optional.                                                                                                         |
          | Export Destination       | Browse for the preferred location in your machine to export the Docker image.                                                                                                  |
      3.  Click **Next**. Select the **Config** projects that you want to include in the Docker image and click **Finish**.

          ![Create docker image](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/select\_artifact\_docker.png)

          Once the Docker image is successfully created, a message similar to the following appears in your screen.

          ![Create docker image](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_image\_successful.png)
    *   **Generate Docker Image with an Existing Project**

        This will use the existing Docker Exporter Project that you selected and create a Docker image. You will receive a message similar to the following:

        ![Create docker image](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_image\_successful.png)
