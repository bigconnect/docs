# SQL Parser

The SQL Parser parses a SQL query in a string field. When parsing a query, the processor generates fields based on the fields defined in the SQL query and specifies the CRUD operation, table, and schema information in record header attributes.

Use the processor to parse SQL queries written to a field when the Oracle CDC Client origin is configured to skip parsing. This avoids the possibility of redo logs switching before they can be processed.

When you configure a SQL Parser, you define the field that contains the SQL statements to parse, and the target field where fields from the SQL query are to be added. If the fields from the SQL query do not exist in the record, they are created. If they already exist, they are overwritten.

You can specify that the processor connects to the database to resolve unknown field types.

If your database is case-sensitive, you can configure the processor to interpret case-sensitive schema, table, and column names.

The processor also includes CDC and CRUD information in record header attributes so records can be easily processed by CRUD-enabled destinations.

### Using Multiple Pipelines <a href="#ariaid-title2" id="ariaid-title2"></a>

When a database contains very wide tables, the Oracle CDC Client origin requires more time to read the change data and to parse SQL queries due to the large amounts of information it now has to process. Note that reading the change data is bound by I/O constraints while parsing the SQL queries is bound by CPU constraints.

Redo logs can switch quite frequently. If it takes longer to read the change data and parse the SQL queries than it does for the redo logs to switch, data is lost.

One solution is to use the SQL Parser processor and multiple pipelines. The first pipeline contains the origin and an intermediate endpoint, like a local file system or Kafka. Configure the origin to not parse SQL queries by clearing the Parse SQL Query property. The second pipeline passes records from the intermediate endpoint to the SQL Parser to parse the SQL query and to update the fields.

The reason for multiple pipelines is that pipelines are synchronous by default. If the Oracle CDC Client origin and the SQL Parser processor are in the same pipeline, the origin reads data only after the pipeline completes processing the previous batch. This results in the same problem where redo logs can switch before the pipeline finishes processing the data.

Using an intermediate endpoint makes the pipeline asynchronous. Meaning, one pipeline can process a batch independent of the other. Using this approach, the origin can read the redo logs without waiting for the SQL Parser to finish and therefore no data is lost.

#### Example <a href="#ariaid-title3" id="ariaid-title3"></a>

You want to create two pipelines and use the SQL Parser to process the SQL queries instead of the Oracle CDC Client. The first pipeline contains the Oracle CDC Client and an intermediate endpoint, like Kafka. For example:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/SQLParser-Pipeline1.png)

The second pipeline reads records from the intermediate endpoint and passes the records to the SQL Parser processor. The processor parses the SQL query located in the /sql field, and then the JDBC processor writes the data to the final destination.

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/SQLParser-Pipeline2.png)

### Resolving the Schema <a href="#ariaid-title4" id="ariaid-title4"></a>

For INSERT operations, you specify where the new fields are to be added as subfields. Use Resolve Schema from DB to resolve all field types. If you do not select this option, all fields are returned as strings in the form they are in the SQL statement.

Note: Field type resolution occurs when the first record for a table is read, and then again only when a field not previously encountered in the table is read.When you select this option, you must install JDBC drivers for the database and configure the JDBC connection properties.&#x20;

Use Case Sensitive Names when your database contains case-sensitive schema, table, and column names. If you do not select this option, the SQL Parser processor submits names in all uppercase.

### Unsupported Data Types <a href="#ariaid-title5" id="ariaid-title5"></a>

You can configure how the processor handles records that contain unsupported data types. The processor can perform the following actions:

* Pass the record to the pipeline without the unsupported data types.
* Pass the record to error without the unsupported data types.
* Discard the record.

You can configure the processor to include the unsupported data types in the record. When you include unsupported types, the processor includes the field names and passes the data as unparsed strings, when possible.

The SQL Parser does not support the same Oracle data types that the Oracle CDC Client does not support. For a complete list of unsupported data types, see Oracle CDC Client Unsupported Data Types.

### Generated Records <a href="#ariaid-title6" id="ariaid-title6"></a>

The SQL Parser parses a SQL query in a field and creates fields based on the query. The processor also includes the CRUD operation type in the sdc.operation.type record header attribute. This enables CRUD-enabled destinations to determine the operation type to use when processing records.

When configuring the SQL Parser, you specify the field containing the SQL query. For example, if the SQL Parser processes the following SQL query statement located in the `/sql` field:

```
INSERT INTO  "sdc"."mc"("Part", "Cost") VALUES('levers', 250)
```

It writes the following fields to the record:

| Part   | Cost |
| ------ | ---- |
| levers | 250  |

If the Part and Cost field already exist, the processor overwrites those fields. If they do not exist, the processor creates them.

SQL Parser supports the following operations:

