# BigQuery connector

The BigQuery connector allows querying the data stored in BigQuery. This can be used to join data between different systems like BigQuery and Hive. The connector uses the BigQuery Storage API to read the data from the tables.

### BigQuery Storage API <a href="#bigquery-storage-api" id="bigquery-storage-api"></a>

The Storage API streams data in parallel directly from BigQuery via gRPC without using Google Cloud Storage as an intermediary. It has a number of advantages over using the previous export-based read flow that should generally lead to better read performance:

**Direct Streaming**

It does not leave any temporary files in Google Cloud Storage. Rows are read directly from BigQuery servers using an Avro wire format.

**Column Filtering**

The new API allows column filtering to only read the data you are interested in. Backed by a columnar datastore, it can efficiently stream data without reading all columns.

**Dynamic Sharding**

The API rebalances records between readers until they all complete. This means that all Map phases will finish nearly concurrently. See this blog article on how dynamic sharding is similarly used in Google Cloud Dataflow.

### Requirements[#](https://trino.io/docs/current/connector/bigquery.html#requirements) <a href="#requirements" id="requirements"></a>

To connect to BigQuery, you need:

* To enable the BigQuery Storage Read API.
* Network access from your Trino coordinator and workers to the Google Cloud API service endpoint. This endpoint uses HTTPS, or port 443.
* To configure BigQuery so that the Trino coordinator and workers have permissions in BigQuery.
*   To set up authentication. Your authentiation options differ depending on whether you are using Dataproc/Google Compute Engine (GCE) or not.

    **On Dataproc/GCE** the authentication is done from the machine’s role.

    **Outside Dataproc/GCE** you have 3 options:

    * Use a service account JSON key and `GOOGLE_APPLICATION_CREDENTIALS` as described in the Google Cloud authentication getting started guide.
    * Set `bigquery.credentials-key` in the catalog properties file. It should contain the contents of the JSON file, encoded using base64.
    * Set `bigquery.credentials-file` in the catalog properties file. It should point to the location of the JSON file.

### Configuration <a href="#configuration" id="configuration"></a>

To configure the BigQuery connector, create a catalog properties file in `etc/catalog` named `example.properties`, to mount the BigQuery connector as the `example` catalog. Create the file with the following contents, replacing the connection properties as appropriate for your setup:

```
connector.name=bigquery
bigquery.project-id=<your Google Cloud Platform project id>
```

#### Multiple GCP projects[#](https://trino.io/docs/current/connector/bigquery.html#multiple-gcp-projects) <a href="#multiple-gcp-projects" id="multiple-gcp-projects"></a>

The BigQuery connector can only access a single GCP project.Thus, if you have data in multiple GCP projects, You need to create several catalogs, each pointing to a different GCP project. For example, if you have two GCP projects, one for the sales and one for analytics, you can create two properties files in `etc/catalog` named `sales.properties` and `analytics.properties`, both having `connector.name=bigquery` but with different `project-id`. This will create the two catalogs, `sales` and `analytics` respectively.

#### Configuring partitioning[#](https://trino.io/docs/current/connector/bigquery.html#configuring-partitioning) <a href="#configuring-partitioning" id="configuring-partitioning"></a>

By default the connector creates one partition per 400MB in the table being read (before filtering). This should roughly correspond to the maximum number of readers supported by the BigQuery Storage API. This can be configured explicitly with the `bigquery.parallelism` property. BigQuery may limit the number of partitions based on server constraints.

#### Arrow serialization support[#](https://trino.io/docs/current/connector/bigquery.html#arrow-serialization-support) <a href="#arrow-serialization-support" id="arrow-serialization-support"></a>

This is an experimental feature which introduces support for using Apache Arrow as the serialization format when reading from BigQuery. Please note there are a few caveats:

