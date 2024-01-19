# Origins

An origin stage represents the source for the pipeline. You can use a single origin stage in a pipeline.

You can use different origins based on the execution mode of the pipeline: **standalone** or **cluster**. To help create or test pipelines, you can use **development origins.**

## Standalone Pipelines

In standalone pipelines, you can use the following origins:

* **Amazon S3 / BigConnect Object Store** - Reads objects from BigConnect Object Store or any  S3-compatible store. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Amazon SQS Consumer** - Reads data from queues in Amazon Simple Queue Services (SQS). Creates multiple threads to enable parallel processing in a multithreaded pipeline.
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
* **Groovy Scripting** - Runs a Groovy script to create BigConnect Pipelines records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* **Hadoop FS Standalone** - Reads fully-written files from HDFS, BigConnect, Azure Data Lake Storage, or Azure HDInsight. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **HTTP Clien**t - Reads data from a streaming HTTP resource URL.
* **HTTP Server** - Listens on an HTTP endpoint and processes the contents of all authorized HTTP POST and PUT requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **HTTP to Kafka (Deprecated)** - Listens on a HTTP endpoint and writes the contents of all authorized HTTP POST requests directly to Kafka.
* **JavaScript Scripting** - Runs a JavaScript script to create BigConnect Pipelines records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* **JDBC Multitable Consumer** - Reads database data from multiple tables through a JDBC connection. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **JDBC Query Consumer** - Reads database data using a user-defined SQL query through a JDBC connection.
* **JMS Consumer** - Reads messages from JMS.
* **Jython Scripting** - Runs a Jython script to create BigConnect Pipelines records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* **Kafka Consumer** - Reads messages from a single Kafka topic.
* **Kafka Multi Topic** Consumer - Reads messages from multiple Kafka topics. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Kinesis Consumer** - Reads data from Kinesis Streams. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **MQTT Subscriber** - Subscribes to a topic on an MQTT broker to read messages from the broker.
* **MySQL Binary Log** - Reads MySQL binary logs to generate change data capture records.
* **NiFi HTTP Server** - Listens for requests from a NiFi PutHTTP processor and processes NiFi FlowFiles.
* **OPC UA Client** - Reads data from a OPC UA server.
* **Oracle Bulkload** - Reads data from multiple Oracle database tables, then stops the pipeline. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Oracle CDC Client** - Reads LogMiner redo logs to generate change data capture records.
* **PostgreSQL CDC Client** - Reads PostgreSQL WAL data to generate change data capture records.
* **Redis Consumer** - Reads messages from Redis.
* **REST Service** - Listens on an HTTP endpoint, parses the contents of all authorized requests, and sends responses back to the originating REST API. Creates multiple threads to enable parallel processing in a multithreaded pipeline. Use only in microservice pipelines.
* **Salesforce** - Reads data from Salesforce.
* **SFTP/FTP/FTPS Client** - Reads files from an SFTP, FTP, or FTPS server.
* **SQL Server CDC Client** - Reads data from Microsoft SQL Server CDC tables. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **SQL Server Change Tracking** - Reads data from Microsoft SQL Server change tracking tables and generates the latest version of each record. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **Start Pipeline** - Starts a pipeline.
* **TCP Server** - Listens at the specified ports and processes incoming data over TCP/IP connections. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **UDP Multithreaded Source** - Reads messages from one or more UDP ports. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* **UDP Source** - Reads messages from one or more UDP ports.
* **UDP to Kafka (Deprecated**) - Reads messages from one or more UDP ports and writes the data to Kafka.
* **WebSocket Client** - Reads data from a WebSocket server endpoint. Can send responses back to the origin system as part of a microservice pipeline.
* **WebSocket Server** - Listens on a WebSocket endpoint and processes the contents of all authorized WebSocket client requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline. Can send responses back to the origin system as part of a microservice pipeline.

## Cluster Pipelines

In cluster pipelines, you can use the following origins:

* **Hadoop FS** - Reads data from HDFS,  BigConnect, Amazon S3, or other file systems using the Hadoop FileSystem interface.
* **Kafka Consumer** - Reads messages from Kafka. Use the cluster version of the origin.

