# REST Service

The REST Service origin is a multithreaded origin that processes all authorized REST API requests. The origin can also send records with status codes back to the originating REST API client when used with one or more Send Response to Origin destinations in the same pipeline.

By default, the REST Service origin listens at an HTTP endpoint for requests. You can configure the origin to use Data Pipelines as an API gateway, which enables using the Data Pipelines URL as an API endpoint.

Use the REST Service origin to create a REST-based microservice. In a microservice pipeline, the REST Service origin works with one or more microservice destinations that specify the HTTP response code to pass back to the originating REST API client.

The REST Service origin generates a batch for each request that it receives. The REST Service origin can use multiple threads to enable parallel processing of requests from multiple clients. The origin can also read requests containing messages with no compression or with the Gzip or Snappy compression format.

When using the REST Service origin, you must configure the REST API clients to send data to the pipeline. The required client configuration depends on how you configure the origin.

When you configure the REST Service origin, you decide if you want to use a listening port or an API gateway to receive requests. To use a listening port, you specify the HTTP listening port. To use an API gateway, you specify the service name and whether to require gateway authentication.

When not using gateway authentication, you can enable Kerberos authentication using SPNEGO, or SSL/TLS with basic authentication.

For additional security, you can require each request to include a valid application ID by listing valid application IDs in the origin. And you can indicate if the application ID will be in the URL instead of the request header.

You can configure the maximum number of concurrent requests to determine how many threads to use. You also configure the format of generated responses.

When you start the pipeline, Data Pipelines displays the endpoint that the pipeline uses in monitor mode.

### HTTP Listening Port <a href="#ariaid-title2" id="ariaid-title2"></a>

By default, the REST Service origin listens for requests at an HTTP endpoint and processes all authorized requests. You can specify the port number that the origin listens at. By default, the origin uses port number `8000`.

The specified listening port should be used by a single pipeline, unless you use application IDs to differentiate between the requests to be processed by different pipelines.

### API Gateway <a href="#ariaid-title3" id="ariaid-title3"></a>

You can configure the origin to use Data Pipelines as an API gateway instead of listening for messages at an HTTP endpoint.

When you enable Data Pipelines as an API gateway, you specify a service name for the pipeline, which Data Pipelines uses to route requests to the pipeline. Then, you configure REST API clients to send requests to the specified service name.

#### Gateway Authentication <a href="#ariaid-title4" id="ariaid-title4"></a>

When the REST Service origin uses Data Pipelines as an API gateway, you can also configure the origin to require gateway authentication.

When you require gateway authentication, the user in each request must be an authenticated Data Pipelines user. This leverages the authentication method configured for Data Pipelines. When you enable gateway authentication, the origin ignores other user authentication options configured in stage properties.

The communication and authentication that occurs with gateway authentication depends on how Data Pipelines is configured:

*   When Data Pipelines is enabled for SSL/TLS, communication between REST API clients and the microservice pipeline use HTTPS.

    As a result, you do not need to enable SSL/TLS in the origin stage properties. If you enable SSL/TLS in the origin, that security is used only for the connection between the gateway and the origin.
*   When Data Pipelines uses LDAP or file-based authentication.

    For example, if Data Pipelines uses LDAP authentication, then the user in each API request must be authenticated by the LDAP server.
*   When Data Pipelines is registered with Control Hub, the security configured for Control Hub is applied to the pipeline.

    For example, if your Control Hub organization uses SAML authentication, then the user in each API request must be authenticated by the SAML identity provider (IdP).

#### REST API URLs <a href="#ariaid-title5" id="ariaid-title5"></a>

Data Pipelines provides a secure REST API and a public REST API. The API that you use for requests depends on whether the origin requires gateway authentication:Secure APIUse the secure REST API when the origin requires gateway authentication.To pass requests to the secure API, the user in the request must have the read permission on the pipeline and the appropriate roles to use the REST API:

* For a standalone Data Pipelines, the user must have the `admin` or `manager` role.
* For a Data Pipelines registered with Control Hub, the user must have the Engine Administrator or Engine Manager Control Hub role.
* For a Data Pipelines registered with Control Hub that uses SAML authentication, the user must also have the Organization Administrator role.

Use the following gateway URL for the secure API:

```
http://<Data Pipelines URL>/rest/v1/gateway/<service name>
```

Public APIUse the public REST API when the origin does not require gateway authentication. The public API can be accessed by any user, but to pass requests to the pipeline, the user must have read permission for the pipeline.Use the following gateway URL for the public API:

```
http://<Data Pipelines URL>/public-rest/v1/gateway/<service name>
```

For more information about constructing the URL, see REST API Client Requirement.

