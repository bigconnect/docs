# Kafka

## GETTING STARTED

### Introduction <a href="#introduction" id="introduction"></a>

#### **What is event streaming?**

Event streaming is the digital equivalent of the human body's central nervous system. It is the technological foundation for the 'always-on' world where businesses are increasingly software-defined and automated, and where the user of software is more software.

Technically speaking, event streaming is the practice of capturing data in real-time from event sources like databases, sensors, mobile devices, cloud services, and software applications in the form of streams of events; storing these event streams durably for later retrieval; manipulating, processing, and reacting to the event streams in real-time as well as retrospectively; and routing the event streams to different destination technologies as needed. Event streaming thus ensures a continuous flow and interpretation of data so that the right information is at the right place, at the right time.

#### **What can I use event streaming for?**

Event streaming is applied to a wide variety of use cases across a plethora of industries and organizations. Its many examples include:

* To process payments and financial transactions in real-time, such as in stock exchanges, banks, and insurances.
* To track and monitor cars, trucks, fleets, and shipments in real-time, such as in logistics and the automotive industry.
* To continuously capture and analyze sensor data from IoT devices or other equipment, such as in factories and wind parks.
* To collect and immediately react to customer interactions and orders, such as in retail, the hotel and travel industry, and mobile applications.
* To monitor patients in hospital care and predict changes in condition to ensure timely treatment in emergencies.
* To connect, store, and make available data produced by different divisions of a company.
* To serve as the foundation for data platforms, event-driven architectures, and microservices.

#### **Apache Kafka® is an event streaming platform. What does that mean?**

Kafka combines three key capabilities so you can implement your use cases for event streaming end-to-end with a single battle-tested solution:

1. To **publish** (write) and **subscribe to** (read) streams of events, including continuous import/export of your data from other systems.
2. To **store** streams of events durably and reliably for as long as you want.
3. To **process** streams of events as they occur or retrospectively.

And all this functionality is provided in a distributed, highly scalable, elastic, fault-tolerant, and secure manner. Kafka can be deployed on bare-metal hardware, virtual machines, and containers, and on-premises as well as in the cloud. You can choose between self-managing your Kafka environments and using fully managed services offered by a variety of vendors.

#### **How does Kafka work in a nutshell?**

