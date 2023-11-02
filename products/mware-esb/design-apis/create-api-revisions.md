# Create API Revisions

[Publishing](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/publish-on-dev-portal/publish-an-api/) displays the API in the Developer Portal, and [deployment](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/deploy-api/deploy-an-api/) makes the API available for invocation via a Gateway. In previous versions of WSO2 API Manager, API publishing and deployment in Gateways were tightly coupled. In the API life cycle, whenever the publisher promotes an API to the `Published` state, the API will be displayed in the Developer Portal as well as deployed in the specified Gateway environments. The publisher only has two options regarding API deployment: either to deploy the current state of the API in a Gateway or not deploy the API at all.

The Publisher Test Console provides a space to test the API before publishing and deploying an API. However, the Test Console does not provide the option to test the API in an actual Gateway with the runtime configurations. Making such changes to an API without proper testings and especially if the changes are reflected instantly in the Developer Portal and all the serving Gateways, is not a good approach for a production setup.

With the introduction of API Revisions and API Product Revisions, previously coupled publishing and deployment operations have been **decoupled**. The API publisher has more control over the API deployments and also has the ability to introduce recoverable changes to the API project. API revisions are similar to a checkpoint in time, capturing the current state of the API when the revision is created. If the changes have an adverse effect on the API once it is deployed in a Gateway environment, you can quickly recover to the previous state of the API. In addition, even if you want to test out a new run time configuration, this concept provides the ability to test it in an independent Gateway before applying the same change to all the Gateways.

## Architectural changes and key features <a href="#architectural-changes-and-key-features" id="architectural-changes-and-key-features"></a>

The revision feature brings in a few architectural changes to the previous versions of WSO2 API Manager.

* Changing the life cycle state to `Published` will only display the API in the Developer Portal and will not handle the deployment anymore.
* Deploying an API in a Gateway requires an additional step. The publisher has to create a revision and deploy that revision in the required Gateway environment(s).
* A single API or API Product can have multiple revisions (a maximum of 5, and once the max limit is reached, you have to delete a revision before creating a new one).
* If there are multiple Gateways, you have the option to deploy the same revision across all the Gateways or different revisions of the API in each Gateway. Each Gateway will handle the API requests differently based on the deployed revision.
* Create and deploy the API in a Gateway without changing the lifecycle state to `Published`.
* The revisions cannot be edited.
* To discard the latest changes, you have the ability to **restore** the API to a specific revision.

## Deploy a revision <a href="#deploy-a-revision" id="deploy-a-revision"></a>

Follow the instructions below to either deploy a new revision or deploy an existing revision in a given Gateway Environment:

### Depoly a new revision <a href="#depoly-a-new-revision" id="depoly-a-new-revision"></a>

Follow the instructions below to deploy a new revision for an API:

1.  Sign in to the Publisher.

    [https://localhost:9443/publisher](https://localhost:9443/publisher)
2.  Navigate to the **Deploy** section and click **Deployments**.

    The Deployments page appears as follows when you do not have any existing revisions.



    The Deployments page appears as follows when you have at least one revision.



    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/design/revision/deploy-first-revision.png" alt=""><figcaption></figcaption></figure>

    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/design/revision/deploy-new-revision.png" alt=""><figcaption></figcaption></figure>
3. Click **Deploy New Revision**.
4. Optionally, provide a description for the revision.
5. Select the API Gateways in which you want to deploy the API.
6. Click **Deploy**.

### Deploy an existing revision <a href="#deploy-an-existing-revision" id="deploy-an-existing-revision"></a>

Follow the instructions below if you need to **deploy an existing revision** on a given Gateway Environment:

1.  Sign in to the Publisher.

    [https://localhost:9443/publisher](https://localhost:9443/publisher)
2. Navigate to the **Deploy** section and click **Deployments**.
3. Select the required revision from the **Deployed Revision** drop-down menu.
4.  Click **Deploy**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/revision/deploy-existing-revision.png" alt=""><figcaption></figcaption></figure>

## Create a new revision <a href="#create-a-new-revision" id="create-a-new-revision"></a>

Follow the instructions below to create a new revision from your existing API:

1.  Sign in to the Publisher.

    [https://localhost:9443/publisher](https://localhost:9443/publisher)
2. Navigate to the **Deploy** section and click **Deployments**.
3.  Click **+** in the revision timeline.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/revision/create-revision.png" alt=""><figcaption></figcaption></figure>
4. Optionally, provide a description for the revision.
5. Click **Create**.

## Undeploy a revision <a href="#undeploy-a-revision" id="undeploy-a-revision"></a>

Follow the instructions below if you need to undeploy a revision from a Gateway Environment:

1.  Sign in to the Publisher.

    [https://localhost:9443/publisher](https://localhost:9443/publisher)
2. Navigate to the **Deploy** section and click **Deployments**
3.  Click on the **Undeploy** button that appears against the respective Gateway Environment from which you want to undeploy the revision.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/revision/undeploy-revision.png" alt=""><figcaption></figcaption></figure>

## Delete a revision <a href="#delete-a-revision" id="delete-a-revision"></a>

If you need to delete a specific revision, click on the **Delete** button that corresponds to the respective revision in the revision timeline as shown below.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/revision/delete-revision.png" alt=""><figcaption></figcaption></figure>

## Restore a revision <a href="#restore-a-revision" id="restore-a-revision"></a>

API revisions can not be updated. Therefore, when you update an API via the Publisher Portal, the changes will get reflected in the **Current API**, which is the current state of the API. Any changes to the API, such as a change in a mediation policy, security configurations, documentation or simply changing the description of the API, etc., will deviate the state of the Current API from the latest revision. If the API is in the `Published` state, the changes available in the Current API state are displayed in the Developer Portal.

If you need to revert these changes and restore a specific revision, click on the **Restore** button in the revision timeline, as shown below. Thereby, this will restore that specific revision into the Current API, overriding the changes that are currently available in the Current API. As the restore option will only update the Current API in the Publisher and Developer Portal, it does not have any effect on the deployed revisions.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/revision/restore-revision.png" alt=""><figcaption></figcaption></figure>
