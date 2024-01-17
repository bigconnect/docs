# JSON Parser

The JSON Parser parses a JSON object embedded in a string field and passes the parsed data to an output field in the record. Based on the contents of the JSON object, the resulting field is either a string, a map, or an array.

### Configuring a JSON Parser <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a JSON Parser to parse a JSON object in a String field.

When you configure a JSON Parser, specify the field to parse and the output field to use.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Parse tab, configure the following properties:

    | JSON Parser Property      | Description                                                                                                                                                                                                                                                   |
    | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Field to Parse            | Field that contains the JSON object.                                                                                                                                                                                                                          |
    | Ignore Control Characters | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                           |
    | Target Field              | <p>Output field for the parsed JSON object.</p><p>You can specify the same field to replace the original data with the parsed data. Or you can specify another existing field or a new field. If the field does not exist, JSON Parser creates the field.</p> |