Kafka is a distributed system consisting of **servers** and **clients** that communicate via a high-performance [TCP network protocol](https://kafka.apache.org/protocol.html). It can be deployed on bare-metal hardware, virtual machines, and containers in on-premise as well as cloud environments.

**Servers**: Kafka is run as a cluster of one or more servers that can span multiple datacenters or cloud regions. Some of these servers form the storage layer, called the brokers. Other servers run Kafka Connect to continuously import and export data as event streams to integrate Kafka with your existing systems such as relational databases as well as other Kafka clusters. To let you implement mission-critical use cases, a Kafka cluster is highly scalable and fault-tolerant: if any of its servers fails, the other servers will take over their work to ensure continuous operations without any data loss.

**Clients**: They allow you to write distributed applications and microservices that read, write, and process streams of events in parallel, at scale, and in a fault-tolerant manner even in the case of network problems or machine failures. Kafka ships with some such clients included, which are augmented by [dozens of clients](https://cwiki.apache.org/confluence/display/KAFKA/Clients) provided by the Kafka community: clients are available for Java and Scala including the higher-level [Kafka Streams](https://kafka.apache.org/documentation/streams/) library, for Go, Python, C/C++, and many other programming languages as well as REST APIs.

#### **Main Concepts and Terminology**

An **event** records the fact that "something happened" in the world or in your business. It is also called record or message in the documentation. When you read or write data to Kafka, you do this in the form of events. Conceptually, an event has a key, value, timestamp, and optional metadata headers. Here's an example event:

* Event key: "Alice"
* Event value: "Made a payment of $200 to Bob"
* Event timestamp: "Jun. 25, 2020 at 2:06 p.m."

**Producers** are those client applications that publish (write) events to Kafka, and **consumers** are those that subscribe to (read and process) these events. In Kafka, producers and consumers are fully decoupled and agnostic of each other, which is a key design element to achieve the high scalability that Kafka is known for. For example, producers never need to wait for consumers. Kafka provides various guarantees such as the ability to process events exactly-once.

Events are organized and durably stored in **topics**. Very simplified, a topic is similar to a folder in a filesystem, and the events are the files in that folder. An example topic name could be "payments". Topics in Kafka are always multi-producer and multi-subscriber: a topic can have zero, one, or many producers that write events to it, as well as zero, one, or many consumers that subscribe to these events. Events in a topic can be read as often as needed—unlike traditional messaging systems, events are not deleted after consumption. Instead, you define for how long Kafka should retain your events through a per-topic configuration setting, after which old events will be discarded. Kafka's performance is effectively constant with respect to data size, so storing data for a long time is perfectly fine.

Topics are **partitioned**, meaning a topic is spread over a number of "buckets" located on different Kafka brokers. This distributed placement of your data is very important for scalability because it allows client applications to both read and write the data from/to many brokers at the same time. When a new event is published to a topic, it is actually appended to one of the topic's partitions. Events with the same event key (e.g., a customer or vehicle ID) are written to the same partition, and Kafka guarantees that any consumer of a given topic-partition will always read that partition's events in exactly the same order as they were written.

<figure><img src="https://kafka.apache.org/images/streams-and-tables-p1_p4.png" alt=""><figcaption><p>Figure: This example topic has four partitions P1–P4. Two different producer clients are publishing, independently from each other, new events to the topic by writing events over the network to the topic's partitions. Events with the same key (denoted by their color in the figure) are written to the same partition. Note that both producers can write to the same partition if appropriate.</p></figcaption></figure>

To make your data fault-tolerant and highly-available, every topic can be **replicated**, even across geo-regions or datacenters, so that there are always multiple brokers that have a copy of the data just in case things go wrong, you want to do maintenance on the brokers, and so on. A common production setting is a replication factor of 3, i.e., there will always be three copies of your data. This replication is performed at the level of topic-partitions.

This primer should be sufficient for an introduction. The [Design](https://kafka.apache.org/documentation/#design) section of the documentation explains Kafka's various concepts in full detail, if you are interested.

#### **Kafka APIs**

In addition to command line tooling for management and administration tasks, Kafka has five core APIs for Java and Scala:

* The **Admin API** to manage and inspect topics, brokers, and other Kafka objects.
* The **Producer API** to publish (write) a stream of events to one or more Kafka topics.
* The **Consumer API** to subscribe to (read) one or more topics and to process the stream of events produced to them.
* The **Kafka Streams API** to implement stream processing applications and microservices. It provides higher-level functions to process event streams, including transformations, stateful operations like aggregations and joins, windowing, processing based on event-time, and more. Input is read from one or more topics in order to generate output to one or more topics, effectively transforming the input streams to output streams.
* The **Kafka Connect API** to build and run reusable data import/export connectors that consume (read) or produce (write) streams of events from and to external systems and applications so they can integrate with Kafka. For example, a connector to a relational database like PostgreSQL might capture every change to a set of tables. However, in practice, you typically don't need to implement your own connectors because the Kafka community already provides hundreds of ready-to-use connectors.

### Use Cases <a href="#uses" id="uses"></a>

Here is a description of a few of the popular use cases for Apache Kafka®.&#x20;

#### **Messaging**

Kafka works well as a replacement for a more traditional message broker. Message brokers are used for a variety of reasons (to decouple processing from data producers, to buffer unprocessed messages, etc). In comparison to most messaging systems Kafka has better throughput, built-in partitioning, replication, and fault-tolerance which makes it a good solution for large scale message processing applications.

In our experience messaging uses are often comparatively low-throughput, but may require low end-to-end latency and often depend on the strong durability guarantees Kafka provides.

In this domain Kafka is comparable to traditional messaging systems such as [ActiveMQ](https://activemq.apache.org/) or [RabbitMQ](https://www.rabbitmq.com/).

#### **Website Activity Tracking**

The original use case for Kafka was to be able to rebuild a user activity tracking pipeline as a set of real-time publish-subscribe feeds. This means site activity (page views, searches, or other actions users may take) is published to central topics with one topic per activity type. These feeds are available for subscription for a range of use cases including real-time processing, real-time monitoring, and loading into Hadoop or offline data warehousing systems for offline processing and reporting.

Activity tracking is often very high volume as many activity messages are generated for each user page view.

#### **Metrics**

Kafka is often used for operational monitoring data. This involves aggregating statistics from distributed applications to produce centralized feeds of operational data.

#### **Log Aggregation**

Many people use Kafka as a replacement for a log aggregation solution. Log aggregation typically collects physical log files off servers and puts them in a central place (a file server or HDFS perhaps) for processing. Kafka abstracts away the details of files and gives a cleaner abstraction of log or event data as a stream of messages. This allows for lower-latency processing and easier support for multiple data sources and distributed data consumption. In comparison to log-centric systems like Scribe or Flume, Kafka offers equally good performance, stronger durability guarantees due to replication, and much lower end-to-end latency.

#### **Stream Processing**

Many users of Kafka process data in processing pipelines consisting of multiple stages, where raw input data is consumed from Kafka topics and then aggregated, enriched, or otherwise transformed into new topics for further consumption or follow-up processing. For example, a processing pipeline for recommending news articles might crawl article content from RSS feeds and publish it to an "articles" topic; further processing might normalize or deduplicate this content and publish the cleansed article content to a new topic; a final processing stage might attempt to recommend this content to users. Such processing pipelines create graphs of real-time data flows based on the individual topics. Starting in 0.10.0.0, a light-weight but powerful stream processing library called [Kafka Streams](https://kafka.apache.org/documentation/streams) is available in Apache Kafka to perform such data processing as described above. Apart from Kafka Streams, alternative open source stream processing tools include [Apache Storm](https://storm.apache.org/) and [Apache Samza](https://samza.apache.org/).

#### **Event Sourcing**

[Event sourcing](https://martinfowler.com/eaaDev/EventSourcing.html) is a style of application design where state changes are logged as a time-ordered sequence of records. Kafka's support for very large stored log data makes it an excellent backend for an application built in this style.

#### **Commit Log**

Kafka can serve as a kind of external commit-log for a distributed system. The log helps replicate data between nodes and acts as a re-syncing mechanism for failed nodes to restore their data. The log compaction feature in Kafka helps support this usage. In this usage Kafka is similar to [Apache BookKeeper](https://bookkeeper.apache.org/) project.

### Docker <a href="#docker" id="docker"></a>

**Introduction**

[Docker](https://www.docker.com/) is a popular container runtime. Docker images for Apache Kafka can be found on [Docker Hub](https://hub.docker.com/r/apache/kafka) and are available from version 3.7.0.

**Getting the kafka docker image**

Docker image can be pulled from Docker Hub using the following command:-

```bash
$ docker pull apache/kafka:3.7.0
```

If you want to fetch the latest version of the docker image use following command:-

```bash
$ docker pull apache/kafka:latest
```

**Start kafka with default configs**

Run docker image on default port 9092:-

```bash
$ docker run -p 9092:9092 apache/kafka:3.7.0
```

## APIS

Kafka includes five core apis:

1. The **Producer API** allows applications to send streams of data to topics in the Kafka cluster.
2. The **Consumer API** allows applications to read streams of data from topics in the Kafka cluster.
3. The **Streams API** allows transforming streams of data from input topics to output topics.
4. The **Connect API** allows implementing connectors that continually pull from some source system or application into Kafka or push from Kafka into some sink system or application.
5. The **Admin API** allows managing and inspecting topics, brokers, and other Kafka objects.

Kafka exposes all its functionality over a language independent protocol which has clients available in many programming languages. However only the Java clients are maintained as part of the main Kafka project, the others are available as independent open source projects. A list of non-Java clients is available [here](https://cwiki.apache.org/confluence/display/KAFKA/Clients).

### Producer API <a href="#producerapi" id="producerapi"></a>

The Producer API allows applications to send streams of data to topics in the Kafka cluster.

Examples showing how to use the producer are given in the [javadocs](https://kafka.apache.org/37/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html).

To use the producer, you can use the following maven dependency:

```xml
<dependency>
	<groupId>org.apache.kafka</groupId>
	<artifactId>kafka-clients</artifactId>
	<version>3.7.0</version>
</dependency>
```

### Consumer API <a href="#consumerapi" id="consumerapi"></a>

The Consumer API allows applications to read streams of data from topics in the Kafka cluster.

Examples showing how to use the consumer are given in the [javadocs](https://kafka.apache.org/37/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html).

To use the consumer, you can use the following maven dependency:

```xml
<dependency>
	<groupId>org.apache.kafka</groupId>
	<artifactId>kafka-clients</artifactId>
	<version>3.7.0</version>
</dependency>
```

### Streams API <a href="#streamsapi" id="streamsapi"></a>

The Streams API allows transforming streams of data from input topics to output topics.

Examples showing how to use this library are given in the [javadocs](https://kafka.apache.org/37/javadoc/index.html?org/apache/kafka/streams/KafkaStreams.html)

Additional documentation on using the Streams API is available [here](https://kafka.apache.org/37/documentation/streams).

To use Kafka Streams you can use the following maven dependency:

```xml
<dependency>
	<groupId>org.apache.kafka</groupId>
	<artifactId>kafka-streams</artifactId>
	<version>3.7.0</version>
</dependency>
```

When using Scala you may optionally include the `kafka-streams-scala` library. Additional documentation on using the Kafka Streams DSL for Scala is available [in the developer guide](https://kafka.apache.org/37/documentation/streams/developer-guide/dsl-api.html#scala-dsl).

To use Kafka Streams DSL for Scala for Scala 2.13 you can use the following maven dependency:

```xml
<dependency>
	<groupId>org.apache.kafka</groupId>
	<artifactId>kafka-streams-scala_2.13</artifactId>
	<version>3.7.0</version>
</dependency>
```

### Connect API <a href="#connectapi" id="connectapi"></a>

The Connect API allows implementing connectors that continually pull from some source data system into Kafka or push from Kafka into some sink data system.

Many users of Connect won't need to use this API directly, though, they can use pre-built connectors without needing to write any code. Additional information on using Connect is available [here](https://kafka.apache.org/documentation.html#connect).

Those who want to implement custom connectors can see the [javadoc](https://kafka.apache.org/37/javadoc/index.html?org/apache/kafka/connect).

### Admin API <a href="#adminapi" id="adminapi"></a>

The Admin API supports managing and inspecting topics, brokers, acls, and other Kafka objects.

To use the Admin API, add the following Maven dependency:

```xml
<dependency>
	<groupId>org.apache.kafka</groupId>
	<artifactId>kafka-clients</artifactId>
	<version>3.7.0</version>
</dependency>
```

For more information about the Admin APIs, see the [javadoc](https://kafka.apache.org/37/javadoc/index.html?org/apache/kafka/clients/admin/Admin.html).
