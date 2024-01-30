# Field Zip

The Field Zip processor merges list data from two fields into a single field. You can use the Field Zip processor to merge two List fields or List-Map fields. Use the processor to merge related lists of data.

The Field Zip processor merges list data with a corresponding list. If either list includes more items than the other, the extra list items are not merged. The processor retains the original list fields so no data is lost.

When you configure the Field Zip processor, you specify the two fields to merge and the field to write the merged data. By default, the processor merges the field names and values from the lists. You can configure the processor to merge only the values.

If one or both fields in a record do not match the specified fields to merge, you can configure the Field Zip processor to pass the record as is without merging any fields, or pass the record to the pipeline for error handling.

### Merging List Data <a href="#ariaid-title2" id="ariaid-title2"></a>

When merging data in List fields, the Field Zip processor merges each item in a list with the corresponding item in the other list. The processor merges the lists using the order of the lists, and returns a list of Map fields.

Any item in a list without a matching item in the other list is not merged.

For example, say you have a record with two List fields:

| `order`  | `itemID`                                                 | `cost`                                            |
| -------- | -------------------------------------------------------- | ------------------------------------------------- |
| 23523482 | <ul><li>2</li><li>113</li><li>954</li><li>6502</li></ul> | <ul><li>89.95</li><li>8.95</li><li>6.95</li></ul> |

Say you configure the Field Zip processor to merge the ItemID and Cost fields and write the data to a Purchase field. The Field Zip processor then generates the following record:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldZip-RecordMap.png)

In the Purchase field, the merged data is a list of Map fields. Each map has the field name and value in the ItemID list merged with the corresponding field name and value from the Cost list. Notice that the fourth ItemID, /ItemID/3, is not included in the merged list because it has no corresponding cost information.

If you configure the Field Zip processor to merge only the values, the processor returns the same data without the original field names, as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldZip-Record-NoNames.png)

### Merging List-Map Data <a href="#ariaid-title3" id="ariaid-title3"></a>

As with List fields, when merging data in List-Map fields, the Field Zip processor merges each item in a list with the corresponding item in the other list. The processor merges the lists using the order of the lists, and returns a list of Map fields.

The difference is, the lists in the Map fields include different pairs of information, rather than the matching pairs of information from List fields.

For example, say you have a record with two List-Map fields, each with a single map in the list:

| `basics`                                                                        | `additional`                                                |
| ------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| <p>0</p><ul><li>id: 23</li><li>color: 20005</li><li>info: &#x3C;null></li></ul> | <p>0</p><ul><li>inventory: 80</li><li>cost: $5.98</li></ul> |

Say you configure the Field Zip processor to merge the Basics and Additional List-Map fields and to write the results to an All field. The Field Zip generates the following record:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldZip-ListMap.png)

Notice how each map contains a different set of fields. When processing List fields, Field Zip returns the matching sets of fields. Also notice, the /Basics/Info field is not included because it has no corresponding item in the other List-Map field.

If you configure the Field Zip processor to merge only the values, the processor returns the same data without the original field names, as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FieldZip-ListMap-NoFields.png)

### Pivoting Merged Lists <a href="#ariaid-title4" id="ariaid-title4"></a>

Depending on your use case, you might want to add a Field Pivoter processor to your pipeline after merging list data with the Field Zip processor.

The Field Pivoter can pivot merged list data and create a record for each first-level item in the list.

For example, say you use the Field Pivoter to process the data generated in the first List example, above, with field names included in the merged list. You configure the Field Pivoter to write the pivoted data to a new field called Purchased\_Item.

The pivot of the single record results in the following three records from the merged list:

| Order    | ItemID                                                   | Cost                                              | Purchase                                                                                                                                                                                                                                                                            | Purchased\_Item                                                           |
| -------- | -------------------------------------------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| 23523482 | <ul><li>2</li><li>113</li><li>954</li><li>6502</li></ul> | <ul><li>89.95</li><li>8.95</li><li>6.95</li></ul> | <ol><li><p></p><ul><li><p></p><ul><li>itemID: 2</li><li>cost: 89.95</li></ul></li></ul></li><li><p></p><ul><li><p></p><ul><li>itemID: 113</li><li>cost: 8.95</li></ul></li></ul></li><li><p></p><ul><li><p></p><ul><li>itemID: 954</li><li>cost: 6.95</li></ul></li></ul></li></ol> | <ul><li><p></p><ul><li>itemID: 2</li><li>cost: 89.95</li></ul></li></ul>  |
| 23523482 | <ul><li>2</li><li>113</li><li>954</li><li>6502</li></ul> | <ul><li>89.95</li><li>8.95</li><li>6.95</li></ul> | <ol><li><p></p><ul><li><p></p><ul><li>itemID: 2</li><li>cost: 89.95</li></ul></li></ul></li><li><p></p><ul><li><p></p><ul><li>itemID: 113</li><li>cost: 8.95</li></ul></li></ul></li><li><p></p><ul><li><p></p><ul><li>itemID: 954</li><li>cost: 6.95</li></ul></li></ul></li></ol> | <ul><li><p></p><ul><li>itemID: 113</li><li>cost: 8.95</li></ul></li></ul> |
| 23523482 | <ul><li>2</li><li>113</li><li>954</li><li>6502</li></ul> | <ul><li>89.95</li><li>8.95</li><li>6.95</li></ul> | <ol><li><p></p><ul><li><p></p><ul><li>itemID: 2</li><li>cost: 89.95</li></ul></li></ul></li><li><p></p><ul><li><p></p><ul><li>itemID: 113</li><li>cost: 8.95</li></ul></li></ul></li><li><p></p><ul><li><p></p><ul><li>itemID: 954</li><li>cost: 6.95</li></ul></li></ul></li></ol> | <ul><li><p></p><ul><li>itemID: 954</li><li>cost: 6.95</li></ul></li></ul> |

### Configuring a Field Zip Processor <a href="#ariaid-title5" id="ariaid-title5"></a>

Use a Field Zip processor to merge two corresponding list fields in a record.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul>                                                 |
2.  On the Zip tab, configure the following properties:

    | Field Zip Property   | Description                                                                                                                                                                                                                                                                                               |
    | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Fields to Zip        | <p>Configure the fields to merge and the output field as follows:</p><ul><li>First Field - The first List or List-Map field to use.</li><li>Second Field - The second List or List-Map field to use.</li><li>Path for Zipped Field - The field where the processor writes the merged list data.</li></ul> |
    | Zip Values Only      | <p>Includes only field values when merging lists. When not selected, the processor includes field names and values.</p><p>For more information about how the processor merges list data, see Merging List Data.</p>                                                                                       |
    | Field Does Not Exist | <p>Action to take when one or both list fields do not exist in a record:</p><ul><li>Include without Processing - Passes the record as is without creating the merged field.</li><li>Send to Error - Passes the record to the pipeline for error handling.</li></ul>                                       |
