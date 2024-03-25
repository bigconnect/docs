# BigConnect Graph Engine

Load data directly into the BigConnect Graph Engine at very high speed.

## Data Formats

The BigConnect Graph Engine destination processes data differently based on the data format that you select. The origin processes the following types of data:

**JSON**

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin processes the object based on the error handling configured for the stage.

**Delimited**

Generates a record for each delimited line.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record. You can also configure the stage to replace a string constant with null values and to ignore control characters.

**Text**

Generates a record for each line of text or for each section of text based on a custom delimiter.When a line or section exceeds the maximum line length defined for the origin, the origin truncates it. The origin adds a boolean field named Truncated to indicate if the line was truncated.

**XML**

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

Note: Field attributes and record header attributes are written to destination systems automatically only when you use the SDC RPC data format in destinations. When a record exceeds the user-defined maximum record length, the origin skips the record and continues processing with the next record. It sends the skipped record to the pipeline for error handling.Use the XML data format to process valid XML documents.

Tip: If you want to process invalid XML documents, you can try using the text data format with custom delimiters.

## Configuring a BigConnect Cypher Processor

In the Configuration tab, configure the following properties:

| Property                  | Description                                                                                |
| ------------------------- | ------------------------------------------------------------------------------------------ |
| Graph Engine Config Path  | Absolute path to the connection properties file                                            |
| Source Field Path         | Path for the source field                                                                  |
| Concept Type              | What concept type to use to create entities                                                |
| ID Seed                   | Seed for the entity ID                                                                     |
| ID fields                 | Fields used to compose the entity ID                                                       |
| Create Relationship       | Whether to create relationships or not                                                     |
| Relationship Name         | The name of the relationship                                                               |
| Relationship End ID Seed  | Seed for the destination entity ID                                                         |
| Fixed Relationship End ID | Other relationship end ID (Relationship End ID Seed will be ignored if this is configured) |
| Relationship Source       | Source entity for the relationship                                                         |
| Field Mapping             | Map fields from incoming rows to BigConnect entities                                       |
