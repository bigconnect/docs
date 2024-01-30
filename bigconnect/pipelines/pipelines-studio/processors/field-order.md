# Field Order

The Field Order processor orders fields in a map or list-map field and outputs the fields into a list-map or list root field.

Use the Field Order processor for the following reasons:

Order the fields in a map fieldFields in a map field have no defined order. Use the Field Order processor to specify the order of the fields and then output the fields to either the list-map or list root field which both retain field order.For example, let’s say your pipeline reads JSON data and you want to write that data using the delimited data format. Data Collector uses a map root field for JSON data. However, for a destination to write records as delimited data, the root field type must be list or list-map. You use the Field Order processor to order the fields in the map root field and output the ordered fields to a list-map root field. Then you can write the records as delimited data.Reorder the fields in a list-map fieldFields in a list-map field have a defined order. Use the Field Order processor when you need to reorder the fields, and then output the fields to the list-map root field.

When you configure the Field Order processor, you define the order of the fields. You specify whether to output the ordered fields to a list-map or list root field.

You configure how the processor handles missing or extra fields in the record. You can also list any fields that you want the processor to discard from the record.

### Missing and Extra Fields <a href="#ariaid-title2" id="ariaid-title2"></a>

When you configure the Field Order processor, you specify the list of fields to order. An incoming record might be missing one of those fields or it might have some extra fields. You configure how the processor handles any missing or extra fields.

The Field Order processor can encounter the following types of fields in each record:

**Missing fields**

Missing fields are fields that you specified in the ordered list but that do not exist in the record. For example, you list these fields in the following order: FirstName, LastName, Age. However, an incoming record doesn't include the Age field.

When the processor encounters a record with a missing field, it can either handle the record based on the error handling configured for the stage or it can use a default value for the missing field. If you configure the processor to use a default value, you enter the default value and the data type to use for the missing fields. The processor assigns the same default value and type to each missing field.

**Extra fields**

Extra fields are fields that you did not specify in the ordered list but that do exist in the record. For example, you list these fields in the following order: FirstName, LastName, Age. However, an incoming record includes a MiddleName field.When the processor encounters a record with an extra field, it can either handle the record based on the error handling configured for the stage or it can discard all extra fields.

### Configuring a Field Order Processor <a href="#ariaid-title3" id="ariaid-title3"></a>

Configure a Field Order processor to order fields in a map or list-map field.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Order tab, configure the following properties:

    | Order Property  | Description                                                                                                                                                                                                                                                                            |
    | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Fields to Order | <p>List of fields in the desired order.</p><p>Type the fields in the list, or select the fields using preview data.</p>                                                                                                                                                                |
    | Output Type     | Root field type to output the ordered fields to: list-map or list.                                                                                                                                                                                                                     |
    | Missing Fields  | <p>Action to take if a field in the ordered list is missing in a record:</p><ul><li>Send record to error</li><li>Use default value</li></ul><p>If multiple fields are missing, the processor assigns the same default value and data type to each field.</p>                           |
    | Default Value   | Default value to use for missing fields.                                                                                                                                                                                                                                               |
    | Default Type    | Default data type to use for missing fields.                                                                                                                                                                                                                                           |
    | Extra Fields    | <p>Action to take if the record includes an extra field that is not specified in the ordered list:</p><ul><li>Send record to error</li><li>Discard all extra fields</li></ul>                                                                                                          |
    | Discard Fields  | <p>List of fields to discard from the record.</p><p>Type the fields in the list, or select the fields using preview data.</p><p>These fields cannot also be specified in the ordered list. This field does not display if the processor is configured to discard all extra fields.</p> |
