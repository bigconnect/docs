# Field Pivoter

The Field Pivoter pivots data in a list, map, or list-map field and creates a record for each item in the field.

The Field Pivoter processor pivots the data in one field. To pivot additional fields or nested structures, use additional Field Pivoters.

When you configure the Field Pivoter, you specify the list, map, or list-map field to pivot and where to write the pivoted data. By default, the processor writes the pivoted data to the original field, but you can specify another field for the data.

You can also specify whether to include the existing fields in the resulting records and configure the action to take when the field does not exist.

### Generated Records <a href="#ariaid-title2" id="ariaid-title2"></a>

When you pivot a field, the Field Pivoter creates a new record for each first-level item in the list or map. To pivot additional fields or nested structures, use additional Field Pivoters.

When pivoting a field, you can include the existing fields in the record or drop them, using only the pivoted data in the new records. You can specify where to write the pivoted data - in the original field or in a different field. You can also specify whether to save the field name of the first-level item in the pivoted field.

For example, say you want to pivot the Color\_List data in the following set of records so you can update the unit costs based on color later in the pipeline:Incoming data

| `PEN_TYPE`    | `COLOR_LIST`                                                             | `UNIT_COST` |
| ------------- | ------------------------------------------------------------------------ | ----------- |
| `ballpoint`   | <p><code>black</code></p><p><code>blue</code></p><p><code>red</code></p> | .`10`       |
| `highlighter` | <p><code>light blue</code></p><p><code>yellow</code></p>                 | .`20`       |
| `felt tip`    | `black`                                                                  | .`15`       |

Pivot to existing field, include existing dataIf you use the Field Processor default to pivot the list in the Color\_List field to the same field and include existing fields, Field Pivoter overwrites the list in the Color\_List field with the pivoted data as follows:

| `PEN_TYPE`    | `COLOR_LIST` | `UNIT_COST` |
| ------------- | ------------ | ----------- |
| `ballpoint`   | `black`      | .`10`       |
| `ballpoint`   | `blue`       | .`10`       |
| `ballpoint`   | `red`        | .`10`       |
| `highlighter` | `light blue` | .`20`       |
| `highlighter` | `yellow`     | .`20`       |
| `felt tip`    | `black`      | .`15`       |

Pivot to new field, include existing dataIf you configure the processor to pivot the list to a new Color field and include the existing records, Field Pivoter generates the following records:

| `PEN_TYPE`    | `COLOR_LIST`                                                             | `COLOR`      | `UNIT_COST` |
| ------------- | ------------------------------------------------------------------------ | ------------ | ----------- |
| `ballpoint`   | <p><code>black</code></p><p><code>blue</code></p><p><code>red</code></p> | `black`      | .`10`       |
| `ballpoint`   | <p><code>black</code></p><p><code>blue</code></p><p><code>red</code></p> | `blue`       | .`10`       |
| `ballpoint`   | <p><code>black</code></p><p><code>blue</code></p><p><code>red</code></p> | `red`        | .`10`       |
| `highlighter` | <p><code>light blue</code></p><p><code>yellow</code></p>                 | `light blue` | .`20`       |
| `highlighter` | <p><code>light blue</code></p><p><code>yellow</code></p>                 | `yellow`     | .`20`       |
| `felt tip`    | `black`                                                                  | `black`      | .`15`       |

Pivot to new field, include existing data, and include field name of the first-level item in the pivoted fieldYou can include the field name of the first-level item in the pivoted field in the new record. For example, let's say the Color\_List field names the first-level items in the pivoted field `value_`_`n`_ as follows:

```
  "Color_List": {
      "value_1": "black",
      "value_2": "blue",
      "value_3": "red"
    }
```

You can include the `value_`_`n`_ field names as fields in the new record. If you configure the processor to pivot the list to a new Color\_Value field, include the existing records, and include the field name of the first-level item in the Color\_FieldName field, Field Pivoter generates the following records:

| `PEN_TYPE`    | `COLOR_LIST`                                                             | `COLOR_VALUE` | `COLOR_FIELDNAME` | `UNIT_COST` |
| ------------- | ------------------------------------------------------------------------ | ------------- | ----------------- | ----------- |
| `ballpoint`   | <p><code>black</code></p><p><code>blue</code></p><p><code>red</code></p> | `black`       | `value_1`         | .`10`       |
| `ballpoint`   | <p><code>black</code></p><p><code>blue</code></p><p><code>red</code></p> | `blue`        | `value_2`         | .`10`       |
| `ballpoint`   | <p><code>black</code></p><p><code>blue</code></p><p><code>red</code></p> | `red`         | `value_3`         | .`10`       |
| `highlighter` | <p><code>light blue</code></p><p><code>yellow</code></p>                 | `light blue`  | `value_1`         | .`20`       |
| `highlighter` | <p><code>light blue</code></p><p><code>yellow</code></p>                 | `yellow`      | `value_2`         | .`20`       |
| `felt tip`    | `black`                                                                  | `black`       | `value_1`         | .`15`       |

Pivot to new field, drop existing dataIf you pivot data to a new Color field without including the existing fields, Field Pivoter produces records with only the Color field. This choice doesn't make sense in this example, but can be useful when pivoting nested lists or maps or when you plan to enrich the data downstream:

| `COLOR`      |
| ------------ |
| `black`      |
| `blue`       |
| `red`        |
| `light blue` |
| `yellow`     |
| `black`      |

### Configuring a Field Pivoter <a href="#ariaid-title3" id="ariaid-title3"></a>

Configure a Field Pivoter to pivot data in a list, map, or list-map field and generate a record for each item in the field.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Field Pivot tab, configure the following properties:

    | Field Pivoter Property   | Description                                                                                                                                                                                |
    | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Field to Pivot           | List, map, or list-map field to pivot.                                                                                                                                                     |
    | Copy All Fields          | Includes all existing fields in the generated records.                                                                                                                                     |
    | Pivoted Items Path       | Field path to write the pivoted data. If the field exists, the processor overwrites any data in the field.Note: When not used, the processor writes pivoted data to the original field.    |
    | Save Original Field Name | Specifies whether to save the field name of the first-level item in the pivoted field. You can save the original field name when you include all existing fields in the generated records. |
    | Original Field Name Path | Field path to write the field name of the first-level item in the pivoted field.                                                                                                           |
    | Field Does Not Exist     | Action to take if the record does not include the specified field to pivot.                                                                                                                |
