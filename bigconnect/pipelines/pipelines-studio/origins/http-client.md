# HTTP Client

The HTTP Client origin reads data from an HTTP resource URL.&#x20;

When you configure HTTP Client, you specify the resource URL, optional headers, and the method to use. For some methods, you can specify the request body and default content type.

You can configure the actions to take based on the response status and configure pagination properties to enable processing large volumes of data from paginated APIs. You can also enable the origin to read compressed and archived files.

The origin provides response header fields as record header attributes so you can use the information in the pipeline when needed.

The origin also provides several different authentication types to access data. You can enter credentials in the origin or you can secure the credentials in runtime resource files and reference the files in the origin. You can also configure the origin to use the OAuth 2 protocol to connect to an HTTP service.

You can optionally use an HTTP proxy and configure SSL/TLS properties. You can also configure the origin to log request and response information.

**Tip**: Data Pipelines provides several HTTP origins to address different needs.&#x20;

When the pipeline stops, HTTP Client notes where it stops reading. When the pipeline starts again, HTTP Client continues processing from where it stopped by default. You can reset the origin to process all requested files.

## Processing Mode <a href="#ariaid-title2" id="ariaid-title2"></a>

HTTP Client can use one of the following processing modes to read source data:

**Streaming**

HTTP Client maintains a connection and processes data as it becomes available. Use to process streaming data in real time.

**Polling**

HTTP Client polls the server at the specified interval for available data. Use to access data periodically, such as metrics and events at a REST endpoint.

**Note**: After the polling interval passes, the origin continues processing from where it stopped. For example, let’s say that you’ve configured the origin to use the polling mode with an interval of two hours and to use page number pagination. After the origin reads 25 pages of results, the 26th page returns no results and so the origin stops reading. After the two hour interval passes, the origin polls the server again, reading the results starting with page 26.

**Batch**

HTTP Client processes all available data and then stops the pipeline. Use to process data as needed.

## Actions for Response Statuses

By default, the HTTP Client origin processes only responses that include a 2xx success status code. When the response includes any other status code, such as a 4xx or 5xx status code, the origin generates an error and handles the record based on the error record handling configured for the stage.

You can configure the origin to perform one of several actions when it encounters an unsuccessful status code, that is any non-2xx status code.

To configure a per-status action, you enter the HTTP status code, such as 403 or 504, and then select one of the following actions for the origin to perform for that code:

* Retry with linear backoff
* Retry with exponential backoff
* Retry immediately
* Cause the stage to fail and stop the pipeline

**Note**: Do not configure a per-status action to generate an error record. This selection is not valid for the origin.

When defining the retry with linear or exponential backoff action, you also specify the backoff interval to wait in milliseconds. When defining any of the retry actions, you specify the maximum number of retries. If the stage receives a 2xx status code during a retry, then it processes the response. If the stage doesn't receive a 2xx status code after the maximum number of retries, then the stage generates an error.

You can add multiple status codes and configure a specific action for each code.

You can also configure the stage to generate records for all unsuccessful statuses that are not added to the Per-Status Actions list. You then specify the output field name that stores the error response body for those records.

For example, if the stage receives a 400 status code, you want the pipeline to process the response body that contains the description of the error. When configuring the stage, you do not add an action for the 400 status code because you don't need the stage to retry the request. You select the Records for Remaining Statuses property and then use the default value `outErrorBody` as the name of the error response body field.

## Pagination <a href="#ariaid-title4" id="ariaid-title4"></a>

The HTTP Client origin can use pagination to retrieve a large volume of data from a paginated API.

When configuring the HTTP Client origin to use pagination, use the pagination type supported by the API of the HTTP client. You will likely need to consult the documentation for the origin system API to determine the pagination type to use and the properties to set.

The HTTP Client origin supports the following common pagination types:

Link in HTTP HeaderAfter processing the current page, uses the link in the HTTP header to access the next page. The link in the header can be an absolute URL or a URL relative to the resource URL configured for the stage. For example, let's say you configure the following resource URL for the stage:

```
https://myapp.com/api/objects?page=1
```

The next link in the HTTP header can include an absolute URL, as follows:

```
link:<https://myapp.com/api/objects?page=2>; rel="next"
```

Or the next link can include a URL relative to the resource URL, as follows:

```
link:<objects?page=2>; rel="next"
```

Link in Response FieldAfter processing the current page, uses the link in a field in the response body to access the next page. The link in the response field can be an absolute URL or a URL relative to the resource URL configured for the stage. For example, let's say you configure the following resource URL for the stage:

```
http://myapp.com/api/tickets.json?start_time=138301982
```

The next link in the response field can include an absolute URL, as follows:

```
"next_page":"http://myapp.com/api/tickets.json?start_time=1389078385", 
```

Or the next link can include a URL relative to the resource URL, as follows:

```
"next_page":"tickets.json?start_time=1389078385", 
```

By Page NumberBegins processing with the specified initial page, and then requests the following page. Use the `${startAt}` variable in the resource URL as the value of the page number to request.By Offset NumberBegins processing with the specified initial offset, and then requests the following offset. Use the `${startAt}` variable in the resource URL as the value of the offset number to request.

For the link in response field pagination type, you must define a stop condition that determines when there are no more pages to process. For all other pagination types, the stage stops reading when it returns a page that does not contain any more records.

When you use any pagination type, you must specify a result field path and can choose whether to include all other fields in the record.

### Page or Offset Number <a href="#ariaid-title5" id="ariaid-title5"></a>

When using page number or offset number pagination, the API of the HTTP client typically requires that you include a page or offset parameter at the end of the resource URL. The parameter determines the next page or offset of data to request.

The name of the parameter used by the API varies. For example, it might be `offset`, `page`, `start`, or `since`. Consult the documentation for the origin system API to determine the name of the page or offset parameter.

The HTTP Client origin provides a `${startAt}` variable that you can use in the URL as the value of the page or offset. For example, your resource URL might be any of the following:

* `http://webservice/object?limit=15&offset=${startAt}`
* `https://myapp.com/product?limit=5&since=${startAt}`
* `https://myotherapp.com/api/v1/products?page=${startAt}`

When the pipeline starts, the HTTP Client stage uses the value of the Initial Page/Offset property as the `${startAt}` variable value. After the stage reads a page of results, the stage increments the `${startAt}` variable by one if using page number pagination or by the number of records read from the page if using offset number pagination.

For example, let’s say that you configure offset number pagination, set the initial offset to 0, and use the following resource URL:

```
https://myapp.com/product?limit=5&since=${startAt}
```

When you start the pipeline, the stage resolves the resource URL to:

```
https://myapp.com/product?limit=5&since=0
```

The first page of results includes items 0 through 4. After reading all 5 records from the first page, the stage increments the `${startAt}` variable by 5, such that the next resource URL is resolved to:

```
https://myapp.com/product?limit=5&since=5
```

The second page of results also includes 5 items, starting at the 5th item.

### Result Field Path <a href="#ariaid-title6" id="ariaid-title6"></a>

When using any pagination type, you must specify the result field path. The result field path is the location in the response that contains the data that you want to process.

The result field path must be a list or array. The origin creates a record for each object in the array.

