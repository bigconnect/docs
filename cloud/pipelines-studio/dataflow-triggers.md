# Dataflow Triggers

## Dataflow Triggers Overview <a href="#ariaid-title2" id="ariaid-title2"></a>

Dataflow triggers are instructions for the event framework to kick off tasks in response to events that occur in the pipeline. For example, you can use dataflow triggers to start a MapReduce job after the pipeline writes a file to HDFS. Or you might use a dataflow trigger to stop a pipeline after the JDBC Query Consumer origin processes all available data.

The event framework consist of the following components:

**event generation**

The event framework generates pipeline-related events and stage-related events. The framework generates pipeline events only when the pipeline starts and stops. The framework generates stage events when specific stage-related actions take place. The action that generates an event differs from stage to stage and is related to how the stage processes data.

For example, the Hive Metastore destination updates the Hive metastore, so it generates events each time it changes the metastore. In contrast, the Hadoop FS destination writes files to HDFS, so it generates events each time it closes a file.

Events produce event records. Pipeline-related event records are passed immediately to the specified event consumer. Stage-related event records are passed through the pipeline in an event stream.

**task execution**

To trigger a task, you need an executor. Executor stages perform tasks in Data Pipelines or external systems. Each time an executor receives an event, it performs the specified task.

For example, the Hive Query executor runs user-defined Hive or Impala queries each time it receives an event, and the MapReduce executor triggers a MapReduce job when it receives events. Within Data Pipelines, the Pipeline Finisher executor stops a pipeline upon receiving an event, transitioning the pipeline to a Finished state.

**event storage**

To store event information, pass the event to a destination. The destination writes the event records to the destination system, just like any other data.

For example, you might store event records to keep an audit trail of the files that the pipeline origin reads.

## Pipeline Event Generation <a href="#ariaid-title3" id="ariaid-title3"></a>

The event framework generates pipeline events in Data Collector standalone pipelines at specific points in the pipeline lifecycle. You can configure the pipeline properties to pass each event to an executor or to another pipeline for more complex processing.

The event framework generates the following pipeline-related events:

**Pipeline Start**

The pipeline start event is generated as the pipeline initializes, immediately after it starts and before individual stages are initialized. This can allow time for an executor to perform a task before stages initialize.Most executors wait for confirmation that a task completes. As a result, the pipeline waits for the executor to complete the task before continuing with stage initialization. For example, if you configure the JDBC Query executor to truncate a table before the pipeline begins, the pipeline waits until the task is complete before processing any data.

The MapReduce executor and Spark executor kick off jobs and do not wait for the submitted jobs to complete. When you use one of these executors, the pipeline waits only for successful job submission before continuing with stage initialization.

If the executor fails to process the event, for example if a Hive Query Executor fails to execute the specified query or if the query fails, then the initialization phase fails and the pipeline does not start. Instead the pipeline transitions to a failure state.

Pipeline StopThe pipeline stop event is generated as the pipeline stops, either manually, programmatically, or due to a failure. The stop event is generated after all stages have completed processing and cleaning up temporary resources, such as removing temporary files. This allows an executor to perform a task after pipeline processing is complete, before the pipeline fully stops.

Similar to start event consumers, the behavior of the executor that consumes the event determines whether the pipeline waits for the executor task to complete before allowing the pipeline to stop. Also, if the processing of the pipeline stop event fails for any reason, the pipeline transitions to a failed state even though the data processing was successful.

Pipeline events differ from stage events as follows:

*   **Virtual processing** - Unlike stage events, pipeline events are not processed by stages that you configure in the canvas. They are passed to an event consumer that you configure in the pipeline properties.

    The event consumer does not display in the pipeline’s canvas. As a result, pipeline events are also not visualized in data preview or pipeline monitoring.
*   **Single-use events** - You can configure only one event consumer for each event type within the pipeline properties: one for the Start event and one for the Stop event.

    When necessary, you can pass pipeline events to another pipeline. In the event consuming pipeline, you can include as many stages as you need for more complex processing.

For a solution that describes a couple ways to use pipeline events, see Offloading Data from Relational Sources to Hadoop.

### Using Pipeline Events <a href="#ariaid-title4" id="ariaid-title4"></a>

You can configure pipeline events in standalone pipelines. When you configure a pipeline event, you can configure it to be consumed by an executor or another pipeline.

Pass an event to an executor when the executor can perform all of the tasks that you need. You can configure one executor for each event type.

