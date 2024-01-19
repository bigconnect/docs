# Base64 Field Encoder

The Base64 Field Encoder encodes binary data using Base64. Use the processor to encode binary data that must be sent over channels that expect ASCII data.

For example, you can use Base64 encoding to embed image data directly in HTML source code. When you encode the data, you prevent characters such as “<” and “>” from being interpreted as tags.

When you configure Base64 Field Encoder, you specify the byte array field to encode and the target byte array field to pass the encoded value to. You can also specify that the processor encode the data so that it can be safely sent in a URL.

### Configuring a Base64 Field Encoder <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a Base64 Field Encoder to encode binary data using Base64. The processor can encode data from a single byte array field. To encode additional fields, add additional Base64 Field Encoders to the pipeline.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Base64 tab, configure the following properties:

    | Base64 Field Encoder Property | Description                                                                                                                                                                                                                                                                                       |
    | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Field to Encode               | Byte array field to encode.                                                                                                                                                                                                                                                                       |
    | Target Field                  | <p>Byte array field in the record to use for the encoded data.</p><p>You can specify the same field to replace the original binary data with the encoded data. Or you can specify another existing field or a new field. If the field does not exist, Base64 Field Encoder creates the field.</p> |
    | URL Safe                      | Specifies whether to encode the field so that it can be safely sent in a URL. For example, when selected, Base64 Field Encoder ensures that the encoded data does not contain a forward slash (/) because it is a URL reserved character.                                                         |
