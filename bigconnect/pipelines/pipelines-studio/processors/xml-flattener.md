# XML Flattener

The XML Flattener processor flattens a well-formed XML document embedded in a string field and adds the flattened data to the record as additional fields or as a map in a single field.

When you configure the XML Flattener, you specify the field that contains the XML data. You can specify a record delimiter to generate multiple records from the XML document. When specifying a record delimiter, use an XML element directly under the root element.

You can configure whether the processor keeps all fields in the original record, or keeps just the flattened fields.

You can also specify an output field. When you define an output field, the processor writes the flattened fields to the output field as a map. You can optionally configure the string used to separate entity names and attributes in the flattened field names.

### Generated Records <a href="#ariaid-title2" id="ariaid-title2"></a>

The XML Flattener generates multiple records from a well-formed XML document based on a user-defined record delimiter. The delimiter specifies the XML element to use to create records. Use an XML element directly under the root element.

When no record delimiter is defined, the processor reads the entire contents of the field as a single record.

For example, a string field contains the following XML:

```
<contacts>
    <contact>
        <name type="maiden">NAME1</name>
        <phone>(111)111-1111</phone>
        <phone>(222)222-2222</phone>
    </contact>
    <contact>
        <name type="maiden">NAME2</name>
        <phone>(333)333-3333</phone>
        <phone>(444)444-4444</phone>      
    </contact>
</contacts>
```

If you specify the `contact` element as the record delimiter, the XML Flattener creates two records. Record 1 contains the following fields:

```
contact.name: NAME1
contact.name#type: maiden
contact.phone(0): (111)111-1111
contact.phone(1): (222)222-2222
```

Record 2 contains the following fields:

```
contact.name: NAME2
contact.name#type: maiden
contact.phone(0): (333)333-3333
contact.phone(1): (444)444-4444
```

Note: When you configure the processor to keep the original fields in the incoming record, each generated record includes the original fields as well.

If you do not specify a record delimiter, the XML Flattener creates one record that contains the following fields:

```
contacts.contact(0).name: NAME1
contacts.contact(0).name#type: maiden
contacts.contact(0).phone(0): (111)111-1111
contacts.contact(0).phone(1): (222)222-2222
contacts.contact(1).name: NAME2
contacts.contact(1).name#type: maiden
contacts.contact(1).phone(0): (333)333-3333
contacts.contact(1).phone(1): (444)444-4444
```

### Configuring an XML Flattener <a href="#ariaid-title3" id="ariaid-title3"></a>

Configure an XML Flattener to flatten XML data embedded in a string field.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Flatten tab, configure the following properties:

    | Flatten Property          | Description                                                                                                                                                                                                                                                                                                                                                                  |
    | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Field to Flatten          | String field that contains the well-formed XML document to flatten.                                                                                                                                                                                                                                                                                                          |
    | Keep Original Fields      | <p>Specifies whether to keep all fields in the original record. When selected, the processor flattens the specified field and keeps all other fields in the record. When cleared, the processor flattens the specified field and removes all other fields in the record.</p><p>To keep the original fields, the root field of the record must be Map or List-Map.</p>        |
    | Overwrite Existing Fields | <p>Overwrites any existing fields with names that match the new flattened fields.</p><p>When writing flattened fields to an output field, allows the processor to overwrite an existing field.</p>                                                                                                                                                                           |
    | Output Field              | Specifies an output field for flattened fields to be written. You can use an existing field or name a new field to be created.                                                                                                                                                                                                                                               |
    | Record Delimiter          | <p>XML element that indicates the data to use to generate records. Use to create multiple records from an XML document. Use an XML element directly under the root element.</p><p>To read the data as a single record, omit this property.</p>                                                                                                                               |
    | Field Delimiter           | <p>String used to separate entity names in the flattened field names. For example, in the following flattened field names, the period (.) is defined as the field delimiter:</p><pre><code>contact.name=NAME1
    contact.name#type=maiden
    </code></pre><p>The following characters cannot be used as a field delimiter: <code>[ ] ' " /</code></p><p>Default is the period.</p> |
    | Attribute Delimiter       | <p>String used to separate attributes in the flattened field names. For example, in the following flattened field name, the hash mark (#) is defined as the attribute delimiter:</p><pre><code>contact.name#type=maiden
    </code></pre><p>The following characters cannot be used as an attribute delimiter: <code>[ ] ' " /</code></p><p>Default is the hash mark.</p>        |
    | Ignore Attributes         | Ignores attributes defined for XML elements. Select if you do not want to include attributes in the flattened fields.                                                                                                                                                                                                                                                        |
    | Ignore Namespace URI      | Ignores namespace URIs defined for XML elements. Select if you do not want to include namespace URIs in the flattened fields.                                                                                                                                                                                                                                                |
