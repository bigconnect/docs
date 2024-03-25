# Enrichment & Streaming

BigConnect Core is designed to react to events that happed whenever data is changed. It includes a streaming engine based on Kafka that streams data change events, as soon as they happen.&#x20;

Data change events are things like:

* Creation and deletion of entities, relationships and properties
* Property value changes
* Metadata value changes

The Kafka broker is enhanced to be able to query and aggregate data events as soon as they come in.