Pass an event to another pipeline when you need to perform more complex tasks in the consuming pipeline, such as passing the event to multiple executors or to an executor and destination for storage.

#### **Pass to an Executor**

You can configure a pipeline to pass each event type to an executor stage. This allows you to trigger a task when the pipeline starts or stops. You configure the behavior for each event type separately. And you can discard any event that you do not want to use.

Note: If the specified executor fails to process the event, for example if a Shell executor fails to execute a script, the pipeline transitions to a failure state.To pass a pipeline event to the executor, perform the following steps:

1. In the pipeline properties, select the executor that you want to consume the event.
2. In the pipeline properties, configure the executor to perform the task.

**Example**

Say you want to send an email when the pipeline starts. First, you configure the pipeline to use the Email executor for the pipeline start event. Since you don't need the Stop event, you can simply use the default discard option:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PEvent-Executor.png)

Then, also in the pipeline properties, you configure the Email executor. You can configure a condition for the email to be sent. If you omit the condition, the executor sends the email each time it receives an event:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PEvent-ConfigConsumer.png)

#### **Pass to Another Pipeline**

Pass pipeline events to another pipeline to perform more complex processing than simply passing the event to a single consumer. The event-consuming pipeline must use the DP RPC origin, then can include as many other stages as you require.

Note: When you pass a pipeline event to another pipeline, the event-consuming pipeline does not report processing failures back to the event-generating pipeline automatically. For example, if you pass a pipeline event to a pipeline that has an executor that fails to complete its task, the failure is not reported back to the event-generating pipeline.

To achieve the same behavior as passing to an executor, where a processing failure causes the event-generating pipeline to fail, configure the relevant stages to stop the event-consuming pipeline upon error. Upon error, the event-consuming pipeline then stops and passes the message back to the event-generating pipeline, which then transitions to a failure state.

For example, say you pass a pipeline event to a pipeline that routes the event to two executors. To ensure that the event-generating pipeline fails if either of the executors fail, configure the On Record Error property on the General tab of both executors, setting the property to Stop Pipeline.

This causes the event-consuming pipeline to stop on error, which causes the event-generating pipeline to transition to a failure state.

To pass an event to another pipeline, perform the following steps:

1. Configure the pipeline to consume the event.
2. Configure the event-generating pipeline to pass the event to the event-consuming pipeline, including details from the SDC RPC origin.
3. Start the event-consuming pipeline before you start the event-generating pipeline.

**Example**

Say you want the Stop event to trigger a shell script that kicks off another process and a JDBC query. To do this, first configure the event-consuming pipeline. Use an SDC RPC origin and note the highlighted properties, because you will use them to configure the event-generating pipeline:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PEvents-AnotherPipe.png)

Then you configure the event-generating pipeline to pass the Stop event to your new pipeline. Note that if you don't need to use the Start event, you can simply use the default discard option:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PEvent-StopEventConfig.png)

Then you configure the Stop Event - Write to Another Pipeline properties, using the SDC RPC details from your event-consuming pipeline:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PEvent-Stop-PipelineConfig.png)

## Stage Event Generation <a href="#ariaid-title7" id="ariaid-title7"></a>

You can configure certain stages to generate events. Event generation differs from stage to stage, based on the way the stage processes data. For details about each the event generation for each stage, see "Event Generation" in the stage documentation.

The following table lists event-generating stages and when they can generate events:

