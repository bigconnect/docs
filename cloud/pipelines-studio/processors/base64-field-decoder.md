# Base64 Field Decoder

The Base64 Field Decoder decodes Base64 encoded data to binary data. Use the processor to decode Base64 encoded data before evaluating data in the field.

When you configure Base64 Field Decoder, you specify the byte array field to decode and the target byte array field to pass the decoded value to.

### Configuring a Base64 Field Decoder <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a Base64 Field Decoder to decode Base64 encoded data to binary data. The processor can decode data from a single byte array field. To decode additional fields, add additional Base64 Field Decoders to the pipeline.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Base64 tab, configure the following properties:

    | Base64 Field Decoder Property | Description                                                                                                                                                                                                                                                                                |
    | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Field to Decode               | Byte array field to decode.                                                                                                                                                                                                                                                                |
    | Target Field                  | <p>Byte array field in the record to use for the decoded data.</p><p>You can specify the same field to replace the original data with the decoded data. Or you can specify another existing field or a new field. If the field does not exist, Base64 Field Decoder creates the field.</p> |
