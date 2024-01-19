# JavaScript Evaluator

The JavaScript Evaluator processor uses JavaScript code to process data. Use the JavaScript Evaluator processor to use JavaScript code to perform custom processing.You can develop the following scripts for the processor:

* Initialization script - Optional initialization script that sets up any required resources or connections. The initialization script is run once when the pipeline starts.
* Main processing script - Main script that processes data. The main script is run for each record or each batch of data, based on the configured processing mode.
* Destroy script - Optional destroy script that closes any resources or connections that were opened by the processor. The destroy script is run once when the pipeline stops.

When you use a JavaScript Evaluator processor in a pipeline, Data Pipelines passes a batch of data to the processor and converts the data to a scripting-friendly data structure for processing.

You can call external Java code from the script. The JavaScript Evaluator processor supports Java version 8u40 and later and ECMAScript version 5.1. The processor runs on the Nashorn JavaScript engine.

The JavaScript Evaluator processor provides extensive sample code that you can use to develop your script.

When configuring the processor, you specify the processing mode, enter the scripts, and specify the method to access records and any script parameters used in the script.

### Processing Mode <a href="#ariaid-title2" id="ariaid-title2"></a>

You can choose the processing mode that the JavaScript Evaluator uses to process the main script. You can use the same script in each processing mode. However, you should include error handling in the main script before you run in batch mode.

The JavaScript Evaluator provides the following processing modes for the main script:

**Record by Record**

The processor calls the script for each record. The processor passes the record to the script as a map and processes each record individually.

The script does not require error handling logic. Error records are passed to the processor for error handling. The processor handles error records based on the On Record Error property.

Use this mode to avoid including error handling logic in the code. Since this mode calls the script for each record, pipeline performance will be negatively affected.

**Batch by Batch**

The processor calls the script for each batch. The processor passes the batch to the script as an array and processes the batch at one time.Include error handling logic in the script. Without error handling logic, a single error record sends the entire batch to the processor for error handling. The processor handles error records based on the On Record Error property.Use this mode to improve performance by processing a batch of data at a time.

### JavaScript Scripting Objects <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use different scripting objects in the JavaScript Evaluator processor, based on the type of script:

| Script Type | Valid Scripting Objects                                                                                                                                                                              |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Init        | <p>You can use the following scripting objects in the initialization script:</p><ul><li>state</li><li>log</li><li>sdc</li><li>sdcFunctions (Deprecated)</li></ul>                                    |
| Main        | <p>You can use the following scripting objects in the main script:</p><ul><li>records</li><li>state</li><li>log</li><li>output</li><li>error</li><li>sdc</li><li>sdcFunctions (Deprecated)</li></ul> |
| Destroy     | <p>You can use the following scripting objects in the destroy script:</p><ul><li>state</li><li>log</li><li>sdc</li><li>sdcFunctions (Deprecated)</li></ul>                                           |

Note: You do not need to use a scripting object to call pipeline runtime parameters from the JavaScript Evaluator processor. You can simply use the following syntax in any of the processor scripts: `${<parameter name>}`.The scripting objects work the same within each script type:recordsA collection of records to process. The `records` object includes different elements based on the processing mode that you use:

* Record by Record - The records array includes one record element. A record includes a single `value` element. The `value` element contains the data for the record.
* Batch by Batch - The records array includes all records in the batch.

Map and list record fields are mapped to JavaScript maps and arrays, respectively.stateAn object to store information between invocations of the init, main, and destroy scripts. A state is a map object that includes a collection of key/value pairs. You can use the `state` object to cache data such as lookups, counters, or a connection to an external system.The `state` object functions much like a member variable:

* The information is transient and is lost when the pipeline stops or restarts.
* The `state` object is available only for the instance of the processor stage it is defined in. If the pipeline executes in cluster mode, the `state` object is not shared across nodes.

The same instance of the `state` object is available to all three scripts. For example, you might use the init script to open a connection to a database and then store a reference to that connection in the `state` object. In the main script, you can access the open connection using the `state` object. Then in the destroy script, you can close the connection using the `state` object.

