# Hashicorp Vault

To use the Hashicorp Vault credential store system, install the Vault credential store stage library and define the configuration properties used to connect to Hashicorp Vault. Then, use credential functions in pipeline stage properties to retrieve stored values.

**Note**: This documentation includes details about Hashicorp Vault to simplify the configuration process. For more information, see the Vault documentation.

#### Step 1. Install the Credential Store Stage Library <a href="#ariaid-title13" id="ariaid-title13"></a>

By default, a full BigConnect Pipelines installation includes the Hashicorp Vault keystore credential store stage library. The core installation does not include the library.

To verify that a BigConnect Pipelines has the Hashicorp Vault credential store stage library installed, click the Package Manager icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PackageManager.png)) to display the list of installed stage libraries. If the library is not installed, install the library before configuring the Hashicorp Vault credential store.

#### Step 2. Configure the Credential Store Properties <a href="#ariaid-title14" id="ariaid-title14"></a>

To enable BigConnect Pipelines to connect to the Hashicorp Vault credential store, configure the Hashicorp Vault properties in the `$SDC_CONF/credential-stores.properties` file.

**Important**: For a Cloudera Manager installation, configure all credential store properties through Cloudera Manager. In Cloudera Manager, select the BigConnect Pipelines service and then click Configuration. Add a line for each credential store property to the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc.properties field as follows:

```
credentialStores=vault
```

1.  Uncomment the **credentialStores** property in the file and specify the credential store ID to use. Use only alphabetic characters for the credential store ID.

    \
    By default, the property lists a default credential store ID for each type of credential store, `aws` for AWS Secrets Manager, `azure` for Azure Key Vault, and so on. When using one credential store of any type, it's simplest to use the default value.

    \
    To use just a single Hashicorp Vault credential store, set the value to `vault`.

    T\
    o enable multiple credential stores, specify comma-separated list of credential store IDs. For example, to use a Java keystore and a Hashicorp Vault credential store, set the value to jks,vault. To use multiple Hashicorp Vault credential stores, simply specify separate IDs for each, such as `vaultDev,vaultProd`.
2.  Uncomment and configure the following properties as needed.\


    If you specified a custom credential store ID, update the names of the following properties, and then configure them as needed. When using the default credential store ID, `vault`, leave the property names intact, and simply configure the properties.

    T\
    o use multiple Hashicorp Vault credential stores, make a copy of the properties for each credential store. Then, update the credential store ID in each set of property names before defining the properties. For an example, see Enabling Credential Stores.\
    \
    **Important**: Instead of entering sensitive data such as passwords in clear text in the configuration file, you can protect the sensitive data by storing the data in an external location and then using functions to retrieve the data.\


    These properties are grouped in the Hashicorp Vault section of the file:

    | Vault Property                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | credentialStore.\<cstore ID>.def                                | <p>Required. Defines the implementation of the Vault credential store.<br></p><p>Do not change the default value.</p>                                                                                                                                                                                                                                                                                                                                                                                                            |
    | credentialStore.\<cstore ID>.config.pathKey.separator           | <p>Optional. Separator to use in the <code>name</code> argument that credential functions use.</p><p>Use the following format for the <code>name</code> argument:</p><pre><code>&#x3C;path>&#x3C;separator>&#x3C;key>
    </code></pre><p>For example, if you keep the default ampersand (&#x26;), the format for the name argument is:</p><pre><code>&#x3C;path>&#x26;&#x3C;key>
    </code></pre>                                                                                                                                      |
    | credentialStore.\<cstore ID>.config.addr                        | <p>Required. Vault server URL entered in the following format:</p><pre><code>https://&#x3C;host name>:&#x3C;port number>
    </code></pre><p>Use HTTPS to avoid unencrypted communication.</p>                                                                                                                                                                                                                                                                                                                                       |
    | credentialStore.\<cstore ID>.config.role.id                     | <p>Required. Vault Role ID that BigConnect Pipelines uses to authenticate with Vault. <br><br>The Role ID is configured within Vault by your Vault administrator. <br><br>The BigConnect Pipelines Vault integration relies on Vault's App Role authentication backend.<br><br><strong>Important</strong>: The App ID authentication backend has been deprecated by Hashicorp and will be removed in a future release. As a result, do not configure the credentialStore.vault.config.app.id property for new installations.</p> |
    | credentialStore.\<cstore ID>.config.secret.id                   | <p>Required. Vault Secret ID that BigConnect Pipelines uses to authenticate with Vault. The Secret ID is configured within Vault by your Vault administrator.<br></p><p>To protect the Secret ID, store the Secret ID in an external location and then use a function to retrieve the Secret ID.</p><p><br>Default uses the <code>file</code> function to retrieve the Secret ID from vault-secret-id in the <code>$SDC_CONF</code> directory.</p>                                                                               |
    | credentialStore.\<cstore ID>.config.lease.renewal.interval.sec  | <p>Optional. Seconds to wait before checking for leases that need renewal.<br></p><p>Default is 60.</p>                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | credentialStore.\<cstore ID>.config.lease.expiration.buffer.sec | <p>Optional. Buffer for expiring leases. BigConnect Pipelines renews leases that expire in less than the specified number of seconds.<br></p><p>Default is 120.</p>                                                                                                                                                                                                                                                                                                                                                              |
    | credentialStore.\<cstore ID>.config.open.timeout                | <p>Optional. Timeout to establish an HTTP connection to Vault in milliseconds.<br></p><p>Default is 0 for no limit.</p>                                                                                                                                                                                                                                                                                                                                                                                                          |
    | credentialStore.\<cstore ID>.config.proxy.address               | Optional. Proxy URL. Configure to use a proxy to access Vault.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | credentialStore.\<cstore ID>.config.proxy.port                  | Optional. Proxy port. Configure to use a proxy to access Vault.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | credentialStore.\<cstore ID>.config.proxy.username              | Optional. Proxy username. Configure to use a proxy to access Vault.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | credentialStore.\<cstore ID>.config.proxy.password              | <p>Optional. Proxy password. Configure to use a proxy to access Vault.<br></p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                                       |
    | credentialStore.\<cstore ID>.config.read.timeout                | <p>Optional. Milliseconds to wait for data before timing out.<br></p><p>Default is 0 for no limit.</p>                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | credentialStore.\<cstore ID>.config.ssl.enabled.protocols       | <p>Optional. SSL/TLS-enabled protocols. Versions TLSv1.2 or later are recommended.<br></p><p>Default is TLSv1.2,TLSv1.3.</p>                                                                                                                                                                                                                                                                                                                                                                                                     |
    | credentialStore.\<cstore ID>.config.ssl.truststore.file         | Optional. Path to a Java truststore file. Required when using a private CA or certificates not trusted by the Java default truststore.                                                                                                                                                                                                                                                                                                                                                                                           |
    | credentialStore.\<cstore ID>.config.ssl.truststore.password     | <p>Optional. Password for the truststore file.</p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                                                                   |
    | credentialStore.\<cstore ID>.config.ssl.verify                  | <p>Optional. Whether to verify that the Vault server hostname matches its certificate.</p><p>Default is true. False is not recommended.</p>                                                                                                                                                                                                                                                                                                                                                                                      |
    | credentialStore.\<cstore ID>.config.ssl.timeout                 | <p>Optional. Timeout for the SSL/TLS handshake in milliseconds.<br></p><p>Default is 0 for no limit.</p>                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | credentialStore.\<cstore ID>.config.timeout                     | <p>Optional. Timeout to read from Vault in milliseconds, after a connection has been established.<br></p><p>Default is 0 for no limit.</p>                                                                                                                                                                                                                                                                                                                                                                                       |
    | credentialStore.\<cstore ID>.config.enforceEntryGroup           | <p>Optional. Requires BigConnect Pipelines to verify if the user who previews, validates, or starts the pipeline belongs to a group that is permitted to access the secret.<br></p><p>When set to true, each secret must have a corresponding <code>&#x3C;secret name>-groups</code> secret that contains a comma-separated list of groups that is permitted to access the secret.<br></p><p>For more information, see Group Access to Secrets.<br></p><p>Default is false.</p>                                                  |
3. Restart BigConnect Pipelines to enable the changes.

#### Step 3. Call Secrets from the Pipeline <a href="#ariaid-title15" id="ariaid-title15"></a>

Use the `credential:get()` or `credential:getWithOptions()` function in pipeline stage properties to retrieve secrets from Hashicorp Vault.

Use the credential functions in any stage property that displays the key icon next to it. For example:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/CredentialStores\_KeyIcon.png)

