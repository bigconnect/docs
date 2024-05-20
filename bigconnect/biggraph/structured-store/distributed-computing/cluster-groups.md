# Cluster Groups

The `ClusterGroup` interface represents a logical group of nodes, which can be used in many of Ignite’s APIs when you want to limit the scope of specific operations to a subset of nodes (instead of the whole cluster). For example, you may wish to deploy a service only on remote nodes or execute a job only on the set of nodes that have a specific attribute.

You can limit job execution, service deployment, messaging, events, and other tasks to run only on a specific set of node. For example, here is how to broadcast a job only to remote nodes (excluding the local node).

```java
Ignite ignite = Ignition.ignite();

IgniteCluster cluster = ignite.cluster();

// Get compute instance which will only execute
// over remote nodes, i.e. all the nodes except for this one.
IgniteCompute compute = ignite.compute(cluster.forRemotes());

// Broadcast to all remote nodes and print the ID of the node
// on which this closure is executing.
compute.broadcast(
        () -> System.out.println("Hello Node: " + ignite.cluster().localNode().id()));
```

For convenience, Ignite comes with a number of predefined cluster groups.

```java
IgniteCluster cluster = ignite.cluster();

// All nodes on which the cache with name "myCache" is deployed,
// either in client or server mode.
ClusterGroup cacheGroup = cluster.forCacheNodes("myCache");

// All data nodes responsible for caching data for "myCache".
ClusterGroup dataGroup = cluster.forDataNodes("myCache");

// All client nodes that can access "myCache".
ClusterGroup clientGroup = cluster.forClientNodes("myCache");
```
