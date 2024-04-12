# Druid connector

The Druid connector allows querying an Apache Druid database from Trino.

### Requirements[#](https://trino.io/docs/current/connector/druid.html#requirements) <a href="#requirements" id="requirements"></a>

To connect to Druid, you need:

* Druid version 0.18.0 or higher.
* Network access from the Trino coordinator and workers to your Druid broker. Port 8082 is the default port.

### Configuration[#](https://trino.io/docs/current/connector/druid.html#configuration) <a href="#configuration" id="configuration"></a>

Create a catalog properties file that specifies the Druid connector by setting the `connector.name` to `druid` and configuring the `connection-url` with the JDBC string to connect to Druid.

For example, to access a database as `example`, create the file `etc/catalog/example.properties`. Replace `BROKER:8082` with the correct host and port of your Druid broker.

```
connector.name=druid
connection-url=jdbc:avatica:remote:url=http://BROKER:8082/druid/v2/sql/avatica/
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can add authentication details to connect to a Druid deployment that is secured by basic authentication by updating the URL and adding credentials:

```
connection-url=jdbc:avatica:remote:url=http://BROKER:port/druid/v2/sql/avatica/;authentication=BASIC
connection-user=root
connection-password=secret
```

Now you can access your Druid database in Trino with the `example` catalog name from the properties file.

The `connection-user` and `connection-password` are typically required and determine the user credentials for the connection, often a service user. You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid actual values in the catalog properties files.

#### Data source authentication[#](https://trino.io/docs/current/connector/druid.html#data-source-authentication) <a href="#data-source-authentication" id="data-source-authentication"></a>

The connector can provide credentials for the data source connection in multiple ways:

* inline, in the connector configuration file
* in a separate properties file
* in a key store file
* as extra credentials set when connecting to Trino

You can use secrets to avoid storing sensitive values in the catalog properties files.

The following table describes configuration properties for connection credentials:

| Property name                           | Description                                                                                                                                |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `credential-provider.type`              | Type of the credential provider. Must be one of `INLINE`, `FILE`, or `KEYSTORE`; defaults to `INLINE`.                                     |
| `connection-user`                       | Connection user name.                                                                                                                      |
| `connection-password`                   | Connection password.                                                                                                                       |
| `user-credential-name`                  | Name of the extra credentials property, whose value to use as the user name. See `extraCredentials` in Parameter reference.                |
| `password-credential-name`              | Name of the extra credentials property, whose value to use as the password.                                                                |
| `connection-credential-file`            | Location of the properties file where credentials are present. It must contain the `connection-user` and `connection-password` properties. |
| `keystore-file-path`                    | The location of the Java Keystore file, from which to read credentials.                                                                    |
| `keystore-type`                         | File format of the keystore file, for example `JKS` or `PEM`.                                                                              |
| `keystore-password`                     | Password for the key store.                                                                                                                |
| `keystore-user-credential-name`         | Name of the key store entity to use as the user name.                                                                                      |
| `keystore-user-credential-password`     | Password for the user name key store entity.                                                                                               |
| `keystore-password-credential-name`     | Name of the key store entity to use as the password.                                                                                       |
| `keystore-password-credential-password` | Password for the password key store entity.                                                                                                |

#### General configuration properties[#](https://trino.io/docs/current/connector/druid.html#general-configuration-properties) <a href="#general-configuration-properties" id="general-configuration-properties"></a>

The following table describes general catalog configuration properties for the connector:

| Property name                                               | Description                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `case-insensitive-name-matching`                            | Support case insensitive schema and table names. Defaults to `false`.                                                                                                                                                                                                                           |
| `case-insensitive-name-matching.cache-ttl`                  | Duration for which case insensitive schema and table names are cached. Defaults to `1m`.                                                                                                                                                                                                        |
| `case-insensitive-name-matching.config-file`                | Path to a name mapping configuration file in JSON format that allows Trino to disambiguate between schemas and tables with similar names in different cases. Defaults to `null`.                                                                                                                |
| `case-insensitive-name-matching.config-file.refresh-period` | Frequency with which Trino checks the name matching configuration file for changes. The duration value defaults to `0s` (refresh disabled).                                                                                                                                                     |
| `metadata.cache-ttl`                                        | Duration for which metadata, including table and column statistics, is cached. Defaults to `0s` (caching disabled).                                                                                                                                                                             |
| `metadata.cache-missing`                                    | Cache the fact that metadata, including table and column statistics, is not available. Defaults to `false`.                                                                                                                                                                                     |
| `metadata.schemas.cache-ttl`                                | Duration for which schema metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                    |
| `metadata.tables.cache-ttl`                                 | Duration for which table metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                     |
| `metadata.statistics.cache-ttl`                             | Duration for which tables statistics are cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                 |
| `metadata.cache-maximum-size`                               | Maximum number of objects stored in the metadata cache. Defaults to `10000`.                                                                                                                                                                                                                    |
| `write.batch-size`                                          | Maximum number of statements in a batched execution. Do not change this setting from the default. Non-default values may negatively impact performance. Defaults to `1000`.                                                                                                                     |
| `dynamic-filtering.enabled`                                 | Push down dynamic filters into JDBC queries. Defaults to `true`.                                                                                                                                                                                                                                |
| `dynamic-filtering.wait-timeout`                            | Maximum duration for which Trino waits for dynamic filters to be collected from the build side of joins before starting a JDBC query. Using a large timeout can potentially result in more detailed dynamic filters. However, it can also increase latency for some queries. Defaults to `20s`. |

#### Appending query metadata[#](https://trino.io/docs/current/connector/druid.html#appending-query-metadata) <a href="#appending-query-metadata" id="appending-query-metadata"></a>

The optional parameter `query.comment-format` allows you to configure a SQL comment that is sent to the datasource with each query. The format of this comment can contain any characters and the following metadata:

* `$QUERY_ID`: The identifier of the query.
* `$USER`: The name of the user who submits the query to Trino.
* `$SOURCE`: The identifier of the client tool used to submit the query, for example `trino-cli`.
* `$TRACE_TOKEN`: The trace token configured with the client tool.

The comment can provide more context about the query. This additional information is available in the logs of the datasource. To include environment variables from the Trino cluster with the comment , use the `${ENV:VARIABLE-NAME}` syntax.

The following example sets a simple comment that identifies each query sent by Trino:

```
query.comment-format=Query sent by Trino.
```

With this configuration, a query such as `SELECT * FROM example_table;` is sent to the datasource with the comment appended:

```
SELECT * FROM example_table; /*Query sent by Trino.*/
```

The following example improves on the preceding example by using metadata:

```
query.comment-format=Query $QUERY_ID sent by user $USER from Trino.
```

If `Jane` sent the query with the query identifier `20230622_180528_00000_bkizg`, the following comment string is sent to the datasource:

```
SELECT * FROM example_table; /*Query 20230622_180528_00000_bkizg sent by user Jane from Trino.*/
```

Note

Certain JDBC driver settings and logging configurations might cause the comment to be removed.

#### Domain compaction threshold[#](https://trino.io/docs/current/connector/druid.html#domain-compaction-threshold) <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improve performance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large dynamic filters. The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` catalog session property can be used to adjust the default value of `32` for this threshold.

