# Management and Monitoring REST API

This section contains a detailed description of the REST API available for administration and monitoring Payara Server domains. This documentation is based on the [OpenAPI specification](https://github.com/OAI/OpenAPI-Specification)(formerly known as Swagger v2.0).

### Overview

This API allows users to interact with the Payara Server’s DAS through a REST interface. It allows execution of administration commands in a similar way as the `asadmin` utility, such as:

* Execute administration commands to modify the domain’s configuration.
* Retrieve monitoring statistics
* Look at the domain’s log entries.

To change the configuration of the domain, it is really necessary to understand how the configuration resources are organized in a tree model. To interact with a resource, it is recommended to first call the `GET` method of the `/management/domain/{resource}` endpoint to understand what operations are supported and other details (parameters, subcommands, etc.)

To execute the API calls to the server, consider the following URL as an example:

```
http(s)://localhost:4848/management/domain
```

|   | Remember to change the administration port (usually **4848** by default on most domain configurations). |
| - | ------------------------------------------------------------------------------------------------------- |

In order to successfully consume the endpoints in this API, it must be taken into consideration if the domain’s access is already secured. If so, then one of the _2_ described security schemes must be used.

### Resources

Configuration, Logging and Session resources are discussed here.

#### Configuration

Used for domain configuration operations

**Add or Update a resource**

```
POST /management/domain/{resource}
```

**Description**

Use this operation to add new resources to the configuration tree or to update the configuration of an existing resource. If the resource is parent resource, then the operation would add a new child resource. Otherwise, the operation would update the resource’s configuration.

**Parameters**

| Type         | Name                          | Description                                                                                                                                                                           | Schema |
| ------------ | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| **Header**   | **X-Requested-By** _required_ | This header must ALWAYS be set to the value _**GlassFish REST HTML interface**_                                                                                                       | string |
| **Path**     | **resource** _required_       | The qualified name of the resource. If adding a child resource, the name of the resource’s parent. If updating a resource’s configuration then it is the name of the target resource. | string |
| **FormData** | **parameter** _optional_      | The name _parameter_ is a placeholder for any parameters the resource uses for its creation or update. If multiple parameters are used, repeat this placeholder for each one.         | string |

**Responses**

| HTTP Code | Description                                                                                                                                                                                                                                                                                                                                                                                                         | Schema                                                                                                                                                                                           |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **200**   | The operation has executed successfully and contextual information about the result is returned.                                                                                                                                                                                                                                                                                                                    | [OperationResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#operationresult) |
| **400**   | <p>The operation has failed. One of the possible reasons are:</p><ul><li>The command or method`s request parameters aren`t set correctly.</li><li>The <code>X-Requested-By</code> header was not set correctly.</li><li>An integrity error has occurred with the target resource, for example a resource already exists with the same JNDI name. (An operation result object is returned for examination)</li></ul> | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult)     |
| **401**   | Users credentials are not valid to execute this operation                                                                                                                                                                                                                                                                                                                                                           | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult)     |
| **404**   | Either the parent resource (when adding new resources) or the target resource (when updating a resource) does not exists.                                                                                                                                                                                                                                                                                           | No Content                                                                                                                                                                                       |

**Produces**

* `application/json`
* `application/xml`
* `text/html`

**Security**

