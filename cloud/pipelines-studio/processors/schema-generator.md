# Schema Generator

The Schema Generator processor generates a schema based on the structure of a record and writes the schema into a record header attribute. The Schema Generator processor generates Avro schemas at this time.

Use the Schema Generator processor to generate a basic schema when the schema is unknown. For example, you might use the processor in a pipeline to generate the latest version of the Avro schema before writing records to destination systems.

Note: The Schema Generator processor offers limited customization. If you need more customization than the processor offers, consider writing your own schema generator.

When you configure a Schema Generator processor, you can specify the namespace and description for the Avro schema. You can specify whether schema fields should allow nulls and whether schema fields should default to null. You can specify default values for most Avro primitive types, and you can allow the processor to use a larger data types for Avro types without a direct equivalent.

You can specify the names for precision and scale attributes for decimal values. And you can configure a default precision and scale for any decimal fields without that information or with an invalid precision or scale.

When appropriate, you can configure the Schema Generator to cache a number of schemas, and to apply the schemas to records based on the expression defined in the Cache Key Expression property.

### Using the avroSchema Header Attribute <a href="#ariaid-title2" id="ariaid-title2"></a>

The Schema Generator processor writes Avro schemas to an avroSchema record header attribute by default. All Avro-processing origins also write the Avro schema of incoming records to the avroSchema header attribute. Also, any destination that writes Avro data can use the schema in the avroSchema header attribute.

When processing Avro data, one logical workflow is to add the Schema Generator immediately before the destination in a pipeline. This allows the processor to generate a new Avro schema before writing the data to destination systems.

If you want retain an earlier version of the avroSchema, you might use an Expression Evaluator processor before the Schema Generator to move the existing schema in the avroSchema header attribute to a different header attribute, such as avroSchema\_previous.

### Generated Avro Schema <a href="#ariaid-title3" id="ariaid-title3"></a>

The Avro schema that the Schema Generator creates includes the following information:

* Schema type set to "record".
* Schema name based on the Schema Name property.
* Namespace based on the Namespace property, when configured.
* Schema description in the doc field based on the Doc property, when configured.
* A map of field names with related attributes based on the record schema and related properties defined in the stage, such as whether fields can include null values.

For example, the following Avro schema is generated when you set the Name property to MyAvroSchema, and omit the optional Namespace and Doc properties:

```
{"type":"record","name":"MyAvroSchema","namespace":"","doc":"","fields":[{"name":"name","type":["null","string"],"default":null},{"name":"id","type":["null","int"],"default":null},{"name":"instock","type":["null","boolean"],"default":false},{"name":"cost","type":["null",{"type":"bytes","logicalType":"decimal","precision":10,"scale":2}],"default":null}]}
```

The record described by this schema includes the following fields:

* name - A string field.
* id - An integer field.
* instock - A boolean field.
* cost - A decimal field.

The processor is configured to allow nulls in schema fields and to use null as the default value.

### Caching Schemas <a href="#ariaid-title4" id="ariaid-title4"></a>

You can configure the Schema Generator to cache a number of schemas, and to apply the schemas to records based on the expression defined in the Cache Key Expression property.

Caching schemas can improve performance when a set of records can logically use the exact same schema, and when the records include a value that can be used to determine the schema to use.

For example, say your pipeline uses the JDBC Multitable Consumer to read from multiple database tables. The origin writes the names of the table used to generate each record to a jdbc.tables record header attribute. Let's assume that all data from each record comes from a single table.

To use the schema associated with each record, you can configure the Cache Key Expression property as follows: `${record:attribute(jdbc.tables)}`.

Warning: Use schema caching with care - applying an incorrect schema to a record can cause errors when writing to destination systems.

### Configuring a Schema Generator <a href="#ariaid-title5" id="ariaid-title5"></a>

Configure a Schema Generator processor to generate a schema for each record and write the schema to a record header attribute.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Schema tab, configure the following properties:

    | Schema Property  | Description                                                                                                                                                                                                                                                                    |
    | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Schema Type      | Type of schema to generate. The processor generates Avro schemas at this time.                                                                                                                                                                                                 |
    | Schema Name      | The name to use for the resulting schema.                                                                                                                                                                                                                                      |
    | Header Attribute | <p>The header attribute to contain the resulting schema.</p><p>Default is avroSchema.</p><p>Destinations can use the avroSchema header attribute to write Avro data when you configure the destination's Avro Schema Location property to use the In Record Header option.</p> |
3.  On the Avro tab, configure the following properties:

    | Avro Property            | Description                                                                                                                                                                                                                                                                                                                                          |
    | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Namespace                | Namespace to use in the Avro schema.                                                                                                                                                                                                                                                                                                                 |
    | Nullable Fields          | <p>Allows fields to include null values by creating a union of the field type and null type.</p><p>By default, fields cannot include null values.</p>                                                                                                                                                                                                |
    | Default to Nullable      | When allowing null values in schema fields, uses null as a default value for all fields.                                                                                                                                                                                                                                                             |
    | Doc                      | Optional description for the Avro schema.                                                                                                                                                                                                                                                                                                            |
    | Default Values for Types | <p>Optionally specify default values for Avro data types. Click Add to configure a default value.</p><p>The default value applies to all fields of the specified data type.</p><p>You can specify default values for the following Avro types:</p><ul><li>Boolean</li><li>Integer</li><li>Long</li><li>Float</li><li>Double</li><li>String</li></ul> |
    | Expand Types             | Allow using a larger Data Pipelines data type for an Avro data type when an exact equivalent is not available.                                                                                                                                                                                                                                       |
4.  On the Types tab, optionally configure the following properties:

    | Type Property             | Description                                                                                                                                                                                                                                                 |
    | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Precision Field Attribute | Name of the schema attribute that stores the precision for a decimal field.                                                                                                                                                                                 |
    | Scale Field Attribute     | Name of the schema attribute that stores the scale for a decimal field.                                                                                                                                                                                     |
    | Default Precision         | <p>Default precision to use for decimal fields when the precision is not specified or is invalid.</p><p>Use -1 to opt out of this option.</p><p>Note: When decimal fields do not have a valid precision and scale, the stage sends the record to error.</p> |
    | Default Scale             | <p>Default scale to use for decimal fields when the precision is not specified or is invalid.</p><p>Use -1 to opt out of this option.</p><p>Note: When decimal fields do not have a valid precision and scale, the stage sends the record to error.</p>     |
5.  On the Advanced tab, optionally configure the following properties:

    | Advanced Property    | Description                                                                                                            |
    | -------------------- | ---------------------------------------------------------------------------------------------------------------------- |
    | Enable Cache         | Enables caching schemas. Can improve performance under specific conditions. For more information, see Caching Schemas. |
    | Cache Size           | Maximum number of schemas to cache.                                                                                    |
    | Cache Key Expression | Expression that evaluates to a valid cache key.                                                                        |
