# Origins

An origin stage represents the source for the pipeline. You can use a single origin stage in a pipeline.

You can use different origins based on the execution mode of the pipeline: standalone, cluster, or edge. To help create or test pipelines, you can use development origins.

### Standalone Pipelines

In standalone pipelines, you can use the following origins:

* **Amazon S3** - Reads objects from Amazon S3. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Amazon SQS Consumer** - Reads data from queues in Amazon Simple Queue Services \(SQS\). Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Azure Data Lake Storage Gen1** - Reads data from Microsoft Azure Data Lake Storage Gen1. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Azure Data Lake Storage Gen2** - Reads data from Microsoft Azure Data Lake Storage Gen2. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Azure IoT/Event Hub Consumer** - Reads data from Microsoft Azure Event Hub. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **CoAP Server** - Listens on a CoAP endpoint and processes the contents of all authorized CoAP requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Cron Scheduler** - Generates a record with the current datetime as scheduled by a cron expression.
* **Directory** - Reads fully-written files from a directory. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Elasticsearch** - Reads data from an Elasticsearch cluster. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **File Tail** - Reads lines of data from an active file after reading related archived files in the directory.
* **Google BigQuery** - Executes a query job and reads the result from Google BigQuery.
* **Google Cloud Storage** - Reads fully written objects from Google Cloud Storage.
* **Google Pub/Sub Subscriber** - Consumes messages from a Google Pub/Sub subscription. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Groovy Scripting** - Runs a Groovy script to create Data Collector records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* **Hadoop FS Standalone** - Reads fully-written files from HDFS, BigConnect, Azure Data Lake Storage, or Azure HDInsight. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **HTTP Clien**t - Reads data from a streaming HTTP resource URL.
* **HTTP Server** - Listens on an HTTP endpoint and processes the contents of all authorized HTTP POST and PUT requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **HTTP to Kafka \(Deprecated\)** - Listens on a HTTP endpoint and writes the contents of all authorized HTTP POST requests directly to Kafka.
* **JavaScript Scripting** - Runs a JavaScript script to create Data Collector records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* **JDBC Multitable Consumer** - Reads database data from multiple tables through a JDBC connection. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **JDBC Query Consumer** - Reads database data using a user-defined SQL query through a JDBC connection.
* **JMS Consumer** - Reads messages from JMS.
* **Jython Scripting** - Runs a Jython script to create Data Collector records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* **Kafka Consumer** - Reads messages from a single Kafka topic.
* **Kafka Multi Topic** Consumer - Reads messages from multiple Kafka topics. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Kinesis Consumer** - Reads data from Kinesis Streams. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **MQTT Subscriber** - Subscribes to a topic on an MQTT broker to read messages from the broker.
* **MySQL Binary Log** - Reads MySQL binary logs to generate change data capture records.
* **NiFi HTTP Server** - Listens for requests from a NiFi PutHTTP processor and processes NiFi FlowFiles.
* **Omniture** - Reads web usage reports from the Omniture reporting API.
* **OPC UA Client** - Reads data from a OPC UA server.
* **Oracle Bulkload** - Reads data from multiple Oracle database tables, then stops the pipeline. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Oracle CDC Client** - Reads LogMiner redo logs to generate change data capture records.
* **PostgreSQL CDC Client** - Reads PostgreSQL WAL data to generate change data capture records.
* **Pulsar Consumer** - Reads messages from Apache Pulsar topics.
* **RabbitMQ Consumer** - Reads messages from RabbitMQ.
* **Redis Consumer** - Reads messages from Redis.
* **REST Service** - Listens on an HTTP endpoint, parses the contents of all authorized requests, and sends responses back to the originating REST API. Creates multiple threads to enable parallel processing in a multithreaded pipeline. Use only in microservice pipelines.
* **Salesforce** - Reads data from Salesforce.
* **SFTP/FTP/FTPS Client** - Reads files from an SFTP, FTP, or FTPS server.
* **SQL Server CDC Client** - Reads data from Microsoft SQL Server CDC tables. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **SQL Server Change Tracking** - Reads data from Microsoft SQL Server change tracking tables and generates the latest version of each record. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Start Pipeline** - Starts a Data Collector, Data Collector Edge, or Transformer pipeline.
* **TCP Server** - Listens at the specified ports and processes incoming data over TCP/IP connections. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Teradata Consumer** - Reads data from Teradata Database tables through a JDBC connection. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **UDP Multithreaded Source** - Reads messages from one or more UDP ports. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **UDP Source** - Reads messages from one or more UDP ports.
* **UDP to Kafka \(Deprecated**\) - Reads messages from one or more UDP ports and writes the data to Kafka.
* **WebSocket Client** - Reads data from a WebSocket server endpoint. Can send responses back to the origin system as part of a microservice pipeline.
* **WebSocket Server** - Listens on a WebSocket endpoint and processes the contents of all authorized WebSocket client requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline. Can send responses back to the origin system as part of a microservice pipeline.

### Cluster Pipelines

In cluster pipelines, you can use the following origins:

* **Hadoop FS** - Reads data from HDFS,  BigConnect, Amazon S3, or other file systems using the Hadoop FileSystem interface.
* **Kafka Consumer** - Reads messages from Kafka. Use the cluster version of the origin.

### Development Origins

To help create or test pipelines, you can use the following development origins:

* Dev Data Generator
* Dev Random Source
* Dev Raw Data Source
* Dev SDC RPC with Buffering
* Dev Snapshot Replaying
* Sensor Reader

