# Deploy Artifacts

Once you have your integration artifacts developed and [packaged in a composite exporter](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/packaging-artifacts), you can deploy the composite exporter in your Micro Integrator server or your container environment.

### Deploy artifacts in the embedded Micro Integrator[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/deploy-artifacts/#deploy-artifacts-in-the-embedded-micro-integrator) <a href="#deploy-artifacts-in-the-embedded-micro-integrator" id="deploy-artifacts-in-the-embedded-micro-integrator"></a>

The light-weight Micro Integrator is already included in your WSO2 Integration Studio package, which allows you to deploy and run the artifacts instantly.

See the instructions in [using the embedded Micro Integrator](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/using-embedded-micro-integrator) of WSO2 Integration Studio.

### Deploy artifacts in a remote Micro Integrator instance[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/deploy-artifacts/#deploy-artifacts-in-a-remote-micro-integrator-instance) <a href="#deploy-artifacts-in-a-remote-micro-integrator-instance" id="deploy-artifacts-in-a-remote-micro-integrator-instance"></a>

Download and set up a Micro Integrator server in your VM and deploy the composite exporter with your integration artifacts.

See the instructions in [using a remote Micro Integrator](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/using-remote-micro-integrator).

### Deploy artifacts in Docker[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/deploy-artifacts/#deploy-artifacts-in-docker) <a href="#deploy-artifacts-in-docker" id="deploy-artifacts-in-docker"></a>

Use the Docker Exporter module in WSO2 Integration Studio to build a Docker image of your Micro Integrator solution and push it to your Docker registry. You can then use this Docker image from your Docker registry to start Docker containers.

See the instructions on using the [Docker Exporter](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-docker-project).

### Deploy artifacts in Kubernetes[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/deploy-artifacts/#deploy-artifacts-in-kubernetes) <a href="#deploy-artifacts-in-kubernetes" id="deploy-artifacts-in-kubernetes"></a>

Use the Kubernetes Exporter module in WSO2 Integration Studio to deploy a Docker image of your Micro Integrator Solution in a Kubernetes environment.

See the instructions on using the [Kubernetes Exporter](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-kubernetes-project).
