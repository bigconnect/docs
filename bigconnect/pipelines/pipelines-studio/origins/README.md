# Origins

An origin stage represents the source for the pipeline. You can use a single origin stage in a pipeline.

You can use different origins based on the execution mode of the pipeline: **standalone** or **cluster**. To help create or test pipelines, you can use **development origins.**

## Standalone Pipelines

In standalone pipelines, you can use the following origins:

* [**Amazon S3**](amazon-s3.md) - Reads objects from any  S3-compatible store. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Amazon SQS Consumer**](amazon-sqs-consumer.md) - Reads data from queues in Amazon Simple Queue Services (SQS). Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Azure Data Lake Storage Gen1**](azure-data-lake-storage-gen1.md) - Reads data from Microsoft Azure Data Lake Storage Gen1. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Azure Data Lake Storage Gen2**](azure-data-lake-storage-gen2.md) - Reads data from Microsoft Azure Data Lake Storage Gen2. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Azure IoT/Event Hub Consumer**](azure-iot-event-hub-consumer.md) - Reads data from Microsoft Azure Event Hub. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [BigConnect Cypher](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/950/bigconnect/pipelines/pipelines-studio/origins/bigconnect-cypher) - Reads data from BigConnect Core using Cypher queries.
* [**CoAP Server**](coap-server.md) - Listens on a CoAP endpoint and processes the contents of all authorized CoAP requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Cron Scheduler**](cron-scheduler.md) - Generates a record with the current datetime as scheduled by a cron expression.
* [**Directory**](directory.md) - Reads fully-written files from a directory. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Elasticsearch**](elasticsearch.md) - Reads data from an Elasticsearch cluster. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**File Tail**](file-tail.md) - Reads lines of data from an active file after reading related archived files in the directory.
* [**Google BigQuer**](google-bigquery.md)**y** - Executes a query job and reads the result from Google BigQuery.
* [**Google Cloud Storage**](google-cloud-storage.md) - Reads fully written objects from Google Cloud Storage.
* [**Google Pub/Sub Subscriber** ](google-pub-sub-subscriber.md)- Consumes messages from a Google Pub/Sub subscription. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Groovy Scripting**](groovy-scripting.md) - Runs a Groovy script to create BigConnect Pipelines records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Hadoop FS**](hadoop-fs.md) - Reads fully-written files from HDFS, BigConnect, Azure Data Lake Storage, or Azure HDInsight. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**HTTP Clien**t](http-client.md) - Reads data from a streaming HTTP resource URL.
* [**HTTP Server**](http-server.md) - Listens on an HTTP endpoint and processes the contents of all authorized HTTP POST and PUT requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**JavaScript Scripting**](javascript-scripting.md) - Runs a JavaScript script to create BigConnect Pipelines records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* [**JDBC Multitable Consumer**](jdbc-multitable-consumer.md) - Reads database data from multiple tables through a JDBC connection. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**JDBC Query Consumer**](jdbc-query-consumer.md) - Reads database data using a user-defined SQL query through a JDBC connection.
* [**JMS Consumer**](jms-consumer.md) - Reads messages from JMS.
* [**Jython Scripting**](jython-scripting.md) - Runs a Jython script to create BigConnect Pipelines records. Can create multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Kafka Consumer**](kafka-consumer.md) - Reads messages from a single Kafka topic.
* [**Kafka Multi Topic Consumer**](kafka-multitopic-consumer.md) - Reads messages from multiple Kafka topics. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**MQTT Subscriber**](mqtt-subscriber.md) - Subscribes to a topic on an MQTT broker to read messages from the broker.
* [**MySQL Binary Log**](mysql-binary-log.md) - Reads MySQL binary logs to generate change data capture records.
* [**Oracle Bulkload**](oracle-bulkload.md) - Reads data from multiple Oracle database tables, then stops the pipeline. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Oracle CDC Client**](oracle-cdc-client.md) - Reads LogMiner redo logs to generate change data capture records.
* [**PostgreSQL CDC Client**](postgresql-cdc-client.md) - Reads PostgreSQL WAL data to generate change data capture records.
* [**Redis Consumer**](redis-consumer.md) - Reads messages from Redis.
* [**REST Service**](rest-service.md) - Listens on an HTTP endpoint, parses the contents of all authorized requests, and sends responses back to the originating REST API. Creates multiple threads to enable parallel processing in a multithreaded pipeline. Use only in microservice pipelines.
* [**Salesforce**](salesforce.md) - Reads data from Salesforce.
* [**SFTP/FTP/FTPS Client**](sftp-ftp-ftps-client.md) - Reads files from an SFTP, FTP, or FTPS server.
* [**SQL Server CDC Client**](sql-server-cdc-client.md) - Reads data from Microsoft SQL Server CDC tables. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**SQL Server Change Tracking**](sql-server-change-tracking.md) - Reads data from Microsoft SQL Server change tracking tables and generates the latest version of each record. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**Start Pipelines**](start-pipelines.md) - Starts a pipeline.
* [**TCP Server**](tcp-server.md) - Listens at the specified ports and processes incoming data over TCP/IP connections. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**UDP Multithreaded Source**](udp-multithreaded-source.md) - Reads messages from one or more UDP ports. Creates multiple threads to enable parallel processing in a multithreaded pipeline.
* [**UDP Source**](udp-source.md) - Reads messages from one or more UDP ports.
* [**WebSocket Client**](websocket-client.md) - Reads data from a WebSocket server endpoint. Can send responses back to the origin system as part of a microservice pipeline.
* [**WebSocket Server**](websocket-server.md) - Listens on a WebSocket endpoint and processes the contents of all authorized WebSocket client requests. Creates multiple threads to enable parallel processing in a multithreaded pipeline. Can send responses back to the origin system as part of a microservice pipeline.