Warning: The `state` object is best used for a fixed or static set of data. Adding to the cache on every record or batch can quickly consume the memory allocated to Data Pipelines and cause out-of-memory exceptions.logAn object that writes messages to the log4j log. Use `sdc.log` to access the object configured for the stage. The object includes methods that correspond to the level in the log file:

* `info(<message template>, <arguments>...)`
* `warn(<message template>, <arguments>...)`
* `error(<message template>, <arguments>...)`
* `trace(<message template>, <arguments>...)`

The message template can contain positional variables, indicated by curly brackets: { }. When writing messages, the method replaces each variable with the argument in the corresponding position - that is, the method replaces the first { } occurrence with the first argument, and so on.

**output**

An object that writes the record to the output batch. Use `sdc.output` to access the object configured for the stage. The object includes a `write(<record>)` method.

**error**

An object that passes error records for error handling. Use `sdc.error` to access the object configured for the stage. The object includes a `write(<record>, <String message>)` method.

**sdc**

A wrapper object that accesses constants, methods, and objects available to the scripts.The `sdc` object contains the following constant:

* `userParams` - Dictionary that contains the script parameters and values configured on the Advanced tab with the Parameters in Script property.

The `sdc` object contains the following methods:

* `createRecord(<String record ID>)` - Returns a new record with the passed ID. Pass a string that uniquely identifies the record and includes enough information to track the record source.
* `isPreview()` - Returns a Boolean value that indicates whether the pipeline is in preview mode.
* `getFieldNull(<record>, <String field path>)` - Returns one of the following:
  * The value of the field at the specified path if the value is not null
  * The null object defined for the field type, such as `NULL_INTEGER` or `NULL_STRING`, if the value is null
  * The unassigned null object `NULL` if there is no field at the specified path
* `createMap(<Boolean list-map>)` - Returns a map for use as a field in a record. Pass `true` to create a list-map field, or `false` to create a map field.
* `createEvent(<String type>, <Integer version>)` - Returns a new event record with the specified event type and version. Verify that the stage enables event generation before implementing event methods.
* `toEvent(<event record>)` - Sends an event record to the event output stream. Verify that the stage enables event generation before implementing event methods.
* `pipelineParameters()` - Returns a map of all runtime parameters defined for the pipeline.

sdcFunctionsAn object that runs functions that evaluate or modify data.Important: The `sdcFunctions` object is now deprecated and will be removed in a future release. To evaluate and modify data, use the methods in the `sdc` object. The `sdcFunctions` object includes the following methods:

* `getFieldNull(<record>, <String field path>)` - Returns one of the following:
  * The value of the field at the specified path if the value is not null
  * The null object defined for the field type, such as `NULL_INTEGER` or `NULL_STRING`, if the value is null
  * The unassigned null object `NULL` if there is no field at the specified path
* `createRecord(<String record ID>)` - Returns a new record with the passed ID. Pass a string that uniquely identifies the record and includes enough information to track the record source.
* `createMap(<Boolean list-map>)` - Returns a map for use as a field in a record. Pass `true` to create a list-map field, or `false` to create a map field.
* `createEvent(<String type>, <Integer version>)` - Returns a new event record with the specified event type and version. Verify that the stage enables event generation before implementing event methods.
* `toEvent(<event record>)` - Sends an event record to the event output stream. Verify that the stage enables event generation before implementing event methods.
* `isPreview()` - Returns a Boolean value that indicates whether the pipeline is in preview mode.

### Accessing Record Details <a href="#ariaid-title4" id="ariaid-title4"></a>

By default, you use native types from the scripting language to access records in scripts. However, with native types, you cannot easily access all the features of Data Pipelines records, such as field attributes.

To access records in scripts directly as Data Pipelines records, configure the stage to use the Data Pipelines Java API to process records in scripts by setting the Record Type advanced property to `Data Pipelines Records`.

