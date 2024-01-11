# Kerberos Authentication

You can use Kerberos authentication to connect to external systems as well as YARN clusters.

By default, BigConnect Pipelines uses the user account that started it to connect to external systems. When you enable Kerberos, it can use the Kerberos principal to connect to external systems.

You can configure Kerberos authentication for the following stages:

* Hadoop FS origin
* Hadoop FS Standalone origin
* HTTP to Kafka origin
* Kafka Consumer origin
* Kafka Multitopic Consumer origin
* MapR FS origin
* MapR FS Standalone origin
* SDC RPC to Kafka origin
* UDP to Kafka origin
* HBase Lookup processor
* Hive Metadata processor
* Kudu Lookup processor
* Cassandra destination, when the DataStax Enterprise Java driver is installed
* Hadoop FS destination
* HBase destination
* Hive Metastore destination
* Kafka Producer destination
* Kudu destination
* MapR DB destination
* MapR FS destination
* Solr destination
* HDFS File Metadata executor
* MapR FS File Metadata executor
* MapReduce executor
* Spark executor

To enable BigConnect Pipelines to use Kerberos authentication, use the required procedure for your installation type.
