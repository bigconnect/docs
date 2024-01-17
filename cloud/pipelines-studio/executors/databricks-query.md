# Databricks Query

The Databricks Query executor runs one or more Spark SQL queries on Databricks each time it receives an event record. Use the executor as part of an event stream in the pipeline.&#x20;

For example, you might use the Databricks Query executor to run a Spark SQL query that executes the VACUUM command to remove leftover files when the pipeline stop event is generated.

The Databricks Query executor uses a JDBC connection to connect to the Databricks cluster. When you configure the executor, you specify the JDBC connection string and credentials to use to connect to the Databricks cluster, and then you define the Spark SQL queries to run.

When needed, you also define the connection information that the executor uses to connect to the storage location in Amazon S3 or Azure Data Lake Storage Gen2.

You can also configure the executor to generate events for another event stream.&#x20;

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you configure the Databricks Query executor, prepare your Databricks cluster.

In Databricks, configure and start your Databricks cluster, generate a personal access token, and locate the JDBC URL used to access the cluster.

Tip: In Databricks, you can locate the JDBC connection string for your cluster on the JDBC tab in the cluster configuration details.

### Spark SQL Queries <a href="#ariaid-title8" id="ariaid-title8"></a>

You define one or more Spark SQL queries that the Databricks Query executor runs in Databricks each time it receives an event record.

The Databricks Query executor waits for each query to complete before continuing with the next query for the same event record. It also waits for all queries to complete before starting the queries for the next event record. Depending on the speed of the pipeline and the complexity of the queries, the wait for query completion can slow pipeline performance.

When running multiple queries for an event record, the executor skips the subsequent queries if a query fails.

You can include the following elements in each query that you define for the executor:

Spark SQLUse Spark SQL syntax in a query, such as:

```
COPY INTO <table identifier>
FROM <location>
FILEFORMAT = <format type>
```

You can use any Spark SQL syntax that is valid for Databricks, as described in the [Databricks documentation](https://docs.databricks.com/spark/latest/spark-sql/index.html).

DP expression language functions

You can include a subset of the functions provided with the DP expression language in a query. For example, when you define the location to copy the data from, you might use the `record:value()` function to define the location as follows:

```
FROM 's3a://${record:value('/bucket')}/${record:value('/objectKey')}'
```

When entering the query in the executor, press Ctrl + Space Bar to view the list of valid functions you can use.

### Storage Connection <a href="#ariaid-title9" id="ariaid-title9"></a>

The Databricks Query executor can connect to a storage location before running Spark SQL queries.

Configure one of the following storage locations:

**None**

When using the executor to run a Spark SQL query that does not require connecting to a storage location, set the storage location to None.

Or, when using the executor to run a query that requires connecting to a storage location and the Databricks cluster has the necessary Amazon S3 or Azure storage connection information configured, then set the storage location to None. In this case, the Databricks cluster connects to the storage location when it runs the Spark SQL query.

**Amazon S3**

When using the executor to run a Spark SQL query that requires connecting to an Amazon S3 storage location, you can specify the Amazon S3 connection information in the executor properties. Any connection information specified in the executor properties takes precedence over the connection information configured in the Databricks cluster.

After selecting Amazon S3 as the storage location, you specify the AWS access key pair that the executor uses to connect to Amazon S3.

**ADLS Gen2**

When using the executor to run a Spark SQL query that requires connecting to an Azure Data Lake Storage Gen2 storage location, you can specify the Azure connection information in the executor properties. Any connection information specified in the executor properties takes precedence over the connection information configured in the Databricks cluster.

After selecting ADLS Gen2 as the storage location, you configure the executor to use the appropriate authentication method to connect to Azure Data Lake Storage Gen2.

#### ADLS Gen2 Authentication Information <a href="#ariaid-title10" id="ariaid-title10"></a>

When you configure the executor to connect to an ADLS Gen2 storage location, you select the authentication method that the executor uses to connect to Azure Data Lake Storage Gen2.

Select one of the following authentication methods:

OAuth 2.0Connections made with OAuth 2.0 authentication require the following information:

*   Application ID - Application ID for the Azure Active Directory Data Pipelines application. Also known as the client ID.

    For information on accessing the application ID from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key).
* Auth Token Endpoint - OAuth 2.0 token endpoint for the Azure Active Directory v1.0 application for Data Pipelines. For example: `https://login.microsoftonline.com/<uuid>/oauth2/token.`
*   Application Key - Authentication key or client secret for the Azure Active Directory application. Also known as the client secret.

    For information on accessing the application key from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key).

Shared KeyConnections made with Shared Key authentication require the following information:

* Account FQDN - Fully qualified domain name of the Azure Data Lake Storage Gen2 account. For example: `<account-name>.dfs.core.windows.net`.
*   Account Shared Key - Shared access key that Azure generated for the storage account.

    For more information on accessing the shared access key from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-manage#access-keys).

### Event Generation <a href="#ariaid-title11" id="ariaid-title11"></a>

The Databricks Query executor can generate events that you can use in an event stream. When you enable event generation, the executor generates events for each successful or failed query.

Databricks Query executor events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title12" id="ariaid-title12"></a>

Event records generated by the Databricks Query executor have the following event-related record header attributes. Record header attributes are stored as String values.

| Record Header Attribute       | Description                                                                                                                                                                                  |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following event types:</p><ul><li>successful-query - Generated after a query successfully completes.</li><li>failed-query - Generated after a query fails.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                 |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                            |

The Databricks Query executor can generate the following types of event records:Successful query

The executor generates a successful-query event record after successfully completing a query.

Successful-query event records have the `sdc.event.type` record header attribute set to `sucessful-query` and include the following fields:

| Event Field Name | Description                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------ |
| query            | Query completed.                                                                                             |
| query-result     | Number of rows affected by query. Included if the Include Query Result Count in Events property is selected. |

Failed query

The executor generates a failed-query event record after failing to complete a query.

Failed-query event records have the `sdc.event.type` record header attribute set to `failed-query` and include the following field:

| Event Field Name | Description      |
| ---------------- | ---------------- |
| query            | Query attempted. |
