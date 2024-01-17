# Static Lookup

The Static Lookup processor performs lookups of key-value pairs that are stored in local memory and passes the lookup values to fields. Use the Static Lookup to store String values in memory that the pipeline can look up at runtime to enrich records with additional data.

When you configure the Static Lookup processor, you specify whether the processor performs a bulk lookup of all keys in a batch, or performs an individual lookup of each key in a record. You use an expression to define the key to look up in local memory, and specify the String output field to write the lookup values to. You also define the key-value pairs to store in local memory.

### Example

Let's assume that you need to develop a pipeline that reads log files created by multiple hosts. Each log file includes the name of the host. You want to process and write the log data to different local directories, based on the host that created the log file.

On the Lookup tab for the Static Lookup processor, you enter an expression that defines the hostname field as the key to look up. You specify a new directory field to write the lookup values to, as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/StaticLookupParameters.png)

On the Static Store tab for the Static Lookup processor, you define each host name and associated directory as the key-value pairs to store in local memory:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/StaticLookupStore.png)

In the Local FS destination, you include the value of the directory field in the expression for the directory template as follows:

```
${record:value('/directory')}/tmp/out/${YYYY()}-${MM()}-${DD()}-${hh()}
```

When you run the pipeline, the Static Lookup processor stores the static key-value pairs in memory. The processor looks up the value of the hostname field and passes the corresponding value to the directory output field. When the Local FS destination writes the data to output files, the destination writes the files to the appropriate directory for each host.

### Configuring a Static Lookup Processor <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a Static Lookup processor to perform key-value lookups in memory.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Lookup tab, configure the following properties:

    | Lookup Property | Description                                                                                                                                                                                                                                                                                                                                                                                          |
    | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Mode            | <p>Mode used to perform the lookups:</p><ul><li>Per Batch - Performs a bulk lookup of all keys in a batch. The processor performs a single lookup for each batch.</li><li>Per Key in Each Record - Performs individual lookups of each key in each record. If you configure multiple key expressions, the processor performs multiple lookups for each record.</li></ul><p>Default is Per Batch.</p> |
    | Key Expression  | <p>Key to look up in local memory. Enter a key name or enter an expression that defines the key.For example, enter the following expression to use the data in the hostname field as the lookup key:</p><pre><code>${record:value('/hostname')}
    </code></pre>                                                                                                                                        |
    | Output Field    | Name of the String field in the record to pass the lookup value. You can specify an existing field or a new field. If the field does not exist, Static Lookup creates the field.                                                                                                                                                                                                                     |
3. On the Static Store tab, configure the key-value pairs to store in local memory. The processor stores each value as a String. Click Add to create additional key-value pairs to store.
