# Microsoft Azure Key Vault

Before BigConnect Pipelines can connect to the Microsoft Azure Key Vault credential store system, you must complete several prerequisites in Azure so that BigConnect Pipelines can access the Azure Key Vault as an application.

After completing the prerequisites, install the Azure Key Vault credential store stage library and define the configuration properties used to connect to Azure Key Vault. Then, use credential functions in pipeline stage properties to retrieve stored values.

**Note**: This documentation includes details about Azure Key Vault to simplify the configuration process. For more information, see the Azure Key Vault documentation.

#### Prerequisites <a href="#ariaid-title23" id="ariaid-title23"></a>

Before BigConnect Pipelines can connect to the Microsoft Azure Key Vault credential store system, complete the following prerequisites within Azure:

**Register BigConnect Pipelines with Azure Active Directory**\
\
Use the Azure portal to register BigConnect Pipelines as an application in Azure Active Directory. When an application such as BigConnect Pipelines accesses keys or secrets in an Azure key vault, the application must use an authentication token from Azure Active Directory.The registration process assigns BigConnect Pipelines the following values, which you specify when you configure the credential store properties:

* application ID
* authentication key

For more information about registering applications in Azure Active Directory, see the Azure Key Vault documentation.\
\
**Authorize BigConnect Pipelines to use keys in the Azure key vault**\
\
Use the Azure portal to authorize BigConnect Pipelines to use the keys, or secrets, in the Azure key vault. Azure Key Vault requires that applications be authorized to access each key vault.\
\
For information about authorizing applications to use keys, see the Azure Key Vault documentation.

#### Step 1. Install the Credential Store Stage Library <a href="#ariaid-title24" id="ariaid-title24"></a>

By default, a full BigConnect Pipelines installation includes the Azure Key Vault credential store stage library. The core installation does not include the library.

To verify that a BigConnect Pipelines has the Azure Key Vault credential store stage library installed, click the Package Manager icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PackageManager.png)) to display the list of installed stage libraries. If the library is not installed, install the library before configuring the Azure Key Vault credential store.

#### Step 2. Configure the Credential Store Properties <a href="#ariaid-title25" id="ariaid-title25"></a>

To enable BigConnect Pipelines to connect to the Azure Key Vault credential store, configure the Azure Key Vault properties in the `$SDC_CONF/credential-stores.properties` file.

**Important**: For a Cloudera Manager installation, configure all credential store properties through Cloudera Manager. In Cloudera Manager, select the BigConnect Pipelines service and then click Configuration. Add a line for each credential store property to the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc.properties field as follows:

```
credentialStores=azure
```

1.  Uncomment the **credentialStores** property in the file and specify the credential store ID to use. Use only alphabetic characters for the credential store ID.\


    By default, the property lists a default credential store ID for each type of credential store, `aws` for AWS Secrets Manager, `azure` for Azure Key Vault, and so on. When using one credential store of any type, it's simplest to use the default value.\


    To use just a single Azure Key Vault, set the value to `azure`.\


    To enable multiple credential stores, specify comma-separated list of credential store IDs. For example, to use a Java keystore and an Azure Key Vault credential store, set the value to jks,azure. To use multiple Azure Key Vault credential stores, simply specify separate IDs for each, such as `azureDev,azureProd`.
