# Groovy Scripting

The Groovy Scripting origin runs a Groovy script to create Data Pipelines records. The Groovy Scripting origin supports Groovy version 2.4.The script runs for the duration of the pipeline. The origin can support a complex multithreaded script or a simple single-threaded script. The script can act on script parameters configured in the stage. The basic flow of a script must do the following:

* Create threads if supporting multithreaded processing
* Create batches
* Create records
* Add the records to a batch
* Process the batch
* Stop when the pipeline stops

The script must handle all necessary processing, such as generating events, sending errors for handling, and stopping when users stop the pipeline or when there is no more data. You can call external Java code from the script.

To handle restarts, the script must maintain an offset to track where the origin stopped and should restart. For the offset, the script requires a key, called an entity, associated with a unique value. For multithreaded processing, the entity must identify the partition of data processed by each thread. The method that processes batches saves an offset value for each entity.

For example, suppose your script processes data about U.S. states, using an API to read data with a URL of the form `../<state>&page=<number>`. In the script, each thread reads data from one state until finished with that state. You can set the entity to the state and the offset to the page number.

You can reset the origin to process all available data.

The origin provides extensive sample code that you can use to develop your script.

When configuring the origin, you enter the script and the inputs required, including the batch size and number of threads, along with any script parameters used in the script.

### Scripting Objects <a href="#ariaid-title2" id="ariaid-title2"></a>

Scripts in the Groovy Scripting origin can use the following objects:recordAn object that contains fields and values to process. Create new `record` objects with the `sdc.createRecord(<String record ID>)` method. The methods available to the object depend on the origin configuration. You can configure the record type to be native objects or Data Pipelines records.For more information see Accessing Record Details.batchAn object that collects records to process together. Create new `batch` objects with the `sdc.createBatch()` method. The object includes the following methods:

* `add(<record>)` - Appends a record to the batch.
* `add(<record[]>)` - Appends a list of records to the batch.
* `addError(<record>,<String message>)` - Appends an error record to the batch. The appended error record contains the associated error message.
* `addEvent(<event record>)` - Appends an event to the batch. Verify that the stage enables event generation before implementing event methods.
* `size()` - Returns the number of records in the batch.
* `process(<String entity>, <String offset>)` - Processes the batch and commits the offset for the named entity.
* `getSourceResponseRecords()` - After processing a batch, retrieves any response records returned by downstream stages.

logAn object that writes messages to the log4j log. Use `sdc.log` to access the object configured for the stage. The object includes methods that correspond to the level in the log file:

* `info(<message template>, <arguments>...)`
* `warn(<message template>, <arguments>...)`
* `error(<message template>, <arguments>...)`
* `trace(<message template>, <arguments>...)`

The message template can contain positional variables, indicated by curly brackets: { }. When writing messages, the method replaces each variable with the argument in the corresponding position - that is, the method replaces the first { } occurrence with the first argument, and so on.

sdcA wrapper object that accesses the constants, methods, and objects available to the user script.The `sdc` object contains the following constants:

* `lastOffsets` - Dictionary that contains the last-saved offset for each entity. Use at the start of the script to read the last value associated with a successfully processed batch.Note: The constant does not update while the pipeline runs.
* `batchSize` - Number of records to create in a single batch. Configured on the Performance tab with the Batch Size property.
* `nThreads` - Number of threads to run simultaneously. Configured on the Performance tab with the Number of Threads property.
* `userParams` - Dictionary that contains the script parameters and values configured on the Advanced tab with the Parameters in Script property.

The `sdc` object contains the following methods:

* `createBatch()` - Returns a new batch.
* `createRecord(<String record ID>)` - Returns a new record with the passed ID. Pass a string that uniquely identifies the record and includes enough information to track the record source.
* `isStopped()` - Returns a Boolean value that indicates whether the pipeline has been stopped.
* `isPreview()` - Returns a Boolean value that indicates whether the pipeline is in preview mode.
* `getFieldNull(<record>, <String field path>)` - Returns one of the following:
  * The value of the field at the specified path if the value is not null
  * The null object defined for the field type, such as `NULL_INTEGER` or `NULL_STRING`, if the value is null
  * The unassigned null object `NULL` if there is no field at the specified path
* `createMap(<Boolean list-map>)` - Returns a map for use as a field in a record. Pass `true` to create a list-map field, or `false` to create a map field.
* `createEvent(<String type>, <Integer version>)` - Returns a new event record with the specified event type and version. Verify that the stage enables event generation before implementing event methods.