#### Required Request Header <a href="#ariaid-title6" id="ariaid-title6"></a>

When you use Data Pipelines as an API gateway, you must include the following header in POST, PUT, and DELETE requests:

```
"X-Requested-By:<user-defined string>"
```

This header is used by the pipeline to provide additional security. The user-defined string can be any string value.

For example, the following request passes a name to the `userService` gateway service using `client` as the requestor:

```
curl -i -X POST http://localhost:18630/public-rest/v1/gateway/userService/user 
--header "X-SDC-APPLICATION-ID:microservice" --header "X-Requested-By:client" -d ‘{"Name": "Joe"}’
```

### Security <a href="#ariaid-title7" id="ariaid-title7"></a>

The REST Service origin provides the following security options:Application IDWhen you specify a list of application IDs, the origin processes only incoming requests that include one of the specified application IDs.For more information, see Using Application IDs.

**Gateway authentication**

When the origin requires gateway authentication, the origin uses the Data Pipelines SSL/TLS properties, if configured. The origin also requires requests to use the secure REST API.

You can enable gateway authentication when you use Data Pipelines as an API gateway. For more information, see Gateway Authentication.

When you enable gateway authentication, the Kerberos with SPNEGO authentication and SSL/TLS basic authentication properties are ignored.

**Kerberos with SPNEGO**

The origin can authenticate using Kerberos with SPNEGO web authentication. You can use this option when your environment is enabled to perform Kerberos authentication over HTTP.

Kerberos with SPNEGO provides Kerberos authentication for the origin without requiring Data Pipelines to use Kerberos.

When you enable Kerberos with SPNEGO, you specify the Kerberos realm where the service is located. You also specify the Kerberos principal and the location of the keytab. The keytab must be local to Data Pipelines.

If Data Pipelines is Kerberos-enabled, you can use the same principal and keytab configured for Data Pipelines.

You can use Kerberos with SPNEGO with SSL/TLS. However, the SSL/TLS basic authentication properties are ignored. When gateway authentication is enabled, the Kerberos with SPNEGO properties are ignored.

**SSL/TLS with additional options**

When you enable SSL/TLS in the origin, you can also enable the following additional security measures:

* Client authentication - Requires mutual authentication of each REST API client before accepting requests.
*   Basic authentication - Requires incoming requests to include one of the user accounts specified in the origin.

    You can use basic authentication when other authentication methods, such as gateway or Kerberos with SPNEGO authentication, are not enabled. When other authentication methods are enabled, basic authentication properties are ignored.

### Using Application IDs <a href="#ariaid-title8" id="ariaid-title8"></a>

You can configure the REST Service origin to only process requests that include a valid application ID. To require application IDs in the requests, define a list of valid application IDs in the REST Service origin.

When you configure application IDs for the origin, you must configure the REST API clients to include one of the listed application IDs in each request.

You might use application IDs to provide additional security or to enable multiple pipelines to listen at the same port. That is, if you specify a different application ID for each pipeline, multiple pipelines can safely share the same port.

Include the application ID for each request in one of the following ways:

In request headersAdd the following information to the HTTP request header for all requests that you want the origin to process:

```
X-SDC-APPLICATION-ID: <application_ID>
```

For example:

```
X-SDC-APPLICATION-ID: sdc_microservice
```

In a query parameter in the URLIf you cannot configure the client request headers - for example if the requests are generated by another system - then configure each REST API client to send data to a URL that includes the application ID in a query parameter.To include the application ID in a query parameter, enable the Application ID in URL property when you configure the origin. Then, include the application ID in a URL query parameter using the following format:

```
sdcApplicationId=<application_ID>
```

For example, the following URL sends a request to a microservice pipeline listening at port `8000` using `sdc_microservice` as the application ID:

```
https://localhost:8000/?sdcApplicationId=sdc_microservice
```

### REST API Client Requirement <a href="#ariaid-title9" id="ariaid-title9"></a>

To enable the REST Service origin to process data, you must configure the REST API clients to send data to the pipeline. The URL that you use depends on how you configure the origin.

You can configure the origin to listen at a port or to use Data Pipelines as an API gateway:Listening portWhen the origin is configured to listen for data at a specified port, you must configure each client to send data to a URL that includes the listening port number.Note: No other pipelines or processes can already be bound to the listening port. The listening port can be used only by a single pipeline.Use the following format for the URL:

```
<http | https>://<sdc_hostname>:<listening_port>/<path>?<queryString>&sdcApplicationId=<application_ID>
```

The URL includes the following components:

