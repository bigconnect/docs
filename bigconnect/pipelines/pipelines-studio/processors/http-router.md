# HTTP Router

The HTTP Router processor passes records to data streams based on the HTTP method and URL path in the record header attributes.&#x20;

You can use the HTTP Router processor in pipelines with origins that create the following record header attributes:

* method - The HTTP method for the request, such as GET, POST, or DELETE.
* path - The path of the URL.

The HTTP Server origin and the REST Service origin generate these record header attributes. For example, in a microservice pipeline you might use the HTTP Router processor to pass data from the REST Service origin based on the method and path in record header attributes.

When you configure the HTTP Router processor, you define data streams by identifying the method and the path found in the record header attributes of records that you want included in the stream. If an input record does not have record header attributes that match a defined stream, the processor applies error handling to the record.

### Configuring an HTTP Router Processor <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure an HTTP Router processor to pass records to one or more data streams based on the HTTP method and URL path in the record header attributes.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Router tab, configure the following properties for each data stream or output location that you want to create:

    | Router Property | Description                                                                                                                                                                               |
    | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | HTTP Method     | <p>HTTP method specified in the record header attribute. Select one of the following methods:</p><ul><li>GET</li><li>PUT</li><li>POST</li><li>PATCH</li><li>HEAD</li><li>DELETE</li></ul> |
    | Path Parameter  | URL path specified in the record header attribute.                                                                                                                                        |

    If an input record does not have record header attributes that match a configured data stream, the processor applies error handling to the record.
