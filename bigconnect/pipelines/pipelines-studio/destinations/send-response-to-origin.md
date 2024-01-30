# Send Response to Origin

The Send Response to Origin destination passes records to the REST Service origin in the pipeline and allows you to specify an HTTP status code for those records.

Use the Send Response to Origin destination with the REST Service origin in a microservices pipeline.

When you configure the Send Response to Origin destination, you specify the name and optional description for the stage and the status code to use for the records that it receives.

### Configuring a Send Response to Origin Destination <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a Send Response to Origin destination to send records with the specified status code to the REST Service origin in the pipeline. This allows the origin to pass the records back to the originating REST API.On the General tab, configure the following properties:

| General Property | Description                                                                                                                                                                                                                                  |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name             | Stage name.                                                                                                                                                                                                                                  |
| Description      | Optional description.                                                                                                                                                                                                                        |
| Status Code      | <p>Status code to include in records passed to the REST Service origin.</p><p>For information about HTTP status codes, see the <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status">Mozilla HTTP Status documentation</a>.</p> |
