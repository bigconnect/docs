# Destinations

A destination stage represents the target for a pipeline. You can use one or more destinations in a pipeline.

You can use different destinations based on the execution mode of the pipeline: standalone, cluster, or edge. To help create or test pipelines, you can use a development destination.

### Standalone Pipelines Only

In standalone pipelines, you can use the following destinations:

* [Rabbit MQ Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MongoDB.html#concept_eth_k5n_4v) - Writes data to RabbitMQ.
* Send Response to Origin - Sends records with the specified response to the microservice origin in the pipeline. Use only in a [microservice pipeline](microservice-pipelines.md).

### Standalone or Cluster Pipelines

In standalone or cluster pipelines, you can use the following destinations:

* [Aerospike](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Aerospike.html#concept_gyq_rpr_4cb) - Writes data to Aerospike.
* [Amazon S3](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/AmazonS3.html#concept_avx_bnq_rt) - Writes data to Amazon S3.
* [Azure Data Lake Storage \(Legacy\)](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/DataLakeStore.html#concept_jzm_kf4_zx) - Writes data to Azure Data Lake Storage Gen1.
* [Azure Data Lake Storage Gen1](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/ADLS-G1-D.html#concept_xzc_wfq_xhb) - Writes data to Azure Data Lake Storage Gen1.
* [Azure Data Lake Storage Gen2](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/ADLS-G2-D.html#concept_ajp_1d2_vhb) - Writes data to Azure Data Lake Storage Gen2.
* [Azure Event Hub Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/AzureEventHubProducer.html#concept_xq5_d5q_1bb) - Writes data to Azure Event Hub.
* [Azure IoT Hub Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/AzureIoTHub.html#concept_pnd_jkq_1bb) - Writes data to Microsoft Azure IoT Hub.
* [Cassandra](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Cassandra.html#concept_hfy_mfd_sr) - Writes data to a Cassandra cluster.
* [CoAP Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/CoAPClient.html#concept_hw5_s3n_sz) - Writes data to a CoAP endpoint.
* [Couchbase](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Couchbase.html#concept_ahq_1wq_h2b) - Writes data to a Couchbase database.
* [Elasticsearch](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Elasticsearch.html#concept_u5t_vpv_4r) - Writes data to an Elasticsearch cluster.
* [Einstein Analytics](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/WaveAnalytics.html#concept_hlx_r53_rx) - Writes data to Salesforce Einstein Analytics.
* [Flume](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Flume.html#concept_pzn_hl4_yr) - Writes data to a Flume source.
* [Google BigQuery](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/BigQuery.html#concept_hj4_brk_dbb) - Streams data into Google BigQuery.
* [Google Bigtable](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Bigtable.html#concept_pl5_tmq_tx) - Writes data to Google Cloud Bigtable.
* [Google Cloud Storage](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/GCS.html#concept_p4n_jrl_nbb) - Writes data to Google Cloud Storage.
* [Google Pub/Sub Publisher](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/PubSubPublisher.html#concept_qsj_hk1_v1b) - Publishes messages to Google Pub/Sub.
* [GPSS Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/GPSS.html#concept_qjf_xdz_q3b) - Writes data to Greenplum Database through Greenplum Stream Server \(GPSS\).
* [Hadoop FS](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/HadoopFS-destination.html#concept_awl_4km_zq) - Writes data to HDFS, Azure Data Lake Storage, or Azure HDInsight.
* [HBase](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/HBase.html#concept_wsz_5t5_vr) - Writes data to an HBase cluster.
* [Hive Metastore](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/HiveMetastore.html#concept_gcr_z2t_zv) - Creates and updates Hive tables as needed.
* [Hive Streaming](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Hive.html#concept_kvs_3hh_ht) - Writes data to Hive.
* [HTTP Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/HTTPClient.html#concept_khl_sg5_lz) - Writes data to an HTTP endpoint. Can send responses to a microservice origin in a microservice pipeline.
* [InfluxDB](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/InfluxDB.html#concept_inf_db_sr) - Writes data to InfluxDB.
* [JDBC Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/JDBCProducer.html#concept_kvs_3hh_ht) - Writes data to JDBC.
* [JMS Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/JMSProducer.html#concept_sfz_ww5_n1b) - Writes data to JMS.
* [Kafka Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/KProducer.html#concept_oq2_5jl_zq) - Writes data to a Kafka cluster. Can send responses to a microservice origin in a microservice pipeline.
* [Kinesis Firehose](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/KinFirehose.html#concept_bjv_dpk_kv) - Writes data to a Kinesis Firehose delivery stream.
* [Kinesis Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/KinProducer.html#concept_swk_h1j_yr) - Writes data to Kinesis Streams. Can send responses to a microservice origin in a microservice pipeline.
* [KineticaDB](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/KineticaDB.html#concept_hxh_5xg_qbb) - Writes data to a table in a Kinetica cluster.
* [Kudu](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Kudu.html#concept_chy_xxg_4v) - Writes data to Kudu.
* [Local FS](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/LocalFS.html#concept_zvc_bv5_1r) - Writes data to a local file system.
* [MapR DB](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MapRDB.html#concept_vxg_w2z_yv) - Writes data as text, binary data, or JSON strings to MapR DB binary tables.
* [MapR DB JSON](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MapRDBJSON.html#concept_i4h_2kj_dy) - Writes data as JSON documents to MapR DB JSON tables.
* [MapR FS](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MapRFS.html#concept_spv_xlc_fv) - Writes data to MapR FS.
* [MapR Streams Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MapRStreamsProd.html#concept_cfj_qbn_2v) - Writes data to MapR Streams.
* [MemSQL Fast Loader](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MemSQLLoader.html#concept_kvs_3hh_ht) - Writes data to MemSQL or MySQL.
* [MongoDB](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MongoDB.html#concept_eth_k5n_4v) - Writes data to MongoDB.
* [MQTT Publisher](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MQTTPublisher.html#concept_odz_txt_lz) - Publishes messages to a topic on an MQTT broker.
* [Named Pipe](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/NamedPipe.html#concept_pl5_tdg_gcb) - Writes data to a named pipe.
* [Pulsar Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/PulsarProducer.html#concept_fq3_kpc_r2b) - Writes data to Apache Pulsar topics.
* [Redis](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Redis.html#concept_ktc_gw2_gw) - Writes data to Redis.
* [Salesforce](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Salesforce.html#concept_rlb_rt3_rx) - Writes data to Salesforce.
* [SDC RPC](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/SDC_RPCdest.html#concept_lfk_hx2_ct) - Passes data to an SDC RPC origin in an SDC RPC pipeline.
* [SFTP/FTP/FTPS Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/SFTP.html#concept_sgt_m2m_xhb) - Sends data to a URL using SFTP, FTP, or FTPS.
* [Snowflake](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Snowflake.html#concept_vxl_zzc_1gb) - Writes data to tables in a Snowflake database.
* [Solr](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Solr.html#concept_z2g_q1r_wr) - Writes data to a Solr node or cluster.
* [Splunk](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Splunk.html#concept_zzr_pqn_xdb) - Writes data to Splunk.
* [Syslog](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Syslog.html#concept_idr_ct5_w2b) - Writes data to a Syslog server.
* [To Error](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/ToError.html#concept_ryn_v3z_lr) - Passes records to the pipeline for error handling.
* [Trash](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/Trash.html#concept_htf_ydj_wq) - Removes records from the pipeline.
* [WebSocket Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/WebSocketClient.html#concept_l4d_mjn_lz) - Writes data to a WebSocket endpoint.

### Edge Pipelines

In edge pipelines, you can use the following destinations:

* [Amazon S3](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/AmazonS3.html#concept_avx_bnq_rt) - Writes data to Amazon S3.
* [CoAP Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/CoAPClient.html#concept_hw5_s3n_sz) - Writes data to a CoAP endpoint.
* [HTTP Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/HTTPClient.html#concept_khl_sg5_lz) - Writes data to an HTTP endpoint.
* [Kafka Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/KProducer.html#concept_oq2_5jl_zq) - Writes data to a Kafka cluster.
* [Kinesis Firehose](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/KinFirehose.html#concept_bjv_dpk_kv) - Writes data to a Kinesis Firehose delivery stream.
* [Kinesis Producer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/KinProducer.html#concept_swk_h1j_yr) - Writes data to Kinesis Streams.
* [MQTT Publisher](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/MQTTPublisher.html#concept_odz_txt_lz) - Publishes messages to a topic on an MQTT broker.
* [To Error](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/ToError.html#concept_ryn_v3z_lr) - Passes records to the pipeline for error handling.
* [WebSocket Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Destinations/WebSocketClient.html#concept_l4d_mjn_lz) - Writes data to a WebSocket endpoint.

### Development Destination

To help create or test pipelines, you can use the following development destination:

* To Event