#### Procedures <a href="#procedures" id="procedures"></a>

*   `system.flush_metadata_cache()`

    Flush JDBC metadata caches. For example, the following system call flushes the metadata caches for all schemas in the `example` catalog

    ```
    USE example.example_schema;
    CALL system.flush_metadata_cache();
    ```

#### Case insensitive matching[#](https://trino.io/docs/current/connector/druid.html#case-insensitive-matching) <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

When `case-insensitive-name-matching` is set to `true`, Trino is able to query non-lowercase schemas and tables by maintaining a mapping of the lowercase name to the actual name in the remote system. However, if two schemas and/or tables have names that differ only in case (such as “customers” and “Customers”) then Trino fails to query them due to ambiguity.

In these cases, use the `case-insensitive-name-matching.config-file` catalog configuration property to specify a configuration file that maps these remote schemas/tables to their respective Trino schemas/tables:

```
{
  "schemas": [
    {
      "remoteSchema": "CaseSensitiveName",
      "mapping": "case_insensitive_1"
    },
    {
      "remoteSchema": "cASEsENSITIVEnAME",
      "mapping": "case_insensitive_2"
    }],
  "tables": [
    {
      "remoteSchema": "CaseSensitiveName",
      "remoteTable": "tablex",
      "mapping": "table_1"
    },
    {
      "remoteSchema": "CaseSensitiveName",
      "remoteTable": "TABLEX",
      "mapping": "table_2"
    }]
}
```

