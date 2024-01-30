# Data Generator

The Data Generator processor serializes a record into a single string or byte array field using the specified data format.

You might use the Data Generator processor when you want to perform processing that is available only at a field level for an entire record. For example, the Encrypt and Decrypt Fields processor can encrypt data in one or more fields, but does not encrypt entire records. To encrypt entire records, you can use the Data Generator to serialize records into a single field, then use the Encrypt and Decrypt Fields processor to encrypt the field.

When you configure the Data Generator, you specify the target field and the output type to use, String or Byte Array. You also specify the data format for the serialized record and related properties.

### Target Field <a href="#ariaid-title2" id="ariaid-title2"></a>

When you use the Data Generator processor, you specify the target field for the serialized record.

When you specify a new field path, the processor creates the new field and writes the serialized record to it. When you enter an existing field path, the processor replaces the data in the existing field with the serialized record. When you enter `/` for the root field, the processor replaces the entire record with a single field containing the serialized record.

### Data Formats <a href="#ariaid-title3" id="ariaid-title3"></a>

The Data Generator processor generates records differently based on the data format that you select. The processor can generate the following types of data:

Avro

The stage writes records based on the Avro schema. You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. The Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in the Confluent Schema Registry by the schema ID or subject.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the stage to register the Avro schema with the Confluent Schema Registry. You can also optionally include the schema definition in the message. Omitting the schema definition can improve performance, but requires the appropriate schema management to avoid losing track of the schema associated with the data.

You can include the Avro schema in the output.You can also compress data with an Avro-supported compression codec. When using Avro compression, avoid configuring any other compression properties in the stage.

Binary

The stage writes binary data to a single field in the record.DelimitedThe destination writes records as delimited data. When you use this data format, the root field must be list or list-map.You can use the following delimited format types:

* Default CSV - File that includes comma-separated values. Ignores empty lines in the file.
* RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.
* MS Excel CSV - Microsoft Excel comma-separated file.
* MySQL CSV - MySQL comma-separated file.
* Tab-Separated Values - File that includes tab-separated values.
* PostgreSQL CSV - PostgreSQL comma-separated file.
* PostgreSQL Text - PostgreSQL text file.
* Custom - File that uses user-defined delimiter, escape, and quote characters.
* Multi Character Delimited - File that uses multiple user-defined characters to delimit fields and lines, and single user-defined escape and quote characters.

JSON

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin processes the object based on the error handling configured for the stage.

Protobuf

Generates a record for every protobuf message. By default, the origin assumes messages contain multiple protobuf messages.Protobuf messages must match the specified message type and be described in the descriptor file.When the data for a record exceeds 1 MB, the origin cannot continue processing data in the message. The origin handles the message based on the stage error handling property and continues reading the next message.

Record

The destination writes records in the Record data format.

Text

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

XML

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

Note: Field attributes and record header attributes are written to destination systems automatically only when you use the SDC RPC data format in destinations.&#x20;

When a record exceeds the user-defined maximum record length, the origin skips the record and continues processing with the next record. It sends the skipped record to the pipeline for error handling.

Use the XML data format to process valid XML documents.&#x20;

Tip: If you want to process invalid XML documents, you can try using the text data format with custom delimiters.&#x20;

### Configuring a Data Generator Processor <a href="#ariaid-title4" id="ariaid-title4"></a>

Configure a Data Generator processor to serialize a record into a single field.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Generator tab, configure the following properties:

    | Generator Property | Description                                                 |
    | ------------------ | ----------------------------------------------------------- |
    | Target Field       | The target field to use when writing the serialized record. |
    | Output Type        | The data type of the target field.                          |
3.  On the Data Formats tab, configure the following property:

    | Data Format Property | Description                                                                                                                                                                                                |
    | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Data Format          | <p>Format of the serialized record. Use one of the following options:</p><ul><li>Avro</li><li>Binary</li><li>Delimited</li><li>JSON</li><li>Protobuf</li><li>SDC Record</li><li>Text</li><li>XML</li></ul> |
4.  For Avro data, on the Data Format tab, configure the following properties:

    | Avro Property          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Avro Schema Location   | <p>Location of the Avro schema definition to use when writing data:</p><ul><li>In Pipeline Configuration - Use the schema that you provide in the stage configuration.</li><li>In Record Header - Use the schema in the avroSchema record header attribute. Use only when the avroSchema attribute is defined for all records.</li><li>Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry.</li></ul>           |
    | Avro Schema            | <p>Avro schema definition used to write the data.</p><p>You can optionally use the <code>runtime:loadResource</code> function to load a schema definition stored in a runtime resource file.</p>                                                                                                                                                                                                                                           |
    | Register Schema        | Registers a new Avro schema with Confluent Schema Registry.                                                                                                                                                                                                                                                                                                                                                                                |
    | Schema Registry URLs   | <p>Confluent Schema Registry URLs used to look up the schema or to register a new schema. To add a URL, click Add and then enter the URL in the following format:</p><pre><code>http://&#x3C;host name>:&#x3C;port number>
    </code></pre>                                                                                                                                                                                                   |
    | Basic Auth User Info   | <p>User information needed to connect to Confluent Schema Registry when using basic authentication.</p><p>Enter the key and secret from the <code>schema.registry.basic.auth.user.info</code> setting in Schema Registry using the following format:</p><pre><code>&#x3C;key>:&#x3C;secret>
    </code></pre><p>Tip: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores.</p> |
    | Look Up Schema By      | <p>Method used to look up the schema in Confluent Schema Registry:</p><ul><li>Subject - Look up the specified Avro schema subject.</li><li>Schema ID - Look up the specified Avro schema ID.</li></ul>                                                                                                                                                                                                                                     |
    | Schema Subject         | <p>Avro schema subject to look up or to register in Confluent Schema Registry.</p><p>If the specified subject to look up has multiple schema versions, the stage uses the latest schema version for that subject. To use an older version, find the corresponding schema ID, and then set the Look Up Schema By property to Schema ID.</p>                                                                                                 |
    | Schema ID              | Avro schema ID to look up in Confluent Schema Registry.                                                                                                                                                                                                                                                                                                                                                                                    |
    | Include Schema         | Includes the schema in each message.Note: Omitting the schema definition can improve performance, but requires the appropriate schema management to avoid losing track of the schema associated with the data.                                                                                                                                                                                                                             |
    | Avro Compression Codec | <p>The Avro compression type to use.</p><p>When using Avro compression, do not enable other compression available in the destination.</p>                                                                                                                                                                                                                                                                                                  |