## Development Origins

To help create or test pipelines, you can use the following development origins:

* Dev Data Generator
* Dev Random Source
* Dev Raw Data Source
* Dev SDC RPC with Buffering
* Dev Snapshot Replaying
* Sensor Reader

## Comparing HTTP Origins <a href="#ariaid-title2" id="ariaid-title2"></a>

We have several HTTP origins, make sure to use the best one for your needs. Here's a quick breakdown of some key differences:

| Origin      | Description                                                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| HTTP Client | <ul><li>Initiates HTTP requests for an external system.</li><li>Processes data synchronously.</li><li>Processes JSON, text, and XML data.</li><li>Can process a range of HTTP requests.</li><li>Can be used in a pipeline with processors.</li></ul>                                                                                                                                       |
| HTTP Server | <ul><li>Listens for incoming HTTP requests and processes them while the sender waits for confirmation.</li><li>Processes data synchronously.</li><li>Creates multithreaded pipelines, thus suitable for high throughput of incoming data.</li><li>Processes virtually all data formats. Processes HTTP POST and PUT requests.</li><li>Can be used in a pipeline with processors.</li></ul> |

BigConnect Pipelines also provides an HTTP to Kafka origin for reading large volumes of data from multiple HTTP clients and writing the data immediately to Kafka, without additional processing. However, the HTTP to Kafka origin is now deprecated and will be removed in a future release. We recommend using the HTTP Server origin.

## Comparing MapR Origins <a href="#ariaid-title3" id="ariaid-title3"></a>

We have several MapR origins, make sure to use the best one for your needs. Here's a quick breakdown of some key differences:

| Origin                  | Description                                                                                                                                                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| MapR DB CDC             | <ul><li>Reads change data capture MapR DB data using MapR Streams.</li><li>Includes CDC information in record header attributes.</li><li>Use in standalone execution mode pipelines.</li></ul>                                 |
| MapR DB JSON            | <ul><li>Reads JSON documents from MapR DB.</li><li>Converts each JSON document to a record.</li><li>Use in standalone execution mode pipelines.</li></ul>                                                                      |
| MapR FS                 | <ul><li>Reads files from MapR FS.</li><li>Can be used with Kerberos Authentication.</li><li>Use in cluster execution mode pipelines.</li></ul>                                                                                 |
| MapR FS Standalone      | <ul><li>Reads files from MapR FS.</li><li>Can use multiple threads to enable the parallel processing of files.</li><li>Can be used with Kerberos Authentication.</li><li>Use in standalone execution mode pipelines.</li></ul> |
| MapR Multitopic Streams | <ul><li>Streams data from MapR Streams.</li><li>Can use multiple threads to read from multiple topics.</li><li>Use in standalone execution mode pipelines.</li></ul>                                                           |
| MapR Streams Consumer   | <ul><li>Streams data from MapR Streams.</li><li>Reads from a single topic using a single thread.</li><li>Use in standalone execution mode pipelines.</li></ul>                                                                 |

## Comparing UDP Source Origins <a href="#ariaid-title4" id="ariaid-title4"></a>

The UDP Source and UDP Multithreaded Source origins are very similar. The main differentiator is that the UDP Multithreaded Source can use multiple threads to process data within the pipeline.

The UDP Multithreaded Source has a processing queue that aids multithreaded processing. But use of this queue can slow processing under certain circumstances.

The following table describes some cases when you might want to use each origin:

| Origin                   | Ideally Used When                                                                                                                                                                                                                                                                                                     |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| UDP Multithreaded Source | <ul><li>Epoll support enables the use of multiple receiver threads to pass data to the pipeline.</li><li>Complex pipeline requires longer processing time.</li></ul><p>or</p><ul><li>Lack of epoll support allows only a single receiver thread to pass data to the pipeline.</li><li>High volumes of data.</li></ul> |
| UDP Source               | <ul><li>Epoll support enables the use of multiple receiver threads to pass data to the pipeline.</li><li>Relatively simple pipeline enables speedy BigConnect Pipelines processing.</li></ul>                                                                                                                         |