For example, to process the following response, you use /results as the result field path:

```json
{  
   "count":"1023",
   "startAt":"2",
   "maxResults":"2",
   "total":"6",
   "results":[  
      {  
         "firstName":"Joe",
         "lastName":"Smith",
         "phone":"555-555-5555"
      },
      {  
         "firstName":"Jimmy",
         "lastName":"Smott",
         "phone":"333-333-3333"
      },
      {  
         "firstName":"Joanne",
         "lastName":"Smythe",
         "phone":"777-777-7777"
      }
   ]
}
```

From this data, the origin creates three records. The first record:

```json
{  
   "firstName":"Joe",
   "lastName":"Smith",
   "phone":"555-555-5555"
}
```

The second record:

```json
{  
   "firstName":"Jimmy",
   "lastName":"Smott",
   "phone":"333-333-3333"
}
```

And the third record:

```json
{  
   "firstName":"Joanne",
   "lastName":"Smythe",
   "phone":"777-777-7777"
}
```

### Keep All Fields <a href="#ariaid-title7" id="ariaid-title7"></a>

When using any pagination type, you can configure the origin to keep all fields in addition to those in the specified result field path. The resulting record includes all fields in the original structure and the result field path that includes one set of data.

By default, the origin returns only the data within the specified result field path.

For example, say we use the same sample data as above, with /results for the result field path. And we configure the origin to keep all fields. The origin generates three records that keep the existing record structure, and includes one set of data in the /results field.

Here's the first record:

```json
{  
   "count":"1023",
   "startAt":"2",
   "maxResults":"2",
   "total":"6",
   "results":{  
      "firstName":"Joe",
      "lastName":"Smith",
      "phone":"555-555-5555"
   }
}
```

The second record:

```json
{  
   "count":"1023",
   "startAt":"2",
   "maxResults":"2",
   "total":"6",
   "results":{  
      "firstName":"Jimmy",
      "lastName":"Smott",
      "phone":"333-333-3333"
   }
}
```

And the third record:

```json
{  
   "count":"1023",
   "startAt":"2",
   "maxResults":"2",
   "total":"6",
   "results":{  
      "firstName":"Joanne",
      "lastName":"Smythe",
      "phone":"777-777-7777"
   }
}
```

#### Pagination Examples <a href="#ariaid-title8" id="ariaid-title8"></a>

Let's look at some examples of how you might configure the supported pagination types.

**Example for Link in HTTP Header**

The API of the HTTP client includes the link to access the next page in the HTTP header. For example, the HTTP header might be:

```
link:<https://myapp.com/api/objects?page=2>; rel="next", 
<https://myapp.com/api/objects?page=9>; rel="last"
```

So after the HTTP Client origin reads the first page of results, it can use the next link in the HTTP header to read the next page.

The API requires that you include a page parameter in the URL to specify the initial page number to return from the results. On the HTTP tab of the stage, you configure the following resource URL such that the stage begins reading from the first page:

```
https://myapp.com/api/objects?page=1
```

You expect to receive a response like the following:

```
{  
   "total":"2000",
   "limit":"10",
   "results":[  
      {  
         "firstName":"Joe",
         "lastName":"Smith"
      },
      ...
      {  
         "firstName":"Joanne",
         "lastName":"Smythe"
      }
   ]
}
```

On the Pagination tab of the stage, you simply set Pagination Mode to link in HTTP header, and then you set the result field path to the `/results` field:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClient-ExampleLinkHeader.png)

**Example for Link in Response Field**

The API of the HTTP client uses a field in the response body to access the next page. It requires that you include a timestamp in the resource URL indicating which items you want to start reading.

On the HTTP tab of the stage, you configure the following resource URL:

```
http://myapp.com/api/tickets.json?start_time=138301982
```

You expect to receive a response like the following:

```
{
  "ticket_events":[
    {
       "ticket_id":27, 
       "timestamp":138561439, 
       "via":"Email"
     },
     ...
     {
       "ticket_id":30, 
       "timestamp":138561445, 
       "via":"Phone"
      }
  ]
  "next_page":"http://myapp.com/api/tickets.json?start_time=1389078385", 
  "count":1000, 
  "end_time":1389078385 
  }
```

On the Pagination tab of the stage, you set Pagination Mode to link in response field, and set the next page link field to the `/next_page` field.

The count field in the response determines the number of items displayed per page. If a page returns less than the specified number of items, it is the last page of data. So you enter the following stop condition to stop processing when the count is less than 1000:

```
${record:value('/count') < 1000}
```

Then you set the result field path to the `/ticket_events` field:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClient-ExampleLinkResponseField.png)

**Example for Page Number**

The API of the HTTP client uses page number pagination. It requires that you include a page parameter in the URL that specifies the page number to return from the results.

On the HTTP tab of the stage, you configure the following resource URL, setting the page parameter to the `${startAt}` variable:

```
https://myotherapp.com/api/v1/products?page=${startAt}
```

You expect to receive a response like the following:

```
{  
   "total":"2000",
   "items":[  
      {  
         "item":"pencil",
         "cost":"2.00"
      },
      ...
      {  
         "item":"eraser",
         "cost":"1.10"
       }
   ]
}
```

On the Pagination tab of the stage, you set Pagination Mode to by page number. You want to begin processing from the first page in the results, so you set the initial page to 0. Then you set the result field path to the `/items` field:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClient-ExamplePageNumber.png)

**Example for Offset Number**

The API of the HTTP client uses offset number pagination. It requires that you include the following parameters in the resource URL:

* `limit` - Specifies the number of results per page.
* `offset` - Specifies the offset value.

On the HTTP tab of the stage, you configure the following resource URL for the origin, setting the limit to 10 results per page and setting the offset to the `${startAt}` variable:

```
https://myapp.com/product?limit=10&offset=${startAt}
```

You expect to receive a response like the following, with 10 items in the results list:

```
{  
   "total":"2000",
   "limit":"10",
   "results":[  
      {  
         "firstName":"Joe",
         "lastName":"Smith"
      },
      ...
      {  
         "firstName":"Joanne",
         "lastName":"Smythe"
      }
   ]
}
```

On the Pagination tab of the stage, you set Pagination Mode to by offset number. You want to begin processing from the first item in the results, so you set the initial offset to 0. Then you set the result field path to the `/results` field:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClient-ExampleOffset.png)

## HTTP Method <a href="#ariaid-title13" id="ariaid-title13"></a>

To request data from an HTTP resource URL, specify the request method to use. Most servers require a GET request, but you should verify the request required by the server you want to access.

You can use the following methods:

* GET
* PUT
* POST
* DELETE
* HEAD

## OAuth 2 Authorization <a href="#ariaid-title14" id="ariaid-title14"></a>

The HTTP Client origin can use the OAuth 2 protocol to connect to an HTTP service.

The origin can use the OAuth 2 protocol to connect to an HTTP service that uses basic, digest, or universal authentication, OAuth 2 client credentials, OAuth 2 username and password, or OAuth 2 JSON Web Tokens (JWT).

