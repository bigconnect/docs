# Asynchronous Message Processing

Asynchronous messaging is a communication method wherein the system puts a message in a message queue and does not require an immediate response to continue processing. Asynchronous messaging is useful for the following:

* Delegate the request to some external system for processing
* Ensure delivery of a message to an external system
* Throttle message rates between two systems
* Batch processing of messages

Note the following about asynchronous message processing:

* Asynchronous messaging solves the problem of intermittent connectivity. The message receiving party does not need to be online to receive the message as the message is stored in a middle layer. This allows the receiver to retrieve the message when it comes online.
* Message consumers do not need to know about the message publishers. They can operate independently.

Disadvantages of asynchronous messaging includes the additional component of a message broker or transfer agent to ensure the message is received. This may affect both performance and reliability. There are various levels of message delivery reliability grantees from publisher to broker and from broker to subscriber. Wire level protocols like AMQP and MQTT can provide those.