## Cluster Pipelines

In cluster pipelines, you can use the following origins:

* [**Hadoop FS**](hadoop-fs.md) - Reads data from HDFS,  BigConnect, Amazon S3, or other file systems using the Hadoop FileSystem interface.
* [**Kafka Consumer**](kafka-consumer.md) - Reads messages from Kafka. Use the cluster version of the origin.

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

| Origin                        | Description                                                                                                                                                                                                                                                                                                                                                                                |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [HTTP Client](http-client.md) | <ul><li>Initiates HTTP requests for an external system.</li><li>Processes data synchronously.</li><li>Processes JSON, text, and XML data.</li><li>Can process a range of HTTP requests.</li><li>Can be used in a pipeline with processors.</li></ul>                                                                                                                                       |
| [HTTP Server](http-server.md) | <ul><li>Listens for incoming HTTP requests and processes them while the sender waits for confirmation.</li><li>Processes data synchronously.</li><li>Creates multithreaded pipelines, thus suitable for high throughput of incoming data.</li><li>Processes virtually all data formats. Processes HTTP POST and PUT requests.</li><li>Can be used in a pipeline with processors.</li></ul> |

## Comparing UDP Source Origins <a href="#ariaid-title4" id="ariaid-title4"></a>

The UDP Source and UDP Multithreaded Source origins are very similar. The main differentiator is that the UDP Multithreaded Source can use multiple threads to process data within the pipeline.

The UDP Multithreaded Source has a processing queue that aids multithreaded processing. But use of this queue can slow processing under certain circumstances.

The following table describes some cases when you might want to use each origin:

| Origin                                                  | Ideally Used When                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [UDP Multithreaded Source](udp-multithreaded-source.md) | <ul><li>Epoll support enables the use of multiple receiver threads to pass data to the pipeline.</li><li>Complex pipeline requires longer processing time.</li></ul><p>or</p><ul><li>Lack of epoll support allows only a single receiver thread to pass data to the pipeline.</li><li>High volumes of data.</li></ul> |
| [UDP Source](udp-source.md)                             | <ul><li>Epoll support enables the use of multiple receiver threads to pass data to the pipeline.</li><li>Relatively simple pipeline enables speedy BigConnect Pipelines processing.</li></ul>                                                                                                                         |

## Comparing WebSocket Origins <a href="#ariaid-title5" id="ariaid-title5"></a>

We have two WebSocket origins, make sure to use the best one for your needs. Here's a quick breakdown of some key differences:

| Origin                                  | Description                                                                                                                                                                                                        |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [WebSocket Client](websocket-client.md) | <ul><li>Initiates a connection to a WebSocket server endpoint and then waits for the WebSocket server to push data.</li></ul>                                                                                      |
| [WebSocket Server](websocket-server.md) | <ul><li>Listens for incoming WebSocket requests and processes them while the sender waits for confirmation.</li><li>Creates multithreaded pipelines, thus suitable for high throughput of incoming data.</li></ul> |

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