**Important**: When you use a credential function in a stage property, the function must be the only value defined in the property. For example, you cannot include another function or a literal value along with the credential function.\
\
The credential functions use the following arguments:

* cstoreId - Unique ID of the credential store to use. Use the ID specified in the $SDC\_CONF/credential-stores.properties file. For more information, see [Enabling Credential Stores](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Configuration/CredentialStores.html#concept\_em2\_xfk\_xlb).
*   userGroup - Group that a user must belong to in order to access the secret. Only users that have execute permission on the pipeline and that belong to this group can validate, preview, or run the pipeline that retrieves the secret.\


    If working with Control Hub, specify the group using the required naming convention: `<group ID>@<organization ID>`.\


    To grant access to all users, specify the default `all` group when working only with BigConnect Pipelines. When working with Control Hub and BigConnect Pipelines version 3.16.0 or later, you can specify the default group using `all` or `all@<organization ID>`. BigConnect Pipelines recommends using `all` so that you do not need to modify credential functions when migrating pipelines from BigConnect Pipelines to Control Hub.\
    \
    **Note**: When working with Control Hub and a BigConnect Pipelines version earlier than 3.16.0, you must use the default `all@<organization ID>` group.
* name - Name of the secret to retrieve from Hashicorp Vault. Use the following format: `"<path><separator><key>"`, where:
  * `<path>` is the path in Vault to read.
  * `<separator>` is the separator defined for the path and key values in the $SDC\_CONF/credential-stores.properties file.
  * `<key>` is the key for the secret that you want returned.
*   storeOptions - Used only by the `credential:getWithOptions()` function. Additional options to communicate with the credential store. For Hashicorp Vault, you can enter a delay in milliseconds to allow time for external processing. Use the delay option when using the Vault AWS secret backend to generate AWS access credentials based on IAM policies. According to Vault documentation, you might need a delay of 10 seconds or more before the credentials can be used successfully.\


    Use the following format to specify an option:

    ```
    "<option>=<option>"
    ```

    For example, to set the Vault delay to 1,000 milliseconds, enter the following for the options argument:

    ```
    "delay=1000"
    ```

For example, the following expression returns the value of the key password stored in the Vault path `/secret/databases/oracle` from the `vault` credential store after waiting for a delay of 1,000 milliseconds. The name argument uses the default ampersand (&) as the separator. The expression allows any user belonging to the `devops` group access to the secret when validating, previewing, or running the pipeline:

```
${credential:getWithOptions("vault", "devops", "/secret/databases/oracle&password", "delay=1000")}
```