* INSERT
* UPDATE
* DELETE

#### CRUD Operation Header Attributes <a href="#ariaid-title7" id="ariaid-title7"></a>

Like the Oracle CDC Client, the SQL Parser specifies the operation type in both of the following record header attributes:

sdc.operation.typeThe SQL Parser evaluates the Oplog operation type associated with each entry that it processes and, when appropriate, it writes the operation type to the sdc.operation.type record header attribute.oracle.cdc.operationThe SQL Parser also writes the Oplog CRUD operation type to the oracle.cdc.operation record header attribute.CRUD-enabled destinations check for this attribute for the operation type after checking the sdc.operation.type attribute.

For more information, see Oracle CDC Client CRUD Operation Header Attributes.

#### CDC Header Attributes <a href="#ariaid-title8" id="ariaid-title8"></a>

The SQL Parser processor preserves the following CDC header attributes if they already exist and creates them if they don't:

* TABLE\_NAME
* sql.table
* TABLE\_SCHEM

The SQL Parser overwrites the following header attributes:

* jdbc.\<columnName>.precision
* jdbc.\<columnName>.scale

These are table column names, not field names. For example, if the column name is `part`, then the headers are jdbc.part.precision and jdbc.part.scale.

### Configuring an SQL Parser Processor <a href="#ariaid-title9" id="ariaid-title9"></a>

Configure the SQL Parser to parse SQL queries.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Parse tab, configure the following properties:

    | SQL Parser Property                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | SQL Field                            | Name of the field containing the SQL query.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Target Field                         | <p>Field in which fields generated by the SQL query are to be placed.</p><p>You can specify an existing field or a new field. If the field exists, any new fields as a result of the SQL query are added as subfields and overwrite the existing field. If the field does not exist, SQL Parser creates the field.</p>                                                                                                                                                                                                                                                                                                                                                                             |
    | Resolve Schema from DB               | <p>Queries the database to resolve the schema and to resolve fields to their correct data type.</p><p>If you do not select this option, all fields are returned as strings in the form they are in the SQL statement.</p><p>When you select this option, you must install JDBC drivers for the database and configure the JDBC connection properties. </p>                                                                                                                                                                                                                                                                                                                                         |
    | Unsupported Field Type               | <p>Determines the behavior when the processor encounters unsupported data types in the record:</p><ul><li>Send Record to Pipeline - The processor ignores unsupported data types and passes the record with only supported data types to the pipeline.</li><li>Send Record to Error - The processor handles the record based on the error record handling configured for the stage. The error record contains only the supported data types.</li><li>Discard Record - The processor discards the record.</li></ul><p>The SQL Parser does not support the same Oracle data types that the Oracle CDC Client does not support. For a list of unsupported data types, see Unsupported Data Types.</p> |
    | Add Unsupported Fields to Records    | Includes fields with unsupported data types in the record. Includes the field names and the unparsed string values of the unsupported fields, when possible.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Case Sensitive Names                 | Enables using case-sensitive schema, table, and column names. When not enabled, the processor changes all names to uppercase.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | Date Format                          | Format for date fields in the incoming SQL.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Timestamp With Local Timezone Format | Format for fields of type Timestamp with Local Timezone.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | Zoned DateTime Format                | <p>Format for the date, datetime, or time data if they are using zoned datetime format.</p><p>For example, you can use yyyy-MM-dd'T'HH:mm:ssX[VV] for datetime values with a UTC offset and time zone. If the datetime value does not include a UTC offset, the stage uses the minimum offset for the specified time stamp.</p>                                                                                                                                                                                                                                                                                                                                                                    |
    | DB Time Zone                         | Time zone of the database. Specify when the database operates in a different time zone from Data Pipelines.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
3.  On the JDBC tab, configure the following properties. You configure this tab only when Resolve Schema from DB is selected.

    | JDBC Property                            | Description                                                                                                                                                                                    |
    | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JDBC Connection String                   | Connection string used to connect to the database. Use the connection string format required by the database vendor.                                                                           |
    | Use Credentials                          | Enables entering credentials on the Credentials tab. Use when you do not include credentials in the JDBC connection string.                                                                    |
    | Additional JDBC Configuration Properties | <p>Additional JDBC configuration properties to use. To add properties, click Add and define the JDBC property name and value.</p><p>Use the property names and values as expected by JDBC.</p> |
4.  If you configured the origin to enter JDBC credentials separately from the JDBC connection string on the JDBC tab, then configure the following properties on the Credentials tab:

    | Credentials Property | Description                                                                                                                       |
    | -------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
    | Username             | <p>User name for the JDBC connection.</p><p>The user account must have the correct permissions or privileges in the database.</p> |
    | Password             | Password for the JDBC user name.                                                                                                  |