| Stage                                          | Generates events when the stage...                                                                                                                                                                                                                                                            |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Amazon S3 origin                               | <ul><li>Completes processing all available objects and the configured batch wait time has elapsed.</li></ul><p>For more information, see Event Generation for the Amazon S3 origin.</p>                                                                                                       |
| Azure Data Lake Storage Gen1 origin            | <ul><li>Starts processing a file.</li><li>Completes processing a file.</li><li>Completes processing all available files and the configured batch wait time has elapsed.</li></ul><p>For more information, see Event Generation for the Azure Data Lake Storage Gen1 origin.</p>               |
| Azure Data Lake Storage Gen2 origin            | <ul><li>Starts processing a file.</li><li>Completes processing a file.</li><li>Completes processing all available files and the configured batch wait time has elapsed.</li></ul><p>For more information, see Event Generation for the Azure Data Lake Storage Gen2 origin.</p>               |
| Directory origin                               | <ul><li>Starts processing a file.</li><li>Completes processing a file.</li><li>Completes processing all available files and the configured batch wait time has elapsed.</li></ul><p>For more information, see Event Generation for the Directory origin.</p>                                  |
| File Tail origin                               | <ul><li>Starts processing a file.</li><li>Completes processing a file.</li></ul><p>For more information, see Event Generation for the File Tail origin.</p>                                                                                                                                   |
| Google BigQuery origin                         | <ul><li>Successfully completes a query.</li></ul><p>For more information, see Event Generation for the Google BigQuery origin.</p>                                                                                                                                                            |
| Google Cloud Storage origin                    | <ul><li>Completes processing all available objects and the configured batch wait time has elapsed.</li></ul><p>For more information, see Event Generation for the Google Cloud Storage origin.</p>                                                                                            |
| Groovy Scripting origin                        | <ul><li>Runs a script that generates events.</li></ul><p>For more information, see Event Generation for the Groovy Scripting Origin.</p>                                                                                                                                                      |
| Hadoop FS Standalone origin                    | <ul><li>Starts processing a file.</li><li>Completes processing a file.</li><li>Completes processing all available files and the configured batch wait time has elapsed.</li></ul><p>For more information, see Event Generation for the Hadoop FS Standalone origin.</p>                       |
| JavaScript Scripting origin                    | <ul><li>Runs a script that generates events.</li></ul><p>For more information, see Event Generation for the JavaScript Scripting Origin.</p>                                                                                                                                                  |
| JDBC Multitable Consumer origin                | <ul><li>Completes processing the data returned by the queries for all tables.</li></ul><p>For more information, see Event Generation for the JDBC Multitable Consumer origin.</p>                                                                                                             |
| JDBC Query Consumer origin                     | <ul><li>Completes processing all data returned by a query.</li><li>Successfully completes a query.</li><li>Fails to complete a query.</li></ul><p>For more information, see Event Generation for the JDBC Query Consumer origin.</p>                                                          |
| Jython Scripting origin                        | <ul><li>Runs a script that generates events.</li></ul><p>For more information, see Event Generation for the Jython Scripting Origin.</p>                                                                                                                                                      |
| MongoDB origin                                 | <ul><li>Completes processing all data returned by a query.</li></ul><p>For more information, see Event Generation for the MongoDB origin.</p>                                                                                                                                                 |
| Oracle Bulkload origin                         | <ul><li>Completes processing data in a table.</li></ul><p>For more information, see Event Generation for the Oracle Bulkload origin.</p>                                                                                                                                                      |
| Oracle CDC Client origin                       | <ul><li>Reads DDL statements in the redo log.</li></ul><p>For more information, see Event Generation for the Oracle CDC Client origin.</p>                                                                                                                                                    |
| Salesforce origin                              | <ul><li>Completes processing all data returned by a query.</li></ul><p>For more information, see Event Generation for the Salesforce origin.</p>                                                                                                                                              |
| SAP HANA Query Consumer origin                 | <ul><li>Completes processing all data returned by a query.</li><li>Successfully completes a query.</li><li>Fails to complete a query.</li></ul><p>For more information, see Event Generation for the SAP HANA Query Consumer origin.</p>                                                      |
| SFTP/FTP/FTPS Client origin                    | <ul><li>Starts processing a file.</li><li>Completes processing a file.</li><li>Completes processing all available files and the configured batch wait time has elapsed.</li></ul><p>For more information, see Event Generation for the SFTP/FTP/FTPS Client origin.</p>                       |
| SQL Server 2019 BDC Multitable Consumer origin | <ul><li>Completes processing the data returned by the queries for all tables.</li></ul><p>For more information, see Event Generation for the SQL Server 2019 BDC Multitable Consumer origin.</p>                                                                                              |
| SQL Server CDC Client origin                   | <ul><li>Completes processing the data in the associated CDC tables.</li><li>When you enable checking for schema changes, the origin generates an event each time it detects a schema change.</li></ul><p>For more information, see Event Generation for the SQL Server CDC Client origin.</p> |
| SQL Server Change Tracking origin              | <ul><li>Completes processing the data in all specified change tracking tables.</li></ul><p>For more information, see Event Generation for the SQL Server Change Tracking origin.</p>                                                                                                          |
| Teradata Consumer origin                       | <ul><li>Completes processing the data returned by the queries for all tables.</li></ul><p>For more information, see Event Generation for the Teradata Consumer origin.</p>                                                                                                                    |
| Windowing Aggregator processor                 | <ul><li>Performs aggregations, based on the configured window type and time window.</li></ul><p>For more information, see Event Generation for the Windowing Aggregator processor.</p>                                                                                                        |
| Groovy Evaluator processor                     | <ul><li>Runs a script that generates events.</li></ul><p>For more information, see Event Generation for the Groovy Evaluator processor.</p>                                                                                                                                                   |
| JavaScript Evaluator processor                 | <ul><li>Runs a script that generates events.</li></ul><p>For more information, see Event Generation for the JavaScript Evaluator processor.</p>                                                                                                                                               |
| Jython Evaluator processor                     | <ul><li>Runs a script that generates events.</li></ul><p>For more information, see Event Generation for the Jython Evaluator processor.</p>                                                                                                                                                   |
| TensorFlow Evaluator processor                 | <ul><li>Evaluates the entire batch at once.</li></ul><p>For more information, see Event Generation for the TensorFlow Evaluator processor.</p>                                                                                                                                                |
| Windowing Aggregator processor                 | <ul><li>Performs aggregations.</li></ul><p>For more information, see Event Generation for the Windowing Aggregator processor.</p>                                                                                                                                                             |
| Amazon S3 destination                          | <ul><li>Completes writing to an object.</li><li>Completes streaming a whole file.</li></ul><p>For more information, see Event Generation for the Amazon S3 destination.</p>                                                                                                                   |
| Azure Data Lake Storage Gen1 destination       | <ul><li>Closes a file.</li><li>Completes streaming a whole file.</li></ul><p>For more information, see Event Generation for the Azure Data Lake Storage Gen1 destination.</p>                                                                                                                 |
| Azure Data Lake Storage Gen2 destination       | <ul><li>Closes a file.</li><li>Completes streaming a whole file.</li></ul><p>For more information, see Event Generation for the Azure Data Lake Storage Gen2 destination.</p>                                                                                                                 |
| Google Cloud Storage destination               | <ul><li>Completes writing to an object.</li><li>Completes streaming a whole file.</li></ul><p>For more information, see Event Generation for the Google Cloud Storage destination.</p>                                                                                                        |
| Hadoop FS destination                          | <ul><li>Closes a file.</li><li>Completes streaming a whole file.</li></ul><p>For more information, see Event Generation for the Hadoop FS destination.</p>                                                                                                                                    |
| Hive Metastore destination                     | <ul><li>Updates the Hive metastore by creating a table, adding columns, or creating a partition.</li><li>Generates and writes a new Avro schema file.</li></ul><p>For more information, see Event Generation for the Hive Metastore destination.</p>                                          |
| Local FS destination                           | <ul><li>Closes a file.</li><li>Completes streaming a whole file.</li></ul><p>For more information, see Event Generation for the Local FS destination.</p>                                                                                                                                     |
| MapR FS destination                            | <ul><li>Closes a file.</li><li>Completes streaming a whole file.</li></ul><p>For more information, see Event Generation for the MapR FS destination.</p>                                                                                                                                      |
| SFTP/FTP/FTPS Client destination               | <ul><li>Closes a file.</li><li>Completes streaming a whole file.</li></ul><p>For more information, see Event Generation for the SFTP/FTP/FTPS Client destination.</p>                                                                                                                         |
| ADLS Gen1 File Metadata executor               | <ul><li>Changes file metadata, such as the file name, location, or permissions.</li><li>Creates an empty file.</li><li>Removes a file or directory.</li></ul><p>For more information, see Event Generation for the ADLS Gen1 File Metadata executor.</p>                                      |
| ADLS Gen2 File Metadata executor               | <ul><li>Changes file metadata, such as the file name, location, or permissions.</li><li>Creates an empty file.</li><li>Removes a file or directory.</li></ul><p>For more information, see Event Generation for the ADLS Gen2 File Metadata executor.</p>                                      |
| Amazon S3 executor                             | <ul><li>Creates a new Amazon S3 object.</li><li>Copies an object another location.</li><li>Adds tags to an existing object.</li></ul><p>For more information, see Event Generation for the Amazon S3 executor.</p>                                                                            |
| Databricks Job Launcher executor               | <ul><li>Starts a Databricks job.</li></ul><p>For more information, see Event Generation for the Databricks Job Launcher executor.</p>                                                                                                                                                         |
| Databricks Query executor                      | <ul><li>Determines that the submitted query completed successfully.</li><li>Determines that the submitted query failed to complete.</li></ul><p>For more information, see Event Generation for the Databricks Query executor.</p>                                                             |
| HDFS File Metadata executor                    | <ul><li>Changes file metadata, such as the file name, location, or permissions.</li><li>Creates an empty file.</li><li>Removes a file or directory.</li></ul><p>For more information, see Event Generation for the HDFS File Metadata executor.</p>                                           |
| Hive Query executor                            | <ul><li>Determines that the submitted query completed successfully.</li><li>Determines that the submitted query failed to complete.</li></ul><p>For more information, see Event Generation for the Hive Query executor.</p>                                                                   |
| JDBC Query executor                            | <ul><li>Determines that the submitted query completed successfully.</li><li>Determines that the submitted query failed to complete.</li></ul><p>For more information, see Event Generation for the JDBC Query executor.</p>                                                                   |
| MapReduce executor                             | <ul><li>Starts a MapReduce job.</li></ul><p>For more information, see Event Generation for the MapReduce executor.</p>                                                                                                                                                                        |
| Spark executor                                 | <ul><li>Starts a Spark application.</li></ul><p>For more information, see Event Generation for the Spark executor.</p>                                                                                                                                                                        |

