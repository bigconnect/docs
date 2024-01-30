# Control Character Removal

You can use several stages to remove control characters - such as escape or end-of-transmission characters - from data. Remove control characters to avoid creating invalid records.

When Data Pipelines removes control characters, it removes ASCII character codes 0-31 and 127 with the following exceptions:

* 9 - Tab
* 10 - Line feed
* 13 - Carriage return

You can use the **Ignore Ctrl Characters** property in stages where it's available.

You can use the Ignore Ctrl Characters property in the following stages to remove control characters:

* Amazon S3 origin
* Amazon SQS Consumer origin
* Azure Data Lake Storage Gen1 origin
* Azure Data Lake Storage Gen2 origin
* Azure IoT/Event Hub Consumer origin
* CoAP Server origin
* Directory origin
* File Tail origin
* Google Cloud Storage origin
* Google Pub/Sub Subscriber origin
* gRPC Client origin
* Hadoop FS origin
* Hadoop FS Standalone origin
* HTTP Client origin
* HTTP Server origin
* JMS Consumer origin
* Kafka Consumer origin
* Kafka Multitopic Consumer origin
* Kinesis Consumer origin
* MapR FS origin
* MapR FS Standalone origin
* MapR Multitopic Streams Consumer origin
* MapR Streams Consumer origin
* MQTT Subscriber origin
* Pulsar Consumer origin
* RabbitMQ Consumer origin
* Redis Consumer origin
* REST Service origin
* SFTP/FTP/FPTS Client origin
* TCP Server origin
* WebSocket Client origin
* WebSocket Server origin
* Data Parser processor
* HTTP Client processor
* JSON Parser processor
* Log Parser processor
* XML Parser processor

