# Load Balancing

Ignite automatically load balances jobs produced by a compute task as well as individual tasks submitted via the distributed computing API. Individual tasks submitted via `IgniteCompute.run(…​)` and other compute methods are treated as tasks producing a single job.

By default, Ignite uses a round-robin algorithm (`RoundRobinLoadBalancingSpi`), which distributes jobs in sequential order across the nodes specified for the compute task.

The load balancing algorithm is controlled by the `IgniteConfiguration.loadBalancingSpi` property.

### Round-Robin Load Balancing

`RoundRobinLoadBalancingSpi` iterates through the available nodes in a round-robin fashion and picks the next sequential node. The available nodes are defined when you get the compute instance through which you execute your tasks.

Round-Robin load balancing supports two modes of operation: per-task and global.

When configured in per-task mode, the implementation picks a random node at the beginning of every task execution and then sequentially iterates through all the nodes in the topology starting from that node. For cases when the split size of a task is equal to the number of nodes, this mode guarantees that all nodes will participate in job execution.

When configured in global mode, a single sequential queue of nodes is maintained for all tasks and the next node in the queue is picked every time. In this mode (unlike in per-task mode), it is possible that even if the split size of a task is equal to the number of nodes, some jobs within the same task will be assigned to the same node whenever multiple tasks are executing concurrently.

The global mode is used by default.

### Random and Weighted Load Balancing

`WeightedRandomLoadBalancingSpi` picks a random node from the list of available nodes. You can also optionally assign weights to nodes, so that nodes with larger weights will end up getting proportionally more jobs routed to them. By default all nodes get a weight of 10.

### Job Stealing

Quite often clusters are deployed across many computers some of which may be more powerful or under-utilized than others. Enabling `JobStealingCollisionSpi` helps avoid jobs being stuck at an over-utilized node, as they will be stolen by an under-utilized node.

`JobStealingCollisionSpi` supports job stealing from over-utilized nodes to under-utilized nodes. This SPI is especially useful if you have some jobs that complete quickly, while others are sitting in the waiting queue on over-utilized nodes. In such a case, the waiting jobs will be stolen from the slower node and moved to the fast/under-utilized node.

`JobStealingCollisionSpi` adopts a "late" load balancing technique, which allows reassigning a job from node A to node B after the job has been scheduled for execution on node A​.
