# Error Record Functions

Error record functions provide information about error records. Use error functions to process error records.

For example, you might use error functions in a Stream Selector to pass data to different error handling pipelines based on the type of error that occurred.

The expression language provides the following error functions:

### record:errorCode()

Returns the error code for the error record.&#x20;

Return type: String.

### record:errorCollectorId()&#x20;

Returns the ID of the Data Collector that sent the record to error. By default, the Data Collector ID uses the format \<hostname>:\<port>. You can verify the Data Collector ID in the Data Collector Configuration page.&#x20;

Return type: String.

### record:errorMessage()

Returns the error message for the error record.&#x20;

Return type: String.

### record:errorPipeline()

The name of pipeline that sent the record to error.

Return type: String.&#x20;

### record:errorStackTrace()&#x20;

Returns the error stack trace for the error record.&#x20;

Return type: String.&#x20;

### record:errorStage()&#x20;

Returns the instance name of the stage that sent the record to error. You can verify the instance name on the General tab of the stage.&#x20;

### Return type: String.&#x20;

record:errorStageLabel() Returns the user-defined label for the stage that sent the record to error.&#x20;

Return type: String.&#x20;

### record:errorTime()&#x20;

The time the record was sent to error.&#x20;

Return type: Long.