* Using Apache Arrow serialization is disabled by default. In order to enable it, set the `bigquery.experimental.arrow-serialization.enabled` configuration property to `true` and add `--add-opens=java.base/java.nio=ALL-UNNAMED` to the Trino [JVM config](https://trino.io/docs/current/installation/deployment.html#jvm-config).

#### Reading from views[#](https://trino.io/docs/current/connector/bigquery.html#reading-from-views) <a href="#reading-from-views" id="reading-from-views"></a>

The connector has a preliminary support for reading from [BigQuery views](https://cloud.google.com/bigquery/docs/views-intro). Please note there are a few caveats:

* Reading from views is disabled by default. In order to enable it, set the `bigquery.views-enabled` configuration property to `true`.
* BigQuery views are not materialized by default, which means that the connector needs to materialize them before it can read them. This process affects the read performance.
* The materialization process can also incur additional costs to your BigQuery bill.
* By default, the materialized views are created in the same project and dataset. Those can be configured by the optional `bigquery.view-materialization-project` and `bigquery.view-materialization-dataset` properties, respectively. The service account must have write permission to the project and the dataset in order to materialize the view.

#### Configuration properties[#](https://trino.io/docs/current/connector/bigquery.html#configuration-properties) <a href="#configuration-properties" id="configuration-properties"></a>

| Property                                            | Description                                                                                                                                                                                               | Default                                                                                                            |
| --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `bigquery.project-id`                               | The Google Cloud Project ID where the data reside                                                                                                                                                         | Taken from the service account                                                                                     |
| `bigquery.parent-project-id`                        | The project ID Google Cloud Project to bill for the export                                                                                                                                                | Taken from the service account                                                                                     |
| `bigquery.parallelism`                              | The number of partitions to split the data into                                                                                                                                                           | The number of executors                                                                                            |
| `bigquery.views-enabled`                            | Enables the connector to read from views and not only tables. Please read [this section](https://trino.io/docs/current/connector/bigquery.html#bigquery-reading-from-views) before enabling this feature. | `false`                                                                                                            |
| `bigquery.view-expire-duration`                     | Expire duration for the materialized view.                                                                                                                                                                | `24h`                                                                                                              |
| `bigquery.view-materialization-project`             | The project where the materialized view is going to be created                                                                                                                                            | The view’s project                                                                                                 |
| `bigquery.view-materialization-dataset`             | The dataset where the materialized view is going to be created                                                                                                                                            | The view’s dataset                                                                                                 |
| `bigquery.skip-view-materialization`                | Use REST API to access views instead of Storage API. BigQuery `BIGNUMERIC` and `TIMESTAMP` types are unsupported.                                                                                         | `false`                                                                                                            |
| `bigquery.views-cache-ttl`                          | Duration for which the materialization of a view will be cached and reused. Set to `0ms` to disable the cache.                                                                                            | `15m`                                                                                                              |
| `bigquery.metadata.cache-ttl`                       | Duration for which metadata retrieved from BigQuery is cached and reused. Set to `0ms` to disable the cache.                                                                                              | `0ms`                                                                                                              |
| `bigquery.max-read-rows-retries`                    | The number of retries in case of retryable server issues                                                                                                                                                  | `3`                                                                                                                |
| `bigquery.credentials-key`                          | The base64 encoded credentials key                                                                                                                                                                        | None. See the [requirements](https://trino.io/docs/current/connector/bigquery.html#bigquery-requirements) section. |
| `bigquery.credentials-file`                         | The path to the JSON credentials file                                                                                                                                                                     | None. See the [requirements](https://trino.io/docs/current/connector/bigquery.html#bigquery-requirements) section. |
| `bigquery.case-insensitive-name-matching`           | Match dataset and table names case-insensitively                                                                                                                                                          | `false`                                                                                                            |
| `bigquery.query-results-cache.enabled`              | Enable query results cache                                                                                                                                                                                | `false`                                                                                                            |
| `bigquery.experimental.arrow-serialization.enabled` | Enable using Apache Arrow serialization when reading data from BigQuery. Please read this section before enabling this feature.                                                                           | `false`                                                                                                            |
| `bigquery.rpc-proxy.enabled`                        | Use a proxy for communication with BigQuery.                                                                                                                                                              | `false`                                                                                                            |
| `bigquery.rpc-proxy.uri`                            | Proxy URI to use if connecting through a proxy.                                                                                                                                                           |                                                                                                                    |
| `bigquery.rpc-proxy.username`                       | Proxy user name to use if connecting through a proxy.                                                                                                                                                     |                                                                                                                    |
| `bigquery.rpc-proxy.password`                       | Proxy password to use if connecting through a proxy.                                                                                                                                                      |                                                                                                                    |
| `bigquery.rpc-proxy.keystore-path`                  | Keystore containing client certificates to present to proxy if connecting through a proxy. Only required if proxy uses mutual TLS.                                                                        |                                                                                                                    |
| `bigquery.rpc-proxy.keystore-password`              | Password of the keystore specified by `bigquery.rpc-proxy.keystore-path`.                                                                                                                                 |                                                                                                                    |
| `bigquery.rpc-proxy.truststore-path`                | Truststore containing certificates of the proxy server if connecting through a proxy.                                                                                                                     |                                                                                                                    |
| `bigquery.rpc-proxy.truststore-password`            | Password of the truststore specified by `bigquery.rpc-proxy.truststore-path`.                                                                                                                             |                                                                                                                    |

### Type mapping[#](https://trino.io/docs/current/connector/bigquery.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

Because Trino and BigQuery each support types that the other does not, this connector modifies some types when reading or writing data. Data types may not map the same way in both directions between Trino and the data source. Refer to the following sections for type mapping in each direction.

#### BigQuery type to Trino type mapping[#](https://trino.io/docs/current/connector/bigquery.html#bigquery-type-to-trino-type-mapping) <a href="#bigquery-type-to-trino-type-mapping" id="bigquery-type-to-trino-type-mapping"></a>

The connector maps BigQuery types to the corresponding Trino types according to the following table:

| BigQuery type | Trino type                    | Notes                                                                                                        |
| ------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `BOOLEAN`     | `BOOLEAN`                     |                                                                                                              |
| `INT64`       | `BIGINT`                      | `INT`, `SMALLINT`, `INTEGER`, `BIGINT`, `TINYINT`, and `BYTEINT` are aliases for `INT64` in BigQuery.        |
| `FLOAT64`     | `DOUBLE`                      |                                                                                                              |
| `NUMERIC`     | `DECIMAL(P,S)`                | The default precision and scale of `NUMERIC` is `(38, 9)`.                                                   |
| `BIGNUMERIC`  | `DECIMAL(P,S)`                | Precision > 38 is not supported. The default precision and scale of `BIGNUMERIC` is `(77, 38)`.              |
| `DATE`        | `DATE`                        |                                                                                                              |
| `DATETIME`    | `TIMESTAMP(6)`                |                                                                                                              |
| `STRING`      | `VARCHAR`                     |                                                                                                              |
| `BYTES`       | `VARBINARY`                   |                                                                                                              |
| `TIME`        | `TIME(6)`                     |                                                                                                              |
| `TIMESTAMP`   | `TIMESTAMP(6) WITH TIME ZONE` | Time zone is UTC                                                                                             |
| `GEOGRAPHY`   | `VARCHAR`                     | In [Well-known text (WKT)](https://wikipedia.org/wiki/Well-known\_text\_representation\_of\_geometry) format |
| `JSON`        | `JSON`                        |                                                                                                              |
| `ARRAY`       | `ARRAY`                       |                                                                                                              |
| `RECORD`      | `ROW`                         |                                                                                                              |

No other types are supported.

#### Trino type to BigQuery type mapping[#](https://trino.io/docs/current/connector/bigquery.html#trino-type-to-bigquery-type-mapping) <a href="#trino-type-to-bigquery-type-mapping" id="trino-type-to-bigquery-type-mapping"></a>

The connector maps Trino types to the corresponding BigQuery types according to the following table:

| Trino type     | BigQuery type | Notes                                                                                                 |
| -------------- | ------------- | ----------------------------------------------------------------------------------------------------- |
| `BOOLEAN`      | `BOOLEAN`     |                                                                                                       |
| `VARBINARY`    | `BYTES`       |                                                                                                       |
| `DATE`         | `DATE`        |                                                                                                       |
| `DOUBLE`       | `FLOAT`       |                                                                                                       |
| `BIGINT`       | `INT64`       | `INT`, `SMALLINT`, `INTEGER`, `BIGINT`, `TINYINT`, and `BYTEINT` are aliases for `INT64` in BigQuery. |
| `DECIMAL(P,S)` | `NUMERIC`     | The default precision and scale of `NUMERIC` is `(38, 9)`.                                            |
| `VARCHAR`      | `STRING`      |                                                                                                       |
| `TIMESTAMP(6)` | `DATETIME`    |                                                                                                       |

No other types are supported.

### System tables <a href="#system-tables" id="system-tables"></a>

For each Trino table which maps to BigQuery view there exists a system table which exposes BigQuery view definition. Given a BigQuery view `example_view` you can send query `SELECT * example_view$view_definition` to see the SQL which defines view in BigQuery.

### Special columns <a href="#special-columns" id="special-columns"></a>

In addition to the defined columns, the BigQuery connector exposes partition information in a number of hidden columns:

* `$partition_date`: Equivalent to `_PARTITIONDATE` pseudo-column in BigQuery
* `$partition_time`: Equivalent to `_PARTITIONTIME` pseudo-column in BigQuery

You can use these columns in your SQL statements like any other column. They can be selected directly, or used in conditional statements. For example, you can inspect the partition date and time for each record:

```
SELECT *, "$partition_date", "$partition_time"
FROM example.web.page_views;
```

Retrieve all records stored in the partition `_PARTITIONDATE = '2022-04-07'`:

```
SELECT *
FROM example.web.page_views
WHERE "$partition_date" = date '2022-04-07';
```

Note

Two special partitions `__NULL__` and `__UNPARTITIONED__` are not supported.

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides read and write access to data and metadata in the BigQuery database. In addition to the globally available and read operation statements, the connector supports the following features:

* INSERT
* DELETE
* TRUNCATE
* CREATE TABLE
* CREATE TABLE AS
* DROP TABLE
* CREATE SCHEMA
* DROP SCHEMA
* COMMENT

#### SQL DELETE <a href="#sql-delete" id="sql-delete"></a>

If a `WHERE` clause is specified, the `DELETE` operation only works if the predicate in the clause can be fully pushed down to the data source.

### Fault-tolerant execution support <a href="#fault-tolerant-execution-support" id="fault-tolerant-execution-support"></a>

The connector supports Fault-tolerant execution of query processing. Read and write operations are both supported with any retry policy.

### Table functions <a href="#table-functions" id="table-functions"></a>

The connector provides specific table functions to access BigQuery.

#### `query(varchar) -> table` <a href="#query-varchar-table" id="query-varchar-table"></a>

The `query` function allows you to query the underlying BigQuery directly. It requires syntax native to BigQuery, because the full query is pushed down and processed by BigQuery. This can be useful for accessing native features which are not available in Trino or for improving query performance in situations where running a query natively may be faster.

The native query passed to the underlying data source is required to return a table as a result set. Only the data source performs validation or security checks for these queries using its own configuration. Trino does not perform these tasks. Only use passthrough queries to read data.

For example, query the `example` catalog and group and concatenate all employee IDs by manager ID:

```
SELECT
  *
FROM
  TABLE(
    example.system.query(
      query => 'SELECT
        manager_id, STRING_AGG(employee_id)
      FROM
        company.employees
      GROUP BY
        manager_id'
    )
  );
```

Note

The query engine does not preserve the order of the results of this function. If the passed query contains an `ORDER BY` clause, the function result may not be ordered as expected.

### FAQ[#](https://trino.io/docs/current/connector/bigquery.html#faq) <a href="#faq" id="faq"></a>

#### What is the Pricing for the Storage API?[#](https://trino.io/docs/current/connector/bigquery.html#what-is-the-pricing-for-the-storage-api) <a href="#what-is-the-pricing-for-the-storage-api" id="what-is-the-pricing-for-the-storage-api"></a>

See the [BigQuery pricing documentation](https://cloud.google.com/bigquery/pricing#storage-api).
