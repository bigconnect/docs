# Streaming

BigConnect Core is designed to react to events that happed whenever data is changed. It includes a streaming engine based on Kafka that streams data change events, as soon as they happen.&#x20;

Data change events are things like:

* Creation and deletion of entities, relationships and properties
* Property value changes
* Metadata value changes

BigConnect Core extends Kafka with some enterprise-level features as follows:

* [Encrypting data at rest](streaming/security.md#message-encryption)
* [User Interface](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/970/bigconnect/biggraph/streaming/kafka-ui) to mage Kafka clusters&#x20;
* [ksqlDB](https://github.com/confluentinc/ksql) for building stream processing applications on top of Apache Kafka
