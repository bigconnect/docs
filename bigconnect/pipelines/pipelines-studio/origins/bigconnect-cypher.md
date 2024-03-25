# BigConnect Cypher

Executes Cypher queries against a BigConnect Bolt server to read data.

## Data Formats

The BigConnect Cypher Origin origin processes data differently based on the data format that you select. The origin processes the following types of data:

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

## Configuring a BigConnect Cypher Origin

In the Connection tab, configure the following properties:

| Property               | Description                                           |
| ---------------------- | ----------------------------------------------------- |
| Bolt Connection String | Connection string for the BigConnect Core Bolt Server |
| Secure Connection      | Whether the connection is made over SSL/TLS           |
| Username               | User to connect to BigConnect Core                    |
| Password               | Password to connect                                   |
| Thread Count           | Number of threads used to to read data                |

In the Cypher tab, configure the following properties:

| Property     | Description                                       |
| ------------ | ------------------------------------------------- |
| Cypher Query | Cypher Query that should be executed to read data |
