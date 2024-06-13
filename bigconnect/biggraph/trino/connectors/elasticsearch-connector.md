# Elasticsearch connector

The Elasticsearch connector allows access to [Elasticsearch](https://www.elastic.co/products/elasticsearch) data from Trino. This document describes how to configure a catalog with the Elasticsearch connector to run SQL queries against Elasticsearch.

### Requirements <a href="#requirements" id="requirements"></a>

* Elasticsearch 7.x or 8.x
* Network access from the Trino coordinator and workers to the Elasticsearch nodes.

### Configuration <a href="#configuration" id="configuration"></a>

To configure the Elasticsearch connector, create a catalog properties file `etc/catalog/example.properties` with the following contents, replacing the properties as appropriate for your setup:

```
connector.name=elasticsearch
elasticsearch.host=localhost
elasticsearch.port=9200
elasticsearch.default-schema-name=default
```



The following table details all general configuration properties:

| Property name                          | Description                                                                                                                                                                                                                               | Default   |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| `elasticsearch.host`                   | The comma-separated list of host names for the Elasticsearch node to connect to. This property is required.                                                                                                                               |           |
| `elasticsearch.port`                   | Port to use to connecto to Elasticsearch.                                                                                                                                                                                                 | `9200`    |
| `elasticsearch.default-schema-name`    | The schema that contains all tables defined without a qualifying schema name.                                                                                                                                                             | `default` |
| `elasticsearch.scroll-size`            | Sets the maximum number of hits that can be returned with each [Elasticsearch scroll request](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-scroll.html#scroll-search-context).                          | `1000`    |
| `elasticsearch.scroll-timeout`         | Duration for Elasticsearch to keep the search context alive for scroll requests.                                                                                                                                                          | `1m`      |
| `elasticsearch.request-timeout`        | Timeout duration for all Elasticsearch requests.                                                                                                                                                                                          | `10s`     |
| `elasticsearch.connect-timeout`        | Timeout duration for all Elasticsearch connection attempts.                                                                                                                                                                               | `1s`      |
| `elasticsearch.backoff-init-delay`     | The minimum [duration](https://trino.io/docs/current/admin/properties.html#prop-type-duration) between backpressure retry attempts for a single request to Elasticsearch. Setting it too low can overwhelm an already struggling cluster. | `500ms`   |
| `elasticsearch.backoff-max-delay`      | The maximum duration between backpressure retry attempts for a single request to Elasticsearch.                                                                                                                                           | `20s`     |
| `elasticsearch.max-retry-time`         | The maximum duration across all retry attempts for a single request to Elasticsearch.                                                                                                                                                     | `30s`     |
| `elasticsearch.node-refresh-interval`  | Duration between requests to refresh the list of available Elasticsearch nodes.                                                                                                                                                           | `1m`      |
| `elasticsearch.ignore-publish-address` | Disable using the address published by the Elasticsearch API to connect for queries. Some deployments map Elasticsearch ports to a random public port and enabling this property can help in these cases.                                 | `false`   |

#### Authentication <a href="#authentication" id="authentication"></a>

The connection to Elasticsearch can use AWS or password authentication.

To enable AWS authentication and authorization using IAM policies, the `elasticsearch.security` option must be set to `AWS`. Additionally, the following options must be configured:

| Property name                   | Description                                                                                                                            |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `elasticsearch.aws.region`      | AWS region of the Elasticsearch endpoint. This option is required.                                                                     |
| `elasticsearch.aws.access-key`  | AWS access key to use to connect to the Elasticsearch domain. If not set, the default AWS credentials provider chain is used.          |
| `elasticsearch.aws.secret-key`  | AWS secret key to use to connect to the Elasticsearch domain. If not set, the default AWS credentials provider chain is used.          |
| `elasticsearch.aws.iam-role`    | Optional ARN of an IAM role to assume to connect to Elasticsearch. Note that the configured IAM user must be able to assume this role. |
| `elasticsearch.aws.external-id` | Optional external ID to pass while assuming an AWS IAM role.                                                                           |

To enable password authentication, the `elasticsearch.security` option must be set to `PASSWORD`. Additionally the following options must be configured:

| Property name                 | Description                                   |
| ----------------------------- | --------------------------------------------- |
| `elasticsearch.auth.user`     | User name to use to connect to Elasticsearch. |
| `elasticsearch.auth.password` | Password to use to connect to Elasticsearch.  |

#### Connection security with TLS <a href="#connection-security-with-tls" id="connection-security-with-tls"></a>

The connector provides additional security options to connect to Elasticsearch clusters with TLS enabled.

If your cluster has globally-trusted certificates, you should only need to enable TLS. If you require custom configuration for certificates, the connector supports key stores and trust stores in PEM or Java Key Store (JKS) format.

The available configuration values are listed in the following table:

| Property name                           | Description                                                                                                                                               |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `elasticsearch.tls.enabled`             | Enables TLS security.                                                                                                                                     |
| `elasticsearch.tls.keystore-path`       | The path to the PEM or JKS key store.                                                                                                                     |
| `elasticsearch.tls.truststore-path`     | The path to [PEM](https://trino.io/docs/current/security/inspect-pem.html) or [JKS](https://trino.io/docs/current/security/inspect-jks.html) trust store. |
| `elasticsearch.tls.keystore-password`   | The key password for the key store specified by `elasticsearch.tls.keystore-path`.                                                                        |
| `elasticsearch.tls.truststore-password` | The key password for the trust store specified by `elasticsearch.tls.truststore-path`.                                                                    |
| `elasticsearch.tls.verify-hostnames`    | Flag to determine if the hostnames in the certificates must be verified. Defaults to `true`.                                                              |

### Type mapping <a href="#type-mapping" id="type-mapping"></a>

Because Trino and Elasticsearch each support types that the other does not, this connector maps some types when reading data.

#### Elasticsearch type to Trino type mapping <a href="#elasticsearch-type-to-trino-type-mapping" id="elasticsearch-type-to-trino-type-mapping"></a>

The connector maps Elasticsearch types to the corresponding Trino types according to the following table:

| Elasticsearch type | Trino type  | Notes                                                                                                                        |
| ------------------ | ----------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `BOOLEAN`          | `BOOLEAN`   |                                                                                                                              |
| `DOUBLE`           | `DOUBLE`    |                                                                                                                              |
| `FLOAT`            | `REAL`      |                                                                                                                              |
| `BYTE`             | `TINYINT`   |                                                                                                                              |
| `SHORT`            | `SMALLINT`  |                                                                                                                              |
| `INTEGER`          | `INTEGER`   |                                                                                                                              |
| `LONG`             | `BIGINT`    |                                                                                                                              |
| `KEYWORD`          | `VARCHAR`   |                                                                                                                              |
| `TEXT`             | `VARCHAR`   |                                                                                                                              |
| `DATE`             | `TIMESTAMP` | For more information, see [Date types](https://trino.io/docs/current/connector/elasticsearch.html#elasticsearch-date-types). |
| `IPADDRESS`        | `IP`        |                                                                                                                              |

No other types are supported.

#### Array types <a href="#array-types" id="array-types"></a>

Fields in Elasticsearch can contain [zero or more values](https://www.elastic.co/guide/en/elasticsearch/reference/current/array.html), but there is no dedicated array type. To indicate a field contains an array, it can be annotated in a Trino-specific structure in the [\_meta](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-meta-field.html) section of the index mapping.

For example, you can have an Elasticsearch index that contains documents with the following structure:

```
{
    "array_string_field": ["trino","the","lean","machine-ohs"],
    "long_field": 314159265359,
    "id_field": "564e6982-88ee-4498-aa98-df9e3f6b6109",
    "timestamp_field": "1987-09-17T06:22:48.000Z",
    "object_field": {
        "array_int_field": [86,75,309],
        "int_field": 2
    }
}
```



The array fields of this structure can be defined by using the following command to add the field property definition to the `_meta.trino` property of the target index mapping with Elasticsearch available at `search.example.com:9200`:

```
curl --request PUT \
    --url search.example.com:9200/doc/_mapping \
    --header 'content-type: application/json' \
    --data '
{
    "_meta": {
        "trino":{
            "array_string_field":{
                "isArray":true
            },
            "object_field":{
                "array_int_field":{
                    "isArray":true
                }
            },
        }
    }
}'
```



Note

It is not allowed to use `asRawJson` and `isArray` flags simultaneously for the same column.

#### Date types[#](https://trino.io/docs/current/connector/elasticsearch.html#date-types) <a href="#date-types" id="date-types"></a>

The Elasticsearch connector supports only the default `date` type. All other [date](https://www.elastic.co/guide/en/elasticsearch/reference/current/date.html) formats including [built-in date formats](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#built-in-date-formats) and [custom date formats](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#custom-date-formats) are not supported. Dates with the [format](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-date-format.html#mapping-date-format) property are ignored.

#### Raw JSON transform <a href="#raw-json-transform" id="raw-json-transform"></a>

Documents in Elasticsearch can include more complex structures that are not represented in the mapping. For example, a single `keyword` field can have widely different content including a single `keyword` value, an array, or a multidimensional `keyword` array with any level of nesting.

The following command configures `array_string_field` mapping with Elasticsearch available at `search.example.com:9200`:

```
curl --request PUT \
    --url search.example.com:9200/doc/_mapping \
    --header 'content-type: application/json' \
    --data '
{
    "properties": {
        "array_string_field":{
            "type": "keyword"
        }
    }
}'
```



All the following documents are legal for Elasticsearch with `array_string_field` mapping:

```
[
    {
        "array_string_field": "trino"
    },
    {
        "array_string_field": ["trino","is","the","best"]
    },
    {
        "array_string_field": ["trino",["is","the","best"]]
    },
    {
        "array_string_field": ["trino",["is",["the","best"]]]
    }
]
```



See the [Elasticsearch array documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/array.html) for more details.

Further, Elasticsearch supports types, such as [dense\_vector](https://www.elastic.co/guide/en/elasticsearch/reference/current/dense-vector.html), that are not supported in Trino. These and other types can cause parsing exceptions for users that use of these types in Elasticsearch. To manage all of these scenarios, you can transform fields to raw JSON by annotating it in a Trino-specific structure in the [\_meta](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-meta-field.html) section of the index mapping. This indicates to Trino that the field, and all nested fields beneath, need to be cast to a `VARCHAR` field that contains the raw JSON content. These fields can be defined by using the following command to add the field property definition to the `_meta.trino` property of the target index mapping.

```
curl --request PUT \
    --url search.example.com:9200/doc/_mapping \
    --header 'content-type: application/json' \
    --data '
{
    "_meta": {
        "trino":{
            "array_string_field":{
                "asRawJson":true
            }
        }
    }
}'
```



This preceding configuration causes Trino to return the `array_string_field` field as a `VARCHAR` containing raw JSON. You can parse these fields with the built-in JSON functions.

Note

It is not allowed to use `asRawJson` and `isArray` flags simultaneously for the same column.

### Special columns <a href="#special-columns" id="special-columns"></a>

The following hidden columns are available:

| Column    | Description                                             |
| --------- | ------------------------------------------------------- |
| `_id`     | The Elasticsearch document ID.                          |
| `_score`  | The document score returned by the Elasticsearch query. |
| `_source` | The source of the original document.                    |

### Full text queries <a href="#full-text-queries" id="full-text-queries"></a>

Trino SQL queries can be combined with Elasticsearch queries by providing the [full text query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax) as part of the table name, separated by a colon. For example:

```
SELECT * FROM "tweets: +trino SQL^2"
```



### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides globally available and read operation statements to access data and metadata in the Elasticsearch catalog.

### Table functions <a href="#table-functions" id="table-functions"></a>

The connector provides specific table functions to access Elasticsearch.

#### `raw_query(varchar) -> table` <a href="#raw-query-varchar-table" id="raw-query-varchar-table"></a>

The `raw_query` function allows you to query the underlying database directly. This function requires Elastic Query DSL syntax. The full DSL query is pushed down and processed in Elasticsearch. This can be useful for accessing native features which are not available in Trino or for improving query performance in situations where running a query natively may be faster.

The native query passed to the underlying data source is required to return a table as a result set. Only the data source performs validation or security checks for these queries using its own configuration. Trino does not perform these tasks. Only use passthrough queries to read data.

The `raw_query` function requires three parameters:

* `schema`: The schema in the catalog that the query is to be executed on.
* `index`: The index in Elasticsearch to be searched.
* `query`: The query to execute, written in [Elastic Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html).

Once executed, the query returns a single row containing the resulting JSON payload returned by Elasticsearch.

For example, query the `example` catalog and use the `raw_query` table function to search for documents in the `orders` index where the country name is `ALGERIA` as defined as a JSON-formatted query matcher and passed to the `raw_query` table function in the `query` parameter:

```
SELECT
  *
FROM
  TABLE(
    example.system.raw_query(
      schema => 'sales',
      index => 'orders',
      query => '{
        "query": {
          "match": {
            "name": "ALGERIA"
          }
        }
      }'
    )
  );
```



Note

The query engine does not preserve the order of the results of this function. If the passed query contains an `ORDER BY` clause, the function result may not be ordered as expected.

### Performance <a href="#performance" id="performance"></a>

The connector includes a number of performance improvements, detailed in the following sections.

#### Parallel data access <a href="#parallel-data-access" id="parallel-data-access"></a>

The connector requests data from multiple nodes of the Elasticsearch cluster for query processing in parallel.

#### Predicate push down <a href="#predicate-push-down" id="predicate-push-down"></a>

The connector supports predicate push down for the following data types:

| Elasticsearch | Trino       |
| ------------- | ----------- |
| `boolean`     | `BOOLEAN`   |
| `double`      | `DOUBLE`    |
| `float`       | `REAL`      |
| `byte`        | `TINYINT`   |
| `short`       | `SMALLINT`  |
| `integer`     | `INTEGER`   |
| `long`        | `BIGINT`    |
| `keyword`     | `VARCHAR`   |
| `date`        | `TIMESTAMP` |

No other data types are supported for predicate push down.
