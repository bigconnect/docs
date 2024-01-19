# Destinations

A destination stage represents the target for a pipeline. You can use one or more destinations in a pipeline.

You can use different destinations based on the execution mode of the pipeline: standalone, cluster, or edge. To help create or test pipelines, you can use a development destination.

### Standalone Pipelines Only

In standalone pipelines, you can use the following destinations:

* Rabbit MQ Producer - Writes data to RabbitMQ.
* Send Response to Origin - Sends records with the specified response to the microservice origin in the pipeline. Use only in a [microservice pipeline](../microservice-pipelines.md).

### Standalone or Cluster Pipelines

In standalone or cluster pipelines, you can use the following destinations:

* Aerospike - Writes data to Aerospike.
* Amazon S3 - Writes data to BigConnect Object Store and Amazon S3.
* Azure Data Lake Storage (Legacy) - Writes data to Azure Data Lake Storage Gen1.
* Azure Data Lake Storage Gen1 - Writes data to Azure Data Lake Storage Gen1.
* Azure Data Lake Storage Gen2 - Writes data to Azure Data Lake Storage Gen2.
* Azure Event Hub Producer - Writes data to Azure Event Hub.
* Azure IoT Hub Producer - Writes data to Microsoft Azure IoT Hub.
* Cassandra - Writes data to a Cassandra cluster.
* CoAP Client - Writes data to a CoAP endpoint.
* Couchbase - Writes data to a Couchbase database.
* Elasticsearch - Writes data to an Elasticsearch cluster.
* Einstein Analytics - Writes data to Salesforce Einstein Analytics.
* Flume - Writes data to a Flume source.
* Google BigQuery - Streams data into Google BigQuery.
* Google Bigtable - Writes data to Google Cloud Bigtable.
* Google Cloud Storage - Writes data to Google Cloud Storage.
* Google Pub/Sub Publisher - Publishes messages to Google Pub/Sub.
* GPSS Producer - Writes data to Greenplum Database through Greenplum Stream Server (GPSS).
* Hadoop FS - Writes data to BigConnect Zion, Azure Data Lake Storage, or Azure HDInsight.
* HBase - Writes data to an HBase cluster.
* Hive Metastore - Creates and updates Hive tables as needed.
* Hive Streaming - Writes data to Hive.
* HTTP Client - Writes data to an HTTP endpoint. Can send responses to a microservice origin in a microservice pipeline.
* InfluxDB - Writes data to InfluxDB.
* JDBC Producer - Writes data to JDBC.
* JMS Producer - Writes data to JMS.
* Kafka Producer - Writes data to a Kafka cluster. Can send responses to a microservice origin in a microservice pipeline.
* Kinesis Firehose - Writes data to a Kinesis Firehose delivery stream.
* Kinesis Producer - Writes data to Kinesis Streams. Can send responses to a microservice origin in a microservice pipeline.
* KineticaDB - Writes data to a table in a Kinetica cluster.
* Kudu - Writes data to Kudu.
* Local FS - Writes data to a local file system.
* MemSQL Fast Loader - Writes data to MemSQL or MySQL.
* MongoDB - Writes data to MongoDB.
* MQTT Publisher - Publishes messages to a topic on an MQTT broker.
* Named Pipe - Writes data to a named pipe.
* Pulsar Producer - Writes data to Apache Pulsar topics.
* Redis - Writes data to Redis.
* Salesforce - Writes data to Salesforce.
* SDC RPC - Passes data to an SDC RPC origin in an SDC RPC pipeline.
* SFTP/FTP/FTPS Client - Sends data to a URL using SFTP, FTP, or FTPS.
* Snowflake - Writes data to tables in a Snowflake database.
* Solr - Writes data to a Solr node or cluster.
* Splunk - Writes data to Splunk.
* Syslog - Writes data to a Syslog server.
* To Error - Passes records to the pipeline for error handling.
* Trash - Removes records from the pipeline.
* WebSocket Client - Writes data to a WebSocket endpoint.

### Development Destination

To help create or test pipelines, you can use the following development destination:

* To Event