### Using Stage Events <a href="#ariaid-title8" id="ariaid-title8"></a>

You can use stage-related events in any way that suits your needs. When configuring the event stream for stage events, you can add additional stages to the stream. For example, you might use a Stream Selector to route different types of events to different executors. But you cannot merge the event stream with a data stream.

There are two general types of event streams that you might create:

* Task execution streams that route events to an executor to perform a task.
* Event storage streams that route events to a destination to store event information.

You can, of course, configure an event stream that performs both tasks by routing event records to both an executor and a destination. You can also configure event streams to route data to multiple executors and destinations, as needed.

**Task Execution Streams**

A task execution stream routes event records from the event-generating stage to an executor stage. The executor performs a task each time it receives an event record.

For example, you have a pipeline that reads from Kafka and writes files to HDFS:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-ParquetBasicPipe.png)

When Hadoop FS closes a file, you would like the file moved to a different directory and the file permissions changed to read-only.

Leaving the rest of the pipeline as is, you can enable event handling in the Hadoop FS destination, connect it to the HDFS File Metadata executor, and configure the HDFS File Metadata executor to files and change permissions. The resulting pipeline looks like this:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-EventPipe.png)

If you needed to set permissions differently based on the file name or location, you could use a Stream Selector to route the event records accordingly, then use two HDFS File Metadata executors to alter file permissions, as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-EventPipe-SSelector.png)

