# Redshift connector

The Redshift connector allows querying and creating tables in an external [Amazon Redshift](https://aws.amazon.com/redshift/) cluster. This can be used to join data between different systems like Redshift and Hive, or between two different Redshift clusters.

### Requirements[#](https://trino.io/docs/current/connector/redshift.html#requirements) <a href="#requirements" id="requirements"></a>

To connect to Redshift, you need:

* Network access from the Trino coordinator and workers to Redshift. Port 5439 is the default port.

### Configuration[#](https://trino.io/docs/current/connector/redshift.html#configuration) <a href="#configuration" id="configuration"></a>

To configure the Redshift connector, create a catalog properties file in `etc/catalog` named, for example, `example.properties`, to mount the Redshift connector as the `example` catalog. Create the file with the following contents, replacing the connection properties as appropriate for your setup:

```
connector.name=redshift
connection-url=jdbc:redshift://example.net:5439/database
connection-user=root
connection-password=secret
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `connection-user` and `connection-password` are typically required and determine the user credentials for the connection, often a service user. You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid actual values in the catalog properties files.

#### Connection security[#](https://trino.io/docs/current/connector/redshift.html#connection-security) <a href="#connection-security" id="connection-security"></a>

If you have TLS configured with a globally-trusted certificate installed on your data source, you can enable TLS between your cluster and the data source by appending a parameter to the JDBC connection string set in the `connection-url` catalog configuration property.

For example, on version 2.1 of the Redshift JDBC driver, TLS/SSL is enabled by default with the `SSL` parameter. You can disable or further configure TLS by appending parameters to the `connection-url` configuration property:

```
connection-url=jdbc:redshift://example.net:5439/database;SSL=TRUE;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

For more information on TLS configuration options, see the [Redshift JDBC driver documentation](https://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-configuration-options.html#jdbc20-ssl-option).

#### Data source authentication[#](https://trino.io/docs/current/connector/redshift.html#data-source-authentication) <a href="#data-source-authentication" id="data-source-authentication"></a>

The connector can provide credentials for the data source connection in multiple ways:

* inline, in the connector configuration file
* in a separate properties file
* in a key store file
* as extra credentials set when connecting to Trino

You can use [secrets](https://trino.io/docs/current/security/secrets.html) to avoid storing sensitive values in the catalog properties files.

The following table describes configuration properties for connection credentials:

| Property name                           | Description                                                                                                                                                                                            |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `credential-provider.type`              | Type of the credential provider. Must be one of `INLINE`, `FILE`, or `KEYSTORE`; defaults to `INLINE`.                                                                                                 |
| `connection-user`                       | Connection user name.                                                                                                                                                                                  |
| `connection-password`                   | Connection password.                                                                                                                                                                                   |
| `user-credential-name`                  | Name of the extra credentials property, whose value to use as the user name. See `extraCredentials` in [Parameter reference](https://trino.io/docs/current/client/jdbc.html#jdbc-parameter-reference). |
| `password-credential-name`              | Name of the extra credentials property, whose value to use as the password.                                                                                                                            |
| `connection-credential-file`            | Location of the properties file where credentials are present. It must contain the `connection-user` and `connection-password` properties.                                                             |
| `keystore-file-path`                    | The location of the Java Keystore file, from which to read credentials.                                                                                                                                |
| `keystore-type`                         | File format of the keystore file, for example `JKS` or `PEM`.                                                                                                                                          |
| `keystore-password`                     | Password for the key store.                                                                                                                                                                            |
| `keystore-user-credential-name`         | Name of the key store entity to use as the user name.                                                                                                                                                  |
| `keystore-user-credential-password`     | Password for the user name key store entity.                                                                                                                                                           |
| `keystore-password-credential-name`     | Name of the key store entity to use as the password.                                                                                                                                                   |
| `keystore-password-credential-password` | Password for the password key store entity.                                                                                                                                                            |

#### Multiple Redshift databases or clusters[#](https://trino.io/docs/current/connector/redshift.html#multiple-redshift-databases-or-clusters) <a href="#multiple-redshift-databases-or-clusters" id="multiple-redshift-databases-or-clusters"></a>

The Redshift connector can only access a single database within a Redshift cluster. Thus, if you have multiple Redshift databases, or want to connect to multiple Redshift clusters, you must configure multiple instances of the Redshift connector.

To add another catalog, simply add another properties file to `etc/catalog` with a different name, making sure it ends in `.properties`. For example, if you name the property file `sales.properties`, Trino creates a catalog named `sales` using the configured connector.

#### General configuration properties[#](https://trino.io/docs/current/connector/redshift.html#general-configuration-properties) <a href="#general-configuration-properties" id="general-configuration-properties"></a>

The following table describes general catalog configuration properties for the connector:

| Property name                                               | Description                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `case-insensitive-name-matching`                            | Support case insensitive schema and table names. Defaults to `false`.                                                                                                                                                                                                                                                                                                     |
| `case-insensitive-name-matching.cache-ttl`                  | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which case insensitive schema and table names are cached. Defaults to `1m`.                                                                                                                                                                                                        |
| `case-insensitive-name-matching.config-file`                | Path to a name mapping configuration file in JSON format that allows Trino to disambiguate between schemas and tables with similar names in different cases. Defaults to `null`.                                                                                                                                                                                          |
| `case-insensitive-name-matching.config-file.refresh-period` | Frequency with which Trino checks the name matching configuration file for changes. The [duration value](https://trino.io/docs/current/admin/properties.html#prop-type-duration) defaults to `0s` (refresh disabled).                                                                                                                                                     |
| `metadata.cache-ttl`                                        | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which metadata, including table and column statistics, is cached. Defaults to `0s` (caching disabled).                                                                                                                                                                             |
| `metadata.cache-missing`                                    | Cache the fact that metadata, including table and column statistics, is not available. Defaults to `false`.                                                                                                                                                                                                                                                               |
| `metadata.schemas.cache-ttl`                                | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which schema metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                    |
| `metadata.tables.cache-ttl`                                 | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which table metadata is cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                     |
| `metadata.statistics.cache-ttl`                             | [Duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which tables statistics are cached. Defaults to the value of `metadata.cache-ttl`.                                                                                                                                                                                                 |
| `metadata.cache-maximum-size`                               | Maximum number of objects stored in the metadata cache. Defaults to `10000`.                                                                                                                                                                                                                                                                                              |
| `write.batch-size`                                          | Maximum number of statements in a batched execution. Do not change this setting from the default. Non-default values may negatively impact performance. Defaults to `1000`.                                                                                                                                                                                               |
| `dynamic-filtering.enabled`                                 | Push down dynamic filters into JDBC queries. Defaults to `true`.                                                                                                                                                                                                                                                                                                          |
| `dynamic-filtering.wait-timeout`                            | Maximum [duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) for which Trino waits for dynamic filters to be collected from the build side of joins before starting a JDBC query. Using a large timeout can potentially result in more detailed dynamic filters. However, it can also increase latency for some queries. Defaults to `20s`. |

#### Appending query metadata[#](https://trino.io/docs/current/connector/redshift.html#appending-query-metadata) <a href="#appending-query-metadata" id="appending-query-metadata"></a>

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

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

With this configuration, a query such as `SELECT * FROM example_table;` is sent to the datasource with the comment appended:

```
SELECT * FROM example_table; /*Query sent by Trino.*/
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following example improves on the preceding example by using metadata:

```
query.comment-format=Query $QUERY_ID sent by user $USER from Trino.
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If `Jane` sent the query with the query identifier `20230622_180528_00000_bkizg`, the following comment string is sent to the datasource:

```
SELECT * FROM example_table; /*Query 20230622_180528_00000_bkizg sent by user Jane from Trino.*/
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note

Certain JDBC driver settings and logging configurations might cause the comment to be removed.

#### Domain compaction threshold[#](https://trino.io/docs/current/connector/redshift.html#domain-compaction-threshold) <a href="#domain-compaction-threshold" id="domain-compaction-threshold"></a>

Pushing down a large list of predicates to the data source can compromise performance. Trino compacts large predicates into a simpler range predicate by default to ensure a balance between performance and predicate pushdown. If necessary, the threshold for this compaction can be increased to improve performance when the data source is capable of taking advantage of large predicates. Increasing this threshold may improve pushdown of large [dynamic filters](https://trino.io/docs/current/admin/dynamic-filtering.html). The `domain-compaction-threshold` catalog configuration property or the `domain_compaction_threshold` [catalog session property](https://trino.io/docs/current/sql/set-session.html#session-properties-definition) can be used to adjust the default value of `32` for this threshold.

#### Procedures[#](https://trino.io/docs/current/connector/redshift.html#procedures) <a href="#procedures" id="procedures"></a>

*   `system.flush_metadata_cache()`

    Flush JDBC metadata caches. For example, the following system call flushes the metadata caches for all schemas in the `example` catalog

    ```
    USE example.example_schema;
    CALL system.flush_metadata_cache();
    ```

    ![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Case insensitive matching[#](https://trino.io/docs/current/connector/redshift.html#case-insensitive-matching) <a href="#case-insensitive-matching" id="case-insensitive-matching"></a>

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

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Queries against one of the tables or schemes defined in the `mapping` attributes are run against the corresponding remote entity. For example, a query against tables in the `case_insensitive_1` schema is forwarded to the CaseSensitiveName schema and a query against `case_insensitive_2` is forwarded to the `cASEsENSITIVEnAME` schema.

At the table mapping level, a query on `case_insensitive_1.table_1` as configured above is forwarded to `CaseSensitiveName.tablex`, and a query on `case_insensitive_1.table_2` is forwarded to `CaseSensitiveName.TABLEX`.

By default, when a change is made to the mapping configuration file, Trino must be restarted to load the changes. Optionally, you can set the `case-insensitive-name-mapping.refresh-period` to have Trino refresh the properties without requiring a restart:

```
case-insensitive-name-mapping.refresh-period=30s
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Non-transactional INSERT[#](https://trino.io/docs/current/connector/redshift.html#non-transactional-insert) <a href="#non-transactional-insert" id="non-transactional-insert"></a>

The connector supports adding rows using [INSERT statements](https://trino.io/docs/current/sql/insert.html). By default, data insertion is performed by writing data to a temporary table. You can skip this step to improve performance and write directly to the target table. Set the `insert.non-transactional-insert.enabled` catalog property or the corresponding `non_transactional_insert` catalog session property to `true`.

Note that with this property enabled, data can be corrupted in rare cases where exceptions occur during the insert operation. With transactions disabled, no rollback can be performed.

### Querying Redshift[#](https://trino.io/docs/current/connector/redshift.html#querying-redshift) <a href="#querying-redshift" id="querying-redshift"></a>

The Redshift connector provides a schema for every Redshift schema. You can see the available Redshift schemas by running `SHOW SCHEMAS`:

```
SHOW SCHEMAS FROM example;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you have a Redshift schema named `web`, you can view the tables in this schema by running `SHOW TABLES`:

```
SHOW TABLES FROM example.web;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can see a list of the columns in the `clicks` table in the `web` database using either of the following:

```
DESCRIBE example.web.clicks;
SHOW COLUMNS FROM example.web.clicks;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Finally, you can access the `clicks` table in the `web` schema:

```
SELECT * FROM example.web.clicks;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you used a different name for your catalog properties file, use that catalog name instead of `example` in the above examples.

### Type mapping[#](https://trino.io/docs/current/connector/redshift.html#type-mapping) <a href="#type-mapping" id="type-mapping"></a>

#### Type mapping configuration properties[#](https://trino.io/docs/current/connector/redshift.html#type-mapping-configuration-properties) <a href="#type-mapping-configuration-properties" id="type-mapping-configuration-properties"></a>

The following properties can be used to configure how data types from the connected data source are mapped to Trino data types and how the metadata is cached in Trino.

| Property name                  | Description                                                                                                                                                                                                                                                                                                              | Default value |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| `unsupported-type-handling`    | <p>Configure how unsupported column data types are handled:</p><ul><li><code>IGNORE</code>, column is not accessible.</li><li><code>CONVERT_TO_VARCHAR</code>, column is converted to unbounded <code>VARCHAR</code>.</li></ul><p>The respective catalog session property is <code>unsupported_type_handling</code>.</p> | `IGNORE`      |
| `jdbc-types-mapped-to-varchar` | Allow forced mapping of comma separated lists of data types to convert to unbounded `VARCHAR`                                                                                                                                                                                                                            |               |

### SQL support[#](https://trino.io/docs/current/connector/redshift.html#sql-support) <a href="#sql-support" id="sql-support"></a>

The connector provides read access and write access to data and metadata in Redshift. In addition to the [globally available](https://trino.io/docs/current/language/sql-support.html#sql-globally-available) and [read operation](https://trino.io/docs/current/language/sql-support.html#sql-read-operations) statements, the connector supports the following features:

* [INSERT](https://trino.io/docs/current/sql/insert.html)
* [UPDATE](https://trino.io/docs/current/sql/update.html)
* [DELETE](https://trino.io/docs/current/sql/delete.html)
* [TRUNCATE](https://trino.io/docs/current/sql/truncate.html)
* [Schema and table management](https://trino.io/docs/current/language/sql-support.html#sql-schema-table-management)

#### UPDATE[#](https://trino.io/docs/current/connector/redshift.html#update) <a href="#update" id="update"></a>

Only `UPDATE` statements with constant assignments and predicates are supported. For example, the following statement is supported because the values assigned are constants:

```
UPDATE table SET col1 = 1 WHERE col3 = 1
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Arithmetic expressions, function calls, and other non-constant `UPDATE` statements are not supported. For example, the following statement is not supported because arithmetic expressions cannot be used with the `SET` command:

```
UPDATE table SET col1 = col2 + 2 WHERE col3 = 1
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `=`, `!=`, `>`, `<`, `>=`, `<=`, `IN`, `NOT IN` operators are supported in predicates. The following statement is not supported because the `AND` operator cannot be used in predicates:

```
UPDATE table SET col1 = 1 WHERE col3 = 1 AND col2 = 3
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

All column values of a table row cannot be updated simultaneously. For a three column table, the following statement is not supported:

```
UPDATE table SET col1 = 1, col2 = 2, col3 = 3 WHERE col3 = 1
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### SQL DELETE[#](https://trino.io/docs/current/connector/redshift.html#sql-delete) <a href="#sql-delete" id="sql-delete"></a>

If a `WHERE` clause is specified, the `DELETE` operation only works if the predicate in the clause can be fully pushed down to the data source.

#### ALTER TABLE RENAME TO[#](https://trino.io/docs/current/connector/redshift.html#alter-table-rename-to) <a href="#alter-table-rename-to" id="alter-table-rename-to"></a>

The connector does not support renaming tables across multiple schemas. For example, the following statement is supported:

```
ALTER TABLE example.schema_one.table_one RENAME TO example.schema_one.table_two
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following statement attempts to rename a table across schemas, and therefore is not supported:

```
ALTER TABLE example.schema_one.table_one RENAME TO example.schema_two.table_two
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### ALTER SCHEMA[#](https://trino.io/docs/current/connector/redshift.html#alter-schema) <a href="#alter-schema" id="alter-schema"></a>

The connector supports renaming a schema with the `ALTER SCHEMA RENAME` statement. `ALTER SCHEMA SET AUTHORIZATION` is not supported.

### Fault-tolerant execution support[#](https://trino.io/docs/current/connector/redshift.html#fault-tolerant-execution-support) <a href="#fault-tolerant-execution-support" id="fault-tolerant-execution-support"></a>

The connector supports [Fault-tolerant execution](https://trino.io/docs/current/admin/fault-tolerant-execution.html) of query processing. Read and write operations are both supported with any retry policy.

### Table functions[#](https://trino.io/docs/current/connector/redshift.html#table-functions) <a href="#table-functions" id="table-functions"></a>

The connector provides specific [table functions](https://trino.io/docs/current/functions/table.html) to access Redshift.

#### `query(varchar) -> table`[#](https://trino.io/docs/current/connector/redshift.html#query-varchar-table) <a href="#query-varchar-table" id="query-varchar-table"></a>

The `query` function allows you to query the underlying database directly. It requires syntax native to Redshift, because the full query is pushed down and processed in Redshift. This can be useful for accessing native features which are not implemented in Trino or for improving query performance in situations where running a query natively may be faster.

The native query passed to the underlying data source is required to return a table as a result set. Only the data source performs validation or security checks for these queries using its own configuration. Trino does not perform these tasks. Only use passthrough queries to read data.

For example, query the `example` catalog and select the top 10 nations by population:

```
SELECT
  *
FROM
  TABLE(
    example.system.query(
      query => 'SELECT
        TOP 10 *
      FROM
        tpch.nation
      ORDER BY
        population DESC'
    )
  );
```

Note

The query engine does not preserve the order of the results of this function. If the passed query contains an `ORDER BY` clause, the function result may not be ordered as expected.