Queries against one of the tables or schemes defined in the `mapping` attributes are run against the corresponding remote entity. For example, a query against tables in the `case_insensitive_1` schema is forwarded to the CaseSensitiveName schema and a query against `case_insensitive_2` is forwarded to the `cASEsENSITIVEnAME` schema.

At the table mapping level, a query on `case_insensitive_1.table_1` as configured above is forwarded to `CaseSensitiveName.tablex`, and a query on `case_insensitive_1.table_2` is forwarded to `CaseSensitiveName.TABLEX`.

By default, when a change is made to the mapping configuration file, Trino must be restarted to load the changes. Optionally, you can set the `case-insensitive-name-mapping.refresh-period` to have Trino refresh the properties without requiring a restart:

```
case-insensitive-name-mapping.refresh-period=30s
```

### Type mapping[#](https://trino.io/docs/current/connector/druid.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

Because Trino and Druid each support types that the other does not, this connector [modifies some types](https://trino.io/docs/current/language/types.html#type-mapping-overview) when reading data.

#### Druid type to Trino type mapping[#](https://trino.io/docs/current/connector/druid.html#druid-type-to-trino-type-mapping) <a href="#druid-type-to-trino-type-mapping" id="druid-type-to-trino-type-mapping"></a>

The connector maps Druid types to the corresponding Trino types according to the following table:

| Druid type  | Trino type  | Notes                                                                  |
| ----------- | ----------- | ---------------------------------------------------------------------- |
| `STRING`    | `VARCHAR`   |                                                                        |
| `FLOAT`     | `REAL`      |                                                                        |
| `DOUBLE`    | `DOUBLE`    |                                                                        |
| `LONG`      | `BIGINT`    | Except for the special `_time` column, which is mapped to `TIMESTAMP`. |
| `TIMESTAMP` | `TIMESTAMP` | Only applicable to the special `_time` column.                         |

No other data types are supported.

Druid does not have a real `NULL` value for any data type. By default, Druid treats `NULL` as the default value for a data type. For example, `LONG` would be `0`, `DOUBLE` would be `0.0`, `STRING` would be an empty string `''`, and so forth.

#### Type mapping configuration properties[#](https://trino.io/docs/current/connector/druid.html#type-mapping-configuration-properties) <a href="#type-mapping-configuration-properties" id="type-mapping-configuration-properties"></a>

The following properties can be used to configure how data types from the connected data source are mapped to Trino data types and how the metadata is cached in Trino.

| Property name                  | Description                                                                                                                                                                                                                                                                                                              | Default value |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `unsupported-type-handling`    | <p>Configure how unsupported column data types are handled:</p><ul><li><code>IGNORE</code>, column is not accessible.</li><li><code>CONVERT_TO_VARCHAR</code>, column is converted to unbounded <code>VARCHAR</code>.</li></ul><p>The respective catalog session property is <code>unsupported_type_handling</code>.</p> | `IGNORE`      |
| `jdbc-types-mapped-to-varchar` | Allow forced mapping of comma separated lists of data types to convert to unbounded `VARCHAR`                                                                                                                                                                                                                            |               |

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides globally available and read operation statements to access data and metadata in the Druid database.

### Table functions <a href="#table-functions" id="table-functions"></a>

The connector provides specific table functions to access Druid.

#### `query(varchar) -> table`[#](https://trino.io/docs/current/connector/druid.html#query-varchar-table) <a href="#query-varchar-table" id="query-varchar-table"></a>

The `query` function allows you to query the underlying database directly. It requires syntax native to Druid, because the full query is pushed down and processed in Druid. This can be useful for accessing native features which are not available in Trino or for improving query performance in situations where running a query natively may be faster.

The native query passed to the underlying data source is required to return a table as a result set. Only the data source performs validation or security checks for these queries using its own configuration. Trino does not perform these tasks. Only use passthrough queries to read data.

As an example, query the `example` catalog and use `STRING_TO_MV` and `MV_LENGTH` from Druid SQL’s multi-value string functions to split and then count the number of comma-separated values in a column:

```
SELECT
  num_reports
FROM
  TABLE(
    example.system.query(
      query => 'SELECT
        MV_LENGTH(
          STRING_TO_MV(direct_reports, ",")
        ) AS num_reports
      FROM company.managers'
    )
  );
```

Note

The query engine does not preserve the order of the results of this function. If the passed query contains an `ORDER BY` clause, the function result may not be ordered as expected.