In the script, reference the needed classes from the Java package `com.streamsets.pipeline.api` and then use the appropriate methods to access records and fields. With the Data Pipelines Java API, scripts can access all the features of Data Pipelines records.

For example, include the following lines in the script to use the Data Pipelines Java API to do the following:

* Create a String field named `new` and set its value to `new-value`.
* Update the existing field named `old` to set the value of the `attr` attribute to `attr-value`.

```
var Field = Java.type('com.streamsets.pipeline.api.Field');
...
records[i].sdcRecord.set('/new', Field.create(Field.Type.STRING, 'new-value'));
records[i].sdcRecord.get('/old').setAttribute('attr', 'attr-value');
...
```

### Processing List-Map Data <a href="#ariaid-title5" id="ariaid-title5"></a>

In scripts that process list-map data, treat the data as maps.

List-map is a Data Pipelines data type that allows you to use standard record functions to work with delimited data. When an origin reads delimited data, it generates list-map fields by default.

The JavaScript Evaluator can read and pass list-map data. But to process data in a list-map field, treat the field as a map in the script.

### Type Handling <a href="#ariaid-title6" id="ariaid-title6"></a>

Though JavaScript does not use type information when processing data, passing data to the rest of the pipeline requires data types. Note the following type information when working with the JavaScript Evaluator processor:Data type of null valuesYou can associate null values with a data type. For example, if the script assigns a null value to an Integer field, the field is returned to the pipeline as an integer with a null value.Use constants in the JavaScript code to create a new field of a specific data type with a null value. For example, you can create a new String field with a null value by assigning the `NULL_STRING` type constant to the field as follows:

```
records[i].value.new_field = sdc.NULL_STRING
```

Date fieldsUse the String data type to create a new field to store a date with a specific format. For example, the following sample code creates a new String field that stores the current date using the format `YYYY-MM-dd`:

```
 // Define a date object to record the current date
var date = new Date()

for(var i = 0; i < records.length; i++) {
  try {
    // Create a string field to store the current date with the specified format
    records[i].value.date = date.getFullYear()+ "-" + date.getMonth() + "-" + date.getDate()
    
    // Write record to the output
    sdc.output.write(records[i]);
  } catch (e) {
    // Send record to error
    sdc.error.write(records[i], e);
  }
}
```

Values retain their original typeValues retain their original type regardless of whether a script modifies the value.

### Event Generation <a href="#ariaid-title7" id="ariaid-title7"></a>

You can use the JavaScript Evaluator processor to generate event records for an event stream. Enable event generation when you want the stage to generate an event record based on scripting logic.

As with any record, you can pass event records downstream to a destination for event storage or to any executor that can be configured to use the event.&#x20;

To generate events:

1.  On the General tab, select the Produce Events property.

    This enables the event output stream for use.
2. Include both of the following methods in the script:
   *   `sdc.createEvent(<String type>, <Integer version>)` - Creates an event record with the specified event type and version number. You can create a new event type or use an existing event type. Existing event types are documented in other event-generating stages.

       The event record contains no record fields. Generate record fields as needed.
   * `sdc.toEvent(<record>)` - Use to pass events to the event output stream.

### Working with Record Header Attributes <a href="#ariaid-title8" id="ariaid-title8"></a>

You can use the JavaScript Evaluator processor to read, update, or create record header attributes.

Use a map when creating or updating a header attribute. If a header attribute exists, the script updates the value. If it does not exist, the script creates the attribute and sets it to the specified value.

All records include a set of internal record header attributes that stages update automatically as they process records. Error records also have their own set of internal header attributes. You cannot change values of internal header attributes in scripts.

Some stages generate custom record header attributes that are meant to be used in particular ways. For example, the Oracle CDC Client origin specifies the operation type for a record in a record header attribute. And event-generating stages create a set of event header attributes for event records.

You can use the following record header variables to work with header attributes:

* `record.<header name>` - Use to return the value of a header attribute.
* `record.attributes` - Use to return a map of custom record header attributes, or to create or update a specific record header attribute.

Tip: Use data preview to view the record header attributes included in a record.

