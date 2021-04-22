# Cluster Pipelines

A cluster pipeline is a pipeline that runs in cluster execution mode. You can run a pipeline in standalone execution mode or cluster execution mode.

In standalone mode, a single Data Pipelines instance process runs the pipeline. A pipeline runs in standalone mode by default.

In cluster mode, the Data Collector uses a cluster manager and a cluster application to spawn additional workers as needed. Use cluster mode to read data from a Kafka cluster, MapR cluster, HDFS, or Amazon S3.

When would you choose standalone or cluster mode? Say you want to ingest logs from application servers and perform a computationally expensive transformation. To do this, you might use a set of standalone pipelines to stream log data from each application server to a Kafka or MapR cluster. And then use a cluster pipeline to process the data from the cluster and perform the expensive transformation.

Or, you might use cluster mode to move data from HDFS to another destination, such as Elasticsearch.

## Cluster Batch and Streaming Execution Mode

Data Pipelines can run a cluster pipeline using cluster batch or cluster streaming execution mode.

The execution mode that Data Pipelines can use depends on the origin system that the cluster pipeline reads from:

### Kafka cluster

Data Pipelines can process data from a Kafka cluster in cluster streaming mode. In cluster streaming mode, Data Pipelines processes data continuously until you stop the pipeline.

Data Pipelines runs as an application within Spark Streaming, an open source cluster-computing application.

Spark Streaming runs on either the Mesos or YARN cluster manager to process data from a Kafka cluster. The cluster manager and Spark Streaming spawn a Data Collector worker for each topic partition in the Kafka cluster. As a result, each partition has a Data Collector worker to process data. If you add a partition to the Kafka topic, you must restart the pipeline to enable the Data Collector to generate a new worker to read from the new partition.

When Spark Streaming runs on YARN, you can limit the number of workers spawned by configuring the Worker Count cluster pipeline property. You can also use the Extra Spark Configuration property to pass Spark configurations to the spark-submit script. In addition, you can configure the Kafka Consumer origin in a cluster streaming pipeline on YARN to connect securely through SSL/TLS, Kerberos, or both.

Use the Kafka Consumer origin to process data from a Kafka cluster in cluster streaming mode.

### BigConnect Cloud HDFS

Data Pipelines can process data from BigConnect Cloud HDFS in cluster batch mode. In cluster batch mode, Data Pipelines processes all available data and then stops the pipeline.

Data Pipelines runs as an application on top of MapReduce, an open-source cluster-computing framework. MapReduce runs on a YARN cluster manager. YARN and MapReduce generate additional worker nodes as needed. MapReduce creates one map task for each HDFS block.

Use the Hadoop FS origin to process data from HDFS in cluster batch mode.

### BigConnect Object Store / Amazon S3

Data Pipelines can process data from BigConnect Object Store. In cluster batch mode, Data Collector runs on BigConnect Cloud Hadoop Cluster to process Object Store data.

In either mode, Data Collector processes all available data and then stops the pipeline.

Data Collector runs as an application on top of MapReduce in the Hadoop cluster. MapReduce runs on a YARN cluster manager. MapReduce creates one map task for each HDFS block.

Use the Hadoop FS origin to process data from Object Store in cluster batch mode.



