# OpenSearch connector

The OpenSearch connector allows access to [OpenSearch](https://opensearch.org/) data from Trino. This document describes how to configure a catalog with the OpenSearch connector to run SQL queries against OpenSearch.

### Requirements <a href="#requirements" id="requirements"></a>

* OpenSearch 1.1.0 or higher.
* Network access from the Trino coordinator and workers to the OpenSearch nodes.

### Configuration <a href="#configuration" id="configuration"></a>

To configure the OpenSearch connector, create a catalog properties file `etc/catalog/example.properties` with the following content, replacing the properties as appropriate for your setup:

```
connector.name=opensearch
opensearch.host=search.example.com
opensearch.port=9200
opensearch.default-schema-name=default
```



The following table details all general configuration properties:

| Property name                       | Description                                                                                                                                                                                         | Default   |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| `opensearch.host`                   | The comma-separated list of host names of the OpenSearch cluster. This property is required.                                                                                                        |           |
| `opensearch.port`                   | Port to use to connect to OpenSearch.                                                                                                                                                               | `9200`    |
| `opensearch.default-schema-name`    | The schema that contains all tables defined without a qualifying schema name.                                                                                                                       | `default` |
| `opensearch.scroll-size`            | Sets the maximum number of hits that can be returned with each OpenSearch scroll request.                                                                                                           | `1000`    |
| `opensearch.scroll-timeout`         | Duration for OpenSearch to keep the search context alive for scroll requests.                                                                                                                       | `1m`      |
| `opensearch.request-timeout`        | Timeout duration for all OpenSearch requests.                                                                                                                                                       | `10s`     |
| `opensearch.connect-timeout`        | Timeout duration for all OpenSearch connection attempts.                                                                                                                                            | `1s`      |
| `opensearch.backoff-init-delay`     | The minimum duration between backpressure retry attempts for a single request to OpenSearch. Setting it too low can overwhelm an already struggling cluster.                                        | `500ms`   |
| `opensearch.backoff-max-delay`      | The maximum duration between backpressure retry attempts for a single request.                                                                                                                      | `20s`     |
| `opensearch.max-retry-time`         | The maximum duration across all retry attempts for a single request.                                                                                                                                | `30s`     |
| `opensearch.node-refresh-interval`  | Duration between requests to refresh the list of available OpenSearch nodes.                                                                                                                        | `1m`      |
| `opensearch.ignore-publish-address` | Disable using the address published by the OpenSearch API to connect for queries. Some deployments map OpenSearch ports to a random public port and enabling this property can help in these cases. | `false`   |

#### Authentication <a href="#authentication" id="authentication"></a>

The connection to OpenSearch can use AWS or password authentication.

To enable AWS authentication and authorization using IAM policies, the `opensearch.security` option must be set to `AWS`. Additionally, the following options must be configured:

| Property name                    | Description                                                                                                                         |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `opensearch.aws.region`          | AWS region of the OpenSearch endpoint. This option is required.                                                                     |
| `opensearch.aws.access-key`      | AWS access key to use to connect to the OpenSearch domain. If not set, the default AWS credentials provider chain is used.          |
| `opensearch.aws.secret-key`      | AWS secret key to use to connect to the OpenSearch domain. If not set, the default AWS credentials provider chain is used.          |
| `opensearch.aws.iam-role`        | Optional ARN of an IAM role to assume to connect to OpenSearch. Note that the configured IAM user must be able to assume this role. |
| `opensearch.aws.external-id`     | Optional external ID to pass while assuming an AWS IAM role.                                                                        |
| `opensearch.aws.deployment-type` | AWS OpenSearch deployment type. Possible values are `PROVISIONED` & `SERVERLESS`. This option is required.                          |

To enable password authentication, the `opensearch.security` option must be set to `PASSWORD`. Additionally the following options must be configured:

| Property name              | Description                                |
| -------------------------- | ------------------------------------------ |
| `opensearch.auth.user`     | User name to use to connect to OpenSearch. |
| `opensearch.auth.password` | Password to use to connect to OpenSearch.  |

#### Connection security with TLS <a href="#connection-security-with-tls" id="connection-security-with-tls"></a>

The connector provides additional security options to connect to OpenSearch clusters with TLS enabled.

If your cluster uses globally-trusted certificates, you only need to enable TLS. If you require custom configuration for certificates, the connector supports key stores and trust stores in PEM or Java Key Store (JKS) format.

The available configuration values are listed in the following table:

| Property name                        | Description                                                                                  |
| ------------------------------------ | -------------------------------------------------------------------------------------------- |
| `opensearch.tls.enabled`             | Enable TLS security. Defaults to `false`.                                                    |
| `opensearch.tls.keystore-path`       | The path to the PEM or JKS key store.                                                        |
| `opensearch.tls.truststore-path`     | The path to PEM or JKS trust store.                                                          |
| `opensearch.tls.keystore-password`   | The password for the key store specified by `opensearch.tls.keystore-path`.                  |
| `opensearch.tls.truststore-password` | The password for the trust store specified by `opensearch.tls.truststore-path`.              |
| `opensearch.tls.verify-hostnames`    | Flag to determine if the hostnames in the certificates must be verified. Defaults to `true`. |

### Type mapping <a href="#type-mapping" id="type-mapping"></a>

Because Trino and OpenSearch each support types that the other does not, the connector maps some types when reading data.

#### OpenSearch type to Trino type mapping <a href="#opensearch-type-to-trino-type-mapping" id="opensearch-type-to-trino-type-mapping"></a>

The connector maps OpenSearch types to the corresponding Trino types according to the following table:

| OpenSearch type | Trino type  | Notes                                 |
| --------------- | ----------- | ------------------------------------- |
| `BOOLEAN`       | `BOOLEAN`   |                                       |
| `DOUBLE`        | `DOUBLE`    |                                       |
| `FLOAT`         | `REAL`      |                                       |
| `BYTE`          | `TINYINT`   |                                       |
| `SHORT`         | `SMALLINT`  |                                       |
| `INTEGER`       | `INTEGER`   |                                       |
| `LONG`          | `BIGINT`    |                                       |
| `KEYWORD`       | `VARCHAR`   |                                       |
| `TEXT`          | `VARCHAR`   |                                       |
| `DATE`          | `TIMESTAMP` | For more information, see Date types. |
| `IPADDRESS`     | `IP`        |                                       |

No other types are supported.

#### Array types <a href="#array-types" id="array-types"></a>

Fields in OpenSearch can contain zero or more values, but there is no dedicated array type. To indicate a field contains an array, it can be annotated in a Trino-specific structure in the \_meta section of the index mapping in OpenSearch.

For example, you can have an OpenSearch index that contains documents with the following structure:

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



The array fields of this structure can be defined by using the following command to add the field property definition to the `_meta.trino` property of the target index mapping with OpenSearch available at `search.example.com:9200`:

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

#### Date types <a href="#date-types" id="date-types"></a>

The OpenSearch connector supports only the default `date` type. All other OpenSearch date formats including built-in date formats and custom date formats are not supported. Dates with the format property are ignored.

#### Raw JSON transform <a href="#raw-json-transform" id="raw-json-transform"></a>

Documents in OpenSearch can include more complex structures that are not represented in the mapping. For example, a single `keyword` field can have widely different content including a single `keyword` value, an array, or a multidimensional `keyword` array with any level of nesting.

The following command configures `array_string_field` mapping with OpenSearch available at `search.example.com:9200`:

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



All the following documents are legal for OpenSearch with `array_string_field` mapping:

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



See the [OpenSearch array documentation](https://opensearch.org/docs/latest/field-types/supported-field-types/index/#arrays) for more details.

Further, OpenSearch supports types, such as [k-NN vector](https://opensearch.org/docs/latest/field-types/supported-field-types/knn-vector/), that are not supported in Trino. These and other types can cause parsing exceptions for users that use of these types in OpenSearch. To manage all of these scenarios, you can transform fields to raw JSON by annotating it in a Trino-specific structure in the [\_meta](https://opensearch.org/docs/latest/field-types/index/) section of the OpenSearch index mapping. This indicates to Trino that the field, and all nested fields beneath, must be cast to a `VARCHAR` field that contains the raw JSON content. These fields can be defined by using the following command to add the field property definition to the `_meta.trino` property of the target index mapping.

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



The preceding configuration causes Trino to return the `array_string_field` field as a `VARCHAR` containing raw JSON. You can parse these fields with the built-in JSON functions.

Note

It is not allowed to use `asRawJson` and `isArray` flags simultaneously for the same column.

### Special columns <a href="#special-columns" id="special-columns"></a>

The following hidden columns are available:

| Column    | Description                                          |
| --------- | ---------------------------------------------------- |
| `_id`     | The OpenSearch document ID.                          |
| `_score`  | The document score returned by the OpenSearch query. |
| `_source` | The source of the original document.                 |

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides globally available and read operation statements to access data and metadata in the OpenSearch catalog.

### Table functions <a href="#table-functions" id="table-functions"></a>

The connector provides specific table functions to access OpenSearch.

#### `raw_query(varchar) -> table` <a href="#raw-query-varchar-table" id="raw-query-varchar-table"></a>

The `raw_query` function allows you to query the underlying database directly using the [OpenSearch Query DSL](https://opensearch.org/docs/latest/query-dsl/index/) syntax. The full DSL query is pushed down and processed in OpenSearch. This can be useful for accessing native features which are not available in Trino, or for improving query performance in situations where running a query natively may be faster.

The native query passed to the underlying data source is required to return a table as a result set. Only the data source performs validation or security checks for these queries using its own configuration. Trino does not perform these tasks. Only use passthrough queries to read data.

The `raw_query` function requires three parameters:

* `schema`: The schema in the catalog that the query is to be executed on.
* `index`: The index in OpenSearch to search.
* `query`: The query to execute, written in [OpenSearch Query DSL](https://opensearch.org/docs/latest/query-dsl).

Once executed, the query returns a single row containing the resulting JSON payload returned by OpenSearch.

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

The connector requests data from multiple nodes of the OpenSearch cluster for query processing in parallel.

#### Predicate push down <a href="#predicate-push-down" id="predicate-push-down"></a>

The connector supports predicate push down for the following data types:

| OpenSearch | Trino       |
| ---------- | ----------- |
| `boolean`  | `BOOLEAN`   |
| `double`   | `DOUBLE`    |
| `float`    | `REAL`      |
| `byte`     | `TINYINT`   |
| `short`    | `SMALLINT`  |
| `integer`  | `INTEGER`   |
| `long`     | `BIGINT`    |
| `keyword`  | `VARCHAR`   |
| `date`     | `TIMESTAMP` |

No other data types are supported for predicate push down.

\