BigConnect Pipelines also provides a UDP to Kafka origin for reading large volumes of data from multiple UDP ports and writing the data immediately to Kafka, without additional processing. However, the UDP to Kafka origin is now deprecated and will be removed in a future release. We recommend using the UDP Multithreaded Source origin.

## Comparing WebSocket Origins <a href="#ariaid-title5" id="ariaid-title5"></a>

We have two WebSocket origins, make sure to use the best one for your needs. Here's a quick breakdown of some key differences:

| Origin           | Description                                                                                                                                                                                                        |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| WebSocket Client | <ul><li>Initiates a connection to a WebSocket server endpoint and then waits for the WebSocket server to push data.</li></ul>                                                                                      |
| WebSocket Server | <ul><li>Listens for incoming WebSocket requests and processes them while the sender waits for confirmation.</li><li>Creates multithreaded pipelines, thus suitable for high throughput of incoming data.</li></ul> |

## Batch Size and Wait Time <a href="#ariaid-title6" id="ariaid-title6"></a>

For origin stages, the batch size determines the maximum number of records sent through the pipeline at one time. The batch wait time determines the time that the origin waits for data before sending a batch. At the end of the wait time, it sends the batch regardless of how many records the batch contains.

For example, a File Tail origin is configured for a batch size of 20 records and a batch wait time of 240 seconds. When data arrives quickly, File Tail fills a batch with 20 records and sends it through the pipeline immediately, creating a new batch and sending it again as soon as it is full. As incoming data slows, a remaining batch contains a few records, gaining an extra record periodically. 240 seconds after creating the batch, File Tail sends the partially-full batch through the pipeline. It immediately creates a new batch and starts a new countdown.

Configure the batch wait time based on your processing needs. You might reduce the batch wait time to ensure all data is processed within a specified time frame or to make regular contact with pipeline destinations. Use the default or increase the wait time if you prefer not to process partial or empty batches.

## Maximum Record Size <a href="#ariaid-title7" id="ariaid-title7"></a>

Most data formats have a property that limits the maximum size of the record that an origin can parse. For example, the delimited data format has a Max Record Length property, the JSON data format has Max Object Length, and the text data format has Max Line Length.

When the origin processes data that is larger than the specified length, the behavior differs based on the origin and the data format. For example, with some data formats, oversized records are handled based on the record error handling configured for the origin. While in other data formats, the origin might truncate the data. For details on how an origin handles size overruns for each data format, see the "Data Formats" section of the origin documentation.

When available, the maximum record size properties are limited by the BigConnect Pipelines parser buffer size, which is 1048576 bytes by default. So, when raising the maximum record size property in the origin does not change the origin's behavior, you might need to increase the BigConnect Pipelines parser buffer size by configuring the parser.limit property in the BigConnect Pipelines configuration file.

Note that most of the maximum record size properties are specified in characters, while the BigConnect Pipelines limit is defined in bytes.

## Previewing Raw Source Data

Some origins allow you to preview raw source data. Preview raw source data when reviewing the data might help with origin configuration.

When you preview file data, you can use the real directory and actual source file. Or when appropriate, you might use a different file that is similar to the source.

When you preview Kafka data, you enter the connection information for the Kafka cluster.

The data used for the raw source preview in an origin stage is not used when previewing data for the pipeline.

1. In the Properties panel for the origin stage, click the Raw Preview tab.
2. For a Directory or File Tail origin, enter a directory and file name.
3.  For a Kafka Consumer or Kafka Multitopic Consumer, enter the following information:\


    | Kafka Raw Preview Property | Description                                                          |
    | -------------------------- | -------------------------------------------------------------------- |
    | Topic                      | Kafka topic to read.                                                 |
    | Partition                  | Partition to read.                                                   |
    | Broker Host                | Broker host name. Use any broker associated with the partition.      |
    | Broker Port                | Broker port number.                                                  |
    | Max Wait Time (secs)       | Maximum amount of time the preview waits to receive data from Kafka. |
4. Click Preview.

The Raw Source Preview area displays the preview.\
