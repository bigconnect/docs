# Executors

An executor stage triggers a task when it receives an event.

Use executors as part of a dataflow trigger in an event stream to perform event-driven, pipeline-related tasks, such as moving a fully-written file when a destination closes it.

For more information about the event framework, see Dataflow Triggers Overview. You can use the following executors in a pipeline:

* [Amazon S3](amazon-s3.md) - Creates new BigConnect Object Store and Amazon S3 objects for the specified content, copies objects within a bucket, or adds tags to existing Amazon S3 objects.
* [ADLS Gen1 File Metadata](adls-gen1-file-metadata.md) - Changes file metadata, creates an empty file, or removes a file or directory in Azure Data Lake Storage Gen1 upon receiving an event.
* [ADLS Gen2 File Metadata](adls-gen2-file-metadata.md) - Changes file metadata, creates an empty file, or removes a file or directory in Azure Data Lake Storage Gen2 upon receiving an event.
* [Databricks Job Launcher](databricks-job-launcher.md) - Starts the specified Databricks job upon receiving an event record.
* [Email](email.md) - Sends custom email to the configured recipients upon receiving an event.
* [HDFS File Metadata](hdfs-file-metadata.md) - Changes file metadata, creates an empty file, or removes a file or directory in HDFS or a local file system upon receiving an event.
* [Hive Query](hive-query.md) - Runs user-defined Hive or Impala queries upon receiving an event record.
* [JDBC Query](jdbc-query.md) - Runs a user-defined SQL query upon receiving an event record.
* [MapReduce](mapreduce.md) - Starts the specified MapReduce job upon receiving an event record.
* [Pipeline Finisher](pipeline-finisher.md) - Stops and transitions the pipeline to a Finished state upon receiving an event record.
* [Shell](shell.md) - Executes a shell script upon receiving an event record.
* [Spark](spark.md) - Starts the specified Spark application upon receiving an event record.
