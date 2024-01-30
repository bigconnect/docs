# HTTP Client

The HTTP Client destination writes data to an HTTP endpoint. The destination sends requests to an HTTP resource URL.&#x20;

Use the HTTP Client destination to perform a range of standard requests or use an expression to determine the request for each record. The destination can also send responses to a microservice origin when used in a microservice pipeline.

When you configure the HTTP Client destination, you define the resource URL, headers, and method to use for the requests. You configure the destination to generate one request for each record or it to generate a single request containing all records in the batch.

You can configure the timeout, request transfer encoding, and authentication type. You can configure the destination to use the Gzip or Snappy compression format to write the messages. You can also configure the processor to log request and response information.

You can optionally use an HTTP proxy and configure SSL/TLS properties. You can also configure the destination to use the OAuth 2 protocol to connect to an HTTP service.

When you want the destination to send responses to a microservice origin within a microservice pipeline, you specify the type of response to send.

### HTTP Method <a href="#ariaid-title2" id="ariaid-title2"></a>

You can use the following methods with the HTTP Client destination:

* GET
* PUT
* POST
* DELETE
* HEAD
* PATCH
* Expression - An expression that evaluates to one of the other methods.

#### Expression Method <a href="#ariaid-title3" id="ariaid-title3"></a>

The Expression method allows you to write an expression that evaluates to a standard HTTP method. Use the Expression method to generate a workflow. For example, you can use an expression that passes data to the server (PUT) based on the data in a field.

### Number of Requests <a href="#ariaid-title4" id="ariaid-title4"></a>

The HTTP Client destination can generate one HTTP request for each record, or it can generate a single request containing all records in the batch.

Configure the destination to generate requests in one of the following ways:

**Multiple requests per batch**

By default, the destination generates one HTTP request for each record in the batch and sends multiple requests at a time. To preserve record order, the destination waits until all requests for the entire batch are completed before processing the next batch.

When the destination generates multiple requests per batch, you specify the maximum number of parallel requests. Default is 1. Increasing the number of parallel requests can improve performance but increases the load on the server. Network latency can also significantly impact the performance when sending multiple parallel requests.

**Single request per batch**

If you enable the One Request per Batch property, the destination generates a single HTTP request containing all records in the batch.

When the destination generates a single request per batch, it ignores the value entered for the maximum number of parallel requests. Generating a single request per batch can improve performance, depending on the amount of data sent in the request.

### Logging Request and Response Data <a href="#ariaid-title5" id="ariaid-title5"></a>

The HTTP Client destination can log request and response data to the Data Pipelines log.

When enabling logging, you configure the following properties:

VerbosityThe type of data to include in logged messages:

* Headers\_Only - Includes request and response headers.
* Payload\_Text - Includes request and response headers as well as any text payloads.
* Payload\_Any - Includes request and response headers and the payload, regardless of type.

**Log Level**

The level of messages to include in the Data Pipelines log. When you select a level, higher level messages are also logged. That is, if you select the Warning log level, then Severe and Warning messages are written to the Data Pipelines log.

Note: The configured log level for Data Pipelines can limit the level of detail that is logged. For example, if you set the log level to Finest to log detailed trace information, but Data Pipelines is configured for ERROR, then the origin only writes Severe level messages.

The following table describes the stage log levels and the corresponding Data Pipelines log levels needed to enable the logging:

| Stage Log Level | Data Pipelines | Description                                |
| --------------- | -------------- | ------------------------------------------ |
| Severe          | ERROR          | Only messages indicating serious failures. |
| Warning         | WARN           | Messages warning of potential problems.    |
| Info            | INFO           | Informational messages.                    |
| Fine            | DEBUG          | Basic tracing information.                 |
| Finer           | DEBUG          | Detailed tracing information.              |
| Finest          | TRACE          | Highly detailed tracing information.       |

The name of this stage logger is `com.streamsets.http.RequestLogger`.

**Max entity size**

The maximum size of message data to write to the log. Use to limit the volume of data written to the Data Pipelines log for any single message.

### Send Microservice Responses <a href="#ariaid-title6" id="ariaid-title6"></a>

The HTTP Client destination can send responses to a microservice origin when you use the destination in a microservice pipeline.

When sending responses to a microservice origin, the destination can send one of the following:

* All successfully written records.
* Responses from the destination system - For information about the possible responses, see the documentation for the destination system.

