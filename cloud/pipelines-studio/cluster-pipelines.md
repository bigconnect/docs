# Cluster Pipelines

A cluster pipeline is a pipeline that runs in cluster execution mode. You can run a pipeline in standalone execution mode or cluster execution mode.

In standalone mode, a single Data Pipelines instance process runs the pipeline. A pipeline runs in standalone mode by default.

In cluster mode, the Data Pipelines uses a cluster manager and a cluster application to spawn additional workers as needed. Use cluster mode to read data from a Kafka cluster, MapR cluster, HDFS, or Amazon S3.

When would you choose standalone or cluster mode? Say you want to ingest logs from application servers and perform a computationally expensive transformation. To do this, you might use a set of standalone pipelines to stream log data from each application server to a Kafka or Hadoop cluster. And then use a cluster pipeline to process the data from the cluster and perform the expensive transformation.

Or, you might use cluster mode to move data from HDFS to another destination, such as Elasticsearch.

## Cluster Batch and Streaming Execution Mode

Data Pipelines can run a cluster pipeline using cluster batch or cluster streaming execution mode.

The execution mode that Data Pipelines can use depends on the origin system that the cluster pipeline reads from:

### Kafka cluster

Data Pipelines can process data from a Kafka cluster in cluster streaming mode. In cluster streaming mode, Data Pipelines processes data continuously until you stop the pipeline.

Data Pipelines runs as an application within Spark Streaming, an open source cluster-computing application.

Spark Streaming runs on either the Mesos or YARN cluster manager to process data from a Kafka cluster. The cluster manager and Spark Streaming spawn a Data Pipelines worker for each topic partition in the Kafka cluster. As a result, each partition has a Data Pipelines worker to process data. If you add a partition to the Kafka topic, you must restart the pipeline to enable the Data Pipelines to generate a new worker to read from the new partition.

When Spark Streaming runs on YARN, you can limit the number of workers spawned by configuring the Worker Count cluster pipeline property. You can also use the Extra Spark Configuration property to pass Spark configurations to the spark-submit script. In addition, you can configure the Kafka Consumer origin in a cluster streaming pipeline on YARN to connect securely through SSL/TLS, Kerberos, or both.

Use the Kafka Consumer origin to process data from a Kafka cluster in cluster streaming mode.

### HDFS

Data Pipelines can process data from HDFS in cluster batch mode. In cluster batch mode, Data Pipelines processes all available data and then stops the pipeline.

Data Pipelines runs as an application on top of MapReduce, an open-source cluster-computing framework. MapReduce runs on a YARN cluster manager. YARN and MapReduce generate additional worker nodes as needed. MapReduce creates one map task for each HDFS block.

Use the Hadoop FS origin to process data from HDFS in cluster batch mode.

### Amazon S3

Data Pipelines can process data from Amazon S2. In cluster batch mode, Data Pipelines runs on Hadoop Cluster to process Object Store data.

In either mode, Data Pipelines processes all available data and then stops the pipeline.

Data Pipelines runs as an application on top of MapReduce in the Hadoop cluster. MapReduce runs on a YARN cluster manager. MapReduce creates one map task for each HDFS block.

Use the Hadoop FS origin to process data from Amazon S3 in cluster EMR or cluster batch mode.

### Data Pipelines Configuration <a href="#ariaid-title3" id="ariaid-title3"></a>

When running cluster pipelines, the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`, defined on the gateway node is propagated to the worker nodes with the exception of the following properties:

* `sdc.base.http.url`
* `http.bindHost`

If you modify the `sdc.base.http.url` and `http.bindHost` properties on the gateway node to configure a specific host name or port number or to configure a specific IP address that Data Pipelines binds to, the modified values are not propagated to the worker nodes. The worker nodes always use the default values for the `sdc.base.http.url` and `http.bindHost` properties so that the worker nodes can dynamically determine the host name and can bind to any IP address.

To prevent additional configuration properties from being propagated to the worker nodes, add the following property to the `sdc.properties` file on the gateway node:

```
cluster.slave.configs.remove=<property1>,<property2>
```

### Enable HTTPS

You can enable Data Collector to use HTTPS when you run cluster pipelines. By default Data Pipelines uses HTTP.

To configure HTTPS for cluster pipelines, you first must configure Data Pipelines to use HTTPS. Then you generate an SSL/TLS certificate for each worker node in the cluster. Data Pipelines runs on the master gateway node in the cluster, so the gateway node uses the same keystore file configured for Data Pipelines.

You then specify the generated keystore file and keystore password file for the worker nodes in the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`. You can optionally generate a truststore file for the gateway and worker nodes.

## Cluster Pipeline Limitations <a href="#ariaid-title1" id="ariaid-title1"></a>

Please note the following limitations in cluster pipelines:

* Non-cluster origins - Do not use non-cluster origins in cluster pipelines. For a description of the origins to use, see Cluster Batch and Streaming Execution Modes.
* Hive Metastore destination - This destination is not supported in cluster pipelines.
* Kafka stages - Kafka stages do not support using the Provide Keytab and related properties to specify credentials for Kerberos authentication. Use JAAS files to provide Kerberos credentials.
* MapReduce executor - This executor is not supported in cluster pipelines.
* Pipeline events - You cannot use pipeline events in cluster pipelines.
* Record Deduplicator processor - This processor is not supported in cluster pipelines at this time.
* RabbitMQ Producer destination - This destination is not supported in cluster pipelines at this time.
* Scripting processors - The state object is available only for the instance of the processor stage it is defined in. If the pipeline executes in cluster mode, the state object is not shared across nodes.
* Spark executor - This executor is not supported in cluster pipelines.
*   Spark Evaluator processor - Use in cluster streaming pipelines only. Do not use in cluster batch pipelines. You can also use the Spark Evaluator in standalone pipelines.

    When using the Spark Evaluator processor, the processor must use the same Spark version as the cluster. For example, if the cluster uses Spark 2.1, the Spark Evaluator must use a Spark 2.1 stage library.

    The processor is available in several CDH stage libraries. To verify the Spark version that a stage library includes, see the CDH documentation.
