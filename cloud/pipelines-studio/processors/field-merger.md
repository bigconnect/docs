# Field Merger

The Field Merger merges one or more fields in a record to a different location in the record. Use only for records with a list or map structure.

When you configure the Field Merger, you select the fields to be merged and define the target fields. You can merge fields into a new field or an existing field:New target fieldWhen you define a new field, the Field Merger creates the new field and merges the selected fields under the new field.

For example, you select the following fields in a JSON map:

```
/City
/State
```

If you create a new target field called `/Location`, the Field Merger merges the fields as follows:

```
/Location/City
         /State
```

Existing target fieldWhen you choose an existing field as the target field, the Field Merger merges the selected fields under the target field. You can optionally overwrite target fields of the same name if they already exist.

### Configuring a Field Merger <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a Field Merger to merge or move fields within a list or map record.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Merge tab, configure the following properties:

    | Field Merger Properties     | Description                                                                                                                                                                                                                         |
    | --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Source Field Does Not Exist | <p>Action to take if the source field does not exist in a record:</p><ul><li>Continue - Passes nulls for the missing values.</li><li>Send to Error - Passes the record to the pipeline for error handling.</li></ul>                |
    | Overwrite Fields            | Overwrites any existing fields in the target field with names that match the merge fields.                                                                                                                                          |
    | Fields to Merge             | <p>One or more fields to merge. Select the existing fields that you want to merge and the corresponding target fields.</p><p>For the target fields, use an existing field or enter a field name to be created before the merge.</p> |
