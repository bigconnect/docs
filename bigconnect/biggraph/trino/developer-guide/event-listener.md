# Event listener

Trino supports custom event listeners that are invoked for the following events:

* Query creation
* Query completion (success or failure)
* Split completion (success or failure)

Event details include session, query execution, resource utilization, timeline, and more.

This functionality enables development of custom logging, debugging and performance analysis plugins.

### Implementation[#](https://trino.io/docs/current/develop/event-listener.html#implementation) <a href="#implementation" id="implementation"></a>

`EventListenerFactory` is responsible for creating an `EventListener` instance. It also defines an `EventListener` name which is used by the administrator in a Trino configuration. Implementations of `EventListener` implement methods for the event types they are interested in handling.

The implementation of `EventListener` and `EventListenerFactory` must be wrapped as a plugin and installed on the Trino cluster.

### Configuration[#](https://trino.io/docs/current/develop/event-listener.html#configuration) <a href="#configuration" id="configuration"></a>

After a plugin that implements `EventListener` and `EventListenerFactory` has been installed on the coordinator, it is configured using an `etc/event-listener.properties` file. All of the properties other than `event-listener.name` are specific to the `EventListener` implementation.

The `event-listener.name` property is used by Trino to find a registered `EventListenerFactory` based on the name returned by `EventListenerFactory.getName()`. The remaining properties are passed as a map to `EventListenerFactory.create()`.

Example configuration file:

```
event-listener.name=custom-event-listener
custom-property1=custom-value1
custom-property2=custom-value2
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Multiple event listeners[#](https://trino.io/docs/current/develop/event-listener.html#multiple-event-listeners) <a href="#multiple-event-listeners" id="multiple-event-listeners"></a>

Trino supports multiple instances of the same or different event listeners. Install and configure multiple instances by setting `event-listener.config-files` in [Config properties](https://trino.io/docs/current/installation/deployment.html#config-properties) to a comma-separated list of the event listener configuration files:

```
event-listener.config-files=etc/event-listener.properties,etc/event-listener-second.properties
```