### Multithreaded Processing <a href="#ariaid-title3" id="ariaid-title3"></a>

The Groovy Scripting origin can use multiple concurrent threads to process data based on the Number of Threads property.

To enable multithreaded processing, write the script to create the configured number of threads. Each thread must create a batch and pass the batch to an available pipeline runner by calling the `batch.process(<String entity>, <String offset>)` method. A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin.

Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

For example, suppose you write the script to use multiple threads to read files in the order of last-modified timestamp, and you configure the origin to use five threads. When you start the pipeline, the origin creates five threads, and Data Pipelines creates a matching number of pipeline runners.

The origin assigns a thread to each of the five oldest files. Each thread processes its assigned file, creating batches of data and passing each batch to a pipeline runner.

After a thread completes processing a file, the origin assigns the thread to the next file based on the last-modified timestamp, until all files are processed.

For more information about multithreaded pipelines, see Multithreaded Pipeline Overview.

### Accessing Record Details <a href="#ariaid-title4" id="ariaid-title4"></a>

By default, you use native types from the scripting language to access records in scripts. However, with native types, you cannot easily access all the features of Data Pipelines records, such as field attributes.

To access records in scripts directly as Data Pipelines records, configure the stage to use the Java API to process records in scripts by setting the Record Type advanced property to `Data Collector Records`.

In the script, reference the needed classes from the Java package `com.streamsets.pipeline.api` and then use the appropriate methods to access records and fields. With the Java API, scripts can access all the features of Data Pipelines records.&#x20;

For example, include the following lines in the script to use the Java API to do the following:

* Create a String field named `new` and set its value to `new-value`.
* Update the existing field named `old` to set the value of the `attr` attribute to `attr-value`.

```
import com.streamsets.pipeline.api.Field
...
record.sdcRecord.set('/new', Field.create(Field.Type.STRING, 'new-value'))
record.sdcRecord.get('/old').setAttribute('attr', 'attr-value')
...
```

### Type Handling <a href="#ariaid-title5" id="ariaid-title5"></a>

Note the following type information when working with the Groovy Scripting origin:

Data type of null valuesYou can associate null values with a data type. For example, if the script assigns a null value to an Integer field, the field is returned to the pipeline as an integer with a null value.Use constants in the Groovy code to create a new field of a specific data type with a null value. For example, you can create a new String field with a null value by assigning the `NULL_STRING` type constant to the field as follows:

```
record.value['new_field'] = sdc.NULL_STRING
```

Date fieldsUse the String data type to create a new field to store a date with a specific format. For example, the following sample code creates a new String field that stores the current date using the format `YYYY-MM-dd`:

```
 // Define a date object to record the current date
def date = new Date()
def curBatch = sdc.createBatch()

for (record in records) {
 try {
   // Create a string field to store the current date with the specified format
   record.value['date'] = date.format('YYYY-MM-dd')
   // Add record to the current batch
   curBatch.add(record)
 } catch (Exception e) {
   // Send record to error
  curBatch.addError(record, e.toString())
 }
}

// Process the current batch
curBatch.process(entityName, offset.toString()))
```

### Event Generation <a href="#ariaid-title6" id="ariaid-title6"></a>

You can use the Groovy Scripting origin to generate event records for an event stream. Enable event generation when you want the stage to generate an event record based on scripting logic.

As with any record, you can pass event records downstream to a destination for event storage or to any executor that can be configured to use the event. For more information about events and the event framework, see Dataflow Triggers Overview.

To generate events:

1.  On the General tab, select the Produce Events property.

    This enables the event output stream for use.
2. Include both of the following methods in the script:
   *   `sdc.createEvent(<String type>, <Integer version>)` - Creates an event record with the specified event type and version number. You can create a new event type or use an existing event type. Existing event types are documented in other event-generating stages.

       The event record contains no record fields. Generate record fields as needed.
   * `batch.toEvent(<record>)` - Use to append an event record to a batch and pass events to the event output stream.

#### Event Record <a href="#ariaid-title7" id="ariaid-title7"></a>

Event records generated by the Groovy Scripting origin have the standard event-related record header attributes:

| Record Header Attribute       | Description                                               |
| ----------------------------- | --------------------------------------------------------- |
| sdc.event.type                | Event type, specified by the `sdc.createEvent` method.    |
| sdc.event.version             | Event version, specified by the `sdc.createEvent` method. |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.         |

