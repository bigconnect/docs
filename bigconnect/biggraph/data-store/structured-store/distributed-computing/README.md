# Distributed Computing

Ignite provides an API for distributing computations across cluster nodes in a balanced and fault-tolerant manner. You can submit individual tasks for execution as well as implement the MapReduce pattern with automatic task splitting.

### Getting the Compute Interface

The main entry point for running distributed computations is the compute interface, which can be obtained from an instance of `Ignite`.

```java
Ignite ignite = Ignition.start();

IgniteCompute compute = ignite.compute();
```

The compute interface provides methods for distributing different types of tasks over cluster nodes and running colocated computations.

### Specifying the Set of Nodes for Computations

Each instance of the compute interface is associated with a set of nodes on which the tasks are executed. When called without arguments, `ignite.compute()` returns the compute interface that is associated with all server nodes. To obtain an instance for a specific subset of nodes, use `Ignite.compute(ClusterGroup group)`. In the following example, the compute interface is bound to the remote nodes only, i.e. all nodes except for the one that runs this code.

```java
Ignite ignite = Ignition.start();

IgniteCompute compute = ignite.compute(ignite.cluster().forRemotes());
```

### Executing Tasks

Ignite provides three interfaces that can be implemented to represent a task and executed via the compute interface:

* `IgniteRunnable` — an extension of `java.lang.Runnable` that can be used to implement calculations that do not have input parameters and return no result.
* `IgniteCallable` — an extension of `java.util.concurrent.Callable` that returns a specific value.
* `IgniteClosure` — a functional interface that accepts a parameter and returns a value.

You can execute a task once (on one of the nodes) or broadcast it to all nodes.

#### Executing a Runnable Task

To execute a runnable task, use the `run(…​)` method of the compute interface. The task is sent to one of the nodes associated with the compute instance.

#### Executing a Callable Task

To execute a callable task, use the `call(…​)` method of the compute interface.

#### Executing an IgniteClosure

To execute an `IgniteClosure`, use the `apply(…​)` method of the compute interface. The method accepts a task and an input parameter for the task. The parameter is passed to the given `IgniteClosure` at the execution time.

#### Broadcasting a Task

The `broadcast()` method executes a task on _all nodes_ associated with the compute instance.

#### Asynchronous Execution

All methods described in the previous sections have asynchronous counterparts:

* `callAsync(…​)`
* `runAsync(…​)`
* `applyAsync(…​)`
* `broadcastAsync(…​)`

The asynchronous methods return an `IgniteFuture` that represents the result of the operation. In the following example, a collection of callable tasks is executed asynchronously.

```java
IgniteCompute compute = ignite.compute();

Collection<IgniteCallable<Integer>> calls = new ArrayList<>();

// Iterate through all words in the sentence and create callable jobs.
for (String word : "Count characters using a callable".split(" "))
    calls.add(word::length);

IgniteFuture<Collection<Integer>> future = compute.callAsync(calls);

future.listen(fut -> {
    // Total number of characters.
    int total = fut.get().stream().mapToInt(Integer::intValue).sum();

    System.out.println("Total number of characters: " + total);
});
```

### Task Execution Timeout

You can set a timeout for task execution. If the task does not finish within the given time frame, it be stopped and all jobs produced by this task are cancelled.

To execute a task with a timeout, use the `withTimeout(…​)` method of the compute interface. The method returns a compute interface that executes the first task given to it in a time-limited manner. Consequent tasks do not have a timeout: you need to call `withTimeout(…​)` for every task that should have a timeout.

### Sharing State Between Jobs on Local Node

It is often useful to share a state between different compute jobs executed on one node. For this purpose, there is a shared concurrent local map available on each node.

Node-local values are similar to thread local variables in that these values are not distributed and kept only on the local node. Node-local data can be used to share the state between compute jobs. It can also be used by deployed services.

In the following example, a job increments a node-local counter every time it executes on some node. As a result, the node-local counter on each node tells us how many times the job has executed on that node.

### Accessing Data from Computational Tasks

If your computational task needs to access the data stored in caches, you can do it via the instance of `Ignite`:

```java
public class MyCallableTask implements IgniteCallable<Integer> {

    @IgniteInstanceResource
    private Ignite ignite;

    @Override
    public Integer call() throws Exception {

        IgniteCache<Long, Person> cache = ignite.cache("person");

        // Get the data you need
        Person person = cache.get(1L);

        // do with the data what you need to do

        return 1;
    }
}
```

Note that the example shown above may not be the most effective way. The reason is that the person object that corresponds to key 1 may be located on a node that is different from the node where the task is executed. In this case, the object is fetched via network. This can be avoided by colocating the task with the data.
