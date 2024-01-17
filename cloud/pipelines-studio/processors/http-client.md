# HTTP Client

The HTTP Client processor sends requests to an HTTP resource URL and writes responses to records.&#x20;

For each request, the processor writes data from the response to the specified output field. When the response contains multiple values, the processor can write either the first value, all values to a list in a single record, or all values to separate records.

You can use the HTTP Client processor to perform a range of standard requests or you can use an expression to determine the request for each record.

When you configure the HTTP Client, you define the resource URL, header attributes, and method to use. For some methods, you can specify the request body and default content type.

You can configure the actions to take based on the response status and configure pagination properties to enable processing large volumes of data from paginated APIs.

You can configure the processor to include response header fields in the record as a set of record header attributes or as a map in a record field. You can configure the processor to log request and response information. And you can write the resolved request URL to the Data Pipelines log.

You can also configure the timeout, request transfer encoding, maximum number of parallel requests, and authentication type. You can optionally use an HTTP proxy and configure SSL/TLS properties.

You can also configure the processor to use the OAuth 2 protocol to connect to an HTTP service.

### HTTP Method <a href="#ariaid-title2" id="ariaid-title2"></a>

You can use the following methods with the HTTP Client processor:

* GET
* PUT
* POST
* DELETE
* HEAD
* PATCH
* Expression - An expression that evaluates to one of the other methods.

#### Expression Method <a href="#ariaid-title3" id="ariaid-title3"></a>

The Expression method allows you to write an expression that evaluates to a standard HTTP method. Use the Expression method to generate a workflow. For example, you can use an expression that performs a lookup (GET) or passes data to the server (PUT) based on the data in a field.

### Actions for Response Statuses <a href="#ariaid-title4" id="ariaid-title4"></a>

By default, the HTTP Client processor processes only responses that include a 2xx success status code. When the response includes any other status code, such as a 4xx or 5xx status code, the processor generates an error and handles the record based on the error record handling configured for the stage.

You can configure the processor to perform one of several actions when it encounters an unsuccessful status code, that is, any non-2xx status code.

To configure a per-status action, you enter the HTTP status code, such as 403 or 504, and then select one of the following actions for the processor to perform for that code:

* Retry with linear backoff
* Retry with exponential backoff
* Retry immediately
* Cause the stage to fail and stop the pipeline
* Generate an error record

When defining the retry with linear or exponential backoff action, you also specify the backoff interval to wait in milliseconds. When defining any of the retry actions, you specify the maximum number of retries. If the stage receives a 2xx status code during a retry, then it processes the response. If the stage doesn't receive a 2xx status code after the maximum number of retries, then the stage generates an error.

You can add multiple status codes and configure a specific action for each code.

You can also configure the stage to generate records for all unsuccessful statuses that are not added to the Per-Status Actions list. You then specify the output field name that stores the error response body for those records.

For example, if the stage receives a 400 status code, you want the pipeline to process the response body that contains the description of the error. When configuring the stage, you do not add an action for the 400 status code because you don't need the stage to retry the request. You select the Records for Remaining Statuses property and then use the default value `outErrorBody` as the name of the error response body field.

### Parallel Requests <a href="#ariaid-title5" id="ariaid-title5"></a>

The HTTP Client processor sends multiple requests at a time. To preserve record order, the processor waits until all requests for the entire batch are completed before processing the next batch.

You can specify the maximum number of parallel requests. Default is 1. Increasing the number of parallel requests can improve performance but increases the load on the server. Network latency can also significantly impact the performance of this processor.

### Pagination <a href="#ariaid-title6" id="ariaid-title6"></a>

The HTTP Client processor can use pagination to retrieve a large volume of data from a paginated API.

When configuring the HTTP Client processor to use pagination, use the pagination type supported by the API of the HTTP client. You will likely need to consult the documentation for the origin system API to determine the pagination type to use and the properties to set.

Important: When configuring the processor to use pagination, set the Multiple Values Behavior property on the HTTP tab to write all values to a list or to write to separate records. When the property is set to write the first value only, the processor reads the first value in the first page of results only. The processor does not access the remaining pages of results, regardless of how pagination is configured.

