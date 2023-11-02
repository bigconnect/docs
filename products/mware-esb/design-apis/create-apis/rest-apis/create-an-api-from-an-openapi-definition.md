# Create an API from an OpenAPI Definition

An OpenAPI definition is a format that describes REST APIs.

Follow the instructions below to create a REST API using an OpenAPI definition for an existing API.

## Create an API using the basic flow <a href="#create-an-api-using-the-basic-flow" id="create-an-api-using-the-basic-flow"></a>

1.  Sign in to the Publisher Portal of WSO2 API Manager.

    `https://<hostname>:9443/publisher` (e.g., `https://localhost:9443/publisher`).

    Note

    The Create API button will only appear if you have the "creator" role permission.
2.  Click **Create API** and then click **Import OpenAPI**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/import-open-api.png" alt=""><figcaption></figcaption></figure>
3. Select one of the following options:
   * **OpenAPI URL** - If you select this option, you need to provide an endpoint URL.
   * **OpenAPI Archive/File** - If you select this option, drag and drop or click **Browse File to Upload** to upload either an individual OpenAPI definition or an archive that has an OpenAPI definition with external [file references](https://swagger.io/docs/specification/using-ref/).

{% hint style="info" %}
* You need to upload an archive if you have remote references in your API definition. - [Sample OpenAPI archive](https://apim.docs.wso2.com/en/4.2.0/assets/attachments/design/sample-archive.zip)
* If it is a single Swagger file without remote references, it can be imported directly, without zipping.
* When uploading an OpenAPI archive, the master Swagger file should be named as swagger.yaml/swagger.json.\
  If not, the validation will fail at the point of API creation.
* Referenced files can be named independently.
* When archiving the Swagger files, make sure that it does not have any hidden folders (e.g., `__MACOSX`).
{% endhint %}

{% hint style="success" %}


The Swagger definitions should be placed under one root folder and zipped.

**Sample folder structures**

```
    -masterFolder
      ---Swagger.json
      ---reference.json
      ---add.json
```

```
    -masterFolder
      ---Swagger.json
      ---schemes
         ---pet.json
      ---add.json
```

In the above sample, every Swagger definition is placed inside one root folder namely `masterFolder`.
{% endhint %}

1.  Select **OpenAPI URL** and provide `https://petstore3.swagger.io/api/v3/openapi.json` as the URL. Click **Next**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/create-rest-api-using-swagger-def-form1.png" alt=""><figcaption></figcaption></figure>
2. Edit the information as given below and click **Create**.

{% hint style="info" %}
Make sure to provide the correct endpoint at this step.
{% endhint %}

<table><thead><tr><th width="235">Field</th><th>Sample value</th></tr></thead><tbody><tr><td>Name</td><td>SwaggerPetstore</td></tr><tr><td>Context</td><td>/petstore</td></tr><tr><td>Version</td><td>1.0.6</td></tr><tr><td>Endpoint</td><td>https://petstore3.swagger.io/api/v3</td></tr></tbody></table>

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/create-rest-api-using-swagger-def-form2.png" alt=""><figcaption></figcaption></figure>

The Petstore API overview page appears.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/overviewpage-rest-api-creating-by-swagger-def.png" alt=""><figcaption></figcaption></figure>

### Resources <a href="#resources" id="resources"></a>

Click **API Configurations** and then click **Resources** to navigate to the Resources page.

You will notice that all the API resources are created automatically when the OpenAPI URL is specified.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/resource-of-pet-store-api.png" alt=""><figcaption></figcaption></figure>

### API Definition <a href="#api-definition" id="api-definition"></a>

1.  Click **API Configurations**, **API Definition**, and then click **Edit** to remove the security headers.

    This is required to invoke the API in the Developer Portal using the OpenAPI UI.

<div data-full-width="true">

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/edit-api-definition-pet-store.png" alt=""><figcaption></figcaption></figure>

</div>

```
 You can also import an API definition.

    1. Click on **Import Definition** from the toolbar

    2. Select one of the following options:
        - OpenAPI URL
        - OpenAPI Archive/File

    3. To try this out, select OpenAPI URL and enter `https://petstore3.swagger.io/api/v3/openapi.json` as the URL.

        [![](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/import-api-definition.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/import-api-definition.png)

        !!! tip
            The Linter results can be viewed here.

    4. If there are any errors present, they can be solved in the built-in editor by clicking on the **Edit and Import** button.

    5. Then click on the **Import Content** on the editor to import the API definition.
```



2. Remove the `petstore_auth` tag related configuration that appears under the `security` tag from the `/pet` POST resource given below.

{% hint style="info" %}
Do not remove the `default` tag related configuration that appears under the `security` tag.
{% endhint %}

**OpenAPI - Post resource**

```java
security:
    - petstore_auth:
        - 'write:pets'
        - 'read:pets'
    - default:
        - 'write:pets'
        - 'read:pets'
```

3. Remove the security `pet/{petId}` GET resource given below:

**OpenAPI - Get resource**

```java
//remove the following code snippet
security:
        - api_key: []
```

4. After removing the security tags, click **Update Content**.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/update-content-pet-store.png" alt=""><figcaption></figcaption></figure>

5. Click **Save** to save the changes.

{% hint style="info" %}
If you have already deployed your API, click on the dropdown option, and click **Save and deploy** so that your API will be redeployed, and your changes will appear in the API Gateway.
{% endhint %}

### Endpoints <a href="#endpoints" id="endpoints"></a>

1. Click **API Configurations** and click **Endpoints** to navigate to the Endpoints page.
2. Enter the information shown below.

| Field               | Sample value                                                                                                   |
| ------------------- | -------------------------------------------------------------------------------------------------------------- |
| Endpoint type       | HTTP/REST Endpoint                                                                                             |
| Production endpoint | `https://petstore3.swagger.io/api/v3`                                                                          |
| Sandbox endpoint    | Let's only work with the production endpoint for this sample. Therefore, deselect the sandbox endpoint option. |

<div align="left">

<img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/endpoint-menu.png" alt="" width="200">

</div>

3. Click **Save** to save the changes.

{% hint style="info" %}
If you have already deployed your API, click on the dropdown option, and click **Save and deploy** so that your API will be redeployed, and your changes will appear in the API Gateway.
{% endhint %}

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-endpoint-configuration-for-pet-store-api.png" alt=""><figcaption></figcaption></figure>

### Runtime Configuration <a href="#runtime-configuration" id="runtime-configuration"></a>

Click **API Configurations** and click **Runtime** to navigate to the Runtime Configurations page.

The Transport Level Security defines the transport protocol on which the API is exposed.

[![](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/runtime-config-menu.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/runtime-config-menu.png)

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/transport-level-security-pet-store.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Both HTTP and HTTPS transports are selected by default. It is able to limit the API availability to only one transport (e.g., HTTPS) by clearing the checkbox of the other transport.
{% endhint %}

{% hint style="info" %}


Transport Level Security defines the transport protocol on which the API is exposed. When creating a new API by using a Swagger or OpenAPI definition, these transport security schemes can be defined using “x-wso2- transports”and "x-wso2-mutual-ssl”extensions.

```yaml
x-wso2-mutual-ssl: "optional"
x-wso2-transports: 
    - "https"
    - “http”
```
{% endhint %}

## Subscriptions <a href="#subscriptions" id="subscriptions"></a>

1.  Click **Portal Configurations** and click **Subscriptions** to navigate to the Business Plans page.

    [![subscriptions menu](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/subscriptions-menu.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/subscriptions-menu.png)
2. Select **Gold** and **Silver** as the Business Plans.

{% hint style="info" %}
The API can be available at different levels of the service. The Business Plans allow you to limit the number of successful hits to an API during a given period of time.
{% endhint %}

3. Click **Save**

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-bussiness-plans-for-pet-store-api.png" alt=""><figcaption></figcaption></figure>

Now, a REST API from an OpenAPI Definition has been created and configured successfully.

Next, [deploy the API](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/deploy-api/deploy-an-api/), [test the API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/test-a-rest-api/), and finally [publish the API](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/publish-on-dev-portal/publish-an-api).
