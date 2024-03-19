# API Builder

The API builder makes it easy for you to create custom API applications in Liferay. Create API schemas and endpoints that meet your exact needs.

### [Using the API Builder](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#using-the-api-builder) <a href="#using-the-api-builder" id="using-the-api-builder"></a>

Each API application can house multiple endpoints and schemas. This is similar to Liferay’s out-of-the-box API applications. For example, the `headless-admin-user` API application houses endpoints for `accounts`, `organizations`, `roles`, `user-accounts`, etc.

NOTE

The API Builder is configured at an instance-level scope. Create different API applications that are unique to each virtual instance. To re-create an API application in another instance, use the [data migration center](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/data-migration-center).

To access the API Builder application,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), select the _Control Panel_ tab, and click _API Builder_ under Object.
2.  A list of your custom API applications appears.

    The title, URL, description, last update, and status appear for each API application.

    ![See a list of your custom API applications.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/api-builder/images/01.png)

From the API Builder, you can also [create](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-custom-api-applications) and [manage](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#managing-custom-api-applications) custom API applications.

#### [Creating Custom API Applications](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-custom-api-applications) <a href="#creating-custom-api-applications" id="creating-custom-api-applications"></a>

1. While in the [API Builder](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#using-the-api-builder), click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and fill in the requested information.
2.  Add the Title, the URL, and (optionally) a description for your API.

    The URL has a limit of 255 characters, and it must contain only numbers, letters, or dashes.
3. Click _Create_.

#### [Managing Custom API Applications](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#managing-custom-api-applications) <a href="#managing-custom-api-applications" id="managing-custom-api-applications"></a>

While in the [API Builder](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#using-the-api-builder), use the Actions menu (![Actions menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the API application to manage it. Select an available option:

**Edit**: Edit your API application.

You can also click on the API application’s name to edit it.

While editing an API application, you can create new [endpoints](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-endpoints) and [schemas](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-schemas) that work like Liferay’s out-of-the-box API applications.

**Delete**: Delete your API application.

**Change Publication Status**: Change your publication status from published to unpublished or vice versa.

An API application must be published before use. Once published, the generated APIs have the same set of features as Liferay’s out-of-the-box APIs (i.e. pagination, filtering, sorting, etc.)

### [Creating and Managing Endpoints](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-endpoints) <a href="#creating-and-managing-endpoints" id="creating-and-managing-endpoints"></a>

Liferay DXP 2024.Q1+ U112

[Beta Feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#beta-feature-flags)

IMPORTANT

This feature is currently behind a [beta feature flag](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#beta-feature-flags) (LPS-178642). API endpoints currently support retrieving and creating collections of entities.

API endpoints are the specific URL locations that accept requests.

To see, manage, and create endpoints,

1.  Start [editing your API application](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#managing-custom-api-applications) and select the _Endpoints_ tab to see a list of your custom endpoints.

    The method, URL path, description, and last update are shown for each endpoint.

To create a new endpoint,

1.  Click _Add_ (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and enter your details.

    ![Create an endpoint with a GET or POST method from the Endpoints tab.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/api-builder/images/02.png)
2.  Click _Create_.

    Once the endpoint is created, you can configure it using the Configuration tab.

    There are different configurations depending on the selected method. See [Creating a GET Method Endpoint](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-a-get-method-endpoint) and [Creating a POST Method Endpoint](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-a-post-method-endpoint) for more information.

To manage endpoints, use the Actions menu (![Actions menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the endpoint and select an available option:

**Edit**: Edit your endpoint.

You can also click on the endpoint’s path to edit it.

**Copy URL**: Copy your endpoint’s URL.

**Delete**: Delete your endpoint.

#### [Creating a GET Method Endpoint](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-a-get-method-endpoint) <a href="#creating-a-get-method-endpoint" id="creating-a-get-method-endpoint"></a>

The GET method requests and retrieves data from a specified source. Using the GET method you can retrieve information from a collection or a single element (i.e. you can retrieve a list of products, or you can retrieve information from a single product).

To create an endpoint using the GET method,

1. While [creating an endpoint](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-endpoints), select the GET method.
2.  Select the _Retrieve Type_. Retrieve a Collection or a Single Element.

    Depending on the type you selected, there are different configurations for the Path and in the Configurations tab.
3. Select the _Scope_ for your endpoint based on your object’s scope. Endpoints can be scoped for the Instance(Company) or a Site.
4.  Enter the _Path_ you want to use in your endpoint.

    If you selected Single Element as your Retrieve Type, you must add a parameter to the path. (e.g. if you want information from a single product, you need to provide a product ID as a parameter).
5. (Optional) Add a _Description_ to your endpoint.
6. Click _Create_.

Once the endpoint is created, select the _Configuration_ tab for additional settings. Depending on the Retrieve Type you chose, different settings appear. In the configuration tab,

1. Select the _Response Body Schema_. [Create a Schema](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-schemas) and apply it to the endpoint to define the structure and format of the data that is returned by the API in response to a client’s request.

If you selected Collection as your Retrieve Type,

1.  Apply _Filters_ to your endpoint.

    JSON filtering can be incorporated into the endpoint. For example, `filter=lastName eq 'Smith'` could be used to filter the endpoint for entries that contain a last name Smith.
2.  Apply _Sorting_ parameters to your endpoint.

    JSON sorting can be incorporated. For example `sort=firstName:asc` could be used to sort the entries alphabetically by first name.

    See [API query parameters](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/api-query-parameters) to learn more about filtering and sorting.
3. Click _Publish_.

![Configure the endpoint using the GET method and a Collection as the Retrieve Type.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/api-builder/images/03.png)

If you selected Single Element as your Retrieve Type,

1. Select the _Path Parameter Property_. This is the object field that is mapped to the provided parameter in the endpoint’s path. It can be a system or custom field (e.g. `id` and `externalReferenceCode` are system fields and `employeeId` is a custom field.).
2. (Optional) Add a _Path Parameter Description_.
3. Click _Publish_.

![Configure the endpoint using the GET method and a Single Element as the Retrieve Type.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/api-builder/images/04.png)

Once the endpoint is created, you can see it on your API explorer page `http://localhost:8080/o/api`.

#### [Creating a POST Method Endpoint](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-a-post-method-endpoint) <a href="#creating-a-post-method-endpoint" id="creating-a-post-method-endpoint"></a>

In the API Builder, the POST method is used to create [object](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects) entries.

To create an endpoint using the POST method,

1. While [creating an endpoint](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-endpoints), select the POST method.
2. Select the _Scope_ for your endpoint based on your object’s scope. Endpoints can be scoped for the instance (Company) or a site.
3. Enter the _Path_ you want to use in your endpoint.
4. (Optional) Add a _Description_ to your endpoint.
5. Click _Create_.

Once the endpoint is created, select the _Configuration_ tab for additional settings. In the configuration tab,

1.  Select the _Request Body Schema_. [Create a Schema](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-schemas) and apply it to the endpoint to define the structure and format of the data that the client should include in the request body when creating a resource on the server.

    The request body must contain all mandatory properties from the main object, and it can only contain properties from the main object.

    If the schema is not selected, the API endpoint won’t work, and it won’t show in the API application explorer page.

    WARNING

    You can use relationship-type properties in your schema to define relationships between entities. The created entities can be related to existing object entries through their ID.

    Although you can define related entities as properties, the values provided for these related entities in the POST request are ignored. Therefore, it’s not possible to create related entities when creating a new entry using the POST method.
2.  (Optional) Select the _Response Body Schema_. [Create a Schema](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-schemas) and apply it to the endpoint to define the structure and format of the data that the server sends back to the client in response to the POST request.

    If you don’t select a response body schema, an empty response is returned when the API is executed successfully.

Once the endpoint is created, you can see it on your API explorer page `http://localhost:8080/o/api`.

#### [Creating a POST Method Endpoint Through an API Call](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-a-post-method-endpoint-through-an-api-call) <a href="#creating-a-post-method-endpoint-through-an-api-call" id="creating-a-post-method-endpoint-through-an-api-call"></a>

If you are creating the POST endpoint through API calls, keep these points in mind:

* POST endpoints must have `singleElement` as their retrieve type. This ensures that only the information related to the posted entry is requested.
* An error occurs if you use `singleElement` as retrieve type and define a path parameter (e.g. `/api/users/{userId}` where `{userId}` is the path parameter) during the API call to create a POST endpoint.
* POST endpoints without a request body schema don’t work.

Here’s an example of a CURL command you can use to create a POST endpoint:

```json
curl -X 'POST' \
  'http://localhost:8080/o/headless-builder/endpoints/' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -u 'test@liferay.com:learn' \
  -d '{
  "externalReferenceCode": "post-boss-endpoint-erc",
  "description": "This is an endpoint to post a Boss entry",
  "httpMethod": "post",
  "name": "Post Boss",
  "path": "/boss",
  "r_apiApplicationToAPIEndpoints_c_apiApplicationERC": "companies-manager-erc",
  "r_requestAPISchemaToAPIEndpoints_c_apiSchemaERC": "boss-schema-erc",
  "r_responseAPISchemaToAPIEndpoints_c_apiSchemaERC": "boss-schema-erc",
  "retrieveType": "singleElement",
  "scope": "company"
}'
```

Here’s how it works, in sections:

```json
curl -X 'POST' \
  'http://localhost:8080/o/headless-builder/endpoints/' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -u 'test@liferay.com:learn' \
```

This section of the command specifies the HTTP request method as POST, defines the URL to which the POST request is being sent (http://localhost:8080/o/headless-builder/endpoints/), sets the Accept and the Content-Type header for the request to JSON format, and includes basic authentication with the specified user and password (-u ‘test@liferay.com:learn’).

```json
  -d '{
  "externalReferenceCode": "post-boss-endpoint-erc",
  "description": "This is an endpoint to post a Boss entry",
  "httpMethod": "post",
  "name": "Post Boss",
  "path": "/boss",
  "r_apiApplicationToAPIEndpoints_c_apiApplicationERC": "company-manager-erc",
  "r_requestAPISchemaToAPIEndpoints_c_apiSchemaERC": "boss-schema-erc",
  "r_responseAPISchemaToAPIEndpoints_c_apiSchemaERC": "boss-schema-erc",
  "retrieveType": "singleElement",
  "scope": "company"
}'
```

This section specifies the data sent in the request body. In this case, a JSON payload contains different properties:

`"externalReferenceCode"`: Defines the endpoint’s external reference code (ERC).

`"description"`: Defines the endpoint’s description.

`"httpMethod"`: Defines the endpoint’s Method.

`"name"`: Define the endpoint’s Name.

`"path"`: Defines the endpoint’s Path.

`"r_apiApplicationToAPIEndpoints_c_apiApplicationERC"`: Points to an ERC to specify the API Application where the endpoint is added.

`"r_requestAPISchemaToAPIEndpoints_c_apiSchemaERC"`: Points to an ERC to specify the schema used for the endpoint’s request body.

`"r_responseAPISchemaToAPIEndpoints_c_apiSchemaERC"`: Points to an ERC to specify the schema used for the endpoint’s response body.

`"retrieveType"`: Defines the endpoint’s Retrieve Type.

`"scope"`: Defines the endpoint’s scope.

### [Creating and Managing Schemas](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-schemas) <a href="#creating-and-managing-schemas" id="creating-and-managing-schemas"></a>

API schemas specify what entities are returned (or received) by your API.

To see, manage, and create schemas,

1.  Start [editing your API application](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#managing-custom-api-applications) and select the _Schemas_ tab to see a list of your schemas.

    The name, description, and last update are shown for each schema.

To create a new schema,

1.  Click _Add_ (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and enter your details.

    Select a name, add a description (optional), and select the object definition associated with the schema.

    ![Select an object definition to create a new schema.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/api-builder/images/05.png)
2.  Click _Create_.

    Once the schema is created, you can [configure its properties](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#configuring-and-managing-schema-properties) using the Properties tab.

To manage schemas, use the Actions menu (![Actions menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the schema and select an available option:

**Edit**: edit your schema.

You can also click on the schema’s name to edit it.

**Delete**: delete your schema.

#### [Configuring and Managing Schema Properties](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#configuring-and-managing-schema-properties) <a href="#configuring-and-managing-schema-properties" id="configuring-and-managing-schema-properties"></a>

1. To configure the schema properties, start [editing a schema](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/api-builder#creating-and-managing-schemas) and select the _Properties_ tab.
2.  Select the entities you want to include in your schema.

    To add an entity to the schema, click the entity’s name or the _Add_ icon (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-app.png)) next to it.

    Select entities available from your [Liferay object](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects). If the object has [object relationships](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/relationships), the entities from those objects can be selected also.

    You can use the search bar to find entities.
3.  Manage the entities. You can rearrange, rename, or exclude them.

    Use the move handle next to the property name to rearrange your properties.

    Hover your mouse over the property name to see the Edit (![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit-pencil.png)) and the Delete (![Delete icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-trash.png)) actions.

    You can edit the object’s property name and description. For example, for the `Boss` object in the screenshot, change `Boss Name` to `bossName`, so it matches the mapped property.

    ![Select the entities you want to include in your schema.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/api-builder/images/06.png)
4. Click _Publish_.

\
