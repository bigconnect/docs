# Resource management properties

### `query.max-cpu-time` <a href="#query-max-cpu-time" id="query-max-cpu-time"></a>

* **Type:** duration
* **Default value:** `1_000_000_000d`

This is the max amount of CPU time that a query can use across the entire cluster. Queries that exceed this limit are killed.

### `query.max-memory-per-node` <a href="#query-max-memory-per-node" id="query-max-memory-per-node"></a>

* **Type:** data size
* **Default value:** (JVM max memory \* 0.3)

This is the max amount of user memory a query can use on a worker. User memory is allocated during execution for things that are directly attributable to, or controllable by, a user query. For example, memory used by the hash tables built during execution, memory used during sorting, etc. When the user memory allocation of a query on any worker hits this limit, it is killed.

Warning

The sum of query.max-memory-per-node and memory.heap-headroom-per-node must be less than the maximum heap size in the JVM on the node. See JVM config.

Note

Does not apply for queries with task level retries enabled (`retry-policy=TASK`)

### `query.max-memory` <a href="#query-max-memory" id="query-max-memory"></a>

* **Type:** data size
* **Default value:** `20GB`

This is the max amount of user memory a query can use across the entire cluster. User memory is allocated during execution for things that are directly attributable to, or controllable by, a user query. For example, memory used by the hash tables built during execution, memory used during sorting, etc. When the user memory allocation of a query across all workers hits this limit it is killed.

Warning

query.max-total-memory must be greater than query.max-memory.

Note

Does not apply for queries with task level retries enabled (`retry-policy=TASK`)

### `query.max-total-memory` <a href="#query-max-total-memory" id="query-max-total-memory"></a>

* **Type:** data size
* **Default value:** (`query.max-memory` \* 2)

This is the max amount of memory a query can use across the entire cluster, including revocable memory. When the memory allocated by a query across all workers hits this limit it is killed. The value of `query.max-total-memory` must be greater than `query.max-memory`.

Warning

query.max-total-memory must be greater than query.max-memory.

Note

Does not apply for queries with task level retries enabled (`retry-policy=TASK`)

### `memory.heap-headroom-per-node` <a href="#memory-heap-headroom-per-node" id="memory-heap-headroom-per-node"></a>

* **Type:** data size
* **Default value:** (JVM max memory \* 0.3)

This is the amount of memory set aside as headroom/buffer in the JVM heap for allocations that are not tracked by Trino.

Warning

The sum of query.max-memory-per-node and memory.heap-headroom-per-node must be less than the maximum heap size in the JVM on the node. See JVM config.

### `exchange.deduplication-buffer-size` <a href="#exchange-deduplication-buffer-size" id="exchange-deduplication-buffer-size"></a>

* **Type:** data size
* **Default value:** `32MB`

Size of the buffer used for spooled data during Fault-tolerant execution.