### OAuth 2 Authorization <a href="#ariaid-title7" id="ariaid-title7"></a>

The HTTP Client destination can use the OAuth 2 protocol to connect to an HTTP service.

The HTTP Client destination can use the OAuth 2 protocol to connect to an HTTP service that uses basic, digest, or universal authentication, OAuth 2 client credentials, OAuth 2 username and password, or OAuth 2 JSON Web Tokens (JWT).

The OAuth 2 protocol authorizes third-party access to HTTP service resources without sharing credentials. The HTTP Client destination uses credentials to request an access token from the service. The service returns the token to the destination, and then the destination includes the token in a header in each request to the resource URL.

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

#### Example for Twitter <a href="#ariaid-title8" id="ariaid-title8"></a>

To use OAuth 2 authorization to write to Twitter, configure HTTP Client to use basic authentication and the client credentials grant.

For more information about configuring OAuth 2 authorization for Twitter, see [https://developer.twitter.com/en/docs/authentication/oauth-2-0/application-only](https://developer.twitter.com/en/docs/authentication/oauth-2-0/application-only).

1. On the HTTP tab, set Authentication Type to Basic, and then select Use OAuth 2.
2.  Change the default value of the Resource URL property to define a Twitter URL that works with OAuth 2 authorization.

    The default URL is not valid when using OAuth2 authorization.

    For example, you might define the following URL that works with OAuth 2 authorization: `https://api.twitter.com/1.1/search/tweets.json?q=%23DataOps`
3. On the Credentials tab, enter the Twitter consumer API key and consumer API secret key for the Username and Password properties.
4. On the OAuth 2 tab, select Client Credentials Grant for the grant type.
5.  In the Token URL property, enter the following URL used to request the access token:

    ```
    https://api.twitter.com/oauth2/token
    ```

The following image shows the OAuth 2 tab configured for Twitter:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HTTPClientProcessor\_OAuth2Twitter.png)

#### Example for Microsoft Azure AD <a href="#ariaid-title9" id="ariaid-title9"></a>

To use OAuth 2 authorization to write to Microsoft Azure AD, configure HTTP Client to use no authentication and the client credentials grant.

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

#### Example for Google <a href="#ariaid-title10" id="ariaid-title10"></a>

To use OAuth 2 authorization to write to Google service accounts, configure HTTP Client to use no authentication and the JSON Web Tokens grant.

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

### Data Formats <a href="#ariaid-title11" id="ariaid-title11"></a>

HTTP Client writes data to HTTP endpoints based on the data format that you select.

The HTTP Client destination processes data formats as follows:

**Avro**

The stage writes records based on the Avro schema. You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. The Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in the Confluent Schema Registry by the schema ID or subject.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the stage to register the Avro schema with the Confluent Schema Registry. You can also optionally include the schema definition in the message. Omitting the schema definition can improve performance, but requires the appropriate schema management to avoid losing track of the schema associated with the data.

You can include the Avro schema in the output.You can also compress data with an Avro-supported compression codec. When using Avro compression, avoid configuring any other compression properties in the stage.

**Binary**

The stage writes binary data to a single field in the record.

**Delimited**

The destination writes records as delimited data. When you use this data format, the root field must be list or list-map.You can use the following delimited format types:

* Default CSV - File that includes comma-separated values. Ignores empty lines in the file.
* RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.
* MS Excel CSV - Microsoft Excel comma-separated file.
* MySQL CSV - MySQL comma-separated file.
* Tab-Separated Values - File that includes tab-separated values.
* PostgreSQL CSV - PostgreSQL comma-separated file.
* PostgreSQL Text - PostgreSQL text file.
* Custom - File that uses user-defined delimiter, escape, and quote characters.
* Multi Character Delimited - File that uses multiple user-defined characters to delimit fields and lines, and single user-defined escape and quote characters.

**JSON**

The destination writes records as JSON data. You can use one of the following formats:

* Array - Each file includes a single array. In the array, each element is a JSON representation of each record.
* Multiple objects - Each file includes multiple JSON objects. Each object is a JSON representation of a record.

**Protobuf**

Writes one record in a message. Uses the user-defined message type and the definition of the message type in the descriptor file to generate the message.

**Record**

The destination writes records in the Record data format.

**Text**

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.

When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.

When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

### &#x20;<a href="#ariaid-title12" id="ariaid-title12"></a>

\


\
