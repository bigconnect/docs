# Record Header Attributes

Record header attributes are attributes in record headers that you can use in pipeline logic, as needed.

Some stages create record header attributes for a particular purpose. For example, CDC-enabled origins include the CRUD operation type in the sdc.operation.type record header attribute. This enables CRUD-enabled destinations to determine the operation type to use when processing records.

Other stages include processing-related information in record header attributes for general use. For example, event-generating stages include the event type in record header attributes in case you want to process the event based on that information. And several origins include information such as the originating file name, location, or partition for each record.

You can use certain processors to create or update record header attributes. For example, you can use an Expression Evaluator to create attributes for record-based writes.

The inclusion of attributes in record headers does not require using them in the pipeline. You can, for example, use the CDC-enabled Salesforce origin in a non-CDC pipeline and ignore the CDC record header attributes that are automatically generated.

When writing data to destination systems, record header attributes are preserved with the record only when using the Google Pub/Sub Publisher destination or when using another destination with the SDC Record data format. To preserve the information when using other data formats, use the Expression Evaluator to copy information from record header attributes to record fields.

## Working with Header Attributes

You can use the Expression Evaluator or any scripting processor to create or update record header attributes. For example, the MongoDB destination requires a CRUD operation to be specified in a record header attribute. If the origin providing the data does not generate that information automatically, you can use the Expression Evaluator or scripting processor to set the attribute value.

Record header attributes are string values. You can use `record:attribute functions` in any expression to include attribute values in calculations.Important: Record header attributes do not have field paths. When using an attribute in an expression, use just the attribute name, surrounded by quotation marks since attributes are strings, as follows:

`{record:attribute('<attribute name>')}`

For example, the following Expression Evaluator adds the file and offset record header attributes created by the Directory origin to the record:

![](../../../.gitbook/assets/image%20%28617%29.png)

## Internal Attributes

Data Pipelines generates and updates some read-only internal record header attributes as records move from stage to stage. These attributes can be viewed for debugging issues, but can only be updated by Data Pipelines.

The record:attribute function does not allow access to internal record header attributes. The following table describes the internal record header attributes and the functions that you can use to access the data in the pipeline:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Internal Record Header Attribute</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Related Function</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">stageCreator</td>
      <td style="text-align:left">The ID of the stage that created the record.</td>
      <td style="text-align:left">record:creator()</td>
    </tr>
    <tr>
      <td style="text-align:left">sourceId</td>
      <td style="text-align:left">Source of the record. Can include different information based on the origin
        type.</td>
      <td style="text-align:left">record:id()</td>
    </tr>
    <tr>
      <td style="text-align:left">stagesPath</td>
      <td style="text-align:left">List of stages that processed the record in order, by stage name.</td>
      <td
      style="text-align:left">record:path()</td>
    </tr>
    <tr>
      <td style="text-align:left">trackingId</td>
      <td style="text-align:left">The route the record has taken through the pipeline, starting with the
        sourceId, then listing the stages that processed the record.</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left">previousTrackingId</td>
      <td style="text-align:left">The tracking ID of the record before it entered the current stage.</td>
      <td
      style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left">errorStage</td>
      <td style="text-align:left">
        <p>The stage that generated the error.</p>
        <p>In error records only.</p>
      </td>
      <td style="text-align:left">record:errorStage()</td>
    </tr>
    <tr>
      <td style="text-align:left">errorStageLabel</td>
      <td style="text-align:left">
        <p>The user-defined name for a stage.</p>
        <p>In error records only.</p>
      </td>
      <td style="text-align:left">record:errorStageLabel()</td>
    </tr>
    <tr>
      <td style="text-align:left">errorCode</td>
      <td style="text-align:left">
        <p>The error code.</p>
        <p>In error records only.</p>
      </td>
      <td style="text-align:left">record:errorCode()</td>
    </tr>
    <tr>
      <td style="text-align:left">errorJobId</td>
      <td style="text-align:left">
        <p>The ID of the job that started the pipeline.</p>
        <p>Only in error records from pipelines started by Control Hub jobs.</p>
      </td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left">errorMessage</td>
      <td style="text-align:left">
        <p>The error message.</p>
        <p>In error records only.</p>
      </td>
      <td style="text-align:left">record:errorMessage()</td>
    </tr>
    <tr>
      <td style="text-align:left">errorTimestamp</td>
      <td style="text-align:left">
        <p>The time that the error occurred.</p>
        <p>In error records only.</p>
      </td>
      <td style="text-align:left">record:errorTime()</td>
    </tr>
    <tr>
      <td style="text-align:left">errorStackTrace</td>
      <td style="text-align:left">
        <p>The stack trace associated with the error.</p>
        <p>In error records only.</p>
      </td>
      <td style="text-align:left">n/a</td>
    </tr>
  </tbody>
</table>

## Header Attribute-Generating Stages

