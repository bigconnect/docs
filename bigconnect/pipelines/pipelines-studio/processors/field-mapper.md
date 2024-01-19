# Field Mapper

The Field Mapper processor maps an expression to a set of fields to alter field paths, field names, or field values. For example, you might use the Field Mapper processor to reorganize fields or to remove special characters from field names.When configuring the Field Mapper processor, you must specify the following:

* The part of the fields that the mapping expression alters, either the path, name, or value.
* An optional conditional expression that specifies the affected fields.
* An expression that specifies how to alter the path, name, or value

When necessary, you configure the processor to change record structure, to append rather than overwrite records, and to copy existing fields leaving current fields in place.

### Field Values <a href="#ariaid-title2" id="ariaid-title2"></a>

You can use the Field Mapper processor to map an expression to a set of fields to alter field values. You might alter field values by converting negative numbers to positive numbers, setting all negative numbers to zero, or appending a prefix to any string value.

For example, suppose you want to change numbers in your data set to show the absolute values. As shown in the following table, you configure the processor to do the following:

* Operate on field values.
* Use a conditional expression to operate only on field values when the value in a field is an integer or double.
* Use a mapping expression to write the current value when the value is greater than zero, otherwise multiply the value by -1 to convert to a positive number.

| Mapper Property        | Value                                            |
| ---------------------- | ------------------------------------------------ |
| Operate On             | Field Values                                     |
| Conditional Expression | ${f:type() == 'INTEGER' or f:type() == 'DOUBLE'} |
| Mapping Expression     | ${f:value() > 0 ? f:value() : -1 \* f:value()}   |

The following image shows that the Field Mapper processor changes negative numbers in input data to positive numbers in output data:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldMapper-ExValue.png)

### Field Names <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use the Field Mapper processor to apply a mapping expression that uniformly alters the names of a set of fields. You might alter field names by replacing special characters or adding a suffix. When altering field names, you can configure the processor to change record structure, to append rather than overwrite records, and to copy existing fields leaving current fields in place.

For example, suppose you want to replace special characters in field names with underscores. As shown in the following table, you configure the processor to do the following:

* Operate on field names.
* Use a mapping expression that replaces special characters in the name with an underscore.

Because you want the mapping expression applied to all field names, you do not need to configure a conditional expression.

| Mapper Property    | Value                                             |
| ------------------ | ------------------------------------------------- |
| Operate On         | Field Names                                       |
| Mapping Expression | ${str:replaceAll(f:name(), '\[.\\\\-/()^]','\_')} |

The following image shows that the Field Mapper processor changes the hyphen in `f1-values` and the slash in `value/list` to underscores:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldMapper-ExFieldName.png)

### Field Paths <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use the Field Mapper processor to map an expression to a set of fields to alter the field paths. You might alter field paths to group fields by type or to copy fields with suspicious names and values to an alternative path to be examined. When mapping multiple fields to a single field path, specify an aggregation expression that defines how to aggregate the fields in that path. When altering field paths, you can configure the processor to change record structure, to append rather than overwrite records, and to copy existing fields leaving current fields in place.

#### Example: Grouping Fields by Type

Suppose you want to group fields with the same data types. You can use the Field Mapper processor to move fields to an already defined map field that contains map fields for each field type.

As shown in the following table, you configure the processor to do the following:

* Operate on field paths.
* Use a mapping expression that moves fields under the appropriate field type.
* Clear the aggregation expression so the processor writes the field value to the field record rather than a list that contains the value.
* Enable the processor to change the structure of the record and write new fields.

Note that the input record must already contain map fields for the path, in this case map fields for all the field types, such as /outputs/STRING, /outputs/INTEGER, and /outputs/DOUBLE.

| Mapper Property          | Value                                                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------- |
| Operate On               | Field Paths                                                                                                   |
| Mapping Expression       | /outputs/${f:type()}/${f:name()}                                                                              |
| Aggregation Expression   | NoneNote: Clear the default expression because the mapping expression maps only one field to each field path. |
| Structure Change Allowed | Enabled                                                                                                       |

The following image shows that the Field Mapper moves fields with values under the `outputs` field by appropriate field type:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldMapper-ExGroupFields.png)

#### Example: Copying Suspicious Fields

Suppose you want to identify suspicious fields but leave the fields in place until you can examine them. You consider a field suspicious if the field name contains the text `name` and the value contains a special character. You can use the Field Mapper processor to map an expression to suspicious fields. The expression maps the suspicious fields to an already defined map field for reviewing. When writing to the review field, the processor creates a map field for each suspicious field and lists the original path from the record along with its value.

As shown in the following table, you configure the processor to do the following:

* Operate on field paths.
* Use a conditional expression to operate only on fields that match your definition of suspicious.
* Use a mapping expression to write the matching fields to a new path.
* Use an aggregation expression to create a map for each field written to the new path and list the original path and value in the field.

In this case, you also configure the processor to change the structure of the record and to maintain the original paths by copying the suspicious fields. The input record must contain a map field for the path, in this case, `review`.

| Mapper Property          | Setting                                                                                                            |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| Operate On               | Field Paths                                                                                                        |
| Conditional Expression   | ${f:type() == 'STRING' and str:contains(f:name(), 'name') and str:matches(f:value(), '^\[;\\\\-\*".&\*\\\\^].\*')} |
| Mapping Expression       | /review/suspiciousNames                                                                                            |
| Aggregation Expression   | ${map(fields, fieldByPreviousPath())}                                                                              |
| Structure Change Allowed | Enabled                                                                                                            |
| Maintain Original Paths  | Enabled                                                                                                            |