The OAuth 2 protocol authorizes third-party access to HTTP service resources without sharing credentials. The HTTP Client origin uses credentials to request an access token from the service. The service returns the token to the origin, and then the origin includes the token in a header in each request to the resource URL.

The credentials that you enter to request an access token depend on the credentials grant type required by the HTTP service. You can define the following OAuth 2 credentials grant types for HTTP Client:

**Client credentials grant**

HTTP Client sends its own credentials - the client ID and client secret or the basic, digest, or universal authentication credentials - to the HTTP service. For example, use the client credentials grant to process data from the Twitter API or from the Microsoft Azure Active Directory (Azure AD) API.

For more information about the client credentials grant, see [https://tools.ietf.org/html/rfc6749#section-4.4](https://tools.ietf.org/html/rfc6749#section-4.4).

**Resource owner password credentials grant**

HTTP Client sends the credentials for the resource owner - the resource owner username and password - to the HTTP service. Or, you can use this grant type to migrate existing clients using basic, digest, or universal authentication to OAuth 2 by converting the stored credentials to an access token.

For example, use this grant to process data from the Getty Images API. For more information about using OAuth 2 to connect to the Getty Images API, see [http://developers.gettyimages.com/api/docs/v3/oauth2.html](https://developers.gettyimages.com/api/oauth2.html).

For more information about the resource owner password credentials grant, see [https://tools.ietf.org/html/rfc6749#section-4.3](https://tools.ietf.org/html/rfc6749#section-4.3).

**JSON Web Tokens (JWT)**

HTTP Client sends a JSON-based security token encoding to the HTTP service. For example, use JSON Web Tokens to process data from the Google API.

Let’s look at some examples of how to configure authentication and OAuth 2 authorization to process data from Twitter, Microsoft Azure AD, and Google APIs.

### Example for Twitter <a href="#ariaid-title15" id="ariaid-title15"></a>

To use OAuth 2 authorization to read from Twitter, configure HTTP Client to use basic authentication and the client credentials grant.

For more information about configuring OAuth 2 authorization for Twitter, see [https://developer.twitter.com/en/docs/authentication/oauth-2-0/application-only](https://developer.twitter.com/en/docs/authentication/oauth-2-0/application-only).

1. On the HTTP tab, set Authentication Type to Basic, and then select Use OAuth 2.
2.  Change the default value of the Resource URL property to define a Twitter URL that works with OAuth 2 authorization.

    The default URL is not valid when using OAuth2 authorization.

    For example, you might define the following URL that works with OAuth 2 authorization: `https://api.twitter.com/1.1/search/tweets.json?q=%23DataOps`
3. On the Credentials tab, enter the Twitter consumer API key and consumer API secret key for the Username and Password properties.Tip: To secure sensitive information such as the consumer API keys, you can use [runtime resources](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Pipeline\_Configuration/RuntimeValues.html#concept\_bs4\_5nm\_2s) or [credential stores.](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Configuration/CredentialStores.html#concept\_bt1\_bpj\_r1b)
4. On the OAuth 2 tab, select Client Credentials Grant for the grant type.
5.  In the Token URL property, enter the following URL used to request the access token:

    ```
    https://api.twitter.com/oauth2/token
    ```

The following image shows the OAuth 2 tab configured for Twitter:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClientOrigin\_OAuth2Twitter.png)

### Example for Microsoft Azure AD <a href="#ariaid-title16" id="ariaid-title16"></a>

To use OAuth 2 authorization to read from Microsoft Azure AD, configure HTTP Client to use no authentication and the client credentials grant.

Note: This example uses Microsoft Azure AD version 1.0.For more information about configuring OAuth 2 authorization for Microsoft Azure AD, see [https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code).

1. On the HTTP tab, set Authentication Type to None, and then select Use OAuth 2.
2. On the OAuth 2 tab, select Client Credentials Grant for the grant type.
3.  In the Token URL property, enter the following URL used to request the access token:

    ```
    https://login.microsoftonline.com/<tenant identifier>/oauth2/token
    ```

    Where `<tenant identifier>` is the Azure AD tenant identifier.
4.  Enter the OAuth 2 client ID and secret.

    The client ID is the Application Id assigned to your app when you registered it with Azure AD, found in the Azure Classic Portal.

    The client secret is the application secret that you created in the app registration portal for your app.

    Tip: To secure sensitive information such as the client ID and secret, you can use [runtime resources](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Pipeline\_Configuration/RuntimeValues.html#concept\_bs4\_5nm\_2s) or [credential stores.](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Configuration/CredentialStores.html#concept\_bt1\_bpj\_r1b)
5.  Add any key-value pairs that the HTTP service requires in the token request.In our example, we are accessing the `graph.microsoft.com` API in our resource URL, so we need to add the following key-value pair:

    ```
    resource : https://graph.microsoft.com/
    ```

The following image shows the OAuth 2 tab configured for Microsoft Azure AD version 1.0:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClientOrigin\_OAuth2AzureAD.png)

### Example for Google <a href="#ariaid-title17" id="ariaid-title17"></a>

To use OAuth 2 authorization to read from Google service accounts, configure HTTP Client to use no authentication and the JSON Web Tokens grant.

For more information about Google service accounts, see the Google Cloud documentation: [Understanding service accounts](https://cloud.google.com/iam/docs/understanding-service-accounts).

For more information about configuring OAuth 2 authorization for Google, see the Google Identity Platform documentation: [Using OAuth 2.0 to Access Google APIs](https://developers.google.com/identity/protocols/OAuth2).

1. On the HTTP tab, set Authentication Type to None, and then select Use OAuth 2.
2. On the OAuth 2 tab, select JSON Web Tokens for the grant type.
3.  In the Token URL property, enter the following URL used to request the access token:

    ```
    https://www.googleapis.com/oauth2/v4/token
    ```
4. Select the following algorithm to sign the JWT: RSASSA-PKCS-v1\_5 using SHA-256.
5.  Enter the Base64 encoded key used to sign the JWT.

    To access the key, download the JSON key file when you generate the Google credentials. Locate the "private\_key" field in the file, which contains a string version of the key. Copy the string into the JWT Signing Key property, and then replace all "\n" literals with new lines.

    Tip: To secure sensitive information such as the JWT signing key, you can use runtime resources or credential stores.
6.  In the JWT Claims property, enter the required claims to use with the JWT token request, in JSON format.

    For information about claim requirements when calling a Google API, see the Google Identity Platform documentation: [Addendum: service account authorization without OAuth](https://developers.google.com/identity/protocols/OAuth2ServiceAccount#jwt-auth).

    For a list of scopes for the scope claim, see the Google Identity Platform documentation: [OAuth 2.0 Scopes for Google APIs](https://developers.google.com/identity/protocols/googlescopes).

    For example, enter the claims in the following JSON format:

    ```
    {
       "iss":"my_name@my_account.iam.gserviceaccount.com",
       "scope":"https://www.googleapis.com/auth/drive",
       "aud":"https://www.googleapis.com/oauth2/v4/token",
       "exp":${(time:dateTimeToMilliseconds(time:now())/1000) + 50 * MINUTES},
       "iat":${time:dateTimeToMilliseconds(time:now())/1000}
    }
    ```

    You can include the expression language in the JWT claims. For example, in the sample claim above, both the "exp" (expiration time) claim and the "iat" (issued at) claim include Data Pipelines time functions to set the expiration time and the issue time.

    Tip: Google access tokens expire after 60 minutes. As a result, set the expiration time claim to be slightly less than 60 minutes so that HTTP Client can request a new token within the time limit.

The following image shows the OAuth 2 tab configured for Google service accounts:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClientOrigin\_OAuth2Google.png)

## Logging Request and Response Data <a href="#ariaid-title18" id="ariaid-title18"></a>

The HTTP Client origin can log request and response data to the Data Pipelines log.

When enabling logging, you configure the following properties:

VerbosityThe type of data to include in logged messages:

* Headers\_Only - Includes request and response headers.
* Payload\_Text - Includes request and response headers as well as any text payloads.
* Payload\_Any - Includes request and response headers and the payload, regardless of type.

Log LevelThe level of messages to include in the Data Pipelines log. When you select a level, higher level messages are also logged. That is, if you select the Warning log level, then Severe and Warning messages are written to the Data Pipelines log.Note: The configured log level for Data Pipelines can limit the level of detail that is logged. For example, if you set the log level to Finest to log detailed trace information, but Data Pipelines is configured for ERROR, then the origin only writes Severe level messages.The following table describes the stage log levels and the corresponding Data Pipelines log levels needed to enable the logging:

| **Stage Log Level** | **Data Pipelines** | **Description**                            |
| ------------------- | ------------------ | ------------------------------------------ |
| Severe              | ERROR              | Only messages indicating serious failures. |
| Warning             | WARN               | Messages warning of potential problems.    |
| Info                | INFO               | Informational messages.                    |
| Fine                | DEBUG              | Basic tracing information.                 |
| Finer               | DEBUG              | Detailed tracing information.              |
| Finest              | TRACE              | Highly detailed tracing information.       |

The name of this stage logger is `com.streamsets.http.RequestLogger`.

Max entity size

The maximum size of message data to write to the log. Use to limit the volume of data written to the Data Pipelines log for any single message.

## Generated Records <a href="#ariaid-title19" id="ariaid-title19"></a>

The HTTP Client origin generates records based on the responses it receives.

Data in the response body is parsed based on the selected data format. For HEAD responses, when the response body contains no data, the origin creates an empty record. Information returned from the HEAD appears in record header attributes. For all other methods, when the response body contains no data, no records are created.

In generated records, all standard response header fields, such as Content-Encoding and Content-Type, are written to corresponding record header attributes. Custom response header fields are also written to record header attributes. Record header attribute names match the original response header names.

When you configure the origin to generate records for unsuccessful statuses that are not added to the Per-Status Actions list, then the record might also include a field that contains the error response body.

## Data Formats <a href="#ariaid-title20" id="ariaid-title20"></a>

The HTTP Client origin processes data differently based on the data format. The HTTP Client origin processes data formats as follows:

**Avro**

Generates a record for every message. Includes a `precision` and `scale` field attribute for each Decimal field.The stage includes the Avro schema in an `avroSchema` record header attribute. You can use one of the following methods to specify the location of the Avro schema definition:

* Message/Data Includes Schema - Use the schema in the message.
* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the stage to look up the schema in Confluent Schema Registry by the schema ID embedded in the message or by the schema ID or subject specified in the stage configuration.

Using a schema in the stage configuration or retrieving a schema from Confluent Schema Registry overrides any schema that might be included in the message and can improve performance.

**Binary**

Generates a record with a single byte array field at the root of the record.When the data exceeds the user-defined maximum data size, the origin cannot process the data. Because the record is not created, the origin cannot pass the record to the pipeline to be written as an error record. Instead, the origin generates a stage error.

**Datagram**

Generates a record for every message. The origin can process [collectd](https://collectd.org/) messages, NetFlow 5 and NetFlow 9 messages, and the following types of syslog messages:

* [RFC 5424](https://tools.ietf.org/html/rfc5424)
* [RFC 3164](https://tools.ietf.org/html/rfc3164)
* Non-standard common messages, such as RFC 3339 dates with no version digit

When processing NetFlow messages, the stage generates different records based on the NetFlow version. When processing NetFlow 9, the records are generated based on the NetFlow 9 configuration properties.&#x20;

**Delimited**

Generates a record for each delimited line.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record.You can also configure the stage to replace a string constant with null values and to ignore control characters.

**JSON**

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin processes the object based on the error handling configured for the stage.

**Log**

Generates a record for every log line.When a line exceeds the user-defined maximum line length, the origin truncates longer lines.You can include the processed log line as a field in the record. If the log line is truncated, and you request the log line in the record, the origin includes the truncated line.You can define the log format or type to be read.

**Protobuf**

Generates a record for every protobuf message. By default, the origin assumes messages contain multiple protobuf messages.Protobuf messages must match the specified message type and be described in the descriptor file.When the data for a record exceeds 1 MB, the origin cannot continue processing data in the message. The origin handles the message based on the stage error handling property and continues reading the next message.For information about generating the descriptor file, see Protobuf Data Format Prerequisites.

**Record**

Generates a record for every record. Use to process records generated by a Data Pipelines pipeline using the Record data format.For error records, the origin provides the original record as read from the origin in the original pipeline, as well as error information that you can use to correct the record.When processing error records, the origin expects the error file names and contents as generated by the original pipeline.

**Text**

Generates a record for each line of text.When a line exceeds the specified maximum line length, the origin truncates the line. The origin adds a boolean field named Truncated to indicate if the line was truncated.

**XML**

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

**Note**: Field attributes and record header attributes are written to destination systems automatically only when you use the Data Pipelines RPC data format in destinations. For more information about working with field attributes and record header attributes, and how to include them in records, see Field Attributes and Record Header Attributes.

When a record exceeds the user-defined maximum record length, the origin skips the record and continues processing with the next record. It sends the skipped record to the pipeline for error handling.Use the XML data format to process valid XML documents. For more information about XML processing, see Reading and Processing XML Data.

**Tip**: If you want to process invalid XML documents, you can try using the text data format with custom delimiters. For more information, see Processing XML Data with Custom Delimiters.

## Configuring an HTTP Client Origin <a href="#ariaid-title21" id="ariaid-title21"></a>

Configure an HTTP Client origin to read data from an HTTP resource URL.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul> |
2.  On the HTTP tab, configure the following properties:

    | HTTP Property                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Resource URL                   | <p>URL where the data resides.</p><p>When using page or offset number pagination, use the <code>${startAt}</code> variable in the resource URL as the value of the page or offset to request.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Mode                           | <p>Processing mode:</p><ul><li>Streaming - Maintains a connection and processes data as it becomes available.</li><li>Polling - Connects periodically to check for data.</li><li>Batch - Processes all available data, and then stops the pipeline.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Polling Interval (ms)          | Milliseconds to wait before checking for new data. Used in the polling mode only.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | HTTP Method                    | HTTP method to use to request data from the server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Request Body                   | <p>Request data to use with the specified method. Available for the PUT, POST, and DELETE methods.</p><p>You can use time functions and datetime variables, such as ${YYYY()}, in the request body.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Default Request Content Type   | <p>Content-Type header to include in the request. Used only when the Content-Type header is not present.</p><p>Available for the PUT, POST, and DELETE methods.</p><p>Default is application/json.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Authentication Type            | <p>Determines the authentication type used to connect to the server:</p><ul><li>None - Performs no authentication.</li><li><p>Basic - Uses basic authentication. Requires a username and password.</p><p>Use with HTTPS to avoid passing unencrypted credentials.</p></li><li>Digest - Uses digest authentication. Requires a username and password.</li><li><p>Universal - Makes an anonymous connection, then provides authentication credentials upon receiving a 401 status and a WWW-Authenticate header request.</p><p>Requires a username and password associated with basic or digest authentication.</p><p>Use only with servers that respond to this workflow.</p></li><li>OAuth - Uses OAuth 1.0 authentication. Requires OAuth credentials.</li></ul> |
    | Use OAuth 2                    | <p>Enables using OAuth 2 authorization to request access tokens.</p><p>You can use OAuth 2 authorization with none, basic, digest, or universal authentication.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Headers                        | Optional headers to include in the request. Using simple or bulk edit mode, click the Add icon to add additional headers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Per-Status Actions             | <p>Actions to take for specific response statuses that are not a 2xx success status code. For example, you can configure the stage to retry the request with an exponential backoff when it receives a 500 HTTP status code.</p><p>Click Add to add an action for an additional status code.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Records for Remaining Statuses | Generate records for all unsuccessful statuses that are not added to the Per-Status Actions list.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Error Response Body Field      | <p>Name of the field that stores the error response body for those records.</p><p>Available when generating records for remaining statuses.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Body Time Zone                 | Time zone to use for evaluating the request body. Use when the request body includes datetime variables or time functions.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Request Transfer Encoding      | <p>Use one of the following encoding types:</p><ul><li>Buffered - The standard transfer encoding type.</li><li>Chunked - Transfers data in chunks. Not supported by all servers.</li></ul><p>Default is Buffered.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Connect Timeout                | Maximum number of milliseconds to wait for a connection.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | Read Timeout                   | Maximum number of milliseconds to wait for data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Use Proxy                      | Enables using an HTTP proxy to connect to the system.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Max Batch Size (records)       | Maximum number of records to include in a batch and send through the pipeline at one time.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Batch Wait Time (ms)           | Maximum number of milliseconds wait before sending a partial or empty batch.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
3.  On the Pagination tab, optionally configure pagination details.

    | Pagination Property          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Pagination Mode              | Method of pagination to use. Use a method supported by the API of the HTTP client.                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Initial Page/Offset          | Initial page for page number pagination, or the initial offset for offset number pagination.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Next Page Link Field         | <p>Field path in the response that contains the URL to the next page.</p><p>For link in response field pagination.</p>                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Stop Condition               | <p>Condition that evaluates to true when there are no more pages to process.</p><p>For link in response field pagination.</p><p>For example, let's say that the API of the HTTP client includes a count property that determines the number of items displayed per page. If the count is set to 1000 and a page returns with less than 1000 items, it is the last page of data. So you'd enter the following expression to stop processing when the count is less than 1000:</p><pre><code>${record:value('/count') &#x3C; 1000}
    </code></pre> |
    | Result Field Path            | <p>Field path in the response that contains the data that you want to process. Must be a list or array field.</p><p>The origin generates a record for each object in the specified field.</p>                                                                                                                                                                                                                                                                                                                                                  |
    | Keep All Fields              | <p>Includes all fields from the response in the resulting record when enabled.</p><p>By default, only the fields in the specified result field path are included in the record.</p>                                                                                                                                                                                                                                                                                                                                                            |
    | Wait Time Between Pages (ms) | The number of milliseconds to wait before requesting the next page of data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
4.  When using Basic authentication, on the Credentials tab, configure the following properties:

    | Credentials Property | Description                                                                                                                                                                        |
    | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Username             | User name for basic, digest, or universal authentication.                                                                                                                          |
    | Password             | Password for basic, digest, or universal authentication.Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores. |
5.  When using OAuth 2 authorization, on the OAuth 2 tab, configure the following properties.For more information about OAuth 2 and for example OAuth 2 configurations to read from Twitter, Microsoft Azure AD, or Google APIs, see OAuth 2 Authorization.

    | OAuth 2 Property           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Credentials Grant Type     | <p>Type of client credentials grant type required by the HTTP service:</p><ul><li>Client credentials grant</li><li>Resource owner password credentials grant</li><li>JSON Web Tokens (JWT)</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Token URL                  | URL to request the access token.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | User Name                  | <p>Resource owner user name.</p><p>Enter for the resource owner password credentials grant.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Password                   | <p>Resource owner password.</p><p>Enter for the resource owner password credentials grant.</p><p>Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Client ID                  | <p>Client ID that the HTTP service uses to identify the HTTP client.</p><p>Enter for the client credentials grant that uses a client ID and secret for authentication. Or, for the resource owner password credentials grant that requires a client ID and secret.</p>                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Client Secret              | <p>Client secret that the HTTP service uses to authenticate the HTTP client.</p><p>Enter for the client credentials grant that uses a client ID and secret for authentication. Or, for the resource owner password credentials grant that requires a client ID and secret.</p><p>Tip: To secure sensitive information such as the client ID and secret, you can use runtime resources or credential stores.</p>                                                                                                                                                                                                                                                           |
    | JWT Signing Algorithm      | <p>Algorithm used to sign the JSON Web Token (JWT).</p><p>Default is none. Enter for the JSON Web Tokens grant.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | JWT Signing Key            | <p>Base64 encoded key used to sign the JSON Web Token, if you selected a signing algorithm.Tip: To secure sensitive information such as the JWT signing key, you can use runtime resources or credential stores.</p><p>Enter for the JSON Web Tokens grant.</p>                                                                                                                                                                                                                                                                                                                                                                                                           |
    | JWT Claims                 | <p>Claims to use in the JSON Web Token request, entered in JSON format. Enter each claim required by the HTTP service. You can include the expression language in the JWT claims.</p><p>For example, to read from Google service accounts, enter the following claims with the appropriate values:</p><pre><code>{
      "iss":"my_name@my_account.iam.gserviceaccount.com",
      "scope":"https://www.googleapis.com/auth/drive",
      "aud":"https://www.googleapis.com/oauth2/v4/token",
      "exp":${(time:dateTimeToMilliseconds(time:now())/1000) + 50 * 60},
      "iat":${time:dateTimeToMilliseconds(time:now())/1000}
    }
    </code></pre><p>Enter for the JSON Web Tokens grant.</p> |
    | Request Transfer Encoding  | <p>Form of encoding to use when the stage requests an access token: buffered or chunked.</p><p>Default is chunked.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Additional Key-Value Pairs | <p>Optional key-value pairs to send to the token URL when requesting an access token. For example, you can define the OAuth 2 <code>scope</code> request parameter.</p><p>Using simple or bulk edit mode, click the Add icon to add additional key-value pairs.</p>                                                                                                                                                                                                                                                                                                                                                                                                       |
6.  To use an HTTP proxy, on the Proxy tab, configure the following properties:

    | HTTP Proxy Property | Description                                                                                                                               |
    | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
    | Proxy URI           | Proxy URI.                                                                                                                                |
    | Username            | Proxy user name.                                                                                                                          |
    | Password            | Proxy password.Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores. |
7.  To use SSL/TLS, on the TLS tab, configure the following properties:

    | TLS Property               | Description                                                                                                                                                                                                                                                                                                                     |
    | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Use TLS                    | Enables the use of TLS.                                                                                                                                                                                                                                                                                                         |
    | Use Remote Keystore        | Enables loading the contents of the keystore from a remote credential store or from values entered in the stage properties. For more information, see Remote Keystore and Truststore.                                                                                                                                           |
    | Private Key                | Private key used in the remote keystore. Enter a credential function that returns the key or enter the contents of the key.                                                                                                                                                                                                     |
    | Certificate Chain          | <p>Each PEM certificate used in the remote keystore. Enter a credential function that returns the certificate or enter the contents of the certificate.</p><p>Using simple or bulk edit mode, click the <strong>Add</strong> icon to add additional certificates.</p>                                                           |
    | Keystore File              | <p>Path to the local keystore file. Enter an absolute path to the file or a path relative to the Data Pipelines resources directory: $SDC_RESOURCES.</p><p>By default, no keystore is used.</p>                                                                                                                                 |
    | Keystore Type              | <p>Type of keystore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                                                                                                                                                |
    | Keystore Password          | Password to the keystore file. A password is optional, but recommended.Tip: To secure sensitive information such as passwords, you can use runtime resources or credential stores.                                                                                                                                              |
    | Keystore Key Algorithm     | <p>Algorithm to manage the keystore.</p><p>Default is SunX509.</p>                                                                                                                                                                                                                                                              |
    | Use Remote Truststore      | Enables loading the contents of the truststore from a remote credential store or from values entered in the stage properties. For more information, see Remote Keystore and Truststore.                                                                                                                                         |
    | Trusted Certificates       | <p>Each PEM certificate used in the remote truststore. Enter a credential function that returns the certificate or enter the contents of the certificate.</p><p>Using simple or bulk edit mode, click the <strong>Add</strong> icon to add additional certificates.</p>                                                         |
    | Truststore File            | <p>Path to the local truststore file. Enter an absolute path to the file or a path relative to the Data Pipelines resources directory: $SDC_RESOURCES.</p><p>By default, no truststore is used.</p>                                                                                                                             |
    | Truststore Type            | <p>Type of truststore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                                                                                                                                              |
    | Truststore Password        | Password to the truststore file. A password is optional, but recommended.Tip: To secure sensitive information such as passwords, you can use runtime resources or credential stores.                                                                                                                                            |
    | Truststore Trust Algorithm | <p>Algorithm to manage the truststore.</p><p>Default is SunX509.</p>                                                                                                                                                                                                                                                            |
    | Use Default Protocols      | Uses the default TLSv1.2 transport layer security (TLS) protocol. To use a different protocol, clear this option.                                                                                                                                                                                                               |
    | Transport Protocols        | TLS protocols to use. To use a protocol other than the default TLSv1.2, click the Add icon and enter the protocol name. You can use simple or bulk edit mode to add protocols.Note: Older protocols are not as secure as TLSv1.2.                                                                                               |
    | Use Default Cipher Suites  | Uses a default cipher suite for the SSL/TLS handshake. To use a different cipher suite, clear this option.                                                                                                                                                                                                                      |
    | Cipher Suites              | <p>Cipher suites to use. To use a cipher suite that is not a part of the default set, click the Add icon and enter the name of the cipher suite. You can use simple or bulk edit mode to add cipher suites.</p><p>Enter the Java Secure Socket Extension (JSSE) name for the additional cipher suites that you want to use.</p> |
8.  On the Timeout Handling tab, configure the following properties:

    | Timeout Handling Property | Description                                                                                                                                               |
    | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Action for Timeout        | Action to take when the request times out because the HTTP service did not respond within the read timeout period.                                        |
    | Max Retries               | <p>Maximum number of times to retry the request before failing the stage. A negative value allows an infinite number of retries.</p><p>Default is 10.</p> |
9.  On the Data Format tab, configure the following property:

    | Data Format Property | Description                                                                                                                                                                                                             |
    | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Data Format          | <p>Format of data. Use one of the following data formats:</p><ul><li>Avro</li><li>Binary</li><li>Datagram</li><li>Delimited</li><li>JSON</li><li>Log</li><li>Protobuf</li><li>Record</li><li>Text</li><li>XML</li></ul> |
10. For Avro data, on the Data Format tab, configure the following properties:

    | Avro Property        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Avro Schema Location | <p>Location of the Avro schema definition to use when processing data:</p><ul><li>Message/Data Includes Schema - Use the schema in the message.</li><li>In Pipeline Configuration - Use the schema provided in the stage configuration.</li><li>Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry.</li></ul><p>Using a schema in the stage configuration or in Confluent Schema Registry can improve performance.</p> |
    | Avro Schema          | <p>Avro schema definition used to process the data. Overrides any existing schema definitions associated with the data.</p><p>You can optionally use the <code>runtime:loadResource</code> function to load a schema definition stored in a runtime resource file.</p>                                                                                                                                                                             |
    | Schema Registry URLs | <p>Confluent Schema Registry URLs used to look up the schema. To add a URL, click Add and then enter the URL in the following format:</p><pre><code>http://&#x3C;host name>:&#x3C;port number>
    </code></pre>                                                                                                                                                                                                                                       |
    | Basic Auth User Info | <p>User information needed to connect to Confluent Schema Registry when using basic authentication.</p><p>Enter the key and secret from the <code>schema.registry.basic.auth.user.info</code> setting in Schema Registry using the following format:</p><pre><code>&#x3C;key>:&#x3C;secret>
    </code></pre><p>Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores.</p>         |
    | Lookup Schema By     | <p>Method used to look up the schema in Confluent Schema Registry:</p><ul><li>Subject - Look up the specified Avro schema subject.</li><li>Schema ID - Look up the specified Avro schema ID.</li><li>Embedded Schema ID - Look up the Avro schema ID embedded in each message.</li></ul><p>Overrides any existing schema definitions associated with the message.</p>                                                                              |
    | Schema Subject       | <p>Avro schema subject to look up in Confluent Schema Registry.</p><p>If the specified subject has multiple schema versions, the stage uses the latest schema version for that subject. To use an older version, find the corresponding schema ID, and then set the Look Up Schema By property to Schema ID.</p>                                                                                                                                   |
    | Schema ID            | Avro schema ID to look up in Confluent Schema Registry.                                                                                                                                                                                                                                                                                                                                                                                            |
11. For binary data, on the Data Format tab, configure the following properties:

    | Binary Property                               | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Max Data Size (bytes)                         | Maximum number of bytes in the message. Larger messages cannot be processed or written to error.                                                                                                                                                                                                                                                                                                      |
12. For datagram data, on the Data Format tab, configure the following properties:

    | Datagram Properties            | Description                                                                                                                                                                                                                                                                                                      |
    | ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Datagram Packet Format         | <p>Packet format of the data:</p><ul><li>collectd</li><li>NetFlow</li><li>syslog</li><li>Raw/separated data</li></ul>                                                                                                                                                                                            |
    | TypesDB File Path              | <p>Path to a user-provided types.db file. Overrides the default types.db file.</p><p>For collectd data only.</p>                                                                                                                                                                                                 |
    | Auth File                      | <p>Path to an optional authentication file. Use an authentication file to accept signed and encrypted data.</p><p>For collectd data only.</p>                                                                                                                                                                    |
    | Convert Hi-Res Time & Interval | <p>Converts the collectd high resolution time format interval and timestamp to UNIX time, in milliseconds.</p><p>For collectd data only.</p>                                                                                                                                                                     |
    | Exclude Interval               | <p>Excludes the interval field from output record.</p><p>For collectd data only.</p>                                                                                                                                                                                                                             |
    | Record Generation Mode         | <p>Determines the type of values to include in the record. Select one of the following options:</p><ul><li>Raw Only</li><li>Interpreted Only</li><li>Both Raw and Interpreted</li></ul><p>For NetFlow 9 data only.</p>                                                                                           |
    | Max Templates in Cache         | <p>The maximum number of templates to store in the template cache. For more information about templates, see Caching NetFlow 9 Templates.</p><p>Default is -1 for an unlimited cache size.</p><p>For NetFlow 9 data only.</p>                                                                                    |
    | Template Cache Timeout (ms)    | <p>The maximum number of milliseconds to cache an idle template. Templates unused for more than the specified time are evicted from the cache. For more information about templates, see Caching NetFlow 9 Templates.</p><p>Default is -1 for caching templates indefinitely.</p><p>For NetFlow 9 data only.</p> |
    | Charset                        | Character encoding of the messages to be processed.                                                                                                                                                                                                                                                              |
    | Ignore Control Characters      | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                              |
13. For delimited data, on the Data Format tab, configure the following properties:

    | Delimited Property                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Header Line                                   | Indicates whether a file contains a header line, and whether to use the header line.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Delimiter Format Type                         | <p>Delimiter format type. Use one of the following options:</p><ul><li>Default CSV - File that includes comma-separated values. Ignores empty lines in the file.</li><li>RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.</li><li>MS Excel CSV - Microsoft Excel comma-separated file.</li><li>MySQL CSV - MySQL comma-separated file.</li><li>Tab-Separated Values - File that includes tab-separated values.</li><li>PostgreSQL CSV - PostgreSQL comma-separated file.</li><li>PostgreSQL Text - PostgreSQL text file.</li><li>Custom - File that uses user-defined delimiter, escape, and quote characters.</li><li>Multi Character Delimited - File that uses multiple user-defined characters to delimit fields and lines, and single user-defined escape and quote characters.</li></ul><p>Available when using the Apache Commons parser type.</p> |
    | Multi Character Field Delimiter               | <p>Characters that delimit fields.</p><p>Default is two pipe characters (||).</p><p>Available when using the Apache Commons parser with the multi-character delimiter format.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Multi Character Line Delimiter                | <p>Characters that delimit lines or records.</p><p>Default is the newline character (\n).</p><p>Available when using the Apache Commons parser with the multi-character delimiter format.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Delimiter Character                           | <p>Delimiter character. Select one of the available options or use Other to enter a custom character.</p><p>You can enter a Unicode control character using the format \uNNNN, where ​<em>N</em> is a hexadecimal digit from the numbers 0-9 or the letters A-F. For example, enter \u0000 to use the null character as the delimiter or \u2028 to use a line separator as the delimiter.</p><p>Default is the pipe character ( | ).</p><p>Available when using the Apache Commons parser with a custom delimiter format.</p>                                                                                                                                                                                                                                                                                                                                                         |
    | Field Separator                               | <p>One or more characters to use as delimiter characters between columns.</p><p>Available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Quote Character                               | <p>Quote character.</p><p>Available when using the Apache Commons parser with the custom or multi-character delimiter format. Also available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Allow Comments                                | <p>Allows commented data to be ignored for custom delimiter format.</p><p>Available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Comment Character                             | <p>Character that marks a comment when comments are enabled for custom delimiter format.</p><p>Available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Enable Comments                               | <p>Allows commented data to be ignored for custom delimiter format.</p><p>Available when using the Apache Commons parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Comment Marker                                | <p>Character that marks a comment when comments are enabled for custom delimiter format.</p><p>Available when using the Apache Commons parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Lines to Skip                                 | Number of lines to skip before reading data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | CSV Parser                                    | <p>Parser to use to process delimited data:</p><ul><li>Apache Commons - Provides robust parsing and a wide range of delimited format types.</li><li>Univocity - Can provide faster processing for wide delimited files, such as those with over 200 columns.</li></ul><p>Default is Apache Commons.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Max Columns                                   | <p>Maximum number of columns to process per record.</p><p>Available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Max Character per Column                      | <p>Maximum number of characters to process in each column.</p><p>Available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Skip Empty Lines                              | <p>Allows skipping empty lines.</p><p>Available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Allow Extra Columns                           | <p>Allows processing records with more columns than exist in the header line.</p><p>Available when using the Apache Commons parser to process data with a header line.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Extra Column Prefix                           | <p>Prefix to use for any additional columns. Extra columns are named using the prefix and sequential increasing integers as follows: <code>&#x3C;prefix>&#x3C;integer></code>.</p><p>For example, <code>_extra_1</code>. Default is <code>_extra_</code>.</p><p>Available when using the Apache Commons parser to process data with a header line while allowing extra columns.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Max Record Length (chars)                     | <p>Maximum length of a record in characters. Longer records are not read.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p><p>Available when using the Apache Commons parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Ignore Empty Lines                            | <p>Allows empty lines to be ignored.</p><p>Available when using the Apache Commons parser with the custom delimiter format.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Root Field Type                               | <p>Root field type to use:</p><ul><li>List-Map - Generates an indexed list of data. Enables you to use standard functions to process data. Use for new pipelines.</li><li>List - Generates a record with an indexed list with a map for header and value. Requires the use of delimited data functions to process data. Use only to maintain pipelines created before 1.1.0.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Parse NULLs                                   | Replaces the specified string constant with null values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | NULL Constant                                 | String constant to replace with null values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
14. For JSON data, on the Data Format tab, configure the following properties:

    | JSON Property                                 | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JSON Content                                  | <p>Type of JSON content. Use one of the following options:</p><ul><li>Array of Objects</li><li>Multiple Objects</li></ul>                                                                                                                                                                                                                                                                             |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Maximum Object Length (chars)                 | <p>Maximum number of characters in a JSON object.</p><p>Longer objects are diverted to the pipeline for error handling.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                                                                               |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                      |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                   |
15. For log data, on the Data Format tab, configure the following properties:

    | Log Property                                  | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Log Format                                    | <p>Format of the log files. Use one of the following options:</p><ul><li>Common Log Format</li><li>Combined Log Format</li><li>Apache Error Log Format</li><li>Apache Access Log Custom Format</li><li>Regular Expression</li><li>Grok Pattern</li><li>Log4j</li><li>Common Event Format (CEF)</li><li>Log Event Extended Format (LEEF)</li></ul>                                                     |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Max Line Length                               | <p>Maximum length of a log line. The origin truncates longer lines.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                                                                                                                                   |
    | Retain Original Line                          | <p>Determines how to treat the original log line. Select to include the original log line as a field in the resulting record.</p><p>By default, the original line is discarded.</p>                                                                                                                                                                                                                   |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                      |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                   |

    * When you select Apache Access Log Custom Format, use Apache log format strings to define the Custom Log Format.
    * When you select Regular Expression, enter the regular expression that describes the log format, and then map the fields that you want to include to each regular expression group.
    *   When you select Grok Pattern, you can use the Grok Pattern Definition field to define custom grok patterns. You can define a pattern on each line.

        In the Grok Pattern field, enter the pattern to use to parse the log. You can use a predefined grok patterns or create a custom grok pattern using patterns defined in Grok Pattern Definition.

        For more information about defining grok patterns and supported grok patterns, see Defining Grok Patterns.
    *   When you select Log4j, define the following properties:

        | Log4j Property        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                 |
        | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
        | On Parse Error        | <p>Determines how to handle information that cannot be parsed:</p><ul><li>Skip and Log Error - Skips reading the line and logs a stage error.</li><li>Skip, No Error - Skips reading the line and does not log an error.</li><li>Include as Stack Trace - Includes information that cannot be parsed as a stack trace to the previously-read log line. The information is added to the message field for the last valid log line.</li></ul> |
        | Use Custom Log Format | Allows you to define a custom log format.                                                                                                                                                                                                                                                                                                                                                                                                   |
        | Custom Log4J Format   | Use log4j variables to define a custom log format.                                                                                                                                                                                                                                                                                                                                                                                          |
16. For protobuf data, on the Data Format tab, configure the following properties:

    | Protobuf Property                             | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Protobuf Descriptor File                      | Descriptor file (.desc) to use. The descriptor file must be in the Data Pipelines resources directory, `$SDC_RESOURCES`.                                                                                                                                                                                                                                                                              |
    | Message Type                                  | <p>The fully-qualified name for the message type to use when reading data.</p><p>Use the following format: <code>&#x3C;package name>.&#x3C;message type></code>.</p><p>Use a message type defined in the descriptor file.</p>                                                                                                                                                                         |
    | Delimited Messages                            | Indicates if a message might include more than one protobuf message.                                                                                                                                                                                                                                                                                                                                  |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
17. For Record data, on the Data Format tab, configure the following properties:

    | Record Property                               | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
18. For text data, on the Data Format tab, configure the following properties:

    | Text Property                                 | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Max Line Length                               | <p>Maximum number of characters allowed for a line. Longer lines are truncated.</p><p>Adds a boolean field to the record to indicate if it was truncated. The field name is Truncated.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                |
    | Use Custom Delimiter                          | Uses custom delimiters to define records instead of line breaks.                                                                                                                                                                                                                                                                                                                                      |
    | Custom Delimiter                              | One or more characters to use to define records.                                                                                                                                                                                                                                                                                                                                                      |
    | Include Custom Delimiter                      | Includes delimiter characters in the record.                                                                                                                                                                                                                                                                                                                                                          |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                      |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                   |
19. For XML data, on the Data Format tab, configure the following properties:

    | XML Property                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Delimiter Element                             | <p>Delimiter to use to generate records. Omit a delimiter to treat the entire XML document as one record. Use one of the following:</p><ul><li><p>An XML element directly under the root element.</p><p>Use the XML element name without surrounding angle brackets ( &#x3C; > ) . For example, msg instead of &#x3C;msg>.</p></li><li><p>A simplified XPath expression that specifies the data to use.</p><p>Use a simplified XPath expression to access data deeper in the XML document or data that requires a more complex access method.</p><p>For more information about valid syntax, see Simplified XPath Syntax.</p></li></ul>         |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul>                                                                                                                                                                                                                                           |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                                                                                                                                                                                                                                                            |
    | Preserve Root Element                         | <p>Includes the root element in the generated records.</p><p>When omitting a delimiter to generate a single record, the root element is the root element of the XML document.</p><p>When specifying a delimiter to generate multiple records, the root element is the XML element specified as the delimiter element or is the last XML element in the simplified XPath expression specified as the delimiter element.</p>                                                                                                                                                                                                                      |
    | Include Field XPaths                          | <p>Includes the XPath to each parsed XML element and XML attribute in field attributes. Also includes each namespace in an xmlns record header attribute.</p><p>When not selected, this information is not included in the record. By default, the property is not selected.</p><p>Note: Field attributes and record header attributes are written to destination systems automatically only when you use the Data Pipelines RPC data format in destinations. For more information about working with field attributes and record header attributes, and how to include them in records, see Field Attributes and Record Header Attributes.</p> |
    | Namespaces                                    | <p>Namespace prefix and URI to use when parsing the XML document. Define namespaces when the XML element being used includes a namespace prefix or when the XPath expression includes namespaces.</p><p>For information about using namespaces with an XML element, see Using XML Elements with Namespaces.</p><p>For information about using namespaces with XPath expressions, see Using XPath Expressions with Namespaces.</p><p>Using simple or bulk edit mode, click the Add icon to add additional namespaces.</p>                                                                                                                        |
    | Output Field Attributes                       | <p>Includes XML attributes and namespace declarations in the record as field attributes. When not selected, XML attributes and namespace declarations are included in the record as fields.Note: Field attributes are automatically included in records written to destination systems only when you use the Data Pipelines RPC data format in the destination. For more information about working with field attributes, see Field Attributes.</p><p>By default, the property is not selected.</p>                                                                                                                                             |
    | Max Record Length (chars)                     | <p>The maximum number of characters in a record. Longer records are diverted to the pipeline for error handling.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                                                                                                                                                                                                                                                                                                                                |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
20. On the Logging tab, configure the following properties to log request and response data:

    | Logging Property       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Enable Request Logging | Enables logging request and response data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Log Level              | <p>The level of detail to be logged. Choose one of the available options.The following list is in order of lowest to highest level of logging. When you select a level, messages generated by the levels above the selected level are also written to the log:</p><ul><li>Severe - Only messages indicating serious failures.</li><li>Warning - Messages warning of potential problems.</li><li>Info - Informational messages.</li><li>Fine - Basic tracing information.</li><li>Finer - Detailed tracing information.</li><li>Finest - Highly detailed tracing information.</li></ul><p>Note: The log level configured for Data Pipelines can limit the level of messages that the stage writes. Verify that the Data Pipelines log level supports the level that you want to use.</p> |
    | Verbosity              | <p>The type of data to include in logged messages:</p><ul><li>Headers_Only - Includes request and response headers.</li><li>Payload_Text - Includes request and response headers as well as any text payloads.</li><li>Payload_Any - Includes request and response headers and the payload, regardless of type.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Max Entity Size        | The maximum size of message data to write to the log. Use to limit the volume of data written to the Data Pipelines log for any single message.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

\
