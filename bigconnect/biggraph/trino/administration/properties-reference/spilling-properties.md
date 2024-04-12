# Spilling properties

These properties control Spill to disk.

### `spill-enabled`# <a href="#spill-enabled" id="spill-enabled"></a>

* **Type:** boolean
* **Default value:** `false`
* **Session property:** `spill_enabled`

Try spilling memory to disk to avoid exceeding memory limits for the query.

Spilling works by offloading memory to disk. This process can allow a query with a large memory footprint to pass at the cost of slower execution times. Spilling is supported for aggregations, joins (inner and outer), sorting, and window functions. This property does not reduce memory usage required for other join types.

### `spiller-spill-path`[#](https://trino.io/docs/current/admin/properties-spilling.html#spiller-spill-path) <a href="#spiller-spill-path" id="spiller-spill-path"></a>

* **Type:** string
* **No default value.** Must be set when spilling is enabled

Directory where spilled content is written. It can be a comma separated list to spill simultaneously to multiple directories, which helps to utilize multiple drives installed in the system.

It is not recommended to spill to system drives. Most importantly, do not spill to the drive on which the JVM logs are written, as disk overutilization might cause JVM to pause for lengthy periods, causing queries to fail.

### `spiller-max-used-space-threshold`[#](https://trino.io/docs/current/admin/properties-spilling.html#spiller-max-used-space-threshold) <a href="#spiller-max-used-space-threshold" id="spiller-max-used-space-threshold"></a>

* **Type:** double
* **Default value:** `0.9`

If disk space usage ratio of a given spill path is above this threshold, this spill path is not eligible for spilling.

### `spiller-threads`[#](https://trino.io/docs/current/admin/properties-spilling.html#spiller-threads) <a href="#spiller-threads" id="spiller-threads"></a>

* **Type:** integer
* **Default value:** `4`

Number of spiller threads. Increase this value if the default is not able to saturate the underlying spilling device (for example, when using RAID).

### `max-spill-per-node`[#](https://trino.io/docs/current/admin/properties-spilling.html#max-spill-per-node) <a href="#max-spill-per-node" id="max-spill-per-node"></a>

* **Type:** data size
* **Default value:** `100GB`

Max spill space to use by all queries on a single node. This only needs to be configured on worker nodes.

### `query-max-spill-per-node`[#](https://trino.io/docs/current/admin/properties-spilling.html#query-max-spill-per-node) <a href="#query-max-spill-per-node" id="query-max-spill-per-node"></a>

* **Type:** data size
* **Default value:** `100GB`

Max spill space to use by a single query on a single node. This only needs to be configured on worker nodes.

### `aggregation-operator-unspill-memory-limit`[#](https://trino.io/docs/current/admin/properties-spilling.html#aggregation-operator-unspill-memory-limit) <a href="#aggregation-operator-unspill-memory-limit" id="aggregation-operator-unspill-memory-limit"></a>

* **Type:** data size
* **Default value:** `4MB`

Limit for memory used for unspilling a single aggregation operator instance.

### `spill-compression-codec`[#](https://trino.io/docs/current/admin/properties-spilling.html#spill-compression-codec) <a href="#spill-compression-codec" id="spill-compression-codec"></a>

* **Type:** string
* **Allowed values:** `NONE`, `LZ4`, `ZSTD`
* **Default value:** `NONE`

The compression codec to use when spilling pages to disk.

### `spill-encryption-enabled`[#](https://trino.io/docs/current/admin/properties-spilling.html#spill-encryption-enabled) <a href="#spill-encryption-enabled" id="spill-encryption-enabled"></a>

* **Type:** boolean
* **Default value:** `false`

Enables using a randomly generated secret key (per spill file) to encrypt and decrypt data spilled to disk.
