# Expression Evaluator

The Expression Evaluator performs calculations and writes the results to new or existing fields. You can also use the Expression Evaluator to add or modify record header attributes and field attributes.

To create an expression, you define the name of the field, record header attribute, or field attribute to receive the results of the expression. And then, use the Data Pipelines expression language to define the expression to use.

You can use runtime parameters in expressions. Define a runtime parameter when you want to specify the value for a pipeline property when you start the pipeline.

You can also use the `time:now()` function to include the Data Pipelines server time in the record.

For more information about configuring expressions, see Expression Configuration.

### Output Fields and Attributes <a href="#ariaid-title2" id="ariaid-title2"></a>

When you configure an expression, the Expression Evaluator writes the results of the expression to an output field or attribute. You can use an existing field or attribute, or create a new one.

When you use an existing field or attribute, the Expression Evaluator replaces the incoming value with the new value. When you use a new field or attribute, the Expression Evaluator adds it to the record and passes the results of the expression.

### Record Header Attribute Expressions <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use expressions to add or modify header attributes for a record.

For example, you might use Expression Evaluator to set the sdc.operation.type attribute to enable writing to MongoDB.

Some destinations can use record header attributes to perform record-based writes. For more information, see Record Header Attributes for Record-Based Writes. For general information about record header attributes, see Record Header Attributes.

### Field Attribute Expressions <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use expressions to add or modify field attributes for a record. For example, you can create a field attribute based on record data, then pass the record to a Stream Selector that routes data based on the attribute value.

You can also use field attribute functions in a field expression to include field attribute values in the record.

For more information about field attributes, see Field Attributes. For more information about field attribute functions, see Record Functions.

### Configuring an Expression Evaluator <a href="#ariaid-title5" id="ariaid-title5"></a>

Configure an Expression Evaluator to perform calculations on a record-by-record basis.

For more information about the StreamSets expression language, see Expression Language. For more information about configuring expressions, see Expression Configuration.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  To configure a field expression, click the Expressions tab and configure the following information:

    | Field Expression Property | Description                                                                                                                                                                                                                                                                                                             |
    | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Output Field              | <p>The field to pass the results of the expression to the next stage. Enter the name of a new or existing field as follows: /FieldName.</p><p>If you use an existing field, the Expression Evaluator replaces existing values.</p><p>You can use the asterisk wildcard to represent array indices and map elements.</p> |
    | Field Expression          | <p>Expression to evaluate.</p><p>Optionally, click Ctrl + Space Bar for help with creating the expression.</p>                                                                                                                                                                                                          |
3. Using simple or bulk edit mode, click the Add icon to add additional field expressions.
4.  To configure a record header attribute expression, configure the following information:

    | Record Header Attribute Expression Property | Description                                                                                                                                                                                                                                                                                                                                                        |
    | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Header Attribute                            | <p>The record header attribute to pass the results of the expression to the next stage. Enter the name of a new or existing attribute as follows: &#x3C;attribute name>.</p><p>When you use an existing attribute, the Expression Evaluator replaces existing values.</p><p>Note: Avoid changing values for header attributes generated by the Data Pipelines.</p> |
    | Header Attribute Expression                 | <p>Expression to evaluate.</p><p>Optionally, click Ctrl + Space Bar for help with creating the expression.</p>                                                                                                                                                                                                                                                     |
5. Using simple or bulk edit mode, click the Add icon to add additional header attribute expressions.
6.  To configure a field attribute expression, configure the following information:

    | Field Attribute Expression Property | Description                                                                                                                                                                                                                                                       |
    | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Field Attribute                     | <p>The field attribute to pass the results of the expression to the next stage. Enter the name of a new or existing attribute as follows: &#x3C;attribute name>.</p><p>When you use an existing attribute, the Expression Evaluator replaces existing values.</p> |
    | Field Attribute Expression          | <p>Expression to evaluate.</p><p>Optionally, click Ctrl + Space Bar for help with creating the expression.</p>                                                                                                                                                    |
7. Using simple or bulk edit mode, click the Add icon to add additional field attribute expressions.
