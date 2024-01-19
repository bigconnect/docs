# Hadoop Impersonation Mode

You can configure how BigConnect Pipelines impersonates a Hadoop user when performing tasks, such as reading or writing data, in Hadoop systems.

By default, BigConnect Pipelines impersonates Hadoop users as follows:

* As the user defined in stage properties - When configured, BigConnect Pipelines uses the user defined in Hadoop-related stages.
* As the currently logged-in BigConnect Pipelines user who starts the pipeline - When no user is defined in a Hadoop-related stage, BigConnect Pipelines uses the user who starts the pipeline.

**Note**: In both cases, the Hadoop systems must be configured to allow the impersonation.

The system administrator can configure BigConnect Pipelines to always use the user who starts the pipeline by enabling the `stage.conf_hadoop.always.impersonate.current.user` property in the BigConnect Pipelines configuration file. When enabled, configuring a user within a stage is not allowed.

Configure BigConnect Pipelines to always impersonate the user who starts the pipeline when you want to prevent access to data in Hadoop systems by stage-level user properties.

For example, say you use roles, groups, and pipeline permissions to ensure that only authorized operators can start pipelines. You expect that the operator user accounts are used to access all external systems. But a pipeline developer can specify a HDFS user in a Hadoop stage and bypass your attempts at security. To close this loophole, configure BigConnect Pipelines to always use the currently logged-in BigConnect Pipelines user to read from or write to Hadoop systems.

To always use the user who starts the pipeline, in the BigConnect Pipelines configuration file, uncomment the `stage.conf_hadoop.always.impersonate.current.user` property and set it to true.

With this property enabled, BigConnect Pipelines prevents configuring an alternate user in the following Hadoop-related stages:

* Hadoop FS origin, Hadoop FS Standalone origin, and Hadoop FS destination
* MapR FS origin, MapR FS Standalone origin, and MapR FS destination
* HBase lookup and destination
* MapR DB destination
* HDFS File Metadata executor
* MapR FS File Metadata executor
* MapReduce executor
