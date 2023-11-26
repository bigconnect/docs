# REST API

### General Flowable REST principles

#### Installation and Authentication <a href="#installation-and-authentication" id="installation-and-authentication"></a>

Flowable includes a REST API to the Flowable engine that can be installed by deploying the flowable-rest.war file to a servlet container like Apache Tomcat. However, it can also be used in another web-application by including the servlet (and/or its mappings) in your application and add all flowable-rest dependencies to the classpath.

By default the Flowable engine will connect to an in-memory H2 database. You can change the database settings in the flowable-app.properties file in the _WEB-INF/META-INF/flowable-app_ folder. The REST API uses JSON format ([http://www.json.org](http://www.json.org/)) and is built upon the Spring MVC ([http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html)).

All REST-resources require a valid user with the _rest-access-api_ privilege to be authenticated by default. If any valid user should be able to call the REST API, the _flowable.rest.app.authentication-mode_ can be set to _any-user_ (this was the default in older versions of Flowable).

A default user that can access the REST API can be configured by settings the following properties:

```
flowable.rest.app.admin.user-id=rest-admin
flowable.rest.app.admin.password=test
flowable.rest.app.admin.first-name=Rest
flowable.rest.app.admin.last-name=Admin
```

When the REST app boots up, the user is created if it doesn’t exist or fetched otherwise. This user will be given the _access-rest-api_ privilege which is needed by default to access the REST API. **Do not forget to change the password of this user afterwards**. If the _flowable.rest.app.admin.user-id_ is not set, no users or privileges will be created. So, after the initial setup, removing this property will not remove the user nor the privilege that has been configured before.

Basic HTTP access authentication is used, so you should always include a _Authorization: Basic …​==_ HTTP-header when performing requests or include the username and password in the request-url (for example, [_http://username:password@localhost:8080/xyz_](http://username:password@localhost:8080/xyz)).

**We recommend that you use Basic Authentication in combination with HTTPS.**

#### Configuration <a href="#configuration" id="configuration"></a>

The Flowable REST web application uses Spring Java Configuration for starting the Flowable Form engine, defining the basic authentication security using Spring security, and to define the variable converters for specific variable handling. A small number of properties can be defined by changing the flowable-app.properties file you can find in the WEB-INF/META-INF/flowable-app/flowable-app.properties folder. If you need more advanced configuration options there’s the possibility to override the default Spring beans in XML in the flowable-custom-context.xml file you can also find in the WEB-INF/classes folder. An example configuration is already in comments in this file. This is also the place to override the default RestResponseFactory by defining a new Spring bean with the name restResponsefactory and use your custom implementation class for it.

#### Usage in Tomcat <a href="#usage-in-tomcat" id="usage-in-tomcat"></a>

Due to [default security properties on Tomcat](http://tomcat.apache.org/tomcat-8.0-doc/security-howto.html), **escaped forward slashes (%2F and %5C) are not allowed by default (400-result is returned).** This may have an impact on the deployment resources and their data-URL, as the URL can potentially contain escaped forward slashes.

When issues are experienced with unexpected 400-results, set the following system-property:

_-Dorg.apache.tomcat.util.buf.UDecoder.ALLOW\_ENCODED\_SLASH=true_

It’s best practice to always set the **Accept** and **Content-Type** (in case of posting/putting JSON) headers to **application/json** on the HTTP requests described below.

#### Methods and return-codes <a href="#methods-and-return-codes" id="methods-and-return-codes"></a>

| Method | Operations                                                                                                                                           |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| GET    | Get a single resource or get a collection of resources.                                                                                              |
| POST   | Create a new resource. Also used for executing resource-queries which have a too complex request-structure to fit in the query-URL of a GET-request. |
| PUT    | Update properties of an existing resource. Also used for invoking actions on an existing resource.                                                   |
| DELETE | Delete an existing resource.                                                                                                                         |

| Response                     | Description                                                                                                                                                                                                                                                                                                       |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200 - Ok                     | The operation was successful and a response has been returned (GET and PUT requests).                                                                                                                                                                                                                             |
| 201 - Created                | The operation was successful and the entity has been created and is returned in the response-body (POST request).                                                                                                                                                                                                 |
| 204 - No content             | The operation was successful and entity has been deleted and therefore there is no response-body returned (DELETE request).                                                                                                                                                                                       |
| 401 - Unauthorized           | The operation failed. The operation requires an Authentication header to be set. If this was present in the request, the supplied credentials are not valid or the user is not authorized to perform this operation.                                                                                              |
| 403 - Forbidden              | The operation is forbidden and should not be re-attempted. This implies an issue with authorization not authentication, it’s an operation that is not allowed. Example: deleting a task that is part of a running process is not allowed and will never be allowed, regardless of the user or process/task state. |
| 404 - Not found              | The operation failed. The requested resource was not found.                                                                                                                                                                                                                                                       |
| 405 - Method not allowed     | The operation failed. The method is not allowed for this resource. For example, trying to update (PUT) a deployment-resource will result in a 405 status.                                                                                                                                                         |
| 409 - Conflict               | The operation failed. The operation causes an update of a resource that has been updated by another operation, which makes the update no longer valid. Can also indicate a resource that is being created in a collection where a resource with that identifier already exists.                                   |
| 415 - Unsupported Media Type | The operation failed. The request body contains an unsupported media type. Also occurs when the request-body JSON contains an unknown attribute or value that doesn’t have the right format/type to be accepted.                                                                                                  |
| 500 - Internal server error  | The operation failed. An unexpected exception occurred while executing the operation. The response-body contains details about the error.                                                                                                                                                                         |

The media-type of the HTTP-responses is always application/json unless binary content is requested (for example, deployment resource data), the media-type of the content is used.

#### Error response body <a href="#error-response-body" id="error-response-body"></a>

When an error occurs (both client and server, 4XX and 5XX status-codes) the response body contains an object describing the error that occurred. An example for a 404-status when a task is not found:

```
{
  "statusCode" : 404,
  "errorMessage" : "Could not find a task with id '444'."
}
```

#### Request parameters <a href="#request-parameters" id="request-parameters"></a>

**URL fragments**

Parameters that are part of the URL (for example, the deploymentId parameter in [http://host/flowable-rest/event-registry-api/event-registry-repository/deployments/](http://host/flowable-rest/event-registry-api/event-registry-repository/deployments/){deploymentId}) need to be properly escaped (see [URL-encoding or Percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding)) in case the segment contains special characters. Most frameworks have this functionality built in, but it should be taken into account. Especially for segments that can contain forward-slashes (for example, deployment resource), this is required.

**Rest URL query parameters**

Parameters added as query-string in the URL (for example, the name parameter used in [http://host/flowable-rest/event-registry-api/event-registry-repository/deployments?name=Deployment](http://host/flowable-rest/event-registry-api/event-registry-repository/deployments?name=Deployment)) can have the following types and are mentioned in the corresponding REST-API documentation:

| Type    | Format                                                                                                                                                                                                                                                                                                                  |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| String  | Plain text parameters. Can contain any valid characters that are allowed in URLs. In the case of a XXXLike parameter, the string should contain the wildcard character % (properly URL-encoded). This allows you to specify the intent of the like-search. For example, 'Tas%' matches all values, starting with 'Tas'. |
| Integer | Parameter representing an integer value. Can only contain numeric non-decimal values, between -2.147.483.648 and 2.147.483.647.                                                                                                                                                                                         |
| Long    | Parameter representing a long value. Can only contain numeric non-decimal values, between -9.223.372.036.854.775.808 and 9.223.372.036.854.775.807.                                                                                                                                                                     |
| Boolean | Parameter representing a boolean value. Can be either true or false. All other values other than these will cause a '405 - Bad request' response.                                                                                                                                                                       |
| Date    | Parameter representing a date value. Use the ISO-8601 date-format (see [ISO-8601 on wikipedia](http://en.wikipedia.org/wiki/ISO\_8601)) using both time and date-components (e.g. 2013-04-03T23:45Z).                                                                                                                   |

**JSON body parameters**

| Type    | Format                                                                                                                                                                                                                                |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| String  | Plain text parameters. In the case of a XXXLike parameter, the string should contain the wildcard character %. This allows you to specify the intent of the like-search. For example, 'Tas%' matches all values, starting with 'Tas'. |
| Integer | Parameter representing an integer value, using a JSON number. Can only contain numeric non-decimal values, between -2.147.483.648 and 2.147.483.647.                                                                                  |
| Long    | Parameter representing a long value, using a JSON number. Can only contain numeric non-decimal values, between -9.223.372.036.854.775.808 and 9.223.372.036.854.775.807.                                                              |
| Date    | Parameter representing a date value, using a JSON text. Use the ISO-8601 date-format (see [ISO-8601 on wikipedia](http://en.wikipedia.org/wiki/ISO\_8601)) using both time and date-components (for example, 2013-04-03T23:45Z).      |

**Paging and sorting**

Paging and order parameters can be added as query-string in the URL (for example, the name parameter used in [http://host/flowable-rest/event-registry-api/event-registry-repository/deployments?sort=name](http://host/flowable-rest/event-registry-api/event-registry-repository/deployments?sort=name)).

| Parameter | Default value                      | Description                                                                                                      |
| --------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| sort      | different per query implementation | Name of the sort key, for which the default value and the allowed values are different per query implementation. |
| order     | asc                                | Sorting order which can be 'asc' or 'desc'.                                                                      |
| start     | 0                                  | Parameter to allow for paging of the result. By default the result will start at 0.                              |
| size      | 10                                 | Parameter to allow for paging of the result. By default the size will be 10.                                     |

> **Note**
>
> Bear in mind that the start parameter is used as the offset of the query. For example, to get tasks in three pages of three items each (9 items), we would use:
>
> ```
> GET /runtime/tasks?start=0&size=3
> GET /runtime/tasks?start=3&size=3
> GET /runtime/tasks?start=6&size=3
> ```

### Deployment <a href="#deployment" id="deployment"></a>

**When using Tomcat, please read** [**Usage in Tomcat**](https://www.flowable.com/open-source/docs/eventregistry/ch07-REST#usage-in-tomcat)**.**

#### List of Event Registry Deployments <a href="#list-of-event-registry-deployments" id="list-of-event-registry-deployments"></a>

```
GET event-registry-repository/deployments
```

| Parameter              | Required | Value                                              | Description                                                                                                   |
| ---------------------- | -------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| name                   | No       | String                                             | Only return deployments with the given name.                                                                  |
| nameLike               | No       | String                                             | Only return deployments with a name like the given name.                                                      |
| category               | No       | String                                             | Only return deployments with the given category.                                                              |
| categoryNotEquals      | No       | String                                             | Only return deployments which don’t have the given category.                                                  |
| parentDeploymentId     | No       | String                                             | Only return deployments with the given parent deployment id.                                                  |
| parentDeploymentIdLike | No       | String                                             | Only return deployments with a parent deployment id like the given value.                                     |
| tenantId               | No       | String                                             | Only return deployments with the given tenantId.                                                              |
| tenantIdLike           | No       | String                                             | Only return deployments with a tenantId like the given value.                                                 |
| withoutTenantId        | No       | Boolean                                            | If true, only returns deployments without a tenantId set. If false, the withoutTenantId parameter is ignored. |
| sort                   | No       | 'id' (default), 'name', 'deploytime' or 'tenantId' | Property to sort on, to be used together with the 'order'.                                                    |

| Response code | Description                           |
| ------------- | ------------------------------------- |
| 200           | Indicates the request was successful. |

**Success response body:**

```
{
  "data": [
    {
      "id": "03ab310d-c1de-11e6-a4f4-62ce84ef239e",
      "name": null,
      "deploymentTime": "2016-12-14T10:16:37.000+01:00",
      "category": null,
      "url": "http://localhost:8080/flowable-rest/event-registry-api/event-registry-repository/deployments/03ab310d-c1de-11e6-a4f4-62ce84ef239e",
      "parentDeploymentId": "17510",
      "tenantId": ""
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "id",
  "order": "asc",
  "size": 1
}
```

#### Get an Event Registry deployment <a href="#get-an-event-registry-deployment" id="get-an-event-registry-deployment"></a>

```
GET event-registry-repository/deployments/{deploymentId}
```

| Parameter    | Required | Value  | Description                              |
| ------------ | -------- | ------ | ---------------------------------------- |
| deploymentId | Yes      | String | The identifier of the deployment to get. |

| Response code | Description                                       |
| ------------- | ------------------------------------------------- |
| 200           | Indicates the deployment was found and returned.  |
| 404           | Indicates the requested deployment was not found. |

**Success response body:**

```
{
  "id": "03ab310d-c1de-11e6-a4f4-62ce84ef239e",
  "name": null,
  "deploymentTime": "2016-12-14T10:16:37.000+01:00",
  "category": null,
  "url": "http://localhost:8080/flowable-rest/event-registry-api/event-registry-repository/deployments/03ab310d-c1de-11e6-a4f4-62ce84ef239e",
  "parentDeploymentId": "17510",
  "tenantId": ""
}
```

#### Create a new Event Registry deployment <a href="#create-a-new-event-registry-deployment" id="create-a-new-event-registry-deployment"></a>

```
POST event-registry-repository/deployments
```

**Request body:**

The request body should contain data of type _multipart/form-data_. There should be exactly one file in the request: any additional files will be ignored. The deployment name is the name of the file-field passed in.

An additional parameter (form-field) can be passed in the request body with name tenantId. The value of this field will be used as the identifier of the tenant in which this deployment is done.

| Response code | Description                                                                                                                                                                  |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 201           | Indicates the deployment was created.                                                                                                                                        |
| 400           | Indicates there was no content present in the request body or the content mime-type is not supported for deployment. The status-description contains additional information. |

**Success response body:**

```
{
  "id": "03ab310d-c1de-11e6-a4f4-62ce84ef239e",
  "name": "newDeployment1",
  "deploymentTime": "2016-12-14T10:16:37.000+01:00",
  "category": null,
  "url": "http://localhost:8080/flowable-rest/event-registry-api/event-registry-repository/deployments/03ab310d-c1de-11e6-a4f4-62ce84ef239e",
  "parentDeploymentId": "17510",
  "tenantId" : "myTenant"
}
```

#### Delete an Event Registry deployment <a href="#delete-an-event-registry-deployment" id="delete-an-event-registry-deployment"></a>

```
DELETE event-registry-repository/deployments/{deploymentId}
```

| Parameter    | Required | Value  | Description                                 |
| ------------ | -------- | ------ | ------------------------------------------- |
| deploymentId | Yes      | String | The identifier of the deployment to delete. |

| Response code | Description                                                                                    |
| ------------- | ---------------------------------------------------------------------------------------------- |
| 204           | Indicates the deployment was found and has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested deployment was not found.                                              |

#### Get an Event Registry deployment resource content <a href="#get-an-event-registry-deployment-resource-content" id="get-an-event-registry-deployment-resource-content"></a>

```
GET event-registry-repository/deployments/{deploymentId}/resourcedata/{resourceId}
```

| Parameter    | Required | Value  | Description                                                                                                                                                                                                         |
| ------------ | -------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| deploymentId | Yes      | String | The identifier of the deployment the requested resource is part of.                                                                                                                                                 |
| resourceId   | Yes      | String | The identifier of the resource to get the data for. **Make sure you URL-encode the resourceId in case it contains forward slashes. For example, use 'events%2Fmy-event.event' instead of 'events/my-event.event'.** |

| Response code | Description                                                                                                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200           | Indicates both deployment and resource have been found and the resource data has been returned.                                                                                       |
| 404           | Indicates the requested deployment was not found or there is no resource with the given identifier present in the deployment. The status-description contains additional information. |

**Success response body:**

The response body will contain the binary resource-content for the requested resource. The response content-type will be the same as the type returned in the resources 'mimeType' property. Also, a content-disposition header is set, allowing browsers to download the file instead of displaying it.

### Event Definitions <a href="#event-definitions" id="event-definitions"></a>

#### List of event definitions <a href="#list-of-event-definitions" id="list-of-event-definitions"></a>

```
GET event-registry-repository/event-definitions
```

| Parameter         | Required | Value                                                                    | Description                                                                                                                                                               |
| ----------------- | -------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version           | No       | integer                                                                  | Only return event definitions with the given version.                                                                                                                     |
| name              | No       | String                                                                   | Only return event definitions with the given name.                                                                                                                        |
| nameLike          | No       | String                                                                   | Only return event definitions with a name like the given name.                                                                                                            |
| key               | No       | String                                                                   | Only return event definitions with the given key.                                                                                                                         |
| keyLike           | No       | String                                                                   | Only return event definitions with a key like the given key.                                                                                                              |
| resourceName      | No       | String                                                                   | Only return event definitions with the given resource name.                                                                                                               |
| resourceNameLike  | No       | String                                                                   | Only return event definitions with a name like the given resource name.                                                                                                   |
| category          | No       | String                                                                   | Only return event definitions with the given category.                                                                                                                    |
| categoryLike      | No       | String                                                                   | Only return event definitions with a category like the given name.                                                                                                        |
| categoryNotEquals | No       | String                                                                   | Only return event definitions which don’t have the given category.                                                                                                        |
| deploymentId      | No       | String                                                                   | Only return event definitions which are part of a deployment with the given identifier.                                                                                   |
| latest            | No       | Boolean                                                                  | Only return the latest event definition versions. Can only be used together with 'key' and 'keyLike' parameters, using any other parameter will result in a 400-response. |
| sort              | No       | 'name' (default), 'id', 'key', 'category', 'deploymentId' and 'tenantId' | Property to sort on, to be used together with the 'order'.                                                                                                                |

| Response code | Description                                                                                                                                                                             |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200           | Indicates request was successful and the event definitions are returned                                                                                                                 |
| 400           | Indicates a parameter was passed in the wrong format or that 'latest' is used with other parameters other than 'key' and 'keyLike'. The status-message contains additional information. |

**Success response body:**

```
{
  "data": [
        {
      "id": "46b0379c-c0a1-11e6-bc93-6ab56fad108a",
      "url": "http://localhost:8080/flowable-rest/event-registry-api/event-registry-repository/event-definitions/46b0379c-c0a1-11e6-bc93-6ab56fad108a",
      "category": null,
      "name": "Event Definition One",
      "key": "EventDefinitionOne",
      "description": null,
      "version": 3,
      "resourceName": "EventDefinitionOne.event",
      "deploymentId": "46aa6b3a-c0a1-11e6-bc93-6ab56fad108a",
      "parentDeploymentId": "5001",
      "tenantId": ""
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### Get an event definition <a href="#get-an-event-definition" id="get-an-event-definition"></a>

```
GET event-registry-repository/event-definitions/{eventDefinitionId}
```

| Parameter         | Required | Value  | Description                                    |
| ----------------- | -------- | ------ | ---------------------------------------------- |
| eventDefinitionId | Yes      | String | The identifier of the event definition to get. |

| Response code | Description                                             |
| ------------- | ------------------------------------------------------- |
| 200           | Indicates the event definition was found and returned.  |
| 404           | Indicates the requested event definition was not found. |

**Success response body:**

```
{
  "id": "46b0379c-c0a1-11e6-bc93-6ab56fad108a",
  "url": "http://localhost:8080/flowable-rest/event-registry-api/event-registry-repository/event-definitions/46b0379c-c0a1-11e6-bc93-6ab56fad108a",
  "category": null,
  "name": "Event Definition One",
  "key": "EventDefinitionOne",
  "description": null,
  "version": 3,
  "resourceName": "EventDefinitionOne.event",
  "deploymentId": "46aa6b3a-c0a1-11e6-bc93-6ab56fad108a",
  "parentDeploymentId": "5001",
  "tenantId": ""
}
```

#### Get an event definition resource content <a href="#get-an-event-definition-resource-content" id="get-an-event-definition-resource-content"></a>

```
GET event-registry-repository/event-definitions/{eventDefinitionId}/resourcedata
```

| Parameter         | Required | Value  | Description                                                          |
| ----------------- | -------- | ------ | -------------------------------------------------------------------- |
| eventDefinitionId | Yes      | String | The identifier of the event definition to get the resource data for. |

**Response:**

Exactly the same response codes as GET event-registry-repository/deployments/{deploymentId}/resourcedata/{resourceId}.

#### Get an event definition model <a href="#get-an-event-definition-model" id="get-an-event-definition-model"></a>

```
GET event-registry-repository/event-definitions/{eventDefinitionId}/model
```

| Parameter         | Required | Value  | Description                                                  |
| ----------------- | -------- | ------ | ------------------------------------------------------------ |
| eventDefinitionId | Yes      | String | The identifier of the event definition to get the model for. |

| Response code | Description                                                         |
| ------------- | ------------------------------------------------------------------- |
| 200           | Indicates the event definition was found and the model is returned. |
| 404           | Indicates the requested event definition was not found.             |

**Response body:** The response body is a JSON representation of the org.flowable.eventregistry.model.EventModel.

```
{
   "key": "myEvent",
   "name": "My event",
   "correlationParameters": [
      {
        "name": "customerId",
        "type": "string"
      }
   ],
   "payload": [
      {
        "name": "payload1",
        "type": "string"
      },
      {
        "name": "payload2",
        "type": "integer"
      }
   ]
}
```

### Channel Definitions <a href="#channel-definitions" id="channel-definitions"></a>

#### List of channel definitions <a href="#list-of-channel-definitions" id="list-of-channel-definitions"></a>

```
GET event-registry-repository/channel-definitions
```

| Parameter         | Required | Value                                                                    | Description                                                                                                                                                                 |
| ----------------- | -------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version           | No       | integer                                                                  | Only return channel definitions with the given version.                                                                                                                     |
| name              | No       | String                                                                   | Only return channel definitions with the given name.                                                                                                                        |
| nameLike          | No       | String                                                                   | Only return channel definitions with a name like the given name.                                                                                                            |
| key               | No       | String                                                                   | Only return channel definitions with the given key.                                                                                                                         |
| keyLike           | No       | String                                                                   | Only return channel definitions with a key like the given key.                                                                                                              |
| resourceName      | No       | String                                                                   | Only return channel definitions with the given resource name.                                                                                                               |
| resourceNameLike  | No       | String                                                                   | Only return channel definitions with a name like the given resource name.                                                                                                   |
| category          | No       | String                                                                   | Only return channel definitions with the given category.                                                                                                                    |
| categoryLike      | No       | String                                                                   | Only return channel definitions with a category like the given name.                                                                                                        |
| categoryNotEquals | No       | String                                                                   | Only return channel definitions which don’t have the given category.                                                                                                        |
| deploymentId      | No       | String                                                                   | Only return channel definitions which are part of a deployment with the given identifier.                                                                                   |
| latest            | No       | Boolean                                                                  | Only return the latest channel definition versions. Can only be used together with 'key' and 'keyLike' parameters, using any other parameter will result in a 400-response. |
| sort              | No       | 'name' (default), 'id', 'key', 'category', 'deploymentId' and 'tenantId' | Property to sort on, to be used together with the 'order'.                                                                                                                  |

| Response code | Description                                                                                                                                                                             |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200           | Indicates request was successful and the channel definitions are returned                                                                                                               |
| 400           | Indicates a parameter was passed in the wrong format or that 'latest' is used with other parameters other than 'key' and 'keyLike'. The status-message contains additional information. |

**Success response body:**

```
{
  "data": [
        {
      "id": "46b0379c-c0a1-11e6-bc93-6ab56fad108a",
      "url": "http://localhost:8080/flowable-rest/event-registry-api/event-registry-repository/channel-definitions/46b0379c-c0a1-11e6-bc93-6ab56fad108a",
      "category": null,
      "name": "Channel Definition One",
      "key": "ChannelDefinitionOne",
      "description": null,
      "version": 3,
      "resourceName": "ChannelDefinitionOne.channel",
      "deploymentId": "46aa6b3a-c0a1-11e6-bc93-6ab56fad108a",
      "parentDeploymentId": "5001",
      "tenantId": ""
    }
  ],
  "total": 1,
  "start": 0,
  "sort": "name",
  "order": "asc",
  "size": 1
}
```

#### Get a channel definition <a href="#get-a-channel-definition" id="get-a-channel-definition"></a>

```
GET event-registry-repository/channel-definitions/{channelDefinitionId}
```

| Parameter           | Required | Value  | Description                                      |
| ------------------- | -------- | ------ | ------------------------------------------------ |
| channelDefinitionId | Yes      | String | The identifier of the channel definition to get. |

| Response code | Description                                               |
| ------------- | --------------------------------------------------------- |
| 200           | Indicates the channel definition was found and returned.  |
| 404           | Indicates the requested channel definition was not found. |

**Success response body:**

```
{
  "id": "46b0379c-c0a1-11e6-bc93-6ab56fad108a",
  "url": "http://localhost:8080/flowable-rest/event-registry-api/event-registry-repository/channel-definitions/46b0379c-c0a1-11e6-bc93-6ab56fad108a",
  "category": null,
  "name": "Channel Definition One",
  "key": "ChannelDefinitionOne",
  "description": null,
  "version": 3,
  "resourceName": "ChannelDefinitionOne.channel",
  "deploymentId": "46aa6b3a-c0a1-11e6-bc93-6ab56fad108a",
  "parentDeploymentId": "5001",
  "tenantId": ""
}
```

#### Get a channel definition resource content <a href="#get-a-channel-definition-resource-content" id="get-a-channel-definition-resource-content"></a>

```
GET event-registry-repository/channel-definitions/{channelDefinitionId}/resourcedata
```

| Parameter           | Required | Value  | Description                                                            |
| ------------------- | -------- | ------ | ---------------------------------------------------------------------- |
| channelDefinitionId | Yes      | String | The identifier of the channel definition to get the resource data for. |

**Response:**

Exactly the same response codes as GET event-registry-repository/deployments/{deploymentId}/resourcedata/{resourceId}.

#### Get a channel definition model <a href="#get-a-channel-definition-model" id="get-a-channel-definition-model"></a>

```
GET event-registry-repository/channel-definitions/{channelDefinitionId}/model
```

| Parameter           | Required | Value  | Description                                                    |
| ------------------- | -------- | ------ | -------------------------------------------------------------- |
| channelDefinitionId | Yes      | String | The identifier of the channel definition to get the model for. |

| Response code | Description                                                           |
| ------------- | --------------------------------------------------------------------- |
| 200           | Indicates the channel definition was found and the model is returned. |
| 404           | Indicates the requested channel definition was not found.             |

**Response body:** The response body is a JSON representation of the org.flowable.eventregistry.model.ChannelModel.

```
{
   "key": "myChannel",
   "category": "channel",
   "name": "My channel",
   "description": "My channel description",
   "channelType": "inbound",
   "type": "jms",
   "destination": "testQueue",
   "deserializerType": "json",
   "channelEventKeyDetection": {
      "fixedValue": "myEvent"
   }
}
```

### Event Registry Service <a href="#event-registry-service" id="event-registry-service"></a>

#### Send an event <a href="#send-an-event" id="send-an-event"></a>

```
POST event-registry-runtime/event-instances
```

**Request body:**

{ "eventDefinitionKey": "myEvent", "eventPayload": { "customerName": "John Doe", "productName": "Laptop" }, "tenantId": "tenant1" }

Instead of an event definition key it's also possible to use an event definition id.

There's no response body when sending an event.

| Response code | Description                        |
| ------------- | ---------------------------------- |
| 201           | Indicates the event has been sent. |

### Engine <a href="#engine" id="engine"></a>

#### Get Event Registry engine info <a href="#get-event-registry-engine-info" id="get-event-registry-engine-info"></a>

```
GET event-registry-management/engine
```

Returns a read-only view of the Event Registry engine that is used in this REST-service.

**Success response body:**

```
{
   "name":"default",
   "version":"6.6.0"
}
```

| Response code | Description                                         |
| ------------- | --------------------------------------------------- |
| 200           | Indicates the engine information has been returned. |
