# REST API

### General REST principles

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

Parameters that are part of the URL (for example, the deploymentId parameter in [http://host/flowable-rest/dmn-api/dmn-repository/deployments/](http://host/flowable-rest/dmn-api/dmn-repository/deployments/){deploymentId}) need to be properly escaped (see [URL-encoding or Percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding)) in case the segment contains special characters. Most frameworks have this functionality built in, but it should be taken into account. Especially for segments that can contain forward-slashes (for example, deployment resource), this is required.

**Rest URL query parameters**

Parameters added as query-string in the URL (for example, the name parameter used in [http://host/flowable-rest/dmn-api/dmn-repository/deployments?name=Deployment](http://host/flowable-rest/dmn-api/dmn-repository/deployments?name=Deployment)) can have the following types and are mentioned in the corresponding REST-API documentation:

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

Paging and order parameters can be added as query-string in the URL (for example, the name parameter used in [http://host/flowable-rest/dmn-api/dmn-repository/deployments?sort=name](http://host/flowable-rest/dmn-api/dmn-repository/deployments?sort=name)).

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

#### JSON query variable format <a href="#json-query-variable-format" id="json-query-variable-format"></a>

```
{
  "name" : "variableName",
  "value" : "variableValue",
  "operation" : "equals",
  "type" : "string"
}
```

| Parameter | Required | Description                                                                                                                                                                                                                                                                                                                                                            |
| --------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name      | No       | Name of the variable to include in a query. Can be empty in the case where 'equals' is used in some queries to query for resources that have **any variable name** with the given value.                                                                                                                                                                               |
| value     | Yes      | Value of the variable included in the query, should include a correct format for the given type.                                                                                                                                                                                                                                                                       |
| operator  | Yes      | Operator to use in query, can have the following values: equals, notEquals, equalsIgnoreCase, notEqualsIgnoreCase, lessThan, greaterThan, lessThanOrEquals, greaterThanOrEquals and like.                                                                                                                                                                              |
| type      | No       | Type of variable to use. When omitted, the type will be deduced from the value parameter. Any JSON text-values will be considered of type string, JSON booleans of type boolean, JSON numbers of type long or integer depending on the size of the number. We recommended you include an explicit type when in doubt. Types supported out of the box are listed below. |

| Type name | Description                                                                                                          |
| --------- | -------------------------------------------------------------------------------------------------------------------- |
| string    | Value is treated as and converted to a java.lang.String.                                                             |
| short     | Value is treated as and converted to a java.lang.Integer.                                                            |
| integer   | Value is treated as and converted to a java.lang.Integer.                                                            |
| long      | Value is treated as and converted to a java.lang.Long.                                                               |
| double    | Value is treated as and converted to a java.lang.Double.                                                             |
| boolean   | Value is treated as and converted to a java.lang.Boolean.                                                            |
| date      | Value is treated as and converted to a java.util.Date. The JSON string will be converted using ISO-8601 date format. |

**Variable representation**

When working with variables (execute decision), the REST API uses some common principles and JSON-format for both reading and writing. The JSON representation of a variable looks like this:

```
{
  "name" : "variableName",
  "value" : "variableValue",
  "valueUrl" : "http://...",
  "type" : "string"
}
```

| Parameter | Required | Description                                                                                                                                                                                                                                                                                                                                                    |
| --------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name      | Yes      | Name of the variable.                                                                                                                                                                                                                                                                                                                                          |
| value     | No       | Value of the variable. When writing a variable and value is omitted, null will be used as value.                                                                                                                                                                                                                                                               |
| valueUrl  | No       | When reading a variable of type binary or serializable, this attribute will point to the URL from where the raw binary data can be fetched.                                                                                                                                                                                                                    |
| type      | No       | Type of the variable. See table below for additional information on types. When writing a variable and this value is omitted, the type will be deduced from the raw JSON-attribute request type and is limited to either string, double, integer and boolean. We advise you to always include a type to make sure no wrong assumption about the type are made. |

| Type name | Description                                                                                                                       |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- |
| string    | Value is treated as a java.lang.String. Raw JSON-text value is used when writing a variable.                                      |
| integer   | Value is treated as a java.lang.Integer. When writing, JSON number value is used as base for conversion, falls back to JSON text. |
| short     | Value is treated as a java.lang.Short. When writing, JSON number value is used as base for conversion, falls back to JSON text.   |
| long      | Value is treated as a java.lang.Long. When writing, JSON number value is used as base for conversion, falls back to JSON text.    |
| double    | Value is treated as a java.lang.Double. When writing, JSON number value is used as base for conversion, falls back to JSON text.  |
| boolean   | Value is treated as a java.lang.Boolean. When writing, JSON boolean value is used for conversion.                                 |
| date      | Value is treated as a java.util.Date. When writing, the JSON text will be converted using ISO-8601 date format.                   |

It’s possible to support additional variable-types with a custom JSON representation (either simple value or complex/nested JSON object). By extending the initializeVariableConverters() method on org.flowable.dmn.rest.service.api.DmnRestResponseFactory, you can add additional org.flowable.rest.variable.RestVariableConverter classes to support converting your POJOs to a format suitable for transferring through REST and converting the REST-value back to your POJO. The actual transformation to JSON is done by Jackson.

### Deployment <a href="#deployment" id="deployment"></a>

**When using Tomcat, please read** [**Usage in Tomcat**](https://www.flowable.com/open-source/docs/dmn/ch07-REST#usage-in-tomcat)**.**

#### List of DMN Deployments <a href="#list-of-dmn-deployments" id="list-of-dmn-deployments"></a>

```
GET dmn-repository/deployments
```

| Parameter         | Required | Value                                              | Description                                                                                                   |
| ----------------- | -------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| name              | No       | String                                             | Only return deployments with the given name.                                                                  |
| nameLike          | No       | String                                             | Only return deployments with a name like the given name.                                                      |
| category          | No       | String                                             | Only return deployments with the given category.                                                              |
| categoryNotEquals | No       | String                                             | Only return deployments which don’t have the given category.                                                  |
| tenantId          | No       | String                                             | Only return deployments with the given tenantId.                                                              |
| tenantIdLike      | No       | String                                             | Only return deployments with a tenantId like the given value.                                                 |
| withoutTenantId   | No       | Boolean                                            | If true, only returns deployments without a tenantId set. If false, the withoutTenantId parameter is ignored. |
| sort              | No       | 'id' (default), 'name', 'deploytime' or 'tenantId' | Property to sort on, to be used together with the 'order'.                                                    |

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
      "url": "http://localhost:8080/flowable-rest/dmn-api/dmn-repository/deployments/03ab310d-c1de-11e6-a4f4-62ce84ef239e",
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

#### Get a DMN deployment <a href="#get-a-dmn-deployment" id="get-a-dmn-deployment"></a>

```
GET dmn-repository/deployments/{deploymentId}
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
  "url": "http://localhost:8080/flowable-rest/dmn-api/dmn-repository/deployments/03ab310d-c1de-11e6-a4f4-62ce84ef239e",
  "parentDeploymentId": "17510",
  "tenantId": ""
}
```

#### Create a new DMN deployment <a href="#create-a-new-dmn-deployment" id="create-a-new-dmn-deployment"></a>

```
POST dmn-repository/deployments
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
  "url": "http://localhost:8080/flowable-rest/dmn-api/dmn-repository/deployments/03ab310d-c1de-11e6-a4f4-62ce84ef239e",
  "tenantId" : "myTenant"
}
```

#### Delete a DMN deployment <a href="#delete-a-dmn-deployment" id="delete-a-dmn-deployment"></a>

```
DELETE dmn-repository/deployments/{deploymentId}
```

| Parameter    | Required | Value  | Description                                 |
| ------------ | -------- | ------ | ------------------------------------------- |
| deploymentId | Yes      | String | The identifier of the deployment to delete. |

| Response code | Description                                                                                    |
| ------------- | ---------------------------------------------------------------------------------------------- |
| 204           | Indicates the deployment was found and has been deleted. Response-body is intentionally empty. |
| 404           | Indicates the requested deployment was not found.                                              |

#### Get a DMN deployment resource content <a href="#get-a-dmn-deployment-resource-content" id="get-a-dmn-deployment-resource-content"></a>

```
GET dmn-repository/deployments/{deploymentId}/resourcedata/{resourceId}
```

| Parameter    | Required | Value  | Description                                                                                                                                                                                                                 |
| ------------ | -------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| deploymentId | Yes      | String | The identifier of the deployment the requested resource is part of.                                                                                                                                                         |
| resourceId   | Yes      | String | The identifier of the resource to get the data for. **Make sure you URL-encode the resourceId in case it contains forward slashes. For example, use 'decisions%2Fmy-decision.dmn' instead of 'decisions/my-decision.dmn'.** |

| Response code | Description                                                                                                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200           | Indicates both deployment and resource have been found and the resource data has been returned.                                                                                       |
| 404           | Indicates the requested deployment was not found or there is no resource with the given identifier present in the deployment. The status-description contains additional information. |

**Success response body:**

The response body will contain the binary resource-content for the requested resource. The response content-type will be the same as the type returned in the resources 'mimeType' property. Also, a content-disposition header is set, allowing browsers to download the file instead of displaying it.

### Decision Tables <a href="#decision-tables" id="decision-tables"></a>

#### List of decision tables <a href="#list-of-decision-tables" id="list-of-decision-tables"></a>

```
GET dmn-repository/decision-tables
```

| Parameter         | Required | Value                                                                   | Description                                                                                                                                                                 |
| ----------------- | -------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version           | No       | integer                                                                 | Only return process definitions with the given version.                                                                                                                     |
| name              | No       | String                                                                  | Only return process definitions with the given name.                                                                                                                        |
| nameLike          | No       | String                                                                  | Only return process definitions with a name like the given name.                                                                                                            |
| key               | No       | String                                                                  | Only return process definitions with the given key.                                                                                                                         |
| keyLike           | No       | String                                                                  | Only return process definitions with a name like the given key.                                                                                                             |
| resourceName      | No       | String                                                                  | Only return process definitions with the given resource name.                                                                                                               |
| resourceNameLike  | No       | String                                                                  | Only return process definitions with a name like the given resource name.                                                                                                   |
| category          | No       | String                                                                  | Only return process definitions with the given category.                                                                                                                    |
| categoryLike      | No       | String                                                                  | Only return process definitions with a category like the given name.                                                                                                        |
| categoryNotEquals | No       | String                                                                  | Only return process definitions which don’t have the given category.                                                                                                        |
| deploymentId      | No       | String                                                                  | Only return process definitions which are part of a deployment with the given identifier.                                                                                   |
| latest            | No       | Boolean                                                                 | Only return the latest process definition versions. Can only be used together with 'key' and 'keyLike' parameters, using any other parameter will result in a 400-response. |
| sort              | No       | 'name' (default), 'id', 'key', 'category', 'deploymentId' and 'version' | Property to sort on, to be used together with the 'order'.                                                                                                                  |

| Response code | Description                                                                                                                                                                             |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200           | Indicates request was successful and the decision tables are returned                                                                                                                   |
| 400           | Indicates a parameter was passed in the wrong format or that 'latest' is used with other parameters other than 'key' and 'keyLike'. The status-message contains additional information. |

**Success response body:**

```
{
  "data": [
        {
      "id": "46b0379c-c0a1-11e6-bc93-6ab56fad108a",
      "url": "http://localhost:8080/flowable-rest/dmn-api/dmn-repository/decision-tables/46b0379c-c0a1-11e6-bc93-6ab56fad108a",
      "category": null,
      "name": "Decision Table One",
      "key": "DecisionTableOne",
      "description": null,
      "version": 3,
      "resourceName": "dmn-DecisionTableOne.dmn",
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

#### Get a decision table <a href="#get-a-decision-table" id="get-a-decision-table"></a>

```
GET dmn-repository/decision-tables/{decisionTableId}
```

| Parameter       | Required | Value  | Description                                  |
| --------------- | -------- | ------ | -------------------------------------------- |
| decisionTableId | Yes      | String | The identifier of the decision table to get. |

| Response code | Description                                           |
| ------------- | ----------------------------------------------------- |
| 200           | Indicates the decision table was found and returned.  |
| 404           | Indicates the requested decision table was not found. |

**Success response body:**

```
{
  "id": "46b0379c-c0a1-11e6-bc93-6ab56fad108a",
  "url": "http://localhost:8080/flowable-rest/dmn-api/dmn-repository/decision-tables/46b0379c-c0a1-11e6-bc93-6ab56fad108a",
  "category": null,
  "name": "Decision Table One",
  "key": "DecisionTableOne",
  "description": null,
  "version": 3,
  "resourceName": "dmn-DecisionTableOne.dmn",
  "deploymentId": "46aa6b3a-c0a1-11e6-bc93-6ab56fad108a",
  "parentDeploymentId": "5001",
  "tenantId": ""
}
```

#### Get a decision table resource content <a href="#get-a-decision-table-resource-content" id="get-a-decision-table-resource-content"></a>

```
GET dmn-repository/decision-tables/{decisionTableId}/resourcedata
```

| Parameter       | Required | Value  | Description                                                        |
| --------------- | -------- | ------ | ------------------------------------------------------------------ |
| decisionTableId | Yes      | String | The identifier of the decision table to get the resource data for. |

**Response:**

Exactly the same response codes/boy as GET dmn-repository/deployment/{deploymentId}/resourcedata/{resourceId}.

#### Get a decision table DMN model <a href="#get-a-decision-table-dmn-model" id="get-a-decision-table-dmn-model"></a>

```
GET dmn-repository/decision-tables/{decisionTableId}/model
```

| Parameter       | Required | Value  | Description                                                |
| --------------- | -------- | ------ | ---------------------------------------------------------- |
| decisionTableId | Yes      | String | The identifier of the decision table to get the model for. |

| Response code | Description                                                       |
| ------------- | ----------------------------------------------------------------- |
| 200           | Indicates the decision table was found and the model is returned. |
| 404           | Indicates the requested decision table was not found.             |

**Response body:** The response body is a JSON representation of the org.flowable.dmn.model.DmnDefinition and contains the full DMN definition model.

```
{
   "processes":[
      {
         "id":"oneTaskProcess",
         "xmlRowNumber":7,
         "xmlColumnNumber":60,
         "extensionElements":{

         },
         "name":"The One Task Process",
         "executable":true,
         "documentation":"One task process description",

    ]
}
```

### DMN Rule Service <a href="#dmn-rule-service" id="dmn-rule-service"></a>

#### Execute a decision <a href="#execute-a-decision" id="execute-a-decision"></a>

```
POST dmn-rule/execute
```

**Request body:**

The request body should contain data of type _multipart/form-data_. The decisionKey is required. The tenantId, parentDeploymentId and a map of inputVariables (restVariables) are optional.

**Response body:**

```
{
  "resultVariables": [
    [
      {
        "name": "output1",
        "type": "string",
        "value": "result 1"
      }
    ],
    [
      {
        "name": "output1",
        "type": "string",
        "value": "result 2"
      }
    ],
    [
      {
        "name": "output1",
        "type": "string",
        "value": "result 3"
      }
    ]
  ],
  "url": "http://localhost:8080/flowable-rest/dmn-api/dmn-rule/execute"
}
```

| Response code | Description                               |
| ------------- | ----------------------------------------- |
| 201           | Indicates the Decision has been executed. |

#### Execute a single result decision <a href="#execute-a-single-result-decision" id="execute-a-single-result-decision"></a>

```
POST dmn-rule/execute/single-result
```

**Request body:**

The request body should contain data of type _multipart/form-data_. The decisionKey is required. The tenantId, parentDeploymentId and a map of inputVariables (restVariables) are optional.

When multiple rules are valid the service will respond with a 500 code. **Note: a single hit with a compound output is valid (see response below)**

**Response body:**

```
{
  "resultVariables": [
    {
      "name": "output1",
      "type": "string",
      "value": "compound 1"
    },
    {
      "name": "output2",
      "type": "string",
      "value": "compound 2"
    }
  ],
  "url": "http://localhost:8080/flowable-rest/dmn-api/dmn-rule/execute/single-result"
}
```

| Response code | Description                                       |
| ------------- | ------------------------------------------------- |
| 201           | Indicates the Decision has been executed.         |
| 500           | Indicates the Decision returned multiple results. |

### Engine <a href="#engine" id="engine"></a>

#### Get DMN engine info <a href="#get-dmn-engine-info" id="get-dmn-engine-info"></a>

```
GET dmn-management/engine
```

Returns a read-only view of the DMN engine that is used in this REST-service.

**Success response body:**

```
{
   "name":"default",
   "version":"6.6.0",
   "resourceUrl":"file://flowable-dmn/flowable.dmn.cfg.xml",
   "exception":null
}
```

| Response code | Description                                         |
| ------------- | --------------------------------------------------- |
| 200           | Indicates the engine information has been returned. |
