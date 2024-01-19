# Field Splitter

The Field Splitter splits string data based on a regular expression and passes the separated data to new fields. Use the Field Splitter to split complex string values into logical components.

For example, if a field contains an error code and error message separated by a comma, you can use the comma to separate the code and message into different fields.

When you configure the Field Splitter, you specify the field to split, the regular expression to use as a separator, and the fields to use for split data. You configure what to do when a record does not have the expected number of splits, and what to do when a record has additional data with more possible splits.

You can keep the original field being split or discard it.

### Not Enough Splits <a href="#ariaid-title2" id="ariaid-title2"></a>

A field has not enough splits when the data does not include as many splits as the number of split fields listed in the processor.

When a field has not enough splits, the Field Splitter can continue or process the record based on the configured stage error handling. When continuing, the processor passes the record with the data split as much as possible, passing nulls for unused split fields.

For example, say the data in a field has only one split separator. This produces data to be written to two split fields, but the processor has three listed split fields. In this case, the processor handles the record based on the Not Enough Splits property.

### Too Many Splits <a href="#ariaid-title3" id="ariaid-title3"></a>

When a field has too many splits, the Field Splitter can include all remaining data in the last listed split field, or it can write the additional splits to a specified list field.

### Example <a href="#ariaid-title4" id="ariaid-title4"></a>

The following Field Splitter uses a comma to split data into two fields: ErrorCode and ErrorMsg. The Not Enough Splits property sends records that do not have enough splits to the stage for error handling, and the stage is configured to discard error records. The Too Many Splits property writes additional data to a MoreInfo list field.

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldSplitter.png)

Say the pipeline passes the processor the following set of records:

| DATETIME              | ERROR                                                                                         |
| --------------------- | --------------------------------------------------------------------------------------------- |
| `21-09-2016 15:33:02` | `GM-302,information that you might need`                                                      |
| `21-09-2016 15:35:53` | `ME-3042,message about error,additional information from server, network error, driver error` |
| `21-09-2016 15:55:48` | `IMD-03234`                                                                                   |

When Field Splitter encounters a comma in the errors field, it passes the data before the comma to the ErrorCode field and the data after the comma to the ErrorMsg field and writes any additional fields to the MoreInfo list field.

The Field Splitter produces the following records. The processor discards IMD-03234 because the record does not include enough data to be split, and the processor is configured to discard those records.

| DATETIME              | ERRORCODE | ERRORMSG                          | MOREINFO                                                                                                                      |
| --------------------- | --------- | --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `21-09-2016 15:33:02` | `GM-302`  | `information that you might need` |                                                                                                                               |
| `21-09-2016 15:35:53` | `ME-3042` | `message about error`             | <p><code>- additional information from server</code></p><p><code>- network error</code></p><p><code>- driver error</code></p> |

### Configuring a Field Splitter <a href="#ariaid-title5" id="ariaid-title5"></a>

Configure a Field Splitter to split data from a single field into multiple fields. You can split data from a single field. To split additional fields, add another Field Splitter to the pipeline.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Split tab, configure the following properties:

    | Field Splitter Property    | Description                                                                                                                                                                                                                                                                                                                                   |
    | -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Field to Split             | The String field to be split.                                                                                                                                                                                                                                                                                                                 |
    | Separator                  | The regular expression to use to split data in a field. For some tips on using regular expressions, see Regular Expressions Overview.                                                                                                                                                                                                         |
    | New Split Fields           | Names of the new fields to pass the split data.Note: Precede each field name with a slash as follows: `/NewField`.                                                                                                                                                                                                                            |
    | Not Enough Splits          | <p>Record handling when the data does not include as many splits as the specified number of split fields:</p><ul><li>Continue - Passes the record split as much as possible with null values in unused split fields.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li></ul>                                   |
    | Too Many Splits            | <p>Record handling when the data contains more potential splits than the specified number of split fields:</p><ul><li>Put Remaining Text in Last Field - Writes any additional data to the last split field.</li><li>Store Remaining Splits as List - Splits the additional data and writes the splits to the specified List field.</li></ul> |
    | Field for Remaining Splits | List field for remaining splits. Used when the data includes more splits than expected by the processor.                                                                                                                                                                                                                                      |
    | Original Field             | <p>Determines how to handle the original field being split:</p><ul><li>Remove</li><li>Keep</li></ul>                                                                                                                                                                                                                                          |
