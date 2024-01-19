# Accessing Hashicorp Vault Secrets with Vault Functions (Deprecated)

BigConnect Pipelines can use Hashicorp Vault functions to access information, called secrets, stored in Hashicorp Vault. However, the Vault functions are now deprecated and will be removed in a future release. We recommend using the credential functions available with the Vault credential store integration.

You can use Vault secrets in place of username and password properties, similar properties such as AWS access key IDs and secret access keys, and in HTTP headers and bodies when using HTTPS.

To access Vault secrets using the Vault functions, configure properties for BigConnect Pipelines to access Vault. Then, use Vault functions in the pipeline to access Vault at run time.

To access information stored in Hashicorp Vault, perform the following steps:

1. Configure the Hashicorp Vault connection properties.
2. In the pipeline, use Vault functions to access Hashicorp Vault secrets.

**Note**: This documentation includes details about Hashicorp Vault to simplify the installation and configuration process. For more information, see the Vault documentation.

### Step 1. Configure Hashicorp Vault Properties <a href="#ariaid-title2" id="ariaid-title2"></a>

To enable BigConnect Pipelines to connect to Hashicorp Vault, configure the following files located in the `$SDC_CONF` directory:

**Vault properties file**\
\
The `vault.properties` file contains a single Vault EL property that you must uncomment to use the deprecated Vault functions.\
\
**Credential stores properties file**\
\
The `credential-stores.properties` file contains the remaining Vault properties. The Vault server URL, Role ID, and Secret ID are required properties. Configure other properties as needed.\
\
**Note**: The `credentialStore.vault.def` and `credentialStore.vault.config.pathKey.separator` properties are not used by the Vault functions. You can ignore these properties.\
\
Configure the following properties in the file:

| Vault Property                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| credentialStore.\<cstore ID>.config.addr                        | <p>Required. Vault server URL entered in the following format:</p><pre><code>https://&#x3C;host name>:&#x3C;port number>
</code></pre><p>Use HTTPS to avoid unencrypted communication.</p>                                                                                                                                                                                                                                                                                                                                        |
| credentialStore.\<cstore ID>.config.role.id                     | <p>Required. Vault Role ID that BigConnect Pipelines uses to authenticate with Vault. <br>The Role ID is configured within Vault by your Vault administrator.</p><p></p><p>The BigConnect Pipelines Vault integration relies on Vault's App Role authentication backend.<br><br><strong>Important</strong>: The App ID authentication backend has been deprecated by Hashicorp and will be removed in a future release. As a result, do not configure the credentialStore.vault.config.app.id property for new installations.</p> |
| credentialStore.\<cstore ID>.config.secret.id                   | <p>Required. Vault Secret ID that BigConnect Pipelines uses to authenticate with Vault. The Secret ID is configured within Vault by your Vault administrator.<br></p><p>To protect the Secret ID, store the Secret ID in an external location and then use a function to retrieve the Secret ID.<br></p><p>Default uses the <code>file</code> function to retrieve the Secret ID from vault-secret-id in the <code>$SDC_CONF</code> directory.</p>                                                                                |
| credentialStore.\<cstore ID>.config.lease.renewal.interval.sec  | <p>Optional. Seconds to wait before checking for leases that need renewal.<br></p><p>Default is 60.</p>                                                                                                                                                                                                                                                                                                                                                                                                                           |
| credentialStore.\<cstore ID>.config.lease.expiration.buffer.sec | <p>Optional. Buffer for expiring leases. BigConnect Pipelines renews leases that expire in less than the specified number of seconds.<br></p><p>Default is 120.</p>                                                                                                                                                                                                                                                                                                                                                               |
| credentialStore.\<cstore ID>.config.open.timeout                | <p>Optional. Timeout to establish an HTTP connection to Vault in milliseconds.<br></p><p>Default is 0 for no limit.</p>                                                                                                                                                                                                                                                                                                                                                                                                           |
| credentialStore.\<cstore ID>.config.proxy.address               | Optional. Proxy URL. Configure to use a proxy to access Vault.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| credentialStore.\<cstore ID>.config.proxy.port                  | Optional. Proxy port. Configure to use a proxy to access Vault.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| credentialStore.\<cstore ID>.config.proxy.username              | Optional. Proxy username. Configure to use a proxy to access Vault.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| credentialStore.\<cstore ID>.config.proxy.password              | <p>Optional. Proxy password. Configure to use a proxy to access Vault.<br></p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                                        |
| credentialStore.\<cstore ID>.config.read.timeout                | <p>Optional. Milliseconds to wait for data before timing out.<br></p><p>Default is 0 for no limit.</p>                                                                                                                                                                                                                                                                                                                                                                                                                            |
| credentialStore.\<cstore ID>.config.ssl.enabled.protocols       | <p>Optional. SSL/TLS-enabled protocols. Versions TLSv1.2 or later are recommended.</p><p><br>Default is TLSv1.2,TLSv1.3.</p>                                                                                                                                                                                                                                                                                                                                                                                                      |
| credentialStore.\<cstore ID>.config.ssl.truststore.file         | Optional. Path to a Java truststore file. Required when using a private CA or certificates not trusted by the Java default truststore.                                                                                                                                                                                                                                                                                                                                                                                            |
| credentialStore.\<cstore ID>.config.ssl.truststore.password     | <p>Optional. Password for the truststore file.</p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                                                                    |
| credentialStore.\<cstore ID>.config.ssl.verify                  | <p>Optional. Whether to verify that the Vault server hostname matches its certificate.</p><p>Default is true. False is not recommended.</p>                                                                                                                                                                                                                                                                                                                                                                                       |
| credentialStore.\<cstore ID>.config.ssl.timeout                 | <p>Optional. Timeout for the SSL/TLS handshake in milliseconds.<br></p><p>Default is 0 for no limit.</p>                                                                                                                                                                                                                                                                                                                                                                                                                          |
| credentialStore.\<cstore ID>.config.timeout                     | <p>Optional. Timeout to read from Vault in milliseconds, after a connection has been established.<br></p><p>Default is 0 for no limit.</p>                                                                                                                                                                                                                                                                                                                                                                                        |

Restart BigConnect Pipelines to enable the changes to both files.

### Step 2. Call Hashicorp Vault from the Pipeline <a href="#ariaid-title3" id="ariaid-title3"></a>

After enabling BigConnect Pipelines to access Hashicorp Vault, use the Vault functions in pipeline and stage properties to access Vault secrets.

The expression language provides Vault functions to return Vault secrets. You can use Vault functions in username, password, and similar properties such as AWS access key IDs and secret access keys. You can also use the functions in HTTP headers and bodies when using HTTPS.

You can use the following functions to access Vault secrets:\
\
**vault:read()**\
\
Use to return the value for the Vault path and key that you provide. Typically, you'll use this function to access secrets.\
\
**vault:readWithDelay()**\
\
Use to return the value for the specified Vault path and key with a delay. Use this function to incorporate a delay in the response to allow time for other processes to complete.\
\
For example, you should use this function when using the Vault AWS secret backend to generate AWS access credentials based on IAM policies. According to Vault documentation, you might need a delay of 10 seconds or more before the credentials can be used successfully.

For more information, see Miscellaneous Functions.
