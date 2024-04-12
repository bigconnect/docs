# Write partitioning properties

### `use-preferred-write-partitioning`[#](https://trino.io/docs/current/admin/properties-write-partitioning.html#use-preferred-write-partitioning) <a href="#use-preferred-write-partitioning" id="use-preferred-write-partitioning"></a>

* **Type:** boolean
* **Default value:** `true`
* **Session property:** `use_preferred_write_partitioning`

Enable preferred write partitioning. When set to `true`, each partition is written by a separate writer. For some connectors such as the Hive connector, only a single new file is written per partition, instead of multiple files. Partition writer assignments are distributed across worker nodes for parallel processing.

\
