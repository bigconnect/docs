# MapReduce API

Ignite provides an API for performing simplified MapReduce operations. The MapReduce pattern is based on the assumption that the task that you want to execute can be split into multiple jobs (the mapping phase), with each job executed separately. The results produced by each job are aggregated into the final results (the reducing phase).

In a distributed system such as Ignite, the jobs are distributed between the nodes according to the preconfigured load balancing strategy and the results are aggregated on the node that submitted the task.

The MapReduce pattern is provided by the `ComputeTask` interface.

### Understanding Compute Task Interface

The `ComputeTask` interface provides a way to implement custom map and reduce logic. The interface has three methods: `map(…​)`, `result()`, and `reduce()`.

The `map()` method should be implemented to create the compute jobs based on the input parameter and map them to worker nodes. The method receives the collection of cluster nodes on which the task is to be run and the task’s input parameter. The method returns a map with jobs as keys and mapped worker nodes as values. The jobs are then sent to the mapped nodes and executed there.

The `result()` method is called after completion of each job and returns an instance of `ComputeJobResultPolicy` indicating how to proceed with the task. The method receives the results of the job and the list of all the job results received so far. The method may return one of the following values:

* `WAIT` - wait for all remaining jobs to complete (if any);
* `REDUCE` - immediately move to the reduce step, discarding all the remaining jobs and results not yet received;
* `FAILOVER` - failover the job to another node (see Fault Tolerance).

The `reduce()` method is called during the reduce step, when all the jobs have completed (or the `result()` method returned the `REDUCE` result policy for a particular job). The method receives a list with all completed results and returns the final result of the computation.

### Executing a Compute Task

To execute a compute task, call the `IgniteCompute.execute(…​)` method and pass the input parameter for the compute task as the last argument.

JavaC#/.NETC++

```java
Ignite ignite = Ignition.start();

IgniteCompute compute = ignite.compute();

int count = compute.execute(new CharacterCountTask(), "Hello Grid Enabled World!");
```

You can limit the execution of jobs to a subset of nodes by using a cluster group.

### Handling Job Failures

If a node crashes or becomes unavailable during a task execution, all jobs scheduled for the node are automatically sent to another available node (due to the built-in failover mechanism). However, if a job throws an exception, you can treat the job as failed and fail it over to another node for re-execution. To do this, return `FAILOVER` in the `result(…​)` method:

```java
@Override
public ComputeJobResultPolicy result(ComputeJobResult res, List<ComputeJobResult> rcvd) {
    IgniteException err = res.getException();

    if (err != null)
        return ComputeJobResultPolicy.FAILOVER;

    // If there is no exception, wait for all job results.
    return ComputeJobResultPolicy.WAIT;
}
```

### Compute Task Adapters

There are several helper classes that provide most commonly used implementations of the `result(…​)` and `map(…​)` methods.

* `ComputeTaskAdapter` — This class implements the `result()` method to return the `FAILOVER` policy if a job throws an exception and the `WAIT` policy otherwise. It means that this implementation will wait for all jobs to finish with a result.
* `ComputeTaskSplitAdapter` — This class extends `ComputeTaskAdapter` and implements the `map(…​)` method to automatically assign jobs to nodes. It introduces a new `split(…​)` method that implements the logic of producing jobs based on the input data.

### Distributed Task Session

For each task, Ignite creates a distributed session that holds information about the task and is visible to the task itself and to all jobs spawned by it. You can use this session to share attributes between jobs. Attributes can be assigned before or during job execution and become visible to other jobs in the same order in which they were set.

```java
@ComputeTaskSessionFullSupport
private static class TaskSessionAttributesTask extends ComputeTaskSplitAdapter<Object, Object> {

    @Override
    protected Collection<? extends ComputeJob> split(int gridSize, Object arg) {
        Collection<ComputeJob> jobs = new LinkedList<>();

        // Generate jobs by number of nodes in the grid.
        for (int i = 0; i < gridSize; i++) {
            jobs.add(new ComputeJobAdapter(arg) {
                // Auto-injected task session.
                @TaskSessionResource
                private ComputeTaskSession ses;

                // Auto-injected job context.
                @JobContextResource
                private ComputeJobContext jobCtx;

                @Override
                public Object execute() {
                    // Perform STEP1.
                    // ...

                    // Tell other jobs that STEP1 is complete.
                    ses.setAttribute(jobCtx.getJobId(), "STEP1");

                    // Wait for other jobs to complete STEP1.
                    for (ComputeJobSibling sibling : ses.getJobSiblings())
                        try {
                            ses.waitForAttribute(sibling.getJobId(), "STEP1", 0);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }

                    // Move on to STEP2.
                    // ...

                    return ...

                }
            });
        }
        return jobs;
    }

    @Override
    public Object reduce(List<ComputeJobResult> results) {
        // No-op.
        return null;
    }

}
```

### Compute Task Example

The following example demonstrates a simple character counting application that splits a given string into words and calculates the length of each word in an individual job. The jobs are distributed to all cluster nodes.

```java
public class ComputeTaskExample {
    public static class CharacterCountTask extends ComputeTaskSplitAdapter<String, Integer> {
        // 1. Splits the received string into words
        // 2. Creates a child job for each word
        // 3. Sends the jobs to other nodes for processing.
        @Override
        public List<ComputeJob> split(int gridSize, String arg) {
            String[] words = arg.split(" ");

            List<ComputeJob> jobs = new ArrayList<>(words.length);

            for (final String word : words) {
                jobs.add(new ComputeJobAdapter() {
                    @Override
                    public Object execute() {
                        System.out.println(">>> Printing '" + word + "' on from compute job.");

                        // Return the number of letters in the word.
                        return word.length();
                    }
                });
            }

            return jobs;
        }

        @Override
        public Integer reduce(List<ComputeJobResult> results) {
            int sum = 0;

            for (ComputeJobResult res : results)
                sum += res.<Integer>getData();

            return sum;
        }
    }

    public static void main(String[] args) {

        Ignite ignite = Ignition.start();

        IgniteCompute compute = ignite.compute();

        // Execute the task on the cluster and wait for its completion.
        int cnt = compute.execute(CharacterCountTask.class, "Hello Grid Enabled World!");

        System.out.println(">>> Total number of characters in the phrase is '" + cnt + "'.");
    }
}
```
