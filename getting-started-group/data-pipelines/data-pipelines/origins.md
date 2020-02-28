# Origins

An origin stage represents the source for the pipeline. You can use a single origin stage in a pipeline.

You can use different origins based on the execution mode of the pipeline: standalone, cluster, or edge. To help create or test pipelines, you can use development origins.

### Standalone Pipelines

In standalone pipelines, you can use the following origins:

* [Amazon S3](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/AmazonS3.html#concept_kvs_3hh_ht) - Reads objects from Amazon S3. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Amazon SQS Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/AmazonSQS.html#concept_xsh_knm_5bb) - Reads data from queues in Amazon Simple Queue Services \(SQS\). Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Azure Data Lake Storage Gen1](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/ADLS-G1.html#concept_osx_qgz_xhb) - Reads data from Microsoft Azure Data Lake Storage Gen1. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Azure Data Lake Storage Gen2](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/ADLS-G2.html#concept_osx_qgz_xhb) - Reads data from Microsoft Azure Data Lake Storage Gen2. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Azure IoT/Event Hub Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/AzureEventHub.html#concept_c1z_15q_1bb) - Reads data from Microsoft Azure Event Hub. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [CoAP Server](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/CoAPServer.html#concept_wfy_ghn_sz) - Listens on a CoAP endpoint and processes the contents of all authorized CoAP requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Cron Scheduler](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/CronScheduler.html#concept_nsz_mnr_2jb) - Generates a record with the current datetime as scheduled by a cron expression.
* [Directory](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/Directory.html#concept_qcq_54n_jq) - Reads fully-written files from a directory. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Elasticsearch](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/Elasticsearch.html#concept_f1q_vpm_2z) - Reads data from an Elasticsearch cluster. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [File Tail](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/FileTail.html#concept_n1y_qyp_5q) - Reads lines of data from an active file after reading related archived files in the directory.
* [Google BigQuery](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/BigQuery.html#concept_cg3_y3v_q1b) - Executes a query job and reads the result from Google BigQuery.
* [Google Cloud Storage](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/GCS.html#concept_iyd_wql_nbb) - Reads fully written objects from Google Cloud Storage.
* [Google Pub/Sub Subscriber](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/PubSub.html#concept_pjw_qtl_r1b) - Consumes messages from a Google Pub/Sub subscription. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Groovy Scripting](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/GroovyScripting.html#concept_chr_zjj_l3b) - Runs a Groovy script to create Data Collector records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* [Hadoop FS Standalone](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/HDFSStandalone.html#concept_djz_pdm_hdb) - Reads fully-written files from HDFS, Azure Data Lake Storage, or Azure HDInsight. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [HTTP Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/HTTPClient.html#concept_wk4_bjz_5r) - Reads data from a streaming HTTP resource URL.
* [HTTP Server](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/HTTPServer.html#concept_s2p_5hb_4y) - Listens on an HTTP endpoint and processes the contents of all authorized HTTP POST and PUT requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [HTTP to Kafka \(Deprecated\)](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/HTTPtoKafka.html#concept_izh_mqd_dy) - Listens on a HTTP endpoint and writes the contents of all authorized HTTP POST requests directly to Kafka.
* [JavaScript Scripting](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/JavaScriptScripting.html#concept_kn5_bvt_m3b) - Runs a JavaScript script to create Data Collector records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* [JDBC Multitable Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MultiTableJDBCConsumer.html#concept_zp3_wnw_4y) - Reads database data from multiple tables through a JDBC connection. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [JDBC Query Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/JDBCConsumer.html#concept_qhf_hjr_bs) - Reads database data using a user-defined SQL query through a JDBC connection.
* [JMS Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/JMS.html#concept_rhh_4nj_dt) - Reads messages from JMS.
* [Jython Scripting](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/JythonScripting.html#concept_fxz_35t_m3b) - Runs a Jython script to create Data Collector records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* [Kafka Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/KConsumer.html#concept_msz_wnr_5q) - Reads messages from a single Kafka topic.
* [Kafka Multitopic Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/KafkaMultiConsumer.html#concept_ccs_fn4_x1b) - Reads messages from multiple Kafka topics. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Kinesis Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/KinConsumer.html#concept_anh_4y3_yr) - Reads data from Kinesis Streams. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [MapR DB CDC](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MapRdbCDC.html#concept_qwj_5vm_pbb) - Reads changed MapR DB data that has been written to MapR Streams. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [MapR DB JSON](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MapRDBJSON.html#concept_ywh_k15_3y) - Reads JSON documents from MapR DB JSON tables.
* [MapR FS Standalone](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MapRFSStandalone.html#concept_b43_3qc_mdb) - Reads fully-written files from MapR FS. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [MapR Multitopic Streams Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MapRStreamsMultiConsumer.html#concept_hvd_hww_lbb) - Reads messages from multiple MapR Streams topics. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [MapR Streams Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MapRStreamsCons.html#concept_cvy_xsf_2v) - Reads messages from MapR Streams.
* [MongoDB](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MongoDB.html#concept_bk4_2rs_ns) - Reads documents from MongoDB.
* [MongoDB Oplog](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MongoDBOplog.html#concept_mjn_yqw_4y) - Reads entries from a MongoDB Oplog.
* [MQTT Subscriber](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MQTTSubscriber.html#concept_ukz_3vt_lz) - Subscribes to a topic on an MQTT broker to read messages from the broker.
* [MySQL Binary Log](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MySQLBinaryLog.html#concept_kqg_1yh_xx) - Reads MySQL binary logs to generate change data capture records.
* [NiFi HTTP Server](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/NiFi.html#concept_ynn_vdb_p3b) - Listens for requests from a NiFi PutHTTP processor and processes NiFi FlowFiles.
* [Omniture](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/Omniture.html#concept_dsr_xmw_1s) - Reads web usage reports from the Omniture reporting API.
* [OPC UA Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/OPCUAClient.html#concept_nmf_1ly_f1b) - Reads data from a OPC UA server.
* [Oracle Bulkload](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/OracleBulk.html#concept_lnz_kzp_zgb) - Reads data from multiple Oracle database tables, then stops the pipeline. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Oracle CDC Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/OracleCDC.html#concept_rs5_hjj_tw) - Reads LogMiner redo logs to generate change data capture records.
* [PostgreSQL CDC Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/PostgreSQL.html#concept_cfs_4m4_n2b) - Reads PostgreSQL WAL data to generate change data capture records.
* [Pulsar Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/PulsarConsumer.html#concept_o2b_1pc_r2b) - Reads messages from Apache Pulsar topics.
* [RabbitMQ Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/RabbitMQ.html#concept_dyg_lq1_h5) - Reads messages from RabbitMQ.
* [Redis Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/Redis.html#concept_plr_t3v_jw) - Reads messages from Redis.
* [REST Service](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/RESTService.html#concept_hfg_2sn_p2b) - Listens on an HTTP endpoint, parses the contents of all authorized requests, and sends responses back to the originating REST API. Creates multiple threads to enable parallel processing in a multithreaded pipeline. Use only in microservice pipelines.
* [Salesforce](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/Salesforce.html#concept_odf_vr3_rx) - Reads data from Salesforce.
* [SDC RPC](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/SDC_RPCorigin.html#concept_agb_5c1_ct) - Reads data from an SDC RPC destination in an SDC RPC pipeline.
* [SDC RPC to Kafka \(Deprecated\)](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/SDCRPCtoKafka.html#concept_tdk_slk_pw) - Reads data from an SDC RPC destination in an SDC RPC pipeline and writes it to Kafka.
* [SFTP/FTP/FTPS Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/SFTP.html#concept_ic5_bzd_5v) - Reads files from an SFTP, FTP, or FTPS server.
* [SQL Server CDC Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/SQLServerCDC.html#concept_ut3_ywc_v1b) - Reads data from Microsoft SQL Server CDC tables. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [SQL Server Change Tracking](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/SQLServerChange.html#concept_ewq_b2s_r1b) - Reads data from Microsoft SQL Server change tracking tables and generates the latest version of each record. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Start Pipeline](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/StartPipeline.html#concept_h1l_xpr_2jb) - Starts a Data Collector, Data Collector Edge, or Transformer pipeline.
* [TCP Server](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/TCPServer.html#concept_ppm_xb1_4z) - Listens at the specified ports and processes incoming data over TCP/IP connections. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [Teradata Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/Teradata.html#concept_zp3_wnw_4y) - Reads data from Teradata Database tables through a JDBC connection. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [UDP Multithreaded Source](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/UDPMulti.html#concept_wng_g5f_5bb) - Reads messages from one or more UDP ports. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [UDP Source](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/UDP.html#concept_rst_2y5_1s) - Reads messages from one or more UDP ports.
* [UDP to Kafka \(Deprecated\)](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/UDPtoKafka.html#concept_jzq_jcz_pw) - Reads messages from one or more UDP ports and writes the data to Kafka.
* [WebSocket Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/WebSocketClient.html#concept_unk_nzk_fbb) - Reads data from a WebSocket server endpoint. Can send responses back to the origin system as part of a microservice pipeline.
* [WebSocket Server](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/WebSocketServer.html#concept_u2r_gpc_3z) - Listens on a WebSocket endpoint and processes the contents of all authorized WebSocket client requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline. Can send responses back to the origin system as part of a microservice pipeline.

### Cluster Pipelines

In cluster pipelines, you can use the following origins:

* [Hadoop FS](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/HadoopFS-origin.html#concept_lw2_tnm_vs) - Reads data from HDFS, Amazon S3, or other file systems using the Hadoop FileSystem interface.
* [Kafka Consumer](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/KConsumer.html#concept_msz_wnr_5q) - Reads messages from Kafka. Use the cluster version of the origin.
* [MapR FS](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MapRFS.html#concept_psz_db4_lx) - Reads data from MapR FS.

### Edge Pipelines

In edge pipelines, you can use the following origins:

* [Directory](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/Directory.html#concept_qcq_54n_jq) - Reads fully-written files from a directory.
* [File Tail](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/FileTail.html#concept_n1y_qyp_5q) - Reads lines of data from an active file after reading related archived files in the directory.
* [gRPC Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/gRPCClient.html#concept_yp1_4zs_yfb) - Reads data from a gRPC server.
* [HTTP Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/HTTPClient.html#concept_wk4_bjz_5r) - Reads data from a streaming HTTP resource URL.
* [HTTP Server](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/HTTPServer.html#concept_s2p_5hb_4y) - Listens on an HTTP endpoint and processes the contents of all authorized HTTP POST and PUT requests.
* [MQTT Subscriber](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/MQTTSubscriber.html#concept_ukz_3vt_lz) - Subscribes to a topic on an MQTT broker to read messages from the broker.
* [System Metrics](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/SystemMetrics.html#concept_gzy_gmv_32b) - Reads system metrics from the edge device where SDC Edge is installed.
* [WebSocket Client](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/WebSocketClient.html#concept_unk_nzk_fbb) - Reads data from a WebSocket server endpoint.
* [Windows Event Log](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Origins/WindowsLog.html#concept_agf_5jv_sbb) - Reads data from a Microsoft Windows event log located on a Windows machine.

### Development Origins

To help create or test pipelines, you can use the following development origins:

* Dev Data Generator
* Dev Random Source
* Dev Raw Data Source
* Dev SDC RPC with Buffering
* Dev Snapshot Replaying
* Sensor Reader

