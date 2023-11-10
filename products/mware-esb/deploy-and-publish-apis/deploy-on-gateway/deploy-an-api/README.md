# Deploy an API

**API Deploying** is the process of making the API available for invocation via a Gateway. You can deploy an API to a selected API Gateway environment via the Publisher Portal. To invoke an API, it needs to be published on the Developer Portal as well as deployed on a Gateway environment. You need to create a revision of an API in order to deploy it.&#x20;

**Undeploying an API** will remove the API from the API Gateway Environment.

Follow the instructions below to deploy a new revision for an API:

1.  Sign in to the Publisher.

    [https://localhost:9443/publisher](https://localhost:9443/publisher)
2.  Navigate to the **Deploy** section and click **Deployments**.

    The Deployments page appears as follows when you do not have any existing revisions.

    [![New revision](https://apim.docs.wso2.com/en/4.1.0/assets/img/design/revision/deploy-first-revision.png)](https://apim.docs.wso2.com/en/4.1.0/assets/img/design/revision/deploy-first-revision.png)

    The Deployments page appears as follows when you have at least one revision.

    [![Deploy new revision option](https://apim.docs.wso2.com/en/4.1.0/assets/img/design/revision/deploy-new-revision.png)](https://apim.docs.wso2.com/en/4.1.0/assets/img/design/revision/deploy-new-revision.png)
3.  Click **Deploy New Revision**.

    Note

    **Skip this step** if you are creating your very first revision.
4. Optionally, provide a description for the revision.
5. Select the API Gateways in which you want to deploy the API.
6. Click **Deploy**.