The following table lists the stages that generate record header attributes to enable special processing:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Stage</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">CDC-enabled origins</td>
      <td style="text-align:left">Include the CRUD operation type in the sdc.operation.type header attribute
        and can include additional CRUD and CDC information in record header attributes.
        For more information, see CDC-Enabled Origins.</td>
    </tr>
    <tr>
      <td style="text-align:left">Origins that process Avro data and the Data Parser processor</td>
      <td style="text-align:left">Include the Avro schema in an avroSchema record header attribute.</td>
    </tr>
    <tr>
      <td style="text-align:left">Stages that generate events</td>
      <td style="text-align:left">Generate record header attributes for event records. For details about
        event record header attributes, see &quot;Event Records&quot; in the stage
        documentation.</td>
    </tr>
    <tr>
      <td style="text-align:left">Amazon S3 origin</td>
      <td style="text-align:left">Can be configured to include system-defined and user-defined object metadata
        in record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Amazon SQS Consumer origin</td>
      <td style="text-align:left">Can be configured to include SQS message attributes in record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Azure Data Lake Storage Gen1 origin</td>
      <td style="text-align:left">Includes information about the originating file for the record in record
        header attributes.
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Azure Data Lake Storage Gen2 origin</td>
      <td style="text-align:left">Includes information about the originating file for the record in record
        header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Directory origin</td>
      <td style="text-align:left">Includes information about the originating file for the record in record
        header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">File Tail origin</td>
      <td style="text-align:left">Includes information about the originating file for the record in record
        header attributes.
        <br />Can be configured to use tag attributes for sets of files.</td>
    </tr>
    <tr>
      <td style="text-align:left">Google Pub/Sub Subscriber origin</td>
      <td style="text-align:left">When available, includes user-defined message attributes in record header
        attributes</td>
    </tr>
    <tr>
      <td style="text-align:left">Groovy Scripting origin</td>
      <td style="text-align:left">Can be configured to create record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Hadoop FS origin</td>
      <td style="text-align:left">Includes information about the file and offset in record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Hadoop FS Standalone origin</td>
      <td style="text-align:left">Includes information about the originating file for the record in record
        header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">HTTP Client origin</td>
      <td style="text-align:left">Includes response header fields in record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">HTTP Server origin</td>
      <td style="text-align:left">Includes information about the requested URL and request header fields
        in record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">JavaScript Scripting origin</td>
      <td style="text-align:left">Can be configured to create record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">JDBC Multitable Consumer origin</td>
      <td style="text-align:left">Includes table and data type information in JDBC record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">JDBC Query Consumer origin</td>
      <td style="text-align:left">Can be configured to include table and data type information in JDBC record
        header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Jython Scripting origin</td>
      <td style="text-align:left">Can be configured to create record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Kafka Consumer origin</td>
      <td style="text-align:left">Includes information about the origins of the record in record header
        attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Kafka Multitopic Consumer origin</td>
      <td style="text-align:left">Includes information about the origins of the record in record header
        attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">MQTT Subscriber origin</td>
      <td style="text-align:left">Includes information about the origins of the record in record header
        attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">RabbitMQ Consumer origin</td>
      <td style="text-align:left">Includes RabbitMQ attributes in record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">REST Service origin</td>
      <td style="text-align:left">Includes information about the URL and request header fields in record
        header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Salesforce origin</td>
      <td style="text-align:left">Includes Salesforce information about the origins of the record in Salesforce
        header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">SAP HANA Query Consumer origin</td>
      <td style="text-align:left">Can be configured to include table and data type information in JDBC header
        attributes.
        <br />Can be configured to include SAP HANA connection information in SAP HANA
        header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">SFTP/FTP/FTPS Client origin</td>
      <td style="text-align:left">Includes information about the originating file for the record in record
        header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">SQL Server 2019 BDC Multitable Consumer origin</td>
      <td style="text-align:left">Includes table and data type information in JDBC record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Teradata Consumer origin</td>
      <td style="text-align:left">Includes table and data type information in JDBC record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Couchbase Lookup processor</td>
      <td style="text-align:left">Includes information about the state of the looked-up document in a record
        header attribute.</td>
    </tr>
    <tr>
      <td style="text-align:left">Expression Evaluator processor</td>
      <td style="text-align:left">Can be configured to create or update record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Groovy Evaluator processor</td>
      <td style="text-align:left">Can be configured to create or update record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Hive Metadata processor</td>
      <td style="text-align:left">
        <p>Generates record header attributes for the data record. These attributes
          can be used for record-based writes as part of the Drift Synchronization
          Solution for Hive.
          <br />
        </p>
        <p>Can be configured to add custom header attributes to the metadata record.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">HTTP Client processor</td>
      <td style="text-align:left">Includes response header fields in record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">JavaScript Evaluator processor</td>
      <td style="text-align:left">Can be configured to create or update record header attributes.</td>
    </tr>
    <tr>
      <td style="text-align:left">Schema Generator processor</td>
      <td style="text-align:left">Generates schemas and write them to a user-defined record header attribute.</td>
    </tr>
  </tbody>
</table>

## Record Header Attributes for Record-Based Writes