The following image shows that the Field Mapper processor copies suspicious fields to the `review` field, where it lists the original path to the field and its value:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldMapper-ExSuspiciousNames.png)

#### Aggregation Expressions <a href="#ariaid-title5" id="ariaid-title5"></a>

When configuring a Field Mapper processor that operates on field paths and that maps multiple fields to the same path, you must include an aggregation expression to define how the processor aggregates the fields into the resulting path. You can configure the processor to:

* Return a single value, such as the sum or the minimum or maximum of the field values.
* Return a list of values from the fields.
* Return a list of strings containing the original field paths - that is, the field paths before applying the mapping.
* Return a list of field maps, each containing the original field path and value.

In the aggregation expression, you can use a numeric function or a field function. When calling a numeric or field function, use a special variable, `fields`, to pass the list of fields to the function. If the processor has a conditional expression, the aggregation expression passes the fields that match the conditional expression. Otherwise, the aggregation passes all the fields.

**Numeric Functions**

Use numeric functions to reduce fields to a single value. All fields passed to a numeric function must be the same numeric type. The data type of the returned value depends on the input data type, as shown in the following table:

| Input           | Returned |
| --------------- | -------- |
| Integer or Long | Long     |
| Float or Double | Double   |
| Decimal         | Decimal  |

Aggregation expressions support the following numeric functions:sum(fields)Returns the sum of the values in the specified fields.min(fields)Returns the minimum value of the specified fields.max(fields)Returns the maximum value of the specified fields.

For example, to return the sum of values, set your aggregation expression to:

${sum(fields)}

**Field Functions**

Use field functions to return values from fields, field paths from fields, or field paths and values from fields.

Aggregation expressions support the following field functions:asFields(\<list of values>)Returns a list of string fields from a list of values. You must use with the `map` and `previousPath` functions to return a list of strings that contain paths to fields.

In the aggregation expression, enter:

${asFields(map(fields, previousPath()))}

fieldsReturns a list of values from the fields.map(fields,\<path function>)Returns a list of the values or map fields returned by a path function for each passed field. Available path functions include:previousPath()Returns a value that contains the path of each passed field. Use in the `asFields` function to return a list of strings that contain field paths of fields.fieldByPreviousPath()Returns a map field for each passed field. The map field contains a single entry, with a key equal to the field path of the passed field and the value equal to the value of the passed field.For example, suppose you have a conditional expression that returns the two fields shown in the following table:

| Field Path      | Value |
| --------------- | ----- |
| /f1-names/first | Ann   |
| /f2-name/first  | Bob   |

When you configure the mapping expression to map these fields to the `output` field, you can use field functions in the aggregation expression to return either the values, the field paths, or the field paths and values:Return list of valuesSet the aggregation expression to:

${fields}

The processor returns the following output:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldMapper-AggEx-ValueOnly.png)

Return list of field pathsSet the aggregation expression to:

${asFields(map(fields, previousPath()))}

The processor returns the following output:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldMapper-AggEx-PathOnly.png)

Return list of field paths and valuesSet the aggregation expression to:

${map(fields, fieldByPreviousPath())}

The processor returns the following output:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldMapper-AggEx-PathValue.png)

### Configuring a Field Mapper Processor <a href="#ariaid-title6" id="ariaid-title6"></a>

Configure a Field Mapper processor to map an expression to a set of fields in a record.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Mapper tab, configure the following properties:

    | Mapper Property                                                                                                                                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Operate On                                                                                                                                                        | <p>Part of the field to be altered:</p><ul><li>Field Paths</li><li>Field Names</li><li>Field Values</li></ul>                                                                                                                                                                                                                                                                                                                                |
    | Conditional Expression                                                                                                                                            | <p>Expression that specifies the set of fields in the record to alter.</p><p>When not used, Field Mapper applies the mapping expression to all fields in the record. The expression can include field, math, record, string, or time <a href="https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Expression_Language/Functions.html#concept_lhz_pyp_1r">functions</a>.</p>                                 |
    | Mapping Expression                                                                                                                                                | <p>Expression applied to the paths, names, or values of matching fields.</p><p>You can use field, math, record, string, or time <a href="https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Expression_Language/Functions.html#concept_lhz_pyp_1r">functions</a>.</p><p>If mapping to paths, the paths must already exist in the record.</p>                                                               |
    | [Aggregation Expression](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Processors/FieldMapper.html#concept\_cg1\_4gr\_zgb) | <p>Expression that specifies how to aggregate multiple fields into the same path. Enter an expression when the mapping expression maps multiple fields to the same path.</p><p>You can use processor-specific numeric or field functions in the aggregation expression. You can also use math, record, string, or time functions.</p><p>The default value is <code>${fields}</code>, which returns a list of the values from each field.</p> |
    | Structure Change Allowed                                                                                                                                          | <p>Enables the processor to change the structure of the record, such as by adding new fields or changing data types.</p><p>Available when operating on field paths or field names.</p>                                                                                                                                                                                                                                                       |
    | Append List Values                                                                                                                                                | <p>Enables the processor to append values to list fields when the mapping expression maps fields to an existing list field. When not enabled, the processor replaces existing values in the list field.</p><p>Available when operating on field paths or field names.</p>                                                                                                                                                                    |
    | Maintain Original Paths                                                                                                                                           | <p>Enables the processor to keep existing fields when a mapping expression maps a field to a new name or path. When not enabled, the processor removes existing fields when creating new names or paths for fields.</p><p>Available when operating on field paths or field names.</p>                                                                                                                                                        |
