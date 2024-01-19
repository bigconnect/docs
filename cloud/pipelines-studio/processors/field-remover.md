# Field Remover

The Field Remover processor removes fields from records. Use the processor to discard field data that you do not need in the pipeline.When you configure the processor, you specify a list of fields and the action for the Field Remover processor to take with those fields:

* Keep the listed fields and remove all other fields
* Remove the listed fields
* Remove any listed field that has a particular value:
  * Null
  * Empty string
  * Specified constant

### Configuring a Field Remover Processor <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a Field Remover processor to remove unwanted fields from the pipeline.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
2.  On the Remove tab, configure the following properties:

    | Field Remover Property | Description                                                                                                                                                                                                                                                                                                                                                   |
    | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Action                 | <p>Action to complete:</p><ul><li>Keep listed fields</li><li>Remove listed fields</li><li>Remove any listed field if its value is null</li><li>Remove any listed field if its value is an empty string</li><li>Remove any listed field if its value is null or an empty string</li><li>Remove any listed field if its value is a specified constant</li></ul> |
    | Fields                 | <p>List of fields to keep or remove.</p><p>You can use the asterisk wildcard to represent array indices and map elements.</p><p>You can specify individual fields or use a field path expression to specify a set of fields.</p>                                                                                                                              |
    | Constant               | <p>Field value that results in removal of the field.</p><p>Available when removing fields with specified constants.</p>                                                                                                                                                                                                                                       |
