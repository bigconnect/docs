# Overview

A high level overview of a production architecture is presented below. This is an ideal deployment scenario, with dedicated machines for each Explorer (Web Console), Data Workers, Job Workers, Queues and Data Ingestion.

![](<../../.gitbook/assets/image (154).png>)

There are different types of BigConnect Core nodes:

* **Data Worker** nodes usually handle data enrichment
* **Long Running Process Worker** nodes usually handle long running processes (such as running Machine Learning models or Graph algorithms) or scheduled tasks
* **External Worker** nodes that handle data ingestion or processing from potentially rate-limited external sources
* **Explorer** nodes serve the BigConnect Explorer application
* **Graph Engine** is BigGraph
* **Bolt Server** **nodes** execute Cypher queries

Each running node can be a combination of one or more types. This means that in a very simple deployment, one running node can be used to serve the Explorer, to run data worker plugins and job worker plugins, whereas in a typical production deployment you will have nodes that handle specific tasks and are also distributed across many machines.

Because nodes need to talk to each-other, each node instance can connect to a WorkQueue to publish and subscribe to messages. The current implementation offers an in-memory work queue for simple deployments and a RabbitMQ work queue for production deployments. Other types of WorkQueues can be easily implemented as well (eg. Apache Kafka, AWS Simple Queue Service etc.).

## The StorageEngine

Handles the persistence of data and provides operations for adding, updating, deleting and querying stored data. It provides a graph-like structure based on **Vertices**, **Edges** and _**multi-valued**_** Properties** that can be applied to Vertices or Edges.&#x20;

Property Graphs have been around for a while and the technology offering has grown consistently in the last years. BigConnect however brings two new important features: **multi-valued properties** and **fine-grained security**.

It makes a clear distinction between storing data and querying data, because not all storage containers can also retrieve the data in a timely manner. This is a common architecture for Property Graphs where a search index is used to perform fast queries.