#### Viewing Record Header Attributes <a href="#ariaid-title9" id="ariaid-title9"></a>

You can use data preview to view the record header attributes associated with a record at any given point in the pipeline. To view record header attributes, enable the Show Record/Field Header data preview property.

For example, the following image shows a record generated by the Directory origin in data preview.

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/HeaderAttribute-Preview.png)

The "Record Header" list displays the set of read-only internal attributes in the record at this point of the pipeline. You can use the `record.<header name>` variable to return values for these attributes.

The header attributes under "values" are the attributes created by the Directory origin. You can use the `record.attributes` variable to return or modify these attributes. When you use the `record.attributes` variable to create a header attribute, it displays in this location during data preview.

### Accessing Whole File Format Records <a href="#ariaid-title10" id="ariaid-title10"></a>

In a pipeline that processes the whole file data format, you can use a JavaScript Evaluator to read the whole file data.

The processor can access the fileref field in a whole file record by creating an input stream using the getInputStream() API. For example, you might use the processor to read the file data in the fileref field and then create new records with the data. The processor can access the fileref field, but cannot modify information in the field.

Use the following lines to create and then read the input stream:

```
input_stream = record.value['fileRef'].getInputStream()
input_stream.read()
```

After the processor reads the input stream, include the following line in the code to close the input stream:

```
input_stream.close()
```

### Calling External Java Code <a href="#ariaid-title11" id="ariaid-title11"></a>

You can call external Java code from the JavaScript Evaluator processor. Simply install the external Java library to make it available to the processor. Then, call the external Java code from the script that you develop for the processor.

Note: The JavaScript Evaluator processor is included in the Data Pipelines basic stage library. When you install an external Java library for the JavaScript Evaluator processor, you must make the external library accessible to the basic stage library.To call the external Java code, simply obtain a reference to the Java type in your script, as follows:

```
var <class name> = Java.type('<package>.<class name>');
```

For example, let's say that you installed the Bouncy Castle JAR file to compute SHA-3 (Secure Hash Algorithm 3) digests. Add the following statement to your script:

```
var DigestSHA3 = Java.type('org.bouncycastle.jcajce.provider.digest.SHA3.DigestSHA3');
```

### Configuring a JavaScript Evaluator <a href="#ariaid-title12" id="ariaid-title12"></a>

Configure a JavaScript Evaluator processor to use custom JavaScript in a pipeline.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Produce Events   | Generates event records when events occur. Use for event handling.                                                                                                                                                                                                   |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the JavaScript tab, configure the following properties:

    | JavaScript Evaluator Property | Description                                                                                                                                                                                                                                                                                     |
    | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Script                        | <p>Main processing script to use.</p><p>Runs for each record or each batch of data, based on the configured processing mode.</p>                                                                                                                                                                |
    | Record Processing Mode        | <p>Determines how the JavaScript Evaluator processes data:</p><ul><li>Record by Record - Processes records individually. Performs error handling.</li><li>Batch by Batch - Processes records in batches. Requires error handling code in the script.</li></ul><p>Default is Batch by Batch.</p> |
    | Init Script                   | <p>Optional initialization script to use.</p><p>Use to set up any required connections or resources. Runs once when the pipeline starts.</p>                                                                                                                                                    |
    | Destroy Script                | <p>Optional destroy script to use.</p><p>Use to close any connections or resources that were used. Runs once when the pipeline stops.</p>                                                                                                                                                       |
3.  On the Advanced tab, configure the following property:

    | Advanced Property    | Description                                                                                                                                                                                                                                                                                     |
    | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Record Type          | <p>Record type to use during script execution:</p><ul><li>Data Pipelines Records - Select when scripts use Data Pipelines Java API methods to access records.</li><li>Native Objects - Select when scripts use native types to access records.</li></ul><p>Default value is Native Objects.</p> |
    | Parameters in Script | <p>Script parameters and their values.</p><p>The script accesses the values with the <code>sdc.userParams</code> dictionary.</p>                                                                                                                                                                |
