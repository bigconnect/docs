# Legacy Azure Storage support

The [Hive connector](https://trino.io/docs/current/connector/hive.html) can be configured to use [Azure Data Lake Storage (Gen2)](https://azure.microsoft.com/products/storage/data-lake-storage/). Trino supports Azure Blob File System (ABFS) to access data in ADLS Gen2.

### Hive connector configuration for Azure Storage credentials[#](https://trino.io/docs/current/object-storage/legacy-azure.html#hive-connector-configuration-for-azure-storage-credentials) <a href="#hive-connector-configuration-for-azure-storage-credentials" id="hive-connector-configuration-for-azure-storage-credentials"></a>

To configure Trino to use the Azure Storage credentials, set the following configuration properties in the catalog properties file. It is best to use this type of configuration if the primary storage account is linked to the cluster.

The specific configuration depends on the type of storage and uses the properties from the following sections in the catalog properties file.

For more complex use cases, such as configuring multiple secondary storage accounts using Hadoop’s `core-site.xml`, see the [Advanced configuration](https://trino.io/docs/current/object-storage/legacy-azure.html#hive-azure-advanced-config) options.

#### ADLS Gen2 / ABFS storage[#](https://trino.io/docs/current/object-storage/legacy-azure.html#adls-gen2-abfs-storage) <a href="#adls-gen2-abfs-storage" id="adls-gen2-abfs-storage"></a>

To connect to ABFS storage, you may either use the storage account’s access key, or a service principal. Do not use both sets of properties at the same time.

| Property name                     | Description                                                |
| --------------------------------- | ---------------------------------------------------------- |
| `hive.azure.abfs-storage-account` | The name of the ADLS Gen2 storage account                  |
| `hive.azure.abfs-access-key`      | The decrypted access key for the ADLS Gen2 storage account |

| Property name                     | Description                                                          |
| --------------------------------- | -------------------------------------------------------------------- |
| `hive.azure.abfs.oauth.endpoint`  | The service principal / application’s OAuth 2.0 token endpoint (v1). |
| `hive.azure.abfs.oauth.client-id` | The service principal’s client/application ID.                       |
| `hive.azure.abfs.oauth.secret`    | A client secret for the service principal.                           |

When using a service principal, it must have the Storage Blob Data Owner, Contributor, or Reader role on the storage account you are using, depending on which operations you would like to use.

#### Advanced configuration[#](https://trino.io/docs/current/object-storage/legacy-azure.html#advanced-configuration) <a href="#advanced-configuration" id="advanced-configuration"></a>

All of the configuration properties for the Azure storage driver are stored in the Hadoop `core-site.xml` configuration file. When there are secondary storage accounts involved, we recommend configuring Trino using a `core-site.xml` containing the appropriate credentials for each account.

The path to the file must be configured in the catalog properties file:

```
hive.config.resources=<path_to_hadoop_core-site.xml>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

One way to find your account key is to ask for the connection string for the storage account. The `abfsexample.dfs.core.windows.net` account refers to the storage account. The connection string contains the account key:

```
az storage account  show-connection-string --name abfswales1
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=abfsexample;AccountKey=examplekey..."
}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

When you have the account access key, you can add it to your `core-site.xml` or Java cryptography extension (JCEKS) file. Alternatively, you can have your cluster management tool to set the option `fs.azure.account.key.STORAGE-ACCOUNT` to the account key value:

```
<property>
  <name>fs.azure.account.key.abfsexample.dfs.core.windows.net</name>
  <value>examplekey...</value>
</property>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

For more information, see [Hadoop Azure Support: ABFS](https://hadoop.apache.org/docs/stable/hadoop-azure/abfs.html).

### Accessing Azure Storage data[#](https://trino.io/docs/current/object-storage/legacy-azure.html#accessing-azure-storage-data) <a href="#accessing-azure-storage-data" id="accessing-azure-storage-data"></a>

#### URI scheme to reference data[#](https://trino.io/docs/current/object-storage/legacy-azure.html#uri-scheme-to-reference-data) <a href="#uri-scheme-to-reference-data" id="uri-scheme-to-reference-data"></a>

Consistent with other FileSystem implementations within Hadoop, the Azure Standard Blob and Azure Data Lake Storage Gen2 (ABFS) drivers define their own URI scheme so that resources (directories and files) may be distinctly addressed. You can access both primary and secondary storage accounts linked to the cluster with the same URI scheme. Following are example URIs for the different systems.

ABFS URI:

```
abfs[s]://<file_system>@<account_name>.dfs.core.windows.net/<path>/<path>/<file_name>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

ADLS Gen1 URI:

```
adl://<data_lake_storage_gen1_name>.azuredatalakestore.net/<path>/<file_name>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Azure Standard Blob URI:

```
wasb[s]://<container>@<account_name>.blob.core.windows.net/<path>/<path>/<file_name>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Querying Azure Storage[#](https://trino.io/docs/current/object-storage/legacy-azure.html#querying-azure-storage) <a href="#querying-azure-storage" id="querying-azure-storage"></a>

You can query tables already configured in your Hive metastore used in your Hive catalog. To access Azure Storage data that is not yet mapped in the Hive metastore, you need to provide the schema of the data, the file format, and the data location.

For example, if you have ORC or Parquet files in an ABFS `file_system`, you need to execute a query:

```
-- select schema in which the table is to be defined, must already exist
USE hive.default;

-- create table
CREATE TABLE orders (
     orderkey BIGINT,
     custkey BIGINT,
     orderstatus VARCHAR(1),
     totalprice DOUBLE,
     orderdate DATE,
     orderpriority VARCHAR(15),
     clerk VARCHAR(15),
     shippriority INTEGER,
     comment VARCHAR(79)
) WITH (
     external_location = 'abfs[s]://<file_system>@<account_name>.dfs.core.windows.net/<path>/<path>/',
     format = 'ORC' -- or 'PARQUET'
);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Now you can query the newly mapped table:

```
SELECT * FROM orders;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Writing data[#](https://trino.io/docs/current/object-storage/legacy-azure.html#writing-data) <a href="#writing-data" id="writing-data"></a>

#### Prerequisites[#](https://trino.io/docs/current/object-storage/legacy-azure.html#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

Before you attempt to write data to Azure Storage, make sure you have configured everything necessary to read data from the storage.

#### Create a write schema[#](https://trino.io/docs/current/object-storage/legacy-azure.html#create-a-write-schema) <a href="#create-a-write-schema" id="create-a-write-schema"></a>

If the Hive metastore contains schema(s) mapped to Azure storage filesystems, you can use them to write data to Azure storage.

If you don’t want to use existing schemas, or there are no appropriate schemas in the Hive metastore, you need to create a new one:

```
CREATE SCHEMA hive.abfs_export
WITH (location = 'abfs[s]://file_system@account_name.dfs.core.windows.net/<path>');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

#### Write data to Azure Storage[#](https://trino.io/docs/current/object-storage/legacy-azure.html#write-data-to-azure-storage) <a href="#write-data-to-azure-storage" id="write-data-to-azure-storage"></a>

Once you have a schema pointing to a location where you want to write the data, you can issue a `CREATE TABLE AS` statement and select your desired file format. The data will be written to one or more files within the `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/my_table` namespace. Example:

```
CREATE TABLE hive.abfs_export.orders_abfs
WITH (format = 'ORC')
AS SELECT * FROM tpch.sf1.orders;
```