Destinations can use information in record header attributes to write data. Destinations that write Avro data can use Avro schemas in the record header. The Hadoop FS destination can use record header attributes to determine the directory to write to and when to roll a file as part of the Drift Synchronization Solution for Hive.

To use a record header attribute, configure the destination to use the header attribute and ensure that the records include the header attribute.

The Hive Metadata processor automatically generates record header attributes for Hadoop FS to use as part of the Drift Synchronization Solution for Hive. For all other destinations, you can use the Expression Evaluator or a scripting processor to add record header attributes.

You can use the following record header attributes in destinations:

**targetDirectory attribute in all the Azure Data Lake Storage destinations and in the Hadoop FS and Local FS destinations**

The targetDirectory record header attribute defines the directory where the record is written. If the directory does not exist, the destination creates the directory. The targetDirectory header attribute replaces the Directory Template property in the destination.

When you use targetDirectory to provide the directory, the time basis configured for the destination is used only for determining whether a record is late. Time basis is not used to determine the output directories to create or to write records to directories. 

To use the targetDirectory header attribute, on the Output tab, select Directory in Header.

**avroSchema attribute in destinations that write Avro data** 

The avroSchema header attribute defines the Avro schema for the record. When you use this header attribute, you cannot define an Avro schema to use in the destination. 

To use the avroSchema header attribute, on the Data Format tab, select the Avro data format, and then for the Avro Schema Location property, select In Record Header.

**roll attribute in all the Azure Data Lake Storage destinations and in the Hadoop FS, and Local FS destination**s

The roll attribute, when present in the record header, triggers a roll of the file.

You can define the name of the roll header attribute. When you use the Hive Metadata processor to generate the roll header attribute, use the default "roll" attribute name. When you use an Expression Evaluator, use the name of the roll attribute that you defined in the processor.

To use a roll header attribute, on the Output tab, select Use Roll Attribute and define the name of the attribute.

## Generating Attributes for Record-Based Writes

You can use the Hive Metadata processor, the Expression Evaluator, or a scripting processor to generate record header attributes for record-based writes. The Hive Metadata processor automatically generates record header attributes for Hadoop FS to use as part of the Drift Synchronization Solution for Hive. For all other destinations, you can use the Expression Evaluator or a scripting processor to add record header attributes.

To use the Expression Evaluator or scripting processor, you must generate record header attributes as expected by the destination. Use the following guidelines to generate record header attributes:

**Generating the target directory**

When using the Expression Evaluator or scripting processor to generate the target directory, note the following details:

* The destination expects the directory in a header attribute named "targetDirectory".
* The destination uses the directory exactly as written in the targetDirectory header attribute. Unlike directory templates, the directory specified in the targetDirectory attribute should not include any components that require evaluation, such as runtime parameters, variables, or runtime properties.
* When you define the expression that evaluates to a directory, you can use any valid component, including expressions that evaluate data in the record.

For example, you want to write records to different directories based on the Data Pipelines that runs the pipeline, and the region and store ID where the transaction took place. You can set up a runtime resource named DIR that defines the base for the directory and define DIR for each Data Data Pipelines instance that runs the pipeline. Then, you can use the following expression in the Expression Evaluator to define the targetDirectory attribute:

`${runtime:conf('DIR')/transactions/${record.value('/region')}/${record.value('/storeID')}`

**Generating the Avro schema**

When using the Expression Evaluator or a scripting processor to generate the Avro schema, note the following details:

* The destination expects the Avro schema in a header attribute named "avroSchema"
* Use the standard Avro schema format, for example:

```javascript
{"type":"record","name":"table_name","namespace":"database_name",
"fields":[{"name":"int_val","type":["null","int"],"default":null},
{"name":"str_val","type":["null","string"],"default":null}]}
```

* The database name and table name must be included in the Avro schema.

{% hint style="info" %}
You might use an Avro schema generator to help generate the Avro schema.
{% endhint %}

**Generating the roll attribute**

When using the Expression Evaluator or scripting processor to generate the roll attribute, note the following details:

* Use any name for the attribute and specify the attribute name in the destination.
* Configure an expression that defines when to roll files.

To define these record header attributes in the Expression Evaluator, perform the following steps:

1. On the **Expressions** tab of the Expression Evaluator, specify the **Header Attribute** name.  
   To generate a target directory, use _targetDirectory_.  
   To generate an Avro schema, use _avroSchema_.

   You can use any name for a roll indicator header attribute.

2. For the **Header Attribute** **Expression**, define the expression that evaluates to the information you want the destination to use.

For information about generating record header attributes with scripting processors, see the scripting processor documentation.

## Viewing Attributes in Data Preview

You can use data preview to view the record header attributes associated with a record at any given point in the pipeline. To view record header attributes, enable the Show Record/Field Header data preview property.

For example, the following image shows a record generated by the Directory origin in data preview.

![](../../../.gitbook/assets/image%20%28334%29.png)

The "Record Header" list displays the set of read-only internal attributes in the record at this point of the pipeline. The header attributes under "values" are the attributes created by the Directory origin.

