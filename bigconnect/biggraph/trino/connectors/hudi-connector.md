# Hudi connector

The Hudi connector enables querying [Hudi](https://hudi.apache.org/docs/overview/) tables.

### Requirements[#](https://trino.io/docs/current/connector/hudi.html#requirements) <a href="#requirements" id="requirements"></a>

To use the Hudi connector, you need:

* Hudi version 0.12.3 or higher.
* Network access from the Trino coordinator and workers to the Hudi storage.
* Access to a Hive metastore service (HMS).
* Network access from the Trino coordinator to the HMS.
* Data files stored in the Parquet file format on a supported file system.

### General configuration[#](https://trino.io/docs/current/connector/hudi.html#general-configuration) <a href="#general-configuration" id="general-configuration"></a>

To configure the Hive connector, create a catalog properties file `etc/catalog/example.properties` that references the `hudi` connector and defines the HMS to use with the `hive.metastore.uri` configuration property:

```
connector.name=hudi
hive.metastore.uri=thrift://example.net:9083
```

There are HMS configuration properties available for use with the Hudi connector. The connector recognizes Hudi tables synced to the metastore by the Hudi sync tool.

Additionally, following configuration properties can be set depending on the use-case:

| Property name                                       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Default             |
| --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `hudi.columns-to-hide`                              | List of column names that are hidden from the query output. It can be used to hide Hudi meta fields. By default, no fields are hidden.                                                                                                                                                                                                                                                                                                                                                                                                                         |                     |
| `hudi.parquet.use-column-names`                     | Access Parquet columns using names from the file. If disabled, then columns are accessed using the index. Only applicable to Parquet file format.                                                                                                                                                                                                                                                                                                                                                                                                              | `true`              |
| `hudi.split-generator-parallelism`                  | Number of threads to generate splits from partitions.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | `4`                 |
| `hudi.split-loader-parallelism`                     | Number of threads to run background split loader. A single background split loader is needed per query.                                                                                                                                                                                                                                                                                                                                                                                                                                                        | `4`                 |
| `hudi.size-based-split-weights-enabled`             | Unlike uniform splitting, size-based splitting ensures that each batch of splits has enough data to process. By default, it is enabled to improve performance.                                                                                                                                                                                                                                                                                                                                                                                                 | `true`              |
| `hudi.standard-split-weight-size`                   | The split size corresponding to the standard weight (1.0) when size-based split weights are enabled.                                                                                                                                                                                                                                                                                                                                                                                                                                                           | `128MB`             |
| `hudi.minimum-assigned-split-weight`                | Minimum weight that a split can be assigned when size-based split weights are enabled.                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | `0.05`              |
| `hudi.max-splits-per-second`                        | Rate at which splits are queued for processing. The queue is throttled if this rate limit is breached.                                                                                                                                                                                                                                                                                                                                                                                                                                                         | `Integer.MAX_VALUE` |
| `hudi.max-outstanding-splits`                       | Maximum outstanding splits in a batch enqueued for processing.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | `1000`              |
| `hudi.per-transaction-metastore-cache-maximum-size` | Maximum number of metastore data objects per transaction in the Hive metastore cache.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | `2000`              |
| `hudi.query-partition-filter-required`              | Set to `true` to force a query to use a partition column in the filter condition. The equivalent catalog session property is `query_partition_filter_required`. Enabling this property causes query failures if the partition column used in the filter condition doesn’t effectively reduce the number of data files read. Example: Complex filter expressions such as `id = 1 OR part_key = '100'` or `CAST(part_key AS INTEGER) % 2 = 0` are not recognized as partition filters, and queries using such expressions fail if the property is set to `true`. | `false`             |

### File system access configuration[#](https://trino.io/docs/current/connector/hudi.html#file-system-access-configuration) <a href="#file-system-access-configuration" id="file-system-access-configuration"></a>

The connector supports native, high-performance file system access to object storage systems:

* Object storage
* Azure Storage file system support
* Google Cloud Storage file system support
* S3 file system support

You must enable and configure the specific native file system access. If none is activated, the legacy support is used and must be configured.

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides read access to data in the Hudi table that has been synced to Hive metastore. The globally available and read operation statements are supported.

#### Basic usage examples <a href="#basic-usage-examples" id="basic-usage-examples"></a>

In the following example queries, `stock_ticks_cow` is the Hudi copy-on-write table referred to in the Hudi quickstart guide.

```
USE example.example_schema;

SELECT symbol, max(ts)
FROM stock_ticks_cow
GROUP BY symbol
HAVING symbol = 'GOOG';
```

```
  symbol   |        _col1         |
-----------+----------------------+
 GOOG      | 2018-08-31 10:59:00  |
(1 rows)
```

```
SELECT dt, symbol
FROM stock_ticks_cow
WHERE symbol = 'GOOG';
```

```
    dt      | symbol |
------------+--------+
 2018-08-31 |  GOOG  |
(1 rows)
```

```
SELECT dt, count(*)
FROM stock_ticks_cow
GROUP BY dt;
```

```
    dt      | _col1 |
------------+--------+
 2018-08-31 |  99  |
(1 rows)
```

#### Schema and table management[#](https://trino.io/docs/current/connector/hudi.html#schema-and-table-management) <a href="#schema-and-table-management" id="schema-and-table-management"></a>

Hudi supports two types of tables depending on how the data is indexed and laid out on the file system. The following table displays a support matrix of tables types and query types for the connector:

| Table type    | Supported query type   |
| ------------- | ---------------------- |
| Copy on write | Snapshot queries       |
| Merge on read | Read-optimized queries |

**Metadata tables**[**#**](https://trino.io/docs/current/connector/hudi.html#metadata-tables)

The connector exposes a metadata table for each Hudi table. The metadata table contains information about the internal structure of the Hudi table. You can query each metadata table by appending the metadata table name to the table name:

```
SELECT * FROM "test_table$timeline"
```

**`$TIMELINE` TABLE**[**#**](https://trino.io/docs/current/connector/hudi.html#timeline-table)

The `$timeline` table provides a detailed view of meta-data instants in the Hudi table. Instants are specific points in time.

You can retrieve the information about the timeline of the Hudi table `test_table` by using the following query:

```
SELECT * FROM "test_table$timeline"
```

```
 timestamp          | action  | state
--------------------+---------+-----------
8667764846443717831 | commit  | COMPLETED
7860805980949777961 | commit  | COMPLETED
```

The output of the query has the following columns:

| Name        | Type      | Description                                                                               |
| ----------- | --------- | ----------------------------------------------------------------------------------------- |
| `timestamp` | `VARCHAR` | Instant time is typically a timestamp when the actions performed.                         |
| `action`    | `VARCHAR` | [Type of action](https://hudi.apache.org/docs/concepts/#timeline) performed on the table. |
| `state`     | `VARCHAR` | Current state of the instant.                                                             |
