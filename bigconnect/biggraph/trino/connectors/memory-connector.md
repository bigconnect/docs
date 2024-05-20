# Memory connector

The Memory connector stores all data and metadata in RAM on workers and both are discarded when Trino restarts.

### Configuration[#](https://trino.io/docs/current/connector/memory.html#configuration) <a href="#configuration" id="configuration"></a>

To configure the Memory connector, create a catalog properties file `etc/catalog/example.properties` with the following contents:

```
connector.name=memory
memory.max-data-per-node=128MB
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

`memory.max-data-per-node` defines memory limit for pages stored in this connector per each node (default value is 128MB).

### Examples[#](https://trino.io/docs/current/connector/memory.html#examples) <a href="#examples" id="examples"></a>

Create a table using the Memory connector:

```
CREATE TABLE example.default.nation AS
SELECT * from tpch.tiny.nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Insert data into a table in the Memory connector:

```
INSERT INTO example.default.nation
SELECT * FROM tpch.tiny.nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Select from the Memory connector:

```
SELECT * FROM example.default.nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop table:

```
DROP TABLE example.default.nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Type mapping[#](https://trino.io/docs/current/connector/memory.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

Trino supports all data types used within the Memory schemas so no mapping is required.

### SQL support[#](https://trino.io/docs/current/connector/memory.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides read and write access to temporary data and metadata stored in memory. In addition to the globally available and read operation statements, the connector supports the following features:

* INSERT
* CREATE TABLE
* CREATE TABLE AS
* DROP TABLE
* CREATE SCHEMA
* DROP SCHEMA
* COMMENT
* Routine management

#### DROP TABLE# <a href="#drop-table" id="drop-table"></a>

Upon execution of a `DROP TABLE` operation, memory is not released immediately. It is instead released after the next write operation to the catalog.

### Dynamic filtering# <a href="#dynamic-filtering" id="dynamic-filtering"></a>

The Memory connector supports the dynamic filtering optimization. Dynamic filters are pushed into local table scan on worker nodes for broadcast joins.

#### Delayed execution for dynamic filters[#](https://trino.io/docs/current/connector/memory.html#delayed-execution-for-dynamic-filters) <a href="#delayed-execution-for-dynamic-filters" id="delayed-execution-for-dynamic-filters"></a>

For the Memory connector, a table scan is delayed until the collection of dynamic filters. This can be disabled by using the configuration property `memory.enable-lazy-dynamic-filtering` in the catalog file.

### Limitations[#](https://trino.io/docs/current/connector/memory.html#limitations) <a href="#limitations" id="limitations"></a>

* When one worker fails/restarts, all data that was stored in its memory is lost. To prevent silent data loss the connector throws an error on any read access to such corrupted table.
* When a query fails for any reason during writing to memory table, the table enters an undefined state. The table should be dropped and recreated manually. Reading attempts from the table may fail, or may return partial data.
* When the coordinator fails/restarts, all metadata about tables is lost. The tables remain on the workers, but become inaccessible.
* This connector does not work properly with multiple coordinators, since each coordinator has different metadata.