5.  For binary data, on the Data Format tab, configure the following property:

    | Binary Property   | Description                          |
    | ----------------- | ------------------------------------ |
    | Binary Field Path | Field that contains the binary data. |
6.  For delimited data, on the Data Format tab, configure the following properties:

    | Delimited Property             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Delimiter Format               | <p>Format for delimited data:</p><ul><li>Default CSV - File that includes comma-separated values. Ignores empty lines in the file.</li><li>RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.</li><li>MS Excel CSV - Microsoft Excel comma-separated file.</li><li>MySQL CSV - MySQL comma-separated file.</li><li>Tab-Separated Values - File that includes tab-separated values.</li><li>PostgreSQL CSV - PostgreSQL comma-separated file.</li><li>PostgreSQL Text - PostgreSQL text file.</li><li>Custom - File that uses user-defined delimiter, escape, and quote characters.</li></ul> |
    | Header Line                    | Indicates whether to create a header line.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Delimiter Character            | <p>Delimiter character for a custom delimiter format. Select one of the available options or use Other to enter a custom character.</p><p>You can enter a Unicode control character using the format \u<em>NNNN</em>, where ​<em>N</em> is a hexadecimal digit from the numbers 0-9 or the letters A-F. For example, enter \u0000 to use the null character as the delimiter or \u2028 to use a line separator as the delimiter.</p><p>Default is the pipe character ( | ).</p>                                                                                                                                        |
    | Escape Character               | <p>Escape character for a custom delimiter format. Select one of the available options or use Other to enter a custom character.</p><p>Default is the backslash character ( \ ).</p>                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Quote Character                | <p>Quote character for a custom delimiter format. Select one of the available options or use Other to enter a custom character.</p><p>Default is the quotation mark character ( " ).</p>                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Replace New Line Characters    | <p>Replaces new line characters with the configured string.</p><p>Recommended when writing data as a single line of text.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | New Line Character Replacement | <p>String to replace each new line character. For example, enter a space to replace each new line character with a space.</p><p>Leave empty to remove the new line characters.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Charset                        | Character set to use when writing data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
7.  For JSON data, on the Data Format tab, configure the following property:

    | JSON Property | Description                                                                                                                                                                                                                                                                                                   |
    | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JSON Content  | <p>Method to write JSON data:</p><ul><li>JSON Array of Objects - Each file includes a single array. In the array, each element is a JSON representation of each record.</li><li>Multiple JSON Objects - Each file includes multiple JSON objects. Each object is a JSON representation of a record.</li></ul> |
    | Charset       | Character set to use when writing data.                                                                                                                                                                                                                                                                       |
8.  For protobuf data, on the Data Format tab, configure the following properties:

    | Protobuf Property        | Description                                                                                                                                                                                                               |
    | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Protobuf Descriptor File | Descriptor file (.desc) to use. The descriptor file must be in the Data Pipelines resources directory, `$SDC_RESOURCES`.                                                                                                  |
    | Message Type             | <p>Fully-qualified name for the message type to use when writing data.</p><p>Use the following format: <code>&#x3C;package name>.&#x3C;message type></code>.</p><p>Use a message type defined in the descriptor file.</p> |
9.  For text data, on the Data Format tab, configure the following properties:

    | Text Property                      | Description                                                                                                                                                                                           |
    | ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Text Field Path                    | Field that contains the text data to be written. All data must be incorporated into the specified field.                                                                                              |
    | Record Separator                   | <p>Characters to use to separate records. Use any valid Java string literal. For example, when writing to Windows, you might use \r to separate records.</p><p>By default, the destination uses .</p> |
    | On Missing Field                   | When a record does not include the text field, determines whether the destination reports the missing field as an error or ignores the missing field.                                                 |
    | Insert Record Separator if No Text | <p>When configured to ignore a missing text field, inserts the configured record separator string to create an empty line.</p><p>When not selected, discards records without the text field.</p>      |
    | Charset                            | Character set to use when writing data.                                                                                                                                                               |
10. For XML data, on the Data Format tab, configure the following properties:

    | XML Property    | Description                                                                                                                                                                                                                                                                                                |
    | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Pretty Format   | Adds indentation to make the resulting XML document easier to read. Increases the record size accordingly.                                                                                                                                                                                                 |
    | Validate Schema | Validates that the generated XML conforms to the specified schema definition. Records with invalid schemas are handled based on the error handling configured for the destination.Important: Regardless of whether you validate the XML schema, the destination requires the record in a specific format.  |
    | XML Schema      | The XML schema to use to validate records.                                                                                                                                                                                                                                                                 |
