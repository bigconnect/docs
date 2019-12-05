# Record Header Attributes

Record header attributes are attributes in record headers that you can use in pipeline logic, as needed.

Some stages create record header attributes for a particular purpose. This enables CRUD-enabled destinations to determine the operation type to use when processing records. Similarly, the Hive Metadata processor generates record header attributes that some destinations can use as part of the Drift Synchronization Solution for Hive.

Other stages include processing-related information in record header attributes for general use. For example, event-generating stages include the event type in record header attributes in case you want to process the event based on that information. And several origins include information such as the originating file name, location, or partition for each record.

You can use certain processors to create or update record header attributes. For example, you can use an Expression Evaluator to create attributes for record-based writes.

The inclusion of attributes in record headers does not require using them in the pipeline. You can, for example, use the CDC-enabled Salesforce origin in a non-CDC pipeline and ignore the CDC record header attributes that are automatically generated.

When writing data to destination systems, record header attributes are preserved with the record only when using the Google Pub/Sub Publisher destination or when using another destination with the SDC Record data format. To preserve the information when using other data formats, use the Expression Evaluator to copy information from record header attributes to record fields.

## Working with Header Attributes

You can use the Expression Evaluator or any scripting processor to create or update record header attributes. For example, the MongoDB destination requires a CRUD operation to be specified in a record header attribute. If the origin providing the data does not generate that information automatically, you can use the Expression Evaluator or scripting processor to set the attribute value.

Record header attributes are string values. You can use `record:attribute functions` in any expression to include attribute values in calculations.Important: Record header attributes do not have field paths. When using an attribute in an expression, use just the attribute name, surrounded by quotation marks since attributes are strings, as follows:

`{record:attribute('<attribute name>')}`

For example, the following Expression Evaluator adds the file and offset record header attributes created by the Directory origin to the record:

![](../../../.gitbook/assets/image%20%28398%29.png)

## Internal Attributes

Data Collector generates and updates some read-only internal record header attributes as records move from stage to stage. These attributes can be viewed for debugging issues, but can only be updated by Data Collector.

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
</table>## Viewing Attributes in Data Preview

You can use data preview to view the record header attributes associated with a record at any given point in the pipeline. To view record header attributes, enable the Show Record/Field Header data preview property.

For example, the following image shows a record generated by the Directory origin in data preview.

![](../../../.gitbook/assets/image%20%28211%29.png)

The "Record Header" list displays the set of read-only internal attributes in the record at this point of the pipeline. The header attributes under "values" are the attributes created by the Directory origin.