### Record Header Attributes <a href="#ariaid-title8" id="ariaid-title8"></a>

The script in the Groovy Scripting origin can create custom record header attributes. Pipeline logic can use record header attributes to affect data flow. Therefore, you might create a custom record header attribute for a specific purpose. For more information, see [Record Header Attributes](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Pipeline\_Design/RecordHeaderAttributes.html#concept\_wn2\_jcz\_dz).

All records include a set of internal record header attributes that stages update automatically as they process records. Error records also have their own set of internal header attributes. You cannot change values of internal header attributes in scripts.

You can use the following record header variables to work with header attributes:

* `record.<header name>` - Use to return the value of a header attribute.
* `record.attributes` - Use to return a map of custom record header attributes, or to create or update a specific record header attribute.

Tip: Use data preview to view the record header attributes included in a record.

### Calling External Java Code <a href="#ariaid-title9" id="ariaid-title9"></a>

You can call external Java code from the Groovy Scripting origin. Simply install the external Java library to make it available to the origin. Then, call the external Java code from the Groovy script that you develop for the origin.

For information about installing additional drivers, see Install External Libraries.

To call external Java code from a Groovy script, simply add an import statement to your script, as follows:

```
import <package>.<class name>
```

For example, let's say that you installed the Bouncy Castle JAR file to compute SHA-3 (Secure Hash Algorithm 3) digests. Add the following statement to your script:

```
import org.bouncycastle.jcajce.provider.digest.SHA3
```

For more information, see the following StreamSets blog post: Calling External Java Code from Script Evaluators.

### Granting Permissions on Groovy Scripts <a href="#ariaid-title10" id="ariaid-title10"></a>

If Data Pipelines is using the Java Security Manager and the Groovy code needs to access network resources, you must update the Data Pipelines security policy to include Groovy scripts.

The Java Security Manager is enabled by default. For more information, see Security Manager.

1.  In the Data Pipelines configuration directory, edit the security policy:

    ```
    $SDC_CONF/sdc-security.policy
    ```
2.  Add the following lines to the file:

    ```
    // groovy source code
    grant codebase "file:///groovy/script" { 
      <permissions>;
    };
    ```

    Where `<permissions>` are the permissions you are granting to the Groovy scripts.

    For example, to grant read permission on all files in the `/data/files` directory and subdirectories, add the following lines:

    ```
    // groovy source code
    grant codebase "file:///groovy/script" { 
      permission java.util.PropertyPermission "*", "read";
      permission java.io.FilePermission "/data/files/*", "read";
    };
    ```
3. Restart Data Pipelines.

### Configuring a Groovy Scripting Origin <a href="#ariaid-title11" id="ariaid-title11"></a>

Configure a Groovy Scripting origin to run a Groovy script to create Data Pipelines records.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                |
    | Produce Events   | Generates event records when events occur. Use for event handling.                                                                                                                                                   |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul> |
2.  On the Performance tab, configure the following properties:

    | Performance Property | Description                                                                                                                                                                                                                                                                                                           |
    | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Batch Size           | <p>Number of records to generate in a single batch.</p><p>The script accesses this value with the <code>sdc.batchSize</code> constant and implements batch processing.</p><p>Default value is 1000. Data Pipelines honors values up to the Data Pipelines maximum batch size. The Data Pipelines default is 1000.</p> |
    | Number of Threads    | <p>Number of threads that generate data concurrently in parallel.</p><p>The script accesses this value with the <code>sdc.numThreads</code> constant and implements multithreaded processing.</p>                                                                                                                     |
3.  On the Script tab, configure the following property:

    | Script Property | Description                                                                                                                                                                  |
    | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | User Script     | Script that runs during pipeline execution.Tip: To toggle full-screen editing, press either F11 or Esc, depending on the operating system, when the cursor is in the editor. |
4.  On the Advanced tab, configure the following properties:

    | Advanced Property    | Description                                                                                                                                                                                                                                                                      |
    | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Record Type          | <p>Record type to use during script execution:</p><ul><li>Data Pipelines Records - Select when scripts use Java API methods to access records.</li><li>Native Objects - Select when scripts use native types to access records.</li></ul><p>Default value is Native Objects.</p> |
    | Parameters in Script | <p>Script parameters and their values.</p><p>The script accesses the values with the <code>sdc.userParams</code> dictionary.</p>                                                                                                                                                 |