* \<http | https> - Use `https` for secure HTTP connections.
* \<sdc\_hostname> - The Data Pipelines host name.
* \<listening\_port> - The port number where the origin listens for data.
* \<path> - Optional. The path of the URL.
* \<queryString> - Optional. The parameters of the URL that come after the path.
* \<application\_ID> - Optional application ID if not passed in the request header.

For example: `https://localhost:8000/` or `https://localhost:8000/rest/v1/user`.

API gatewayWhen the origin is configured to use Data Pipelines as an API gateway, you specify a service name to use and indicate if authentication is required.To pass data to the pipeline, configure each REST API client to send data to a URL that includes the Data Pipelines URL and the specified service name.Use the following format for the URL:

```
<http | https>://<sdc_URL>/<rest | public-rest>/v1/gateway/<service name><path>?<queryString>&sdcApplicationId=<application_ID>
```

The URL includes the following components:

* \<http | https> - Use `https` for secure HTTP connections.
* \<sdc\_URL> - The Data Pipelines URL.
* \<rest | public-rest> - Use `rest` when requiring gateway authentication. Use `public-rest` when no authentication is required. For more information, see
* \<service name> - Service name specified in the origin.
* \<path> - Optional. The path of the URL.
* \<queryString> - Optional. The parameters of the URL that come after the path.
* \<application\_ID> - Optional application ID if not passed in the request header.

For example, the following URL sends data to a local Data Pipelines using the default port number. Data Pipelines routes the data to the microservice pipeline with the `checkUser` service name:

```
https://localhost:18630/rest/v1/gateway/checkUser
```

### Multithreaded Processing <a href="#ariaid-title10" id="ariaid-title10"></a>

The REST Service origin performs parallel processing and enables the creation of a multithreaded pipeline.

The REST Service origin uses multiple concurrent threads based on the Max Concurrent Requests property. When you start the pipeline, the origin creates the number of threads specified in the Max Concurrent Requests property. Each thread generates a batch from an incoming request and passes the batch to an available pipeline runner.

A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin. Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

For example, say you set the Max Concurrent Requests property to 5. When you start the pipeline, the origin creates five threads, and Data Pipelines creates a matching number of pipeline runners. Upon receiving data, the origin passes a batch to each of the pipeline runners for processing. In the batch, REST Service includes only the REST API requests with the specified application ID.

Each pipeline runner performs the processing associated with the rest of the pipeline. After a batch is written to pipeline destinations, the pipeline runner becomes available for another batch of data. Each batch is processed and written as quickly as possible, independent from other batches processed by other pipeline runners, so batches may be written differently from the read-order.

At any given moment, the five pipeline runners can each process a batch, so this multithreaded pipeline processes up to five batches at a time. When incoming data slows, the pipeline runners sit idle, available for use as soon as the data flow increases.

### Generated Response <a href="#ariaid-title11" id="ariaid-title11"></a>

In a microservice pipeline, the REST Service origin can send a response back to the originating REST API client.

The generated response contains:

* Records received from microservice destinations
* Pipeline error records received when the pipeline is configured to use the Send Response to Origin pipeline error handling

The origin generates a single response for each batch of records received. The origin can generate the response in JSON or XML format. The response can include an envelope or only raw data.

#### Responses with an Envelope

When configured to send a response with an envelope, the origin generates a response that includes the status code, data from microservice destinations, error records from the pipeline, and the first error message. The generated response includes the following JSON keys or XML elements:

| Response Key or Element | Value                                                                                                                                                                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| httpStatusCode          | <p>The status code associated with the records in the response.</p><p>If the records in the generated response share the same status code, the code is written to the httpStatusCode key or element. If the records have different status codes, the httpStatusCode is set to 207 for multiple statuses.</p> |
| data                    | A list of records passed to the origin by the microservice destinations used in the pipeline.                                                                                                                                                                                                                |
| error                   | A list of pipeline error records passed to the origin by the Send Response to Origin pipeline error handling.                                                                                                                                                                                                |
| errorMessage            | <p>The error message associated with the first error record in the response.</p><p>Used only when the response includes error records.</p>                                                                                                                                                                   |

For example, in JSON format, generated responses with envelopes have the following structure:

```
{
"httpStatusCode":<status code>,
"data":[<list of success records>],
"error":[<list of error records>],
"errorMessage": <error message, if any>
}
```

#### Raw Responses

When configured to send a raw response, the origin generates a response that contains either the list of records passed from the microservice destinations or the list of error records passed by the Send Response to Origin pipeline error handling. If the origin receives data records from destinations and error records from the pipeline, then the origin includes only the error records in the response. If the origin receives no data records from destinations and no error records from the pipeline, then the origin generates an empty response.

