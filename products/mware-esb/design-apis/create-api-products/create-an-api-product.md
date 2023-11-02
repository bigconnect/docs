# Create an API Product

Let's create an API Product named customer leasing that includes resources from the Customer Info API & Leasing API, as discussed in the [previous example](https://apim.docs.wso2.com/en/4.2.0/design/create-api-product/api-product-overview/#how-it-works).

1.  Sign in to the API Publisher.\


    `https://<hostname>:9443/publisher`

    (e.g., `https://localhost:9443/publisher`).
2.  Create the following two APIs by [importing the attached OpenAPI definition (a.k.a Swagger definition) files](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api-from-an-openapi-definition/).

    * Customer Info API - [customer-info-api.yaml](https://apim.docs.wso2.com/en/4.2.0/assets/attachments/learn/customer-info-api.yaml)
    * Leasing API - [leasing-api.yaml](https://apim.docs.wso2.com/en/4.2.0/assets/attachments/learn/leasing-api.yaml)

    After you create each of the APIs, click **Try Out**, to view the respective resources for each of the APIs as follows:

    *   Customer info API



        <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/customer-info-api.png" alt=""><figcaption></figcaption></figure>
    *   Leasing API



        <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/leasing-api.png" alt=""><figcaption></figcaption></figure>


3.  Click **API Products** on the left menu, and then click on **API Product**.



    The **Create an API Product** page appears.

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/create-api-product.png" alt=""><figcaption></figcaption></figure>
4.  Enter the API Product details.

    | Name    | customer-leasing |
    | ------- | ---------------- |
    | Context | customer-leasing |



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/define-api-product.png" alt=""><figcaption></figcaption></figure>
5.  Click **NEXT**.

    The Add Resources page appears.



    * The leftmost pane shows the available list of APIs.
    * The central pane lists the available resources of a given API, which is selected in the left pane.
    * You can select a resource in the central pane and add it to the right pane, which shows the resources that will be added to the API Product.

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/add-resources.png" alt=""><figcaption></figcaption></figure>
6.  Add resources from the `customer-info` API.

    1. Click on the `customer-info` API in the left pane.
    2. Select the `GET /customers` and `GET /customers/{customerId}` resources in the central pane.
    3. Click **Add Selected** to add these resources to the right pane so that you can use them to create the API Product.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/select-customer-info-resources.png" alt=""><figcaption></figcaption></figure>
7.  Add resources from the `leasing` API.

    1. Click on the `leasing` API in the left pane.
    2. Select the `GET /assets/{assetId}` and `POST /assets` resources in the central pane.
    3. Click **Add Selected** to add these resources to the right pane so that you can use them to create the API Product.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/select-leasing-resources.png" alt=""><figcaption></figcaption></figure>
8. Create the API Product.
   *   If you only want to create the API Product without deploying and publishing the API Product, click **Create**.

       The API Product will appear in the API Publisher, as shown below.



       <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/api-product-publisher-details.png" alt=""><figcaption></figcaption></figure>
   *   If you want to create, deploy and publish the API Product, click **Create & Publish**.

       Deploying the API Product will make it available for invocation via the Gateway, and publishing the API Product will make it available in the Developer Portal.



       <div data-full-width="true">

       <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/api-product-portal-listing.png" alt=""><figcaption></figcaption></figure>

       </div>
9.  [Deploy the API Product](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/deploy-api/deploy-an-api).

    This makes the API Product available for invocation via the Gateway.


10. Publish the API Product to make it available in the Developer Portal.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/publish-api-product.png" alt=""><figcaption></figcaption></figure>
11. Sign in to the Developer Portal to subscribe and consume the API Product in the same way you would do for an API.\


    `https://<hostname>:9443/devportal`

    (e.g., `https://localhost:9443/devportal`).\


    For more information on how to subscribe to an API, see [Subscribe to an API](https://apim.docs.wso2.com/en/4.2.0/consume/manage-subscription/subscribe-to-an-api/).
