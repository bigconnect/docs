# System connector

The System connector provides information and metrics about the currently running Trino cluster. It makes this available via normal SQL queries.

### Configuration <a href="#configuration" id="configuration"></a>

The System connector doesn’t need to be configured: it is automatically available via a catalog named `system`.

### Using the System connector <a href="#using-the-system-connector" id="using-the-system-connector"></a>

List the available system schemas:

```
SHOW SCHEMAS FROM system;
```



List the tables in one of the schemas:

```
SHOW TABLES FROM system.runtime;
```



Query one of the tables:

```
SELECT * FROM system.runtime.nodes;
```



Kill a running query:

```
CALL system.runtime.kill_query(query_id => '20151207_215727_00146_tx3nr', message => 'Using too many resources');
```



### System connector tables <a href="#system-connector-tables" id="system-connector-tables"></a>

#### `metadata.catalogs` <a href="#metadata-catalogs" id="metadata-catalogs"></a>

The catalogs table contains the list of available catalogs.

#### `metadata.schema_properties` <a href="#metadata-schema-properties" id="metadata-schema-properties"></a>

The schema properties table contains the list of available properties that can be set when creating a new schema.

#### `metadata.table_properties` <a href="#metadata-table-properties" id="metadata-table-properties"></a>

The table properties table contains the list of available properties that can be set when creating a new table.

#### `metadata.materialized_views` <a href="#metadata-materialized-views" id="metadata-materialized-views"></a>

The materialized views table contains the following information about all materialized views:

| Column            | Description                                                                                                                                                                                              |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `catalog_name`    | Name of the catalog containing the materialized view.                                                                                                                                                    |
| `schema_name`     | Name of the schema in `catalog_name` containing the materialized view.                                                                                                                                   |
| `name`            | Name of the materialized view.                                                                                                                                                                           |
| `storage_catalog` | Name of the catalog used for the storage table backing the materialized view.                                                                                                                            |
| `storage_schema`  | Name of the schema in `storage_catalog` used for the storage table backing the materialized view.                                                                                                        |
| `storage_table`   | Name of the storage table backing the materialized view.                                                                                                                                                 |
| `freshness`       | Freshness of data in the storage table. Queries on the materialized view access the storage table if not `STALE`, otherwise the `definition` is used to access the underlying data in the source tables. |
| `last_fresh_time` | Date and time of the last refresh of the materialized view.                                                                                                                                              |
| `comment`         | User supplied text about the materialized view.                                                                                                                                                          |
| `definition`      | SQL query that defines the data provided by the materialized view.                                                                                                                                       |

#### `metadata.materialized_view_properties` <a href="#metadata-materialized-view-properties" id="metadata-materialized-view-properties"></a>

The materialized view properties table contains the list of available properties that can be set when creating a new materialized view.

#### `metadata.table_comments` <a href="#metadata-table-comments" id="metadata-table-comments"></a>

The table comments table contains the list of table comment.

#### `runtime.nodes` <a href="#runtime-nodes" id="runtime-nodes"></a>

The nodes table contains the list of visible nodes in the Trino cluster along with their status.

#### `runtime.optimizer_rule_stats` <a href="#runtime-optimizer-rule-stats" id="runtime-optimizer-rule-stats"></a>

The `optimizer_rule_stats` table contains the statistics for optimizer rule invocations during the query planning phase. The statistics are aggregated over all queries since the server start-up. The table contains information about invocation frequency, failure rates and performance for optimizer rules. For example, you can look at the multiplication of columns `invocations` and `average_time` to get an idea about which rules generally impact query planning times the most.

#### `runtime.queries` <a href="#runtime-queries" id="runtime-queries"></a>

The queries table contains information about currently and recently running queries on the Trino cluster. From this table you can find out the original query SQL text, the identity of the user who ran the query, and performance information about the query, including how long the query was queued and analyzed.

#### `runtime.tasks` <a href="#runtime-tasks" id="runtime-tasks"></a>

The tasks table contains information about the tasks involved in a Trino query, including where they were executed, and how many rows and bytes each task processed.

#### `runtime.transactions` <a href="#runtime-transactions" id="runtime-transactions"></a>

The transactions table contains the list of currently open transactions and related metadata. This includes information such as the create time, idle time, initialization parameters, and accessed catalogs.

### System connector procedures <a href="#system-connector-procedures" id="system-connector-procedures"></a>

runtime.kill\_query(_query\_id_, _message_)[#](https://trino.io/docs/current/connector/system.html#runtime.kill\_query)

Kill the query identified by `query_id`. The query failure message includes the specified `message`. `message` is optional.

### Type mapping <a href="#type-mapping" id="type-mapping"></a>

Trino supports all data types used within the System schemas so no mapping is required.

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides globally available and read operation statements to access Trino system data and metadata.
