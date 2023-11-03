# Develop Integrations

The contents on this page will walk you through the topics related to developing integration solutions using WSO2 Integration Studio.

### WSO2 Integration Studio[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/intro-integration-development/#wso2-integration-studio) <a href="#wso2-integration-studio" id="wso2-integration-studio"></a>

WSO2 Integration Studio is the comprehensive developer tool, which you will use to develop, build, and test your integration solutions before the solutions are pushed to your production environments. See the topics given below for details.

### Development workflow[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/intro-integration-development/#development-workflow) <a href="#development-workflow" id="development-workflow"></a>

Integration developers will follow the workflow illustrated by the following diagram.

![developer workflow](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/development\_workflow.png)

#### Set up the workspace[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/intro-integration-development/#set-up-the-workspace) <a href="#set-up-the-workspace" id="set-up-the-workspace"></a>

To start developing integration solutions, you need to first [install and set up WSO2 Integration Studio](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/installing-wso2-integration-studio).

#### Develop[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/intro-integration-development/#develop) <a href="#develop" id="develop"></a>

* Create projects and modules
* Create artifacts
* Secure the artifacts

#### Build and run[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/intro-integration-development/#build-and-run) <a href="#build-and-run" id="build-and-run"></a>

1.  Package

    The artifacts and modules should be packaged in a Composite Exporter before they can be deployed in any environment.
2.  Deploy

    You can easily deploy and try out the packaged integration artifacts on your preferred environment:
3.  Unit Tests

    Use the integration test suite of WSO2 Integration Studio to run unit tests on the developed integration solution.

#### Iterate and improve[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/intro-integration-development/#iterate-and-improve) <a href="#iterate-and-improve" id="iterate-and-improve"></a>

As you build and run the integration flow, you may identify errors that need to be fixed, and changes that need to be done to the synapse artifacts.

You must redeploy the integration artifacts after applying changes.

* If you are testing on a VM, the artifacts will be instantly deployed when you [redeploy the synapse artifacts](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/deploy-artifacts).
* If you are testing on containers, you need to rebuild the [Docker images](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-docker-project) or [Kubernetes artifacts](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-kubernetes-project).

#### Push to production[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/intro-integration-development/#push-to-production) <a href="#push-to-production" id="push-to-production"></a>

It is recommended to use a CICD pipeline to deploy your tested integration solutions in the production environment.
