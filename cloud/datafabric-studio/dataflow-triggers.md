# Dataflow Triggers

Dataflow triggers are instructions for the event framework to kick off tasks in response to events that occur in the pipeline. For example, you can use dataflow triggers to start a MapReduce job after the pipeline writes a file to HDFS. Or you might use a dataflow trigger to stop a pipeline after the JDBC Query Consumer origin processes all available data.

The event framework consist of the following components:

**event generation**

The event framework generates pipeline-related events and stage-related events. The framework generates pipeline events only when the pipeline starts and stops. The framework generates stage events when specific stage-related actions take place. The action that generates an event differs from stage to stage and is related to how the stage processes data.

For example, the Hive Metastore destination updates the Hive metastore, so it generates events each time it changes the metastore. In contrast, the Hadoop FS destination writes files to HDFS, so it generates events each time it closes a file.

Events produce event records. Pipeline-related event records are passed immediately to the specified event consumer. Stage-related event records are passed through the pipeline in an event stream.

**task execution**

To trigger a task, you need an executor. Executor stages perform tasks in DataFabric or external systems. Each time an executor receives an event, it performs the specified task.

For example, the Hive Query executor runs user-defined Hive or Impala queries each time it receives an event, and the MapReduce executor triggers a MapReduce job when it receives events. Within DataFabric, the Pipeline Finisher executor stops a pipeline upon receiving an event, transitioning the pipeline to a Finished state.

**event storage**

To store event information, pass the event to a destination. The destination writes the event records to the destination system, just like any other data.

For example, you might store event records to keep an audit trail of the files that the pipeline origin reads.

