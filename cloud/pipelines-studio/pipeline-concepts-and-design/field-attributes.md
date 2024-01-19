# Field Attributes

Field attributes are attributes that provide additional information about each field that you can use in pipeline logic, as needed.

Some stages generate field attributes. For example, the Salesforce origin includes the original Salesforce data type in the `salesforce.salesforceType` attribute for every field.

You can create, modify, and evaluate field attributes in the pipeline. The Expression Evaluator enables creating and modifying field-level attributes. You can use field attribute functions to evaluate field attributes.

For example, you can use an Expression Evaluator to create a field attribute based on record data, then pass the record to a Stream Selector that routes data based on the value of the attribute.

Field attributes are automatically included in records written to destination systems only when you use the SDC RPC data format in the destination.

To include field attributes in the record data or to use field attributes in calculations, use the `record:fieldAttribute` and `record:fieldAttributeOrDefault` functions. For more information about field attribute functions, see **Record Functions**.

When using data preview, you can enable viewing field attributes and record header attributes to aid with pipeline development.

## Working with Field Attributes

Like record header attributes, you can use the Expression Evaluator or any scripting processor to create or update field attributes. For example, when processing Avro data, origins place the precision and scale information for Decimal data into field attributes. If you want to increase those values before writing them to the destination system, you can use the Expression Evaluator or any scripting processor to set the attribute value.

Field attributes are string values. You can also use `record:fieldAttribute` functions in any expression to include attribute values in calculations.Important: Field attributes do not have field paths.&#x20;

**Important**: Field attributes do not have field paths. When using an attribute in an expression, use just the attribute name surrounded by quotation marks after specifying the path to the associated field, as follows:

&#x20;`​${record:fieldAttribute(<path to the field>,'<attribute name>')}`

For example, say you are processing Avro data as part of the Drift Synchronization Solution for Hive. All Decimal fields include automatically generated precision and scale field attributes. Before passing the data to the Hive Metadata processor, you want to create a new Price Decimal field based on the existing cost data.

The following Expression Evaluator creates the Price field and defines the precision and scale field attributes based on the existing Cost precision and scale.

![](<../../../.gitbook/assets/image (716).png>)

## Field Attribute-Generating Stages <a href="#ariaid-title3" id="ariaid-title3"></a>

The following table lists the stages that generate field attributes to enable special processing:

<table data-full-width="true"><thead><tr><th>Stage</th><th>Description</th></tr></thead><tbody><tr><td>Origins that process Avro data</td><td>Include precision and scale field attributes for every decimal field.</td></tr><tr><td>Origins that process XML data</td><td>Can include field XPath information, XML attribute, and namespace declarations in field attributes.</td></tr><tr><td>Google BigQuery origin</td><td>Includes original precision for datetime, time, and timestamp fields in a field attribute.</td></tr><tr><td>JDBC Multitable Consumer origin</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>JDBC Query Consumer origin</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>Oracle Bulkload origin</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>Oracle CDC Client origin</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>Salesforce origin</td><td>Includes data type information in field attributes.</td></tr><tr><td>SQL Server 2019 BDC Multitable Consumer origin</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>SQL Server CDC Client origin</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>SQL Server Change Tracking origin</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>Teradata Consumer origin</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>JDBC Lookup processor</td><td><p>Includes the following field attributes:</p><ul><li>Precision and scale for every decimal field.</li><li>Nanoseconds for every datetime field.</li></ul></td></tr><tr><td>Salesforce Lookup processor</td><td>Includes data type information in field attributes.</td></tr><tr><td>XML Parser</td><td>Can include field XPath information, XML attribute, and namespace declarations in field attributes.</td></tr></tbody></table>

## Viewing Field Attributes in Data Preview

As with record header attributes, you can use data preview to view field attributes at any point in the pipeline. To view field attributes, enable the Show Record/Field Header data preview property.

