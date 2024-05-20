# Streaming

BigConnect Core is designed to react to events that happed whenever data is changed. It includes a streaming engine based on Kafka that streams data change events, as soon as they happen.&#x20;

Data change events are things like:

* Creation and deletion of entities, relationships and properties
* Property value changes
* Metadata value changes

BigConnect Core extends Kafka with some enterprise-level features as follows:

* [Encrypting data at rest](broken-reference)
* [User Interface](https://docs.kafka-ui.provectus.io/overview/features) to mage Kafka clusters&#x20;
* [ksqlDB](https://github.com/confluentinc/ksql) for building stream processing applications on top of Apache Kafka