| Type     | Name                                                                                                                                                                                        |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `basic`  | [simple\_auth](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#simple\_auth)   |
| `apiKey` | [token\_cookie](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#token\_cookie) |

**Get information for a resource**

```
GET /management/domain/{resource}
```

**Description**

Use this operation to determine the current configuration of a resource and what are its supported methods, method parameters and available commands.

**Parameters**

| Type   | Name                    | Description                                                                                                                                                                                                                             | Schema |
| ------ | ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| `Path` | **resource** _required_ | The qualified name of the resource on the configuration tree. Input this parameter using **dotted** path notation, e.g. if the object’s name is `nodes.node.localhost-domain1` then the correct value is `nodes/node/localhost-domain1` | string |

**Responses**

| HTTP Code | Description                                                        | Schema                                                                                                                                                                                           |
| --------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **200**   | An object representing the contextual information for the resource | [OperationResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#operationresult) |
| **401**   | Users credentials are not valid to execute this operation          | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult)     |
| **404**   | The specified resource does not exist                              | No Content                                                                                                                                                                                       |

**Produces**

* `application/json`
* `application/xml`
* `text/html`

**Security**

| Type     | Name                                                                                                                                                                                        |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `basic`  | [simple\_auth](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#simple-auth)    |
| `apiKey` | [token\_cookie](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#token\_cookie) |

**Delete a resource**

```
DELETE /management/domain/{resource}
```

**Description**

Removes the specified resource from the configuration’s tree. The deleted resource will no longer be present in the _child-resources_ list of the parent’s resource.

**Parameters**

| Type     | Name                          | Description                                                                    | Schema |
| -------- | ----------------------------- | ------------------------------------------------------------------------------ | ------ |
| `Header` | **X-Requested-By** _required_ | This header must ALWAYS be se to the value _**GlassFish REST HTML interface**_ | string |
| `Path`   | **resource** _required_       | The qualified name of the resource.                                            | string |

**Responses**

| HTTP Code | Description                                                                                      | Schema                                                                                                                                                                                           |
| --------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **200**   | The operation has executed successfully and contextual information about the result is returned. | [OperationResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#operationresult) |
| **400**   | The operation has failed. The `X-Requested-By` header was not set correctly.                     | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult)     |
| **401**   | Users credentials are not valid to execute this operation                                        | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult)     |
| **404**   | The target resource does not exists.                                                             | No Content                                                                                                                                                                                       |

**Produces**

* `application/json`
* `application/xml`
* `text/html`

**Security**

| Type     | Name                                                                                                                                                                                        |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `basic`  | [simple\_auth](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#simple-auth)    |
| `apiKey` | [token\_cookie](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#token\_cookie) |

#### Logging

Used for domain logging purposes

**Get domain log**

```
GET /management/domain/view-log
```

**Description**

Get the server’s log contents of the target domain. Each time this operation is executed, the response will include the `X-Text-Append-Next` header to retrieve changes made to the log since the operation was called.

**Parameters**

| Type    | Name                 | Description                                                                   | Schema          |
| ------- | -------------------- | ----------------------------------------------------------------------------- | --------------- |
| `Query` | **start** _optional_ | Use this parameter to skip a determined number of characters in the log file. | number(integer) |

**Responses**

| HTTP Code | Description                                                                                                                                                                                      | Schema                                                                                                                                                                                       |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **200**   | The contents of the domain’s log as requested. **Headers** : `X-Text-Append-Next` (string(url)) : An URL that can be used to retrieve the changes made to this log after this call was executed. | No Content                                                                                                                                                                                   |
| **401**   | Users credentials are not valid to execute this operation                                                                                                                                        | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult) |

**Produces**

* `text/plain`

**Get log entries**

```
GET /management/view-log/details
```

**Description**

Get the server’s log details of the target domain. The details can be retrieved on both JSON or XML formats. If the 'Accept' header is omitted, the default format is XML.

**Responses**

| HTTP Code | Description                                               | Schema                                                                                                                                                                                       |
| --------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **200**   | An array of the log records retrieved.                    | <[LogRecord](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#logrecord)> array |
| **401**   | Users credentials are not valid to execute this operation | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult) |

**Produces**

* `application/json`
* `application/xml`

#### Monitoring

Used for monitoring operations

**Get monitoring statistics**

```
GET /monitoring/domain/{resource}
```

**Description**

Retrieves the monitoring statistics for a monitorable resource in the domain. In order to retrieve the monitoring statistics for a specific resource, you must configure the monitoring level of the specific category the resource falls into (JVM, Connection Pools, ORB, etc.) first.

**Parameters**

| Type   | Name                    | Description                                                                                                                                                                                                                                                         | Schema |
| ------ | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| `Path` | **resource** _required_ | The name of the monitored resource under the special monitoring tree structure used for Payara. Input this parameter using **dotted** path notation, e.g. if the resource’s name is `server.jvm.class-loading` then the correct value is `server/jvm/class-loading` | string |

**Responses**

| HTTP Code | Description                                                                                                                                                                                                                  | Schema                                                                                                                                                                                             |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **200**   | An object representing the contextual information for the resource, with the monitoring statistics if a child resource. If the resource is parent resource, then the child resources that can be monitored will be returned. | [MonitoringResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#monitoringresult) |
| **401**   | Users credentials are not valid to execute this operation                                                                                                                                                                    | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult)       |
| **404**   | Either the resource does not exist or the category (parent resource) of the resource has not been configured for being monitored yet.                                                                                        | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult)       |

**Produces**

* `application/json`
* `application/xml`
* `text/html`

**Security**

| Type     | Name                                                                                                                                                                                        |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `basic`  | [simple\_auth](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#simple-auth)    |
| `apiKey` | [token\_cookie](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#token\_cookie) |

#### Sessions

Used for session management

**Get session token**

```
POST /management/sessions
```

**Description**

Use this method for retrieving a special session token that can be used to authenticate an user when executing administration or monitoring operations.

**Parameters**

| Type     | Name                          | Description                                                                     | Schema |
| -------- | ----------------------------- | ------------------------------------------------------------------------------- | ------ |
| `Header` | **X-Requested-By** _required_ | This header must ALWAYS be set to the value _**GlassFish REST HTML interface**_ | string |

**Responses**

| HTTP Code | Description                                                                  | Schema                                                                                                                                                                                       |
| --------- | ---------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **200**   | Returns the session token successfully                                       | [TokenResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#tokenresult)     |
| **400**   | The operation has failed. The `X-Requested-By` header was not set correctly. | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult) |
| **401**   | Users credentials are not valid to execute this operation                    | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult) |

**Produces**

* `application/json`
* `application/xml`
* `text/html`

**Security**

| Type    | Name                                                                                                                                                                                     |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `basic` | [simple\_auth](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#simple-auth) |

**Delete session token**

```
DELETE /management/sessions/{token}
```

**Description**

Retires a session token so that users can no longer do requests that authenticate with it.

**Parameters**

| Type     | Name                          | Description                                                                     | Schema |
| -------- | ----------------------------- | ------------------------------------------------------------------------------- | ------ |
| `Header` | **X-Requested-By** _required_ | This header must ALWAYS be set to the value _**GlassFish REST HTML interface**_ | string |
| `Path`   | **token** _required_          | The session token to delete                                                     | string |

**Responses**

| HTTP Code | Description                                                                                                              | Schema                                                                                                                                                                                       |
| --------- | ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **200**   | The session token was deleted successfully.                                                                              | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult) |
| **400**   | The operation has failed. Either the `X-Requested-By` header was not set correctly or the supplied token does not exist. | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult) |
| **401**   | Users credentials are not valid to execute this operation                                                                | [GeneralResult](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Definitions.html#generalresult) |

**Produces**

* `application/json`
* `application/xml`
* `text/html`

**Security**

| Type    | Name                                                                                                                                                                                     |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `basic` | [simple\_auth](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Management%20and%20Monitoring%20REST%20API/Rest%20API.html#simple-auth) |
