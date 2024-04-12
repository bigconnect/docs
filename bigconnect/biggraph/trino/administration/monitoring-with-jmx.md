# Monitoring with JMX

Trino exposes a large number of different metrics via the Java Management Extensions (JMX).

You have to enable JMX by setting the ports used by the RMI registry and server in the config.properties file:

```
jmx.rmiregistry.port=9080
jmx.rmiserver.port=9081
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

* `jmx.rmiregistry.port`: Specifies the port for the JMX RMI registry. JMX clients should connect to this port.
* `jmx.rmiserver.port`: Specifies the port for the JMX RMI server. Trino exports many metrics, that are useful for monitoring via JMX.

Additionally configure a Java system property in the jvm.config with the RMI server port:

```
-Dcom.sun.management.jmxremote.rmi.port=9081
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

JConsole (supplied with the JDK), [VisualVM](https://visualvm.github.io/), and many other tools can be used to access the metrics in a client application. Many monitoring solutions support JMX. You can also use the [JMX connector](https://trino.io/docs/current/connector/jmx.html) and query the metrics using SQL.

Many of these JMX metrics are a complex metric object such as a `CounterStat` that has a collection of related metrics. For example, `InputPositions` has `InputPositions.TotalCount`, `InputPositions.OneMinute.Count`, and so on.

A small subset of the available metrics are described below.

### JVM[#](https://trino.io/docs/current/admin/jmx.html#jvm) <a href="#jvm" id="jvm"></a>

* Heap size: `java.lang:type=Memory:HeapMemoryUsage.used`
* Thread count: `java.lang:type=Threading:ThreadCount`

### Trino cluster and nodes[#](https://trino.io/docs/current/admin/jmx.html#trino-cluster-and-nodes) <a href="#trino-cluster-and-nodes" id="trino-cluster-and-nodes"></a>

* Active nodes: `trino.failuredetector:name=HeartbeatFailureDetector:ActiveCount`
* Free memory (general pool): `trino.memory:type=ClusterMemoryPool:name=general:FreeDistributedBytes`
* Cumulative count (since Trino started) of queries that ran out of memory and were killed: `trino.memory:name=ClusterMemoryManager:QueriesKilledDueToOutOfMemory`

### Trino queries[#](https://trino.io/docs/current/admin/jmx.html#trino-queries) <a href="#trino-queries" id="trino-queries"></a>

* Active queries currently executing or queued: `trino.execution:name=QueryManager:RunningQueries`
* Queries started: `trino.execution:name=QueryManager:StartedQueries.FiveMinute.Count`
* Failed queries from last 5 min (all): `trino.execution:name=QueryManager:FailedQueries.FiveMinute.Count`
* Failed queries from last 5 min (internal): `trino.execution:name=QueryManager:InternalFailures.FiveMinute.Count`
* Failed queries from last 5 min (external): `trino.execution:name=QueryManager:ExternalFailures.FiveMinute.Count`
* Failed queries (user): `trino.execution:name=QueryManager:UserErrorFailures.FiveMinute.Count`
* Execution latency (P50): `trino.execution:name=QueryManager:ExecutionTime.FiveMinutes.P50`
* Input data rate (P90): `trino.execution:name=QueryManager:WallInputBytesRate.FiveMinutes.P90`

### Trino tasks[#](https://trino.io/docs/current/admin/jmx.html#trino-tasks) <a href="#trino-tasks" id="trino-tasks"></a>

* Input data bytes: `trino.execution:name=SqlTaskManager:InputDataSize.FiveMinute.Count`
* Input rows: `trino.execution:name=SqlTaskManager:InputPositions.FiveMinute.Count`

### Connectors[#](https://trino.io/docs/current/admin/jmx.html#connectors) <a href="#connectors" id="connectors"></a>

Many connectors provide their own metrics. The metric names typically start with `trino.plugin`.