#### Sample Responses <a href="#ariaid-title12" id="ariaid-title12"></a>

The following samples in JSON format show responses with an envelope and raw responses without an envelope when the origin receives single records and multiple records:Single recordThe origin receives a single record from the Send Response to Origin destination. The destination is configured to use the 200 status code.For a response with an envelope, the origin sends the following response:

```
{
"httpStatusCode":200,
"data":[{"ID":"103","NAME":"Jack","AGE":"37","STATE":"MD"}],
"error":[],
"errorMessage":null
}
```

For a raw response, the origin sends the following response:

```
{"ID":"103","NAME":"Jack","AGE":"37","STATE":"MD"}
```

Multiple data and error recordsThe origin receives several records, data and error. Because each record has a different status code, the response uses status code 207 for multiple statuses. The errorMessage key includes the error associated with the first record which has a missing ID. The origin is configured to present multiple records as multiple JSON objects.For a response with an envelope, the origin sends the following response:

```
{
"httpStatusCode":207,
"data":[{"ID":"248","NAME":"Pina","AGE":"24","STATE":"RI"}],
"error":[{"NAME":"Liz","AGE":"37","STATE":"DE"}, {"ID":"302","NAME":"Roco","AGE":"","STATE":"CA"}],
"errorMessage":"COMMON_0001 - Stage precondition: CONTAINER_0051 - Unsatisfied precondition(s) '${record:exists('/ID')}'"
}
```

For a raw response, the origin sends the following response:

```
{"NAME":"Liz","AGE":"37","STATE":"DE"}, {"ID":"302","NAME":"Roco","AGE":"","STATE":"CA"}
```

### Record Header Attributes <a href="#ariaid-title13" id="ariaid-title13"></a>

The REST Service origin creates record header attributes that include information about the requested URL.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes.&#x20;

The REST Service origin creates the following record header attributes:

* method - The method for the request, such as GET, POST, or DELETE.
* path - The path of the URL.
* queryString - The parameters of the URL that come after the path.
* remoteHost - The name of the client or proxy that made the request.

The REST Service origin also includes HTTP request header fields – such as Host or Content-Type – in records as record header attributes. The attribute names match the original HTTP request header field name.

### Data Formats <a href="#ariaid-title14" id="ariaid-title14"></a>

The REST Service origin processes data differently based on the data format that you select. The origin processes the following types of data:

Avro

Generates a record for every Avro record. The origin includes the Avro schema in the `avroSchema` record header attribute. It also includes a `precision` and `scale` field attribute for each Decimal field.The origin expects each file to contain the Avro schema and uses the schema to process the Avro data.The origin reads files compressed by Avro-supported compression codecs without requiring additional configuration.

Binary

Generates a record with a single byte array field at the root of the record.When the data exceeds the user-defined maximum data size, the origin cannot process the data. Because the record is not created, the origin cannot pass the record to the pipeline to be written as an error record. Instead, the origin generates a stage error.

Datagram

Generates a record for every message. The origin can process [collectd](https://collectd.org/) messages, NetFlow 5 and NetFlow 9 messages, and the following types of syslog messages:

* [RFC 5424](https://tools.ietf.org/html/rfc5424)
* [RFC 3164](https://tools.ietf.org/html/rfc3164)
* Non-standard common messages, such as RFC 3339 dates with no version digit

When processing NetFlow messages, the stage generates different records based on the NetFlow version. When processing NetFlow 9, the records are generated based on the NetFlow 9 configuration properties.&#x20;

Delimited

Generates a record for each delimited line.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record.You can also configure the stage to replace a string constant with null values and to ignore control characters.

JSON

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin processes the object based on the error handling configured for the stage.

Protobuf

Generates a record for every protobuf message. By default, the origin assumes messages contain multiple protobuf messages.Protobuf messages must match the specified message type and be described in the descriptor file.When the data for a record exceeds 1 MB, the origin cannot continue processing data in the message. The origin handles the message based on the stage error handling property and continues reading the next message.

Record

Generates a record for every record. Use to process records generated by a pipeline using the Record data format.For error records, the origin provides the original record as read from the origin in the original pipeline, as well as error information that you can use to correct the record.When processing error records, the origin expects the error file names and contents as generated by the original pipeline.

XML

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

Note: Field attributes and record header attributes are written to destination systems automatically only when you use the SDC RPC data format in destinations.&#x20;

When a record exceeds the user-defined maximum record length, the origin skips the record and continues processing with the next record. It sends the skipped record to the pipeline for error handling.Use the XML data format to process valid XML documents.

Tip: If you want to process invalid XML documents, you can try using the text data format with custom delimiters.&#x20;
