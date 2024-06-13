# Azure Storage file system support

Trino includes a native implementation to access Azure Storage with a catalog using the Delta Lake, Hive, Hudi, or Iceberg connectors.

Enable the native implementation with `fs.native-azure.enabled=true` in your catalog properties file.

### General configuration <a href="#general-configuration" id="general-configuration"></a>

Use the following properties to configure general aspects of Azure Storage file system support:

| Property                       | Description                                                                                                                                                                                   |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `fs.native-azure.enabled`      | Activate the native implementation for Azure Storage support, and deactivate all legacy support. Defaults to `false`. Must be set to `true` for all other properties be used.                 |
| `azure.auth-type`              | Authentication type to use for Azure Storage access. Defaults no authentication used with `NONE`. Use `ACCESS_KEY` for Access key authentication or and `OAUTH` for OAuth 2.0 authentication. |
| `azure.read-block-size`        | Data size for blocks during read operations. Defaults to `4MB`.                                                                                                                               |
| `azure.write-block-size`       | Data size for blocks during write operations. Defaults to `4MB`.                                                                                                                              |
| `azure.max-write-concurrency`  | Maximum number of concurrent write operations. Defaults to 8.                                                                                                                                 |
| `azure.max-single-upload-size` | Data size Defaults to `4MB`.                                                                                                                                                                  |

### Access key authentication <a href="#access-key-authentication" id="access-key-authentication"></a>

Use the following properties to configure access key authentication to Azure Storage:

| Property           | Description                                                                                         |
| ------------------ | --------------------------------------------------------------------------------------------------- |
| `azure.auth-type`  | Must be set to `ACCESS_KEY`.                                                                        |
| `azure.access-key` | The decrypted access key for the Azure Storage account. Requires authentication type `ACCESSS_KEY`. |

### OAuth 2.0 authentication <a href="#oauth-2-0-authentication" id="oauth-2-0-authentication"></a>

Use the following properties to configure OAuth 2.0 authentication to Azure Storage:

| Property                | Description                                                 |
| ----------------------- | ----------------------------------------------------------- |
| `azure.auth-type`       | Must be set to `OAUTH`.                                     |
| `azure.oauth.tenant-id` | Tenant ID for Azure authentication.                         |
| `azure.oauth.endpoint`  | The endpoint URL for OAuth 2.0 authentication.              |
| `azure.oauth.client-id` | The OAuth 2.0 service principal’s client or application ID. |
| `azure.oauth.secret`    | A OAuth 2.0 client secret for the service principal.        |

\
