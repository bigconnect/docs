# Job Scheduling

When jobs arrive at the destination node, they are submitted to a thread pool and scheduled for execution in random order. However, you can change job ordering by configuring `CollisionSpi`. The `CollisionSpi` interface provides a way to control how jobs are scheduled for processing on each node.

Ignite provides several implementations of the `CollisionSpi` interface:

* `FifoQueueCollisionSpi` — simple FIFO ordering in multiple threads. This implementation is used by default;
* `PriorityQueueCollisionSpi` — priority ordering;
* `JobStealingFailoverSpi` — use this implementation to enable job stealing.

To enable a specific collision spi, change the `IgniteConfiguration.collisionSpi` property.

### FIFO Ordering

`FifoQueueCollisionSpi` provides FIFO ordering of jobs as they arrive. The jobs are executed in multiple threads. The number of threads is controlled by the `parallelJobsNumber` parameter. The default value equals 2 times the number of processor cores.

### Priority Ordering

Use `PriorityQueueCollisionSpi` to assign priorities to individual jobs, so that jobs with higher priority are executed ahead of lower priority jobs. You can also specify the number of threads to process jobs.

Task priorities are set in the task session via the `grid.task.priority` attribute. If no priority is assigned to a task, then the default priority of 0 is used.
