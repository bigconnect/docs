# Field Flattener

The Field Flattener processor flattens list and map fields. The processor can flatten the entire record to produce a record with no nested fields. Or it can flatten specific list or map fields.

Use the Field Flattener processor when you have nested fields that you need to flatten. For example, the Drift Synchronization Solution for Hive cannot process records with nested fields, so you can use the Field Flattener processor to flatten records before passing them to the Hive Metadata processor.

When you configure a Field Flattener processor, you configure whether to flatten the entire record or specific fields in the record. When flattening specific fields, you configure whether to flatten the fields in place or into another field. You also configure the name separator to use for flattened field names.

### Flatten the Entire Record <a href="#ariaid-title2" id="ariaid-title2"></a>

When the Field Flattener flattens the entire record, it flattens all nested structures in the record until the record is flat.

For example, say you have the following record with nested map fields:

```
{
  "store": {
     "id": "10342",
     "location": {
         "street": "34 2nd St",
         "city": "Wilma",
         "state": "OH",
         "zipcode": "33333"
      },
     "ip": "234.56.7890"
  }
}
```

If you configure Field Flattener to flatten the entire record and to use a period as the name separator, the processor generates the following record:

| store.id | store.location.street | store.location.city | store.location.state | store.location.zipcode | store.ip    |
| -------- | --------------------- | ------------------- | -------------------- | ---------------------- | ----------- |
| 10342    | 34 2nd St             | Wilma               | OH                   | 33333                  | 234.56.7890 |

### Flatten Specific Fields <a href="#ariaid-title3" id="ariaid-title3"></a>

The Field Flattener processor can flatten specified fields, either list or map fields, that contain additional nested list or map fields. When flattening a list or map field, the processor flattens all nested structures in the field until the field is flat. The processor can flatten fields in place (at their current position in the record), or the processor can flatten fields into another list or map field in the record, such as into the root field.

For example, say you have the following record with nested map fields:

```
{
  "contact": {
     "name": "Jane Smith",
     "id": "557",
     "address": {
       "home": {
         "street": "101 3rd St",
         "city": "Huntsville",
         "state": "NC",
         "zipcode": "27023"
          },
       "work": {
         "street": "15 Main St",
         "city": "Jonestown",
         "state": "NC",
         "zipcode": "27011"
       }
      }
  }
}
```

If you configure Field Flattener to flatten the `address` map field in place using a period as the name separator, the processor generates the following record:

```
{
  "contact": {
     "name": "Jane Smith",
     "id": "557",
     "address": {
         "home.street": "101 3rd St",
         "home.city": "Huntsville",
         "home.state": "NC",
         "home.zipcode": "27023",
         "work.street": "15 Main St",
         "work.city": "Jonestown",
         "work.state": "NC",
         "work.zipcode": "27011"
      }
  }
}
```

Rather than flattening in place, if you configure Field Flattener to flatten the `address` map field into the target field `contact` using a period as the name separator, the processor generates the following record:

```
{
  "contact": {
     "name": "Jane Smith",
     "id": "557",
     "address": {
       "home": {
         "street": "101 3rd St",
         "city": "Huntsville",
         "state": "NC",
         "zipcode": "27023"
          },
       "work": {
         "street": "15 Main St",
         "city": "Jonestown",
         "state": "NC",
         "zipcode": "27011"
     "home.street": "101 3rd St",
     "home.city": "Huntsville",
     "home.state": "NC",
     "home.zipcode": "27023",
     "work.street": "15 Main St",
     "work.city": "Jonestown",
     "work.state": "NC",
     "work.zipcode": "27011"
  }
}
```

If you configure the processor to flatten the `address` field into the `contact` field and remove the flattened field, the processor generates the following record:

```
{
  "contact": {
     "name": "Jane Smith",
     "id": "557",
     "home.street": "101 3rd St",
     "home.city": "Huntsville",
     "home.state": "NC",
     "home.zipcode": "27023",
     "work.street": "15 Main St",
     "work.city": "Jonestown",
     "work.state": "NC",
     "work.zipcode": "27011"
  }
}
```

### Configuring a Field Flattener <a href="#ariaid-title4" id="ariaid-title4"></a>

Configure a Field Flattener processor to flatten fields.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Flatten tab, configure the following properties:

    | Flatten Property       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Flatten                | Select whether to flatten the entire record or to flatten specific fields.                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Fields                 | <p>When flattening specific fields, the fields to flatten. You can flatten list and map fields that contain additional nested list or map fields. Specify the path to the field, for example: /contact/address.To specify fields, you can:</p><ul><li>Type field paths separated by commas.</li><li>Click the Fields text box and select each field path from the list of available field paths.</li><li>Click Select Fields Using Preview Data to open the Field Selector dialog box, and select fields from the preview data.</li></ul> |
    | Flatten in Place       | When flattening specific fields, enables the processor to flatten the fields at their current position in the record.                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Target Field           | When not flattening in place, the map or list-map field to flatten the fields into. The field must already exist in the record.                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Collision Field Action | <p>When not flattening in place, the action the processor takes when the target field already contains a field with the same name as the flattened field. Possible actions:</p><ul><li>Send record for error handling</li><li>Override with new value</li><li>Discard new value</li></ul>                                                                                                                                                                                                                                                 |
    | Remove Flattened Field | When not flattening in place, enables the processor to remove original fields that contained nonflattened fields from the record after successfully flattening fields.                                                                                                                                                                                                                                                                                                                                                                    |
    | Name Separator         | <p>One or more characters to use between nested field names to create a flattened field name.</p><p>For example, if you use the underscore character, a <code>location</code> field nested in a <code>store</code> field is flattened as follows: <code>store_location</code>.</p>                                                                                                                                                                                                                                                        |