**Event Storage Streams**

An event storage stream routes event records from the event-generating stage to a destination. The destination writes the event record to a destination system.

Event records include information about the event in record header attributes and record fields. You can add processors to the event stream to enrich the event record before writing it to the destination.

For example, you have a pipeline that uses the Directory origin to process weblogs:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Directory.png)

Directory generates event records each time it starts and completes reading a file, and the event record includes a field with the file path of the file. For auditing purposes, you'd like to write this information to a database table.

Leaving the rest of the pipeline as is, you can enable event handling for the Directory origin and simply connect it to the JDBC Producer as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Directory-JDBC.png)

But you want to know when events occur. The Directory event record stores the event creation time in the sdc.event.creation\_timestamp record header attribute. So you can use an Expression Evaluator with the following expression to add the creation date and time to the record:

```
${record:attribute('sdc.event.creation_timestamp')}
```

And if you have multiple pipelines writing events to the same location, you can use the following expression to include the pipeline name in the event record as well:

```
${pipeline:name()}
```

The Expression Evaluator and the final pipeline looks like this:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Event-Directory-ExpJDBC.png)

## Executors <a href="#ariaid-title11" id="ariaid-title11"></a>

Executors perform tasks when they receive event records.

You can use the following executor stages for event handling:

**ADLS Gen1 File Metadata executor**&#x20;

Changes file metadata, creates an empty file, or removes a file or directory in Azure Data Lake Storage Gen1 upon receiving an event. When changing file metadata, the executor can rename and move files in addition to specifying the owner and group, and updating permissions and ACLs for files. When creating an empty file, the executor can specify the owner and group, and set permissions and ACLs for the file. When removing files and directories, the executor performs the task recursively. You can use the executor in any logical way, such as changing permissions after an Azure Data Lake Storage Gen1 destination closes a file.

