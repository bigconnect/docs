# Field Renamer

Use the Field Renamer to rename fields in a record. You can specify individual fields to rename or use regular expressions to rename sets of fields.

You can configure the behavior when a source field does not exist, when a target field with a matching name already exists, and when a source field matches multiple source field expressions.

Fields that are not renamed or overwritten pass to the next stage.

Note: The Field Renamer processor cannot rename fields that contain a backslash character ( \ ).

### Renaming Sets of Fields <a href="#ariaid-title2" id="ariaid-title2"></a>

You can use regular expressions, or regex, along with the Data Pipelines expression language to rename sets of fields. You can use regex to define the set of source fields to rename and to define the target field names. You can also use the StreamSets expression language to define target field names.

Below are some examples of how to use expressions to rename sets of fields:Remove a prefix or suffixSay you want to remove an OPS prefix from a set of fields. You can do this by using the following expression to define the source fields to change:

```
/'OPS(.*)'
```

Then use the following expression to remove the OPS prefix:

```
/$1
```

Or, say after using a Field Flattener processor to flatten XML data, all fields have a suffix of .0.value. You can remove the suffix by using the following expression to specify the source field names:

```
/'(.*)\.0\.value'
```

Then use the following expression to define the target field names:

```
/$1
```

Remove special charactersTo remove special characters from field names, you can use the following expression for the source field names:

```
/'([A-Z a-z]*)[^a-z A-Z 0-9]([A-Z a-z 0-9]*)'
```

Then use the following expression for target field names:

```
/$1_invalid_character_removed_$2
```

Change capitalizationTo change field names to all uppercase, use the following expression for the source field names:

```
/(.*)
```

Then use the following expression for the target field names:

```
/${str:toUpper("$1")}
```

To change field names to all lowercase, use the following expression for the source field names:

```
/(.*)
```

Then use the following expression for the target field names:

```
/${str:toLower("$1")}
```

Note: To include regex special characters such as the pipe symbol (|) in field names, use single quotation marks to surround the field name. For example, if you have a field named `tag|attr`, enter the field name as follows:

```
/'tag|attr'
```

### Configuring a Field Renamer <a href="#ariaid-title3" id="ariaid-title3"></a>

Configure a Field Renamer to rename fields in a record.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Rename tab, configure the following properties:

    | Field Renamer Property        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Fields to Rename              | <p>To rename fields, enter or select the fields to rename in Source Field Expression, and enter the new name for the field in Target Field Expression. Click Add to rename additional fields.Note: The Field Renamer processor cannot rename fields that contain a backslash character ( \ ).To rename sets of fields, use expressions:</p><ul><li>You can use regex in both properties.</li><li>You can use the StreamSets expression language in the Target Field Expression property.</li></ul><p>To rename an array or map, you can specify a single array index or map element, or you can use the asterisk wildcard to represent all array indices and map elements.</p><p>You cannot use regex to select a subset of array indices and map elements. For example, if the Division array contains 20 indices, you cannot use the following regex to rename the field path for the first 10 indices:</p><pre><code>/Division[0-9]
    </code></pre><p>Note: If you rename fields within a list or list-map field, the processor lists the renamed fields at the end of the list or list-map field. You can use a Field Order processor to reorder fields in a list-map field.</p> |
    | Source Field Does Not Exist   | <p>Behavior when a source field does not exist in a record:</p><ul><li>Continue - Continues processing the record, ignoring missing source fields.</li><li>Send to Error - Handles the record based on the error handling configured for the stage.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Target Field Already Exists   | <p>Behavior when the record includes a field name that matches a proposed target field:</p><ul><li>Replace - Replaces the existing field with the renamed field.</li><li>Append Numbers - Appends numbers to any duplicates in the renamed fields.</li><li>Continue - Continues processing the record, making no change for existing fields.</li><li>Send to Error - Handles the record based on the error handling configured for the stage.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | Multiple Source Field Matches | <p>Behavior when a source field matches more than one source field expression:</p><ul><li>Continue - Continues processing the record, skipping fields with multiple matches.</li><li>Send to Error - Handles the record based on the error handling configured for the stage.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