2.  Uncomment and configure the following properties as needed.\


    If you specified a custom credential store ID, update the names of the following properties, and then configure them as needed. When using the default credential store ID, `azure`, leave the property names intact, and simply configure the properties.\


    To use multiple Azure Key Vault credential stores, make a copy of the properties for each credential store. Then, update the credential store ID in each set of property names before defining the properties. For an example, see Enabling Credential Stores.\
    \
    **Important**: Instead of entering sensitive data such as passwords in clear text in the configuration file, you can protect the sensitive data by storing the data in an external location and then using functions to retrieve the data.\
    \
    These properties are grouped in the Azure Key Vault section of the file:

    | Azure Key Vault Property                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | credentialStore.\<cstore ID>.def                              | <p>Required. Defines the implementation of the Azure Key Vault credential store.<br></p><p>Do not change the default value.</p>                                                                                                                                                                                                                                                                                                                                                 |
    | credentialStore.\<cstore ID>.config.credential.refresh.millis | Optional. Number of milliseconds that BigConnect Pipelines locally caches a credential. When the time expires, BigConnect Pipelines retrieves the credential from Azure Key Vault.                                                                                                                                                                                                                                                                                              |
    | credentialStore.\<cstore ID>.config.credential.retry.millis   | Optional. Number of milliseconds that BigConnect Pipelines waits before attempting to retry a retrieval of a credential from Azure Key Vault, in the case of an error.                                                                                                                                                                                                                                                                                                          |
    | credentialStore.\<cstore ID>.config.vault.url                 | <p>Required. URL to the key vault created in Azure Key Vault.</p><p>Use the following format:</p><pre><code>https://&#x3C;key vault name>.vault.azure.net/
    </code></pre>                                                                                                                                                                                                                                                                                                        |
    | credentialStore.\<cstore ID>.config.client.id                 | Required. Application ID assigned to this BigConnect Pipelines when you registered BigConnect Pipelines as an application in Azure Active Directory, as described in prerequisites.                                                                                                                                                                                                                                                                                             |
    | credentialStore.\<cstore ID>.config.client.key                | Required. Authentication key assigned to this BigConnect Pipelines when you registered BigConnect Pipelines as an application in Azure Active Directory, as described in prerequisites.                                                                                                                                                                                                                                                                                         |
    | credentialStore.\<cstore ID>.config.enforceEntryGroup         | <p>Optional. Requires BigConnect Pipelines to verify if the user who previews, validates, or starts the pipeline belongs to a group that is permitted to access the secret.<br></p><p>When set to true, each secret must have a corresponding <code>&#x3C;secret name>-groups</code> secret that contains a comma-separated list of groups that is permitted to access the secret.<br></p><p>For more information, see Group Access to Secrets.<br></p><p>Default is false.</p> |
3. Restart BigConnect Pipelines to enable the changes.

#### Step 3. Call Secrets from the Pipeline <a href="#ariaid-title26" id="ariaid-title26"></a>

Use the `credential:get()` or `credential:getWithOptions()` function in pipeline stage properties to retrieve keys or secrets from Azure Key Vault.

Use the credential functions in any stage property that displays the key icon next to it. For example:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/CredentialStores\_KeyIcon.png)

**Important**: When you use a credential function in a stage property, the function must be the only value defined in the property. For example, you cannot include another function or a literal value along with the credential function.The credential functions use the following arguments:

* cstoreId - Unique ID of the credential store to use. Use the ID specified in the $SDC\_CONF/credential-stores.properties file. For more information, see Enabling Credential Stores.
*   userGroup - Group that a user must belong to in order to access the secret. Only users that have execute permission on the pipeline and that belong to this group can validate, preview, or run the pipeline that retrieves the secret.\


    If working with Control Hub, specify the group using the required naming convention: `<group ID>@<organization ID>`.\


    To grant access to all users, specify the default `all` group when working only with BigConnect Pipelines. When working with Control Hub and DBigConnect Pipelines version 3.16.0 or later, you can specify the default group using `all` or `all@<organization ID>`. BigConnect Pipelines recommends using `all` so that you do not need to modify credential functions when migrating pipelines from BigConnect Pipelines to Control Hub.\
    \
    **Note**: When working with Control Hub and a BigConnect Pipelines version earlier than 3.16.0, you must use the default `all@<organization ID>` group.\

* name - Name of the key or secret to retrieve from Azure Key Vault.\

*   storeOptions - Used only by the `credential:getWithOptions()` function. Additional options to communicate with the credential store. For Azure Key Vault, you can use the following options:

    * `url` - Overrides the `credentialStore.azure.config.vault.url` property in the $SDC\_CONF/credential-stores.properties file.
    * `retry` - Overrides the `credentialStore.azure.config.credential.retry.millis` property in the $SDC\_CONF/credential-stores.properties file.
    * `refresh` - Overrides the `credentialStore.azure.config.credential.refresh.millis` property in the $SDC\_CONF/credential-stores.properties file.
    * `credentialType=certificate` - Instructs Azure Key Vault to retrieve the stored PEM certificate as a certificate rather than as a secret. Use to retrieve a PEM certificate stored in Azure Key Vault when you configure a stage to use a remote keystore or truststore for SSL/TLS encryption.\


    Use the following format to specify options:

    ```
    "<option1>=<value>,<option2>=<value>"
    ```

For example, the following expression returns the value of the `SQLpassword` secret from the `azure` credential store. The expression allows any user belonging to the `devops` group access to the credential when validating, previewing, or running the pipeline:

```
${credential:get("azure", "devops", "SQLpassword")}
```

The following expression returns the same secret value, but overrides the retry time configured in the `$SDC_CONF/credential-stores.properties` file:

```
${credential:getWithOptions("azure", "devops", "SQLpassword", "retry=3000")}
```