**ADLS Gen2 File Metadata executor**&#x20;

Changes file metadata, creates an empty file, or removes a file or directory in Azure Data Lake Storage Gen2 upon receiving an event. When changing file metadata, the executor can rename and move files in addition to specifying the owner and group, and updating permissions and ACLs for files. When creating an empty file, the executor can specify the owner and group, and set permissions and ACLs for the file. When removing files and directories, the executor performs the task recursively. You can use the executor in any logical way, such as moving a file after an Azure Data Lake Storage Gen2 destination closes it.

**Amazon S3 executor**&#x20;

Creates new Amazon S3 objects for the specified content, copies objects within a bucket, or adds tags to existing Amazon S3 objects upon receiving an event. You can use the executor in any logical way, such as writing information from an event record to a new S3 object, or copying or tagging objects after they are written by the Amazon S3 destination.

**Databricks Job Launcher executor**&#x20;

Starts a Databricks job for each event. You can use the executor in any logical way, such as running Databricks jobs after the Hadoop FS, MapR FS, or Amazon S3 destination closes files.&#x20;

**Databricks Query executor**&#x20;

Runs a Spark SQL query on Databricks upon receiving an event. You can use the executor in any logical way.

**Email executor**

Sends a custom email to the configured recipients upon receiving an event. You can optionally configure a condition that determines when to send the email.You can use the executor in any logical way, such as sending an email each time the Azure Data Lake Storage destination completes streaming a whole file.

**Hive Query executor**

Executes user-defined Hive or Impala queries for each event.You can use the executor in any logical way, such as running Hive or Impala queries after the Hive Metadata destination updates the Hive metastore, or after the Hadoop FS destination closes files.

For example, you might use the Hive Query executor as part of the Drift Synchronization Solution for Hive if you read data with Impala. Impala requires you to run the Invalidate Metadata command when the table structure or data changes.Instead of trying to time this action manually, you can use the Hive Query executor to submit the command automatically each time the Hive Metastore destination changes the structure of a table and each time the Hadoop FS destination closes a file.

**HDFS File Metadata executor**&#x20;

Changes file metadata, creates an empty file, or removes a file or directory in HDFS or a local file system upon receiving an event. When changing file metadata, the executor can rename and move files in addition to specifying the owner and group, and updating permissions and ACLs for files. When creating an empty file, the executor can specify the owner and group, and set permissions and ACLs for the file. When removing files and directories, the executor performs the task recursively. You can use the executor in any logical way, such as changing file metadata after receiving file closure events from the Hadoop FS or Local FS destinations.

**Pipeline Finisher executor**&#x20;

Stops the pipeline when it receives an event, transitioning the pipeline to a Finished state. Allows the pipeline to complete all expected processing before stopping. You can use the Pipeline Finisher executor in any logical way, such as stopping a pipeline upon receiving a no-more-data event from the JDBC Query Consumer origin. This enables you to achieve "batch" processing - stopping the pipeline when all available data is processed rather than leaving the pipeline to sit idle indefinitely. For example, you might use the Pipeline Finisher executor with the JDBC Multitable Consumer to stop the pipeline when it processes all queried data in the specified tables.

**JDBC Query executor**

Connects to a database using JDBC and runs one or more specified SQL queries. Use to run SQL queries on a database after an event occurs.

**MapReduce executor**

Connects to HDFS or MapR FS and starts a MapReduce job for each event. You can use the executor in any logical way, such as running MapReduce jobs after the Hadoop FS destination closes files. For example, you might use the MapReduce executor with the Hadoop FS destination to convert Avro files to Parquet when Hadoop FS closes a file.

**SFTP/FTP/FTPS Client executor**

Connects to an SFTP, FTP, or FTPS server and moves or removes a file upon receiving an event. You can use the executor in any logical way, such as moving a file after the SFTP/FTP/FTPS Client origin finishes reading it.

**Shell executor**

Executes a user-defined shell script for each event. You can use the executor in any logical way.

**Spark executor**

Connects to Spark on YARN and starts a Spark application for each event. You can use the executor in any logical way, such as running Spark applications after the Hadoop FS, MapR FS, or Amazon S3 destination closes files. For example, you might use the Spark executor with the Hadoop FS destination to convert Avro files to Parquet when Hadoop FS closes a file.
