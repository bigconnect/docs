# Create Docker Exporter

Create a Docker Exporter if you want to deploy your integration solutions inside a Docker environment. This project directory allows you to package multiple [integration modules](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project) into a single Docker image and then build and push to the Docker registries.

Note

When using Kubernetes/Docker exporter project, make sure to use Integration Studio 8.0.0 with the latest WUM update. mi-1.2.0 pack we have to change the miVersion of the config-mapper-parser plugin into 1.2.0/1631645406425 in the pom

### Creating the Docker exporter[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/create-docker-project/#creating-the-docker-exporter) <a href="#creating-the-docker-exporter" id="creating-the-docker-exporter"></a>

Follow the steps given below.

1. [Create a new integration project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project) and create a Docker Exporter by doing one of the following.
   1. As part of creating an integration project, you can select the **Docker Exporter** check box.
   2. You can right click on an existing integration project and select **New** -> **Docker Exporter**.
2.  In the **New Docker Exporter** dialog box that opens, enter a name for the Docker exporter and other parameters as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/new-project/docker-exporter.png)

    Enter the following information:

    | Parameter               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Docker Exporter Name    | Required. Give a name for the Docker project.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Base Image Repository   | <p>Required. Select the base Micro Integrator Docker image for your solution. Use one of the following options:</p><ul><li>wso2/wso2mi: This is the community version of the Micro Integrator Docker image, which is stored in the <a href="https://hub.docker.com/r/wso2/wso2mi">public WSO2 Docker registry</a>. This is selected by default.</li><li>docker.wso2.com: This is the Micro Integrator Docker image that includes product updates. This image is stored in the <a href="https://docker.wso2.com/tags.php?repo=wso2mi">private WSO2 Docker registry</a>. Note that you need a valid <a href="https://wso2.com/subscription/free-trial">WSO2 subscription</a> to use the Docker image with updates.</li><li>You can also use a custom Docker image from a custom repository.</li></ul><p>If you specify a Docker image from a private repository, note that you need to log in to your repository from a terminal before you build the image (as explained below).</p> |
    | Base Image Tag          | Required. Specify the tag of the base image that you are using.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    |                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Target Image Repository | <p>Required. The Docker repository to which you will later push this Docker image.</p><ul><li>If your repository is in Docker Hub, use the docker_user_name/repository_name format.</li><li>If you are using any other repository, use the repository_url/repository_user_name/repository_name forrmat.</li></ul><p>If required, you can update this information later when you build the Docker image or when you push the image to the relevant repository.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Target Image Tag        | Required. Give a tag name for the Docker image.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Environment Variables   | You can enter multiple environment variables as key-value pairs.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
3.  Optionally, click **Next** and configure Maven details for the Docker exporter.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/new\_docker\_project\_maven\_info.png)
4. Click **Finish**. The Docker exporter is created in the project explorer.
5.  This step is only required if you already have a Docker image (in your local Docker repository) with the same name as the base image specified above.

    Info

    In this scenario, WSO2 Integration Studio will first check if there is a difference in the two images before pulling the image specified in the **Base Image Repository** field. If the given base image is more updated, the existing image will be overwritten by this new image. Therefore, if you are currently using an older version, or if you have custom changes in your existing image, they will be replaced.

    To avoid your existing custom/older images from being replaced, add the following property under **dockerfile-maven-plugin -> executions -> execution -> configurations** in the `pom.xml` file of your Docker Exporter project. This configuration will ensure that the base image will not be pulled when a Docker image already exists with the same name.

    ```xml
    <pullNewerImage>false</pullNewerImage>
    ```

### The Docker Exporter directory[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/create-docker-project/#the-docker-exporter-directory) <a href="#the-docker-exporter-directory" id="the-docker-exporter-directory"></a>

Expand the **Docker Exporter** in the project explorer. See that the following folders and files are created:

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/proj\_explorer\_docker\_project.png)

| Directory       | Description                                                                                                                                                                                              |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Libs            | This folder stores libraries that should be copied to the Docker image. During the build time, the libraries inside this directory will be copied to the image.                                          |
| Resources       | This folder stores additional files and resources that should be copied to the Docker image. During the build time, the resources inside this directory will be copied to the image.                     |
| deployment.toml | The [product configuration file](https://apim.docs.wso2.com/en/4.2.0/reference/config-catalog).                                                                                                          |
| Dockerfile      | The Dockerfile containing the build details.                                                                                                                                                             |
| pom.xml         | The file for selecting the relevant composite exporters that should be included in the Docker image. This information is also used when you later build and push Docker images to the Docker registries. |

### Build and Push Docker images[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/create-docker-project/#build-and-push-docker-images) <a href="#build-and-push-docker-images" id="build-and-push-docker-images"></a>

Before you begin:

*   Create your integration artifacts in an [ESB Config sub project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#sub-projects) and package the artifacts in a [Composite Exporter](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/packaging-artifacts/#sub-projects). For example, see the HelloWorld sample given below.

    ![Integration artifacts for Docker](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/integration-projects-for-docker.png)
* Be sure to start your Docker instance before building the image. If Docker is not started, the build process will fail.
* If you are using a Micro Integrator Docker image from a private registry as your base image:
  1.  Open a terminal and use the following command to log in to Docker:

      ```bash
      docker login -u username -p password 
      ```
  2. In the next step, specify the name of the private Docker registry.

To build and push the Docker image:

Note

As an alternative, you can skip the steps given below and manually build and push the Docker images using maven. Open a terminal, navigate to the Docker exporter and execute the following command:

```bash
mvn clean install -Dmaven.test.skip=true -Ddockerfile.username={username} -Ddockerfile.password={password} 
```

However, note that you need **Maven 3.5.2** or a later version when you build the Docker image manually (without using WSO2 Integration Studio).

1.  Open the **pom.xml** file inside the Docker project and click **Refresh** on the top-right. Your composite application project with integration artifacts will be listed under **Dependencies** as follows:

    ![Docker Pom view](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/docker-pom.png)
2. Select the composite exporters that you want to package inside the Docker image.
3. If required, you can update the **Target Repository** to which the image should be pushed and the **Target Tag**.
4. Save the POM file and click **Build** to start the Docker image build.
5.  It will build the Docker image based on the Dockerfile and the Target details. When the image is created, the following message will display.

    ![Docker Build Success](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/build.png)
6.  Click Push to push the Docker image to your Docker registry.

    In the dialog box that opens, provide the details of your Docker registry as shown below.

    [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/docker-registry-credentials.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/docker-registry-credentials.png)

    When the image is pushed to the registry, you will see the following message.

    ![Docker Push Success](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/docker\_k8s\_project/push.png)
