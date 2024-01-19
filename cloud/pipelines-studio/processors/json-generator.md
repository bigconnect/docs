# JSON Generator

The JSON Generator serializes data in a field to a JSON-encoded string. You can serialize data from List, Map, or List-Map fields.

When you configure the processor, you select the field that you want to convert and the field to write the JSON string data to.

If the target field exists, the JSON Generator overwrites the data in the field. If the data does not exist, the JSON Generator creates the field.

### Configuring a JSON Generator <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a JSON Generator to serialize data in a field to JSON data in a string field.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Parse tab, configure the following properties:

    | JSON Generator Property | Description                                                                                                                                                                                  |
    | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Field to Serialize      | The field that contains the data to process. Select a List, Map, or List-Map field.                                                                                                          |
    | Target Field            | <p>The field to write the resulting JSON data to.</p><p>If you specify an existing field, the existing data is overwritten. If you specify a new field, the processor creates the field.</p> |
