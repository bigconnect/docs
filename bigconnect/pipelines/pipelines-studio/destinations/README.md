# Destinations

A destination stage represents the target for a pipeline. You can use one or more destinations in a pipeline.

You can use different destinations based on the execution mode of the pipeline: standalone, cluster, or edge. To help create or test pipelines, you can use a development destination.

### Standalone Pipelines Only

In standalone pipelines, you can use the following destinations:

* [Rabbit MQ Producer ](rabbitmq-producer.md)- Writes data to RabbitMQ.
* [Send Response to Origin](send-response-to-origin.md) - Sends records with the specified response to the microservice origin in the pipeline. Use only in a [microservice pipeline](../microservice-pipelines.md).

### Standalone or Cluster Pipelines

In standalone or cluster pipelines, you can use the following destinations:

* [Aerospike](aerospike.md) - Writes data to Aerospike.
* [Amazon S3](amazon-s3.md) - Writes data to BigConnect Object Store and Amazon S3.
* [Azure Data Lake Storage Gen1](azure-data-lake-storage-gen1.md) - Writes data to Azure Data Lake Storage Gen1.
* [Azure Data Lake Storage Gen2](azure-data-lake-storage-gen2.md) - Writes data to Azure Data Lake Storage Gen2.
* [Azure Event Hub Producer](azure-event-hub-producer.md) - Writes data to Azure Event Hub.
* [Azure IoT Hub Producer](azure-iot-hub-producer.md) - Writes data to Microsoft Azure IoT Hub.
* [Cassandra](cassandra.md) - Writes data to a Cassandra cluster.
* [Databricks Delta Lake](databricks-delta-lake.md) - Writes data to the Databricks Delta Lake
* [Elasticsearch](elasticsearch.md) - Writes data to an Elasticsearch cluster.
* [Flume](flume.md) - Writes data to a Flume source.
* [Google BigQuery](google-bigquery.md) - Streams data into Google BigQuery.
* [Google Bigtable](google-bigtable.md) - Writes data to Google Cloud Bigtable.
* [Google Cloud Storage](google-cloud-storage.md) - Writes data to Google Cloud Storage.
* [Google Pub/Sub Publisher ](google-pub-sub-publisher.md)- Publishes messages to Google Pub/Sub.
* [Hadoop FS](hadoop-fs.md) - Writes data to HDFS, Azure Data Lake Storage, or Azure HDInsight.
* [HBase](hbase.md) - Writes data to an HBase cluster.
* [Hive Metastore](hive-metastore.md) - Creates and updates Hive tables as needed.
* [Hive Streaming](hive-streaming.md) - Writes data to Hive.
* [HTTP Client](http-client.md) - Writes data to an HTTP endpoint. Can send responses to a microservice origin in a microservice pipeline.
* [InfluxDB](influxdb.md) - Writes data to InfluxDB.
* [JDBC Producer](jdbc-producer.md) - Writes data to JDBC.
* [JMS Producer](jms-producer.md) - Writes data to JMS.
* [Kafka Producer](kafka-producer.md) - Writes data to a Kafka cluster. Can send responses to a microservice origin in a microservice pipeline.
* [Local FS](local-fs.md) - Writes data to a local file system.
* [MemSQL Fast Loader](memsql-fast-loader.md) - Writes data to MemSQL or MySQL.
* [MongoDB](mongodb.md) - Writes data to MongoDB.
* [Named Pipe](named-pipe.md) - Writes data to a named pipe.
* [Redis](redis.md) - Writes data to Redis.
* [Salesforce](salesforce.md) - Writes data to Salesforce.
* [DP RPC](dp-rpc.md) - Passes data to a DP RPC origin in a DP RPC pipeline.
* [SFTP/FTP/FTPS Client](sftp-ftp-ftps-client.md) - Sends data to a URL using SFTP, FTP, or FTPS.
* [Snowflake](snowflake.md) - Writes data to tables in a Snowflake database.
* [Solr](solr.md) - Writes data to a Solr node or cluster.
* [Splunk](splunk.md) - Writes data to Splunk.
* [Syslog](syslog.md) - Writes data to a Syslog server.
* [To Error](to-error.md) - Passes records to the pipeline for error handling.
* [Trash](trash.md) - Removes records from the pipeline.
* [WebSocket Client](websocket-client.md) - Writes data to a WebSocket endpoint.

### Development Destination

To help create or test pipelines, you can use the following development destination:

* To Event