The HTTP Client processor supports the following common pagination types:

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

#### Page or Offset Number <a href="#ariaid-title7" id="ariaid-title7"></a>

When using page number or offset number pagination, the API of the HTTP client typically requires that you include a page or offset parameter at the end of the resource URL. The parameter determines the next page or offset of data to request.

The name of the parameter used by the API varies. For example, it might be `offset`, `page`, `start`, or `since`. Consult the documentation for the origin system API to determine the name of the page or offset parameter.

The HTTP Client processor provides a `${startAt}` variable that you can use in the URL as the value of the page or offset. For example, your resource URL might be any of the following:

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

#### Result Field Path <a href="#ariaid-title8" id="ariaid-title8"></a>

When using any pagination type, you must specify the result field path. The result field path is the location in the response that contains the data that you want to process.

The result field path must be a list or array.

For example, to process the following response, you use /results as the result field path:

```
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

The processor creates records from the result field path based on how you configure the Multiple Values Behavior property on the HTTP tab:

First value onlyWhen configured to write the first value only, the processor creates the following single record from this sample data:

```
{  
   "firstName":"Joe",
   "lastName":"Smith",
   "phone":"555-555-5555"
}
```

The processor does not request any remaining pages of results.All values as a listWhen configured to write all values to a list in a single record, the processor creates the following single record from this sample data:

```
[  
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
```

The processor then accesses the next page of results, creating a single record from each page of results.Split into multiple recordsWhen configured to write all values, each to a separate record, the processor creates three records from this sample data:The first record:

```
{  
   "firstName":"Joe",
   "lastName":"Smith",
   "phone":"555-555-5555"
}
```

The second record:

```
{  
   "firstName":"Jimmy",
   "lastName":"Smott",
   "phone":"333-333-3333"
}
```

And the third record:

```
{  
   "firstName":"Joanne",
   "lastName":"Smythe",
   "phone":"777-777-7777"
}
```

The processor then accesses the next page of results, creating multiple records from each page of results.

#### Keep All Fields <a href="#ariaid-title9" id="ariaid-title9"></a>

When using any pagination type, you can configure the processor to keep all fields in addition to those in the specified result field path. The resulting record includes all fields in the original structure and the result field path that includes the data.

By default, the processor returns only the data within the specified result field path.

For example, say we use the same sample data as above, with /results for the result field path. And we configure the processor to keep all fields. The processor creates records from the result field path based on how you configure the Multiple Values Behavior property on the HTTP tab:

First value onlyWhen configured to write the first value only, the processor creates the following single record that keeps the existing record structure and the first set of data in the /results field:

```
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

All values as a listWhen configured to write all values to a list in a single record, the processor creates the following single record that keeps the existing record structure with each set of data in the /results field:

```
[
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
  },
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
  },
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
]
```

Split into multiple recordsWhen configured to write all values, each to a separate record, the processor generates three records that keep the existing record structure, and includes one set of data in the /results field:Here's the first record:

```
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

```
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

```
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

#### Pagination Examples <a href="#ariaid-title10" id="ariaid-title10"></a>

Let's look at some examples of how you might configure the supported pagination types.

**Example for Link in HTTP Header**

The API of the HTTP client includes the link to access the next page in the HTTP header. For example, the HTTP header might be:

```
link:<https://myapp.com/api/objects?page=2>; rel="next", 
<https://myapp.com/api/objects?page=9>; rel="last"
```

So after the HTTP Client processor reads the first page of results, it can use the next link in the HTTP header to read the next page.

The API requires that you include a page parameter in the URL to specify the initial page number to return from the results. On the HTTP tab of the stage, you configure the following resource URL such that the stage begins reading from the first page:

```
https://myapp.com/api/objects?page=1
```

Then, you set the Multiple Values Behavior property to write all values to a list in a single record or all values to separate records.

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

<figure><img src="https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClient-ExampleLinkHeader.png" alt=""><figcaption></figcaption></figure>

**Example for Link in Response Field**

The API of the HTTP client uses a field in the response body to access the next page. It requires that you include a timestamp in the resource URL indicating which items you want to start reading.

On the HTTP tab of the stage, you configure the following resource URL:

```
http://myapp.com/api/tickets.json?start_time=138301982
```

Then, you set the Multiple Values Behavior property to write all values to a list in a single record or all values to separate records.

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

Then, you set the Multiple Values Behavior property to write all values to a list in a single record or all values to separate records.

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

Then, you set the Multiple Values Behavior property to write all values to a list in a single record or all values to separate records.

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

### OAuth 2 Authorization <a href="#ariaid-title15" id="ariaid-title15"></a>

You can configure the HTTP Client processor to use the OAuth 2 protocol to connect to an HTTP service that uses basic, digest, or universal authentication, OAuth 2 client credentials, OAuth 2 username and password, or OAuth 2 JSON Web Tokens (JWT).

The OAuth 2 protocol authorizes third-party access to HTTP service resources without sharing credentials. The HTTP Client processor uses credentials to request an access token from the service. The service returns the token to the processor, and then the processor includes the token in a header in each request to the resource URL.

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

#### Example for Twitter <a href="#ariaid-title16" id="ariaid-title16"></a>

To use OAuth 2 authorization to read from Twitter, configure HTTP Client to use basic authentication and the client credentials grant.

For more information about configuring OAuth 2 authorization for Twitter, see [https://developer.twitter.com/en/docs/authentication/oauth-2-0/application-only](https://developer.twitter.com/en/docs/authentication/oauth-2-0/application-only).

1. On the HTTP tab, set Authentication Type to Basic, and then select Use OAuth 2.
2. On the Credentials tab, enter the Twitter consumer API key and consumer API secret key for the Username and Password properties.
3. On the OAuth 2 tab, select Client Credentials Grant for the grant type.
4.  In the Token URL property, enter the following URL used to request the access token:

    ```
    https://api.twitter.com/oauth2/token
    ```

The following image shows the OAuth 2 tab configured for Twitter:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClientProcessor\_OAuth2Twitter.png)

#### Example for Microsoft Azure AD <a href="#ariaid-title17" id="ariaid-title17"></a>

To use OAuth 2 authorization to read from Microsoft Azure AD, configure HTTP Client to use no authentication and the client credentials grant.

Note: This example uses Microsoft Azure AD version 1.0.

For more information about configuring OAuth 2 authorization for Microsoft Azure AD, see [https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code).

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
5.  Add any key-value pairs that the HTTP service requires in the token request.In our example, we are accessing the `graph.microsoft.com` API in our resource URL, so we need to add the following key-value pair:

    ```
    resource : https://graph.microsoft.com/
    ```

The following image shows the OAuth 2 tab configured for Microsoft Azure AD version 1.0:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClientProcessor\_OAuth2AzureAD.png)

#### Example for Google <a href="#ariaid-title18" id="ariaid-title18"></a>

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

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClientProcessor\_OAuth2Google.png)

### Generated Output <a href="#ariaid-title19" id="ariaid-title19"></a>

For each request that returns a 2xx success status code, the HTTP Client processor writes the response to the specified output field. The processor parses data in the response body into values based on the selected data format. You configure how the processor writes multiple values. The processor can write either the first value to a single record, all values to a list in a single record, or all values to separate records.

When you configure the processor to generate records for unsuccessful statuses that are not added to the Per-Status Actions list, then the HTTP Client processor might also write an error response body to the specified error response body field.

For HEAD responses, the response body contains no data. Therefore, the processor writes output only to record header attributes, leaving the output field empty.

#### Response Headers <a href="#ariaid-title20" id="ariaid-title20"></a>

The HTTP Client processor can include response headers in records. You can write the data to a record as follows:

**Record header attributes**

The processor writes data in response headers to corresponding record header attributes.

When writing to record header attributes, you define a prefix for the attributes to avoid name conflicts. The processor then appends the response header name to the prefix.

For example, if you define `info-` as the prefix, then data in the content-encoding response header is written to the "info-content-encoding" record header attribute.For general information about record header attributes, see Record Header Attributes.

**Record field**

The processor can also write the response headers to a field in the record. The processor writes the response headers to the record field as a map of key-value pairs where the key is the response header name.When writing to a field, you specify the field to use. If the field includes data, the processor overwrites the data. If the field does not exist, the processor creates the field.

### Logging Request and Response Data <a href="#ariaid-title21" id="ariaid-title21"></a>

The HTTP Client processor can log request and response data to the Data Pipelines log.

When enabling logging, you configure the following properties:

VerbosityThe type of data to include in logged messages:

* Headers\_Only - Includes request and response headers.
* Payload\_Text - Includes request and response headers as well as any text payloads.
* Payload\_Any - Includes request and response headers and the payload, regardless of type.

Log LevelThe level of messages to include in the Data Pipelines log. When you select a level, higher level messages are also logged. That is, if you select the Warning log level, then Severe and Warning messages are written to the Data Pipelines log.

Note: The configured log level for Data Pipelines can limit the level of detail that is logged. For example, if you set the log level to Finest to log detailed trace information, but Data Pipelines is configured for ERROR, then the origin only writes Severe level messages.The following table describes the stage log levels and the corresponding Data Pipelines log levels needed to enable the logging:

| Stage Log Level | Data Pipelines | Description                                |
| --------------- | -------------- | ------------------------------------------ |
| Severe          | ERROR          | Only messages indicating serious failures. |
| Warning         | WARN           | Messages warning of potential problems.    |
| Info            | INFO           | Informational messages.                    |
| Fine            | DEBUG          | Basic tracing information.                 |
| Finer           | DEBUG          | Detailed tracing information.              |
| Finest          | TRACE          | Highly detailed tracing information.       |

The name of this stage logger is `com.streamsets.http.RequestLogger`.

Max entity size

The maximum size of message data to write to the log. Use to limit the volume of data written to the Data Pipelines log for any single message.

### Logging the Resolved Resource URL <a href="#ariaid-title22" id="ariaid-title22"></a>

You can write the resolved resource URL to the Data Pipelines log.

The resolved resource URL is the URL that is defined in the Resource URL property after resolving any expressions included in the URL.

For example, say you configure the Resource URL property with the following URL:

```
https://api.twitter.com/1.1/search/tweets.json?q=${record:value('/text')}
```

This allows building the URL based on information in the /text field of each record. So, when a record contains `%23DataOps` in the /text field, then the resolved URL is:

```
https://api.twitter.com/1.1/search/tweets.json?q=%23DataOps
```

To write the resolved resource URL to the Data Pipelines log, set the Data Pipelines log level to DEBUG or higher. You do not need to use the Enable Request Logging property in the processor to log the resolved resource URL.

### Data Formats <a href="#ariaid-title23" id="ariaid-title23"></a>

The HTTP Client processor parses each server response based on the selected data format and writes the response to the specified output field in the selected format.

You configure how the processor writes parsed responses that contain multiple values. The processor can write either the first value to a single record, all values to a list in a single record, or all values to separate records.

Available data formats include:

Delimited

The processor parses each line in the response as a value, and either writes only the first delimited line to a single record, writes all delimited lines to a single record with each line written to a list item, or writes each delimited line to separate records.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record.You can also configure the stage to replace a string constant with null values and to ignore control characters.

JSON

The processor parses each object in the response into a value, and either writes only the first object to a single record, writes all objects to a list in a single record, or writes each object to separate records.When an object exceeds the specified maximum object length, the processor processes the object based on the error handling configured for the stage.

Text

If you specify a custom delimiter, the processor parses the data into values based on the delimiter. Otherwise, the processor parses each line into a value. Then, the processor either writes only the first value to a single record, writes all values to a list in a single record, or writes each value to separate records.When a line exceeds the specified maximum line length, the processor truncates the line and adds a Boolean field named Truncated.

XML

If you specify a delimiter element, the processor uses the delimiter element to parse the response into values. The processor either writes only the first delimited element to a single record, writes all delimited elements to a list in a single record, or writes each delimited element to separate records.If you do not specify a delimiter element, the processor writes the entire response to single record.When a record exceeds the specified maximum record length, the processor skips the record and continues processing with the next record. It sends the skipped record to the pipeline for error handling.

### Configuring HTTP Client Processor <a href="#ariaid-title24" id="ariaid-title24"></a>

Configure an HTTP Client processor to perform requests against a resource URL.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the HTTP tab, configure the following properties:

    | HTTP Property                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Resource URL                   | HTTP resource URL.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Output Field                   | Field to use for the response. You can use a new or existing field.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Multiple Values Behavior       | <p>Action to take when responses contain multiple values:</p><ul><li>First value only - Write the first value.</li><li>All values as a list - Write all values to a list in a single record.</li><li>Split into multiple records - Write all values, each to a separate record.</li></ul><p>When the processor uses pagination, set to All values as a list or Split into multiple records.</p>                                                                                                                                                                                                                                                                                                                                                                   |
    | Headers                        | The headers to include in the request. Using simple or bulk edit mode, click the Add icon to add additional headers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | HTTP Method                    | HTTP request method. Use one of the standard methods or use Expression to enter an expression.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | HTTP Method Expression         | <p>Expression that evaluates to a standard HTTP method.</p><p>Used for the Expression method only.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Request Data                   | Request data to use with the specified method. Available for the PUT, POST, DELETE, PATCH, and Expression methods.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Default Request Content Type   | <p>Content-Type header to include in the request. Used only when the Content-Type header is not present.</p><p>Available for the PUT, POST, DELETE, PATCH, and Expression methods.</p><p>Default is application/json.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Maximum Parallel Requests      | Maximum number of requests to send to the server at one time.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Authentication Type            | <p>Determines the authentication type used to connect to the server:</p><ul><li>None - Performs no authentication.</li><li><p>Basic - Uses basic authentication. Requires a username and password.</p><p>Use with HTTPS to avoid passing unencrypted credentials.</p></li><li>Digest - Uses digest authentication. Requires a username and password.</li><li><p>Universal - Makes an anonymous connection, then provides authentication credentials upon receiving a 401 status and a WWW-Authenticate header request.</p><p>Requires a username and password associated with basic or digest authentication.</p><p>Use only with servers that respond to this workflow.</p></li><li>OAuth - Uses OAuth 1.0 authentication. Requires OAuth credentials.</li></ul> |
    | OAuth2                         | <p>Enables using OAuth 2 authorization to request access tokens.</p><p>You can use OAuth 2 authorization with none, basic, digest, or universal authentication.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Per-Status Actions             | <p>Actions to take for specific response statuses that are not a 2xx success status code. For example, you can configure the stage to retry the request with an exponential backoff when it receives a 500 HTTP status code.</p><p>Click Add to add an action for an additional status code.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Records for Remaining Statuses | Generate records for all unsuccessful statuses that are not added to the Per-Status Actions list.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Error Response Body Field      | <p>Name of the field that stores the error response body for those records.</p><p>Available when generating records for remaining statuses.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Header Output Location         | Location to write response header field information.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Header Output Field            | Field to use when writing response header field information to a field in the record.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Header Attribute Prefix        | Prefix to use when writing response header field information to record header attributes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Request Transfer Encoding      | <p>Use one of the following encoding types:</p><ul><li>Buffered - The standard transfer encoding type.</li><li>Chunked - Transfers data in chunks. Not supported by all servers.</li></ul><p>Default is Buffered.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Connect Timeout                | Maximum number of milliseconds to wait for a connection.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | Read Timeout                   | Maximum number of milliseconds to wait for data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Use Proxy                      | Enables using an HTTP proxy to connect to the system.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Rate Limit                     | <p>Minimum amount of time between requests in milliseconds. Set a rate limit when sending requests to a rate-limited API.</p><p>Default is 0, which means there is no delay between requests.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Maximum Request Time           | Maximum number of seconds to wait for a request to complete.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Batch Wait Time (ms)           | Maximum number of milliseconds wait before sending a partial or empty batch.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
3.  On the Pagination tab, optionally configure pagination details.

    | Pagination Property          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Pagination Mode              | Method of pagination to use. Use a method supported by the API of the HTTP client.                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Initial Page/Offset          | Initial page for page number pagination, or the initial offset for offset number pagination.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Next Page Link Field         | <p>Field path in the response that contains the URL to the next page.</p><p>For link in response field pagination.</p>                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Stop Condition               | <p>Condition that evaluates to true when there are no more pages to process.</p><p>For link in response field pagination.</p><p>For example, let's say that the API of the HTTP client includes a count property that determines the number of items displayed per page. If the count is set to 1000 and a page returns with less than 1000 items, it is the last page of data. So you'd enter the following expression to stop processing when the count is less than 1000:</p><pre><code>${record:value('/count') &#x3C; 1000}
    </code></pre> |
    | Result Field Path            | <p>Field path in the response that contains the data that you want to process. Must be a list or array field.</p><p>The processor generates records from the specified field based on the Multiple Values Behavior property on the HTTP tab.</p>                                                                                                                                                                                                                                                                                               |
    | Keep All Fields              | <p>Includes all fields from the response in the resulting record when enabled.</p><p>By default, only the fields in the specified result field path are included in the record.</p>                                                                                                                                                                                                                                                                                                                                                            |
    | Wait Time Between Pages (ms) | The number of milliseconds to wait before requesting the next page of data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
4.  When using Basic authentication, on the Credentials tab, configure the following properties:

    | Credentials Property | Description                                               |
    | -------------------- | --------------------------------------------------------- |
    | Username             | User name for basic, digest, or universal authentication. |
    | Password             | Password for basic, digest, or universal authentication.  |
5.  When using OAuth 2 authorization, on the OAuth 2 tab, configure the following properties.For more information about OAuth 2 and for example OAuth 2 configurations to read from Twitter, Microsoft Azure AD, or Google APIs, see OAuth 2 Authorization.

    | OAuth 2 Property           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Credentials Grant Type     | <p>Type of client credentials grant type required by the HTTP service:</p><ul><li>Client credentials grant</li><li>Resource owner password credentials grant</li><li>JSON Web Tokens (JWT)</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Token URL                  | URL to request the access token.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | User Name                  | <p>Resource owner user name.</p><p>Enter for the resource owner password credentials grant.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Password                   | <p>Resource owner password.</p><p>Enter for the resource owner password credentials grant.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Client ID                  | <p>Client ID that the HTTP service uses to identify the HTTP client.</p><p>Enter for the client credentials grant that uses a client ID and secret for authentication. Or, for the resource owner password credentials grant that requires a client ID and secret.</p>                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Client Secret              | <p>Client secret that the HTTP service uses to authenticate the HTTP client.</p><p>Enter for the client credentials grant that uses a client ID and secret for authentication. Or, for the resource owner password credentials grant that requires a client ID and secret.</p>                                                                                                                                                                                                                                                                                                                                                                                            |
    | JWT Signing Algorithm      | <p>Algorithm used to sign the JSON Web Token (JWT).</p><p>Default is none. Enter for the JSON Web Tokens grant.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | JWT Signing Key            | <p>Base64 encoded key used to sign the JSON Web Token, if you selected a signing algorithm.</p><p>Enter for the JSON Web Tokens grant.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
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

    | HTTP Proxy Property | Description      |
    | ------------------- | ---------------- |
    | Proxy URI           | Proxy URI.       |
    | Username            | Proxy user name. |
    | Password            | Proxy password.  |
7.  To use SSL/TLS, on the TLS tab, configure the following properties:

    | TLS Property               | Description                                                                                                                                                                                                                                                                                                                     |
    | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Use TLS                    | Enables the use of TLS.                                                                                                                                                                                                                                                                                                         |
    | Use Remote Keystore        | Enables loading the contents of the keystore from a remote credential store or from values entered in the stage properties. For more information, see Remote Keystore and Truststore.                                                                                                                                           |
    | Private Key                | Private key used in the remote keystore. Enter a credential function that returns the key or enter the contents of the key.                                                                                                                                                                                                     |
    | Certificate Chain          | <p>Each PEM certificate used in the remote keystore. Enter a credential function that returns the certificate or enter the contents of the certificate.</p><p>Using simple or bulk edit mode, click the <strong>Add</strong> icon to add additional certificates.</p>                                                           |
    | Keystore File              | <p>Path to the local keystore file. Enter an absolute path to the file or a path relative to the Data Pipelines resources directory: $SDC_RESOURCES.</p><p>By default, no keystore is used.</p>                                                                                                                                 |
    | Keystore Type              | <p>Type of keystore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                                                                                                                                                |
    | Keystore Password          | Password to the keystore file. A password is optional, but recommended.                                                                                                                                                                                                                                                         |
    | Keystore Key Algorithm     | <p>Algorithm to manage the keystore.</p><p>Default is SunX509.</p>                                                                                                                                                                                                                                                              |
    | Use Remote Truststore      | Enables loading the contents of the truststore from a remote credential store or from values entered in the stage properties. For more information, see Remote Keystore and Truststore.                                                                                                                                         |
    | Trusted Certificates       | <p>Each PEM certificate used in the remote truststore. Enter a credential function that returns the certificate or enter the contents of the certificate.</p><p>Using simple or bulk edit mode, click the <strong>Add</strong> icon to add additional certificates.</p>                                                         |
    | Truststore File            | <p>Path to the local truststore file. Enter an absolute path to the file or a path relative to the Data Pipelines resources directory: $SDC_RESOURCES.</p><p>By default, no truststore is used.</p>                                                                                                                             |
    | Truststore Type            | <p>Type of truststore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                                                                                                                                              |
    | Truststore Password        | Password to the truststore file. A password is optional, but recommended.                                                                                                                                                                                                                                                       |
    | Truststore Trust Algorithm | <p>Algorithm to manage the truststore.</p><p>Default is SunX509.</p>                                                                                                                                                                                                                                                            |
    | Use Default Protocols      | Uses the default TLSv1.2 transport layer security (TLS) protocol. To use a different protocol, clear this option.                                                                                                                                                                                                               |
    | Transport Protocols        | <p>TLS protocols to use. To use a protocol other than the default TLSv1.2, click the Add icon and enter the protocol name. You can use simple or bulk edit mode to add protocols.</p><p>Note: Older protocols are not as secure as TLSv1.2.</p>                                                                                 |
    | Use Default Cipher Suites  | Uses a default cipher suite for the SSL/TLS handshake. To use a different cipher suite, clear this option.                                                                                                                                                                                                                      |
    | Cipher Suites              | <p>Cipher suites to use. To use a cipher suite that is not a part of the default set, click the Add icon and enter the name of the cipher suite. You can use simple or bulk edit mode to add cipher suites.</p><p>Enter the Java Secure Socket Extension (JSSE) name for the additional cipher suites that you want to use.</p> |
8.  On the Data Format tab, configure the following property:

    | Data Format Property | Description                                                                                          |
    | -------------------- | ---------------------------------------------------------------------------------------------------- |
    | Data Format          | <p>Data format of the response:</p><ul><li>Delimited</li><li>JSON</li><li>Text</li><li>XML</li></ul> |
9.  For delimited data, on the Data Format tab, configure the following properties:

    | Delimited Property                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Header Line                                   | Indicates whether a file contains a header line, and whether to use the header line.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Delimiter Format Type                         | <p>Delimiter format type. Use one of the following options:</p><ul><li>Default CSV - File that includes comma-separated values. Ignores empty lines in the file.</li><li>RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.</li><li>MS Excel CSV - Microsoft Excel comma-separated file.</li><li>MySQL CSV - MySQL comma-separated file.</li><li>Tab-Separated Values - File that includes tab-separated values.</li><li>PostgreSQL CSV - PostgreSQL comma-separated file.</li><li>PostgreSQL Text - PostgreSQL text file.</li><li>Custom - File that uses user-defined delimiter, escape, and quote characters.</li><li>Multi Character Delimited - File that uses multiple user-defined characters to delimit fields and lines, and single user-defined escape and quote characters.</li></ul><p>Available when using the Apache Commons parser type.</p> |
    | Multi Character Field Delimiter               | <p>Characters that delimit fields.</p><p>Default is two pipe characters (||).</p><p>Available when using the Apache Commons parser with the multi-character delimiter format.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Multi Character Line Delimiter                | <p>Characters that delimit lines or records.</p><p>Default is the newline character (\n).</p><p>Available when using the Apache Commons parser with the multi-character delimiter format.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | Delimiter Character                           | <p>Delimiter character. Select one of the available options or use Other to enter a custom character.</p><p>You can enter a Unicode control character using the format \uNNNN, where ​<em>N</em> is a hexadecimal digit from the numbers 0-9 or the letters A-F. For example, enter \u0000 to use the null character as the delimiter or \u2028 to use a line separator as the delimiter.</p><p>Default is the pipe character ( | ).</p><p>Available when using the Apache Commons parser with a custom delimiter format.</p>                                                                                                                                                                                                                                                                                                                                                         |
    | Field Separator                               | <p>One or more characters to use as delimiter characters between columns.</p><p>Available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Escape Character                              | <p>Escape character.</p><p>Available when using the Apache Commons parser with the custom or multi-character delimiter format. Also available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Quote Character                               | <p>Quote character.</p><p>Available when using the Apache Commons parser with the custom or multi-character delimiter format. Also available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Line Separator                                | <p>Line separator.</p><p>Available when using the Univocity parser.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
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
10. For JSON data, on the Data Format tab, configure the following properties:

    | JSON Property                                 | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Max Object Length (chars)                     | <p>Maximum number of characters in a JSON object.</p><p>Longer objects are diverted to the pipeline for error handling.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                                                                               |
    | Charset                                       | Character encoding of the data to be processed.                                                                                                                                                                                                                                                                                                                                                       |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                   |
11. For text data, on the Data Format tab, configure the following properties:

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
12. For XML data, on the Data Format tab, configure the following properties:

    | XML Property                                  | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Delimiter Element                             | XML element that acts as a delimiter. Omit a delimiter to treat the entire XML document as one field.                                                                                                                                                                                                                                                                                                 |
    | Preserve Root Element                         | <p>Includes the root element in the generated records.</p><p>When omitting a delimiter, the root element is the root element of the XML document. When specifying a delimiter, the root element is the XML element specified as the delimiter element.</p>                                                                                                                                            |
    | Max Record Length (chars)                     | <p>The maximum number of characters in a field. Longer fields are diverted to the pipeline for error handling.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                                                                                        |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                      |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                   |
13. On the Logging tab, configure the following properties to log request and response data:

    | Logging Property       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Enable Request Logging | <p>Enables logging request and response data.</p><p>For information about logging the resolved resource URL, see Logging the Resolved Resource URL.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Log Level              | <p>The level of detail to be logged. Choose one of the available options.The following list is in order of lowest to highest level of logging. When you select a level, messages generated by the levels above the selected level are also written to the log:</p><ul><li>Severe - Only messages indicating serious failures.</li><li>Warning - Messages warning of potential problems.</li><li>Info - Informational messages.</li><li>Fine - Basic tracing information.</li><li>Finer - Detailed tracing information.</li><li>Finest - Highly detailed tracing information.</li></ul><p>Note: The log level configured for Data Pipelines can limit the level of messages that the stage writes. Verify that the Data Pipelines log level supports the level that you want to use.</p> |
    | Verbosity              | <p>The type of data to include in logged messages:</p><ul><li>Headers_Only - Includes request and response headers.</li><li>Payload_Text - Includes request and response headers as well as any text payloads.</li><li>Payload_Any - Includes request and response headers and the payload, regardless of type.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Max Entity Size        | The maximum size of message data to write to the log. Use to limit the volume of data written to the Data Pipelines log for any single message.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
14. On the Timeout tab, configure the following properties:

    | Timeout Property   | Description                                                                                                                                               |
    | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Action for Timeout | Action to take when the request times out because the HTTP service did not respond within the read timeout period.                                        |
    | Max Retries        | <p>Maximum number of times to retry the request before failing the stage. A negative value allows an infinite number of retries.</p><p>Default is 10.</p> |
