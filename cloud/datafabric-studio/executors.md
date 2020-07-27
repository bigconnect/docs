# Executors

An executor stage triggers a task when it receives an event.

Use executors as part of a dataflow trigger in an event stream to perform event-driven, pipeline-related tasks, such as moving a fully-written file when a destination closes it.

For more information about the event framework, see [Dataflow Triggers](dataflow-triggers.md) Overview.You can use the following executors in a pipeline:

* [Amazon S3](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/AmazonS3.html#concept_mvh_bnm_f1b) - Creates new Amazon S3 objects for the specified content, copies objects within a bucket, or adds tags to existing Amazon S3 objects.
* [ADLS Gen1 File Metadata](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/ADLS-G1-FileMeta.html#concept_zhp_ldk_xhb) - Changes file metadata, creates an empty file, or removes a file or directory in Azure Data Lake Storage Gen1 upon receiving an event.
* [ADLS Gen2 File Metadata](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/ADLS-G2-FileMeta.html#concept_i22_k2k_xhb) - Changes file metadata, creates an empty file, or removes a file or directory in Azure Data Lake Storage Gen2 upon receiving an event.
* [Databricks](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/Databricks.html#concept_fdc_qrx_jz) - Starts the specified Databricks job upon receiving an event record.
* [Email](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/Email.html#concept_sjs_sfp_qz) - Sends custom email to the configured recipients upon receiving an event.
* [HDFS File Metadata](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/HDFSMetadata.html#concept_wgj_slk_fx) - Changes file metadata, creates an empty file, or removes a file or directory in HDFS or a local file system upon receiving an event.
* [Hive Query](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/HiveQuery.html#concept_kjw_llk_fx) - Runs user-defined Hive or Impala queries upon receiving an event record.
* [JDBC Query](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/JDBCQuery.html#concept_j3r_gcv_sx) - Runs a user-defined SQL query upon receiving an event record.
* [MapR FS File Metadata](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/MapRFSFileMeta.html#concept_ohx_r5h_z1b) - Changes file metadata, creates an empty file, or removes a file or directory in MapR FS upon receiving an event.
* [MapReduce](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/MapReduce.html#concept_bj2_zlk_fx) - Starts the specified MapReduce job upon receiving an event record.
* [Pipeline Finisher](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/PipelineFinisher.html#concept_qzm_l4r_kz) - Stops and transitions the pipeline to a Finished state upon receiving an event record.
* [Shell](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/Shell.html#concept_jsr_zpw_tz) - Executes a shell script upon receiving an event record.
* [Spark](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Executors/Spark.html#concept_cvy_vxb_1z) - Starts the specified Spark application upon receiving an event record.

