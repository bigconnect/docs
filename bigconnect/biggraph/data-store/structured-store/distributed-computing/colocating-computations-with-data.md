# Colocating Computations with Data

Colocated computation is type of distributed data processing wherein the computational task you want to perform over a specific data set is sent to the nodes where the required data is located and only the results of the computations are sent back. This approach minimizes data transfer between nodes and can significantly reduce the task execution time.

Ignite provides several ways to perform colocated computations, all of which use the affinity function to determine the location of the data.

The compute interface provides the `affinityCall(…​)` and `affinityRun(…​)` methods that colocate a task with data either by key or by partition.

### Colocating by Key

To send a computational task to the node where a given key is located, use the following methods:

* `IgniteCompute.affinityCall(String cacheName, Object key, IgniteCallable<R> job)`
* `IgniteCompute.affinityRun(String cacheName, Object key, IgniteRunnable job)`

Ignite calls the configured affinity function to determine the location of the given key.

### Colocating by Partition

The `affinityCall(Collection<String> cacheNames, int partId, IgniteCallable job)` and `affinityRun(Collection<String> cacheNames, int partId, IgniteRunnable job)` send a given task to the node where the partition with a given ID is located. This is useful when you need to retrieve objects for multiple keys and you know that the keys belong to the same partition. In this case, you can create one task instead of multiple task for each key.

For example, let’s say you want to calculate the arithmetic mean of a specific field for a specific subset of keys. If you want to distribute the computation, you can group the keys by partitions and send each group of keys to the node where the partition is located to get the values. The number of groups and, therefore, the number of tasks is no more than the total number of partitions (default is 1024). Below is a code snippet that illustrates this example.

If you want to process all the data in the cache, you can iterate over all cache partitions and send tasks that process the data stored on each individual partition.

### Entry Processor

An entry processor is used to process cache entries on the nodes where they are stored and return the result of the processing. With an entry processor, you do not have to transfer the entire object to perform an operation with it, you can perform the operation remotely and only transfer the results.

If an entry processor sets the value for an entry that does not exist, the entry is added to the cache.

Entry processors are executed atomically within a lock on the given cache key.
