# Java Keystore

To use the Java keystore credential store system, install the Java keystore credential store stage library and define the configuration properties used to connect to the credential store.

Use the `stagelib-cli jks-credentialstore` command to add credentials to the credential store. Then, use credential functions in pipeline stage properties to retrieve stored values.\
\
**Important**: Use the Java keystore credential store system in development environments only. In a production environment, use one of the other supported credential stores.

A Java keystore credential storage system requires the distribution of a keystore file, which complicates security. Before using a Java keystore system, decide how the keystore will be distributed and consult with your IT security team to ensure that the system meets IT policies.

#### Step 1. Install the Credential Store Stage Library <a href="#ariaid-title17" id="ariaid-title17"></a>

By default, a full BigConnect Pipelines installation includes the Java keystore credential store stage library. The core installation does not include the library.

To verify that a BigConnect Pipelines has the Java keystore credential store stage library installed, click the Package Manager icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PackageManager.png)) to display the list of installed stage libraries. If the library is not installed, install the library before configuring the Java keystore credential store.

#### Step 2. Configure the Credential Store Properties <a href="#ariaid-title18" id="ariaid-title18"></a>

To enable BigConnect Pipelines to connect to the Java keystore credential store, configure the Java keystore properties in the $SDC\_CONF/credential-stores.properties file.

**Important**: For a Cloudera Manager installation, configure all credential store properties through Cloudera Manager. In Cloudera Manager, select the BigConnect Pipelines service and then click Configuration. Add a line for each credential store property to the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc.properties field as follows:

```
credentialStores=jks
```

1.  Uncomment the **credentialStores** property in the file and specify the credential store ID to use. Use only alphabetic characters for the credential store ID.\


    By default, the property lists a default credential store ID for each type of credential store, `aws` for AWS Secrets Manager, `azure` for Azure Key Vault, and so on. When using one credential store of any type, it's simplest to use the default value.\


    To use just a single Java keystore credential store, set the value to `jks`.\


    To enable multiple credential stores, specify comma-separated list of credential store IDs. For example, to use a Java keystore and a Hashicorp Vault credential store, set the value to jks,vault. To use multiple Java keystore credential stores, simply specify separate IDs for each, such as `jksDev1,jksDev2`.
2.  Uncomment and configure the following properties as needed.

    If you specified a custom credential store ID, update the names of the following properties, and then configure them as needed. When using the default credential store ID, `jks`, leave the property names intact, and simply configure the properties.

    To use multiple Java keystore credential stores, make a copy of the properties for each credential store. Then, update the credential store ID in each set of property names before defining the properties. For an example, see Enabling Credential Stores.\
    \
    **Important**: Instead of entering sensitive data such as passwords in clear text in the configuration file, you can protect the sensitive data by storing the data in an external location and then using functions to retrieve the data.\


    These properties are grouped in the Java keystore section of the file:

    | Java Keystore Property                                               | Description                                                                                                                                                                                                                                                                                  |
    | -------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | credentialStore.\<cstore ID>.def                                     | <p>Defines the implementation of the Java Keystore credential store.<br></p><p>Do not change the default value.</p>                                                                                                                                                                          |
    | credentialStore.\<cstore ID>.config.keystore.type                    | <p>Format of the Java keystore file:</p><ul><li>JCEKS</li><li>PKCS12</li></ul><p>Default is PKCS12.</p>                                                                                                                                                                                      |
    | credentialStore.\<cstore ID>.config.keystore.file                    | <p>Path and name of the Java keystore file. Enter an absolute path to the file, or a path relative to the BigConnect Pipelines configuration directory, $SDC_CONF.<br></p><p>Default is jks-credentialStore.pkcs12.</p>                                                                      |
    | credentialStore.\<cstore ID>.config.keystore.storePassword           | <p>Password that BigConnect Pipelines uses to access the Java keystore file.<br></p><p>You must change the default value before using the keystore file.<br></p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p> |
    | credentialStore.\<cstore ID>.config.keystore.file.min.refresh.millis | <p>Milliseconds that BigConnect Pipelines waits before reloading the keystore file.<br></p><p>Default is 10000, or ten seconds.</p>                                                                                                                                                          |
3. Restart BigConnect Pipelines to enable the changes.

#### Step 3. Add Secrets to the Credential Store <a href="#ariaid-title19" id="ariaid-title19"></a>

Use the `stagelib-cli jks-credentialstore` command to add secrets to the Java keystore file. You can add multiple secrets to the file.

Use the command from the $SDC\_DIST directory as follows:

```
bin/streamsets stagelib-cli jks-credentialstore add -i <cstore ID> -n <secret name> -c <secret value>
```

For example, the following command adds a secret named `OracleDBPassword` with the value `278yT6u` to the `devjks` Java keystore credential store:

```
bin/streamsets stagelib-cli jks-credentialstore add -i devjks -n OracleDBPassword -c 278yT6u
```

**Note**: The `stagelib-cli jks-credentialstore` command also includes `delete` and `list` subcommands that you use to manage the secrets defined in the keystore file. For information on using these commands, see jks-credentialstore Command.

#### Step 4. Call Secrets from the Pipeline <a href="#ariaid-title20" id="ariaid-title20"></a>

Use the `credential:get()` function in pipeline stage properties to retrieve secrets from the Java keystore.

Use the credential function in any stage property that displays the key icon next to it. For example:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/CredentialStores\_KeyIcon.png)

**Important**: When you use a credential function in a stage property, the function must be the only value defined in the property. For example, you cannot include another function or a literal value along with the credential function.The `credential:get()` function uses the following arguments:

* cstoreId - Unique ID of the credential store to use. Use the ID specified in the $SDC\_CONF/credential-stores.properties file. For more information, see Enabling Credential Stores.
*   userGroup - Group that a user must belong to in order to access the secret. Only users that have execute permission on the pipeline and that belong to this group can validate, preview, or run the pipeline that retrieves the secret.\


    If working with Control Hub, specify the group using the required naming convention: `<group ID>@<organization ID>`.\


    To grant access to all users, specify the default `all` group when working only with BigConnect Pipelines. When working with Control Hub and BigConnect Pipelines version 3.16.0 or later, you can specify the default group using `all` or `all@<organization ID>`. BigConnect Pipelines recommends using `all` so that you do not need to modify credential functions when migrating pipelines from BigConnect Pipelines to Control Hub.\
    \
    **Note**: When working with Control Hub and a BigConnect Pipelines version earlier than 3.16.0, you must use the default `all@<organization ID>` group.\

* name - Name of the secret to retrieve from the credential store.

For example, the following expression returns the value of the `OracleDBPassword` secret from the `devjks` credential store. It also allows any user belonging to the `devops` group access to the secret when validating, previewing, or running the pipeline:

```
${credential:get("devjks", "devops", "OracleDBPassword")}
```

#### jks-credentialstore Command <a href="#ariaid-title21" id="ariaid-title21"></a>

The `stagelib-cli jks-credentialstore` command provides subcommands to add, list, and delete secrets in the Java keystore credential store.

Any changes made to the Java keystore file take effect immediately. For example, if you change the value of an existing secret in the file, running pipelines that require a new connection to the external system use the updated secret.\
\
**Note**: In previous releases, the `jks-cs` command provided the same subcommands to add, list, and delete secrets in the Java keystore credential store. However, the `jks-cs` command is now deprecated and will be removed in a future release.\
\
You can use the following subcommands with the `stagelib-cli jks-credentialstore` command:\
\
**add**\
\
Adds a secret to the Java keystore credential store.\
\
Use the command from the `$SDC_DIST` directory as follows:

```
bin/streamsets stagelib-cli jks-credentialstore add \
(-i <cstore ID> | --id <cstore ID>) \
(-n <secret name> | --name <secret name>) \
(-c <secret value> | --credential <secret value>)
```

| Add Option                                                                    | Description                                                                                                                                                                               |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>-i &#x3C;cstore ID></p><p>or</p><p>--id &#x3C;cstore ID></p>               | <p>Required. Unique ID for the credential store.</p><p><br>The default ID for a Java keystore is <code>jks</code>.</p>                                                                    |
| <p>-n &#x3C;secret name></p><p>or</p><p>--name &#x3C;secret name></p>         | <p>Required. Name of the secret to add to the Java keystore credential store.<br></p><p>If the name includes non-alphanumeric characters, use single quotation marks around the name.</p> |
| <p>-c &#x3C;secret value></p><p>or</p><p>--credential &#x3C;secret value></p> | <p>Required. Value to add to the Java keystore credential store.<br></p><p>If the value includes non-alphanumeric characters, use single quotation marks around the value.</p>            |

For example, the following command adds a secret named `OracleDBPassword` with the value `df35yT_&5` to the `devjks` Java keystore credential store:

```
bin/streamsets stagelib-cli jks-credentialstore add -i devjks -n OracleDBPassword -c 'df35yT_&5'
```

\
**delete**\
\
Deletes a secret from the Java keystore credential store.\
\
Use the command from the $SDC\_DIST directory as follows:

```
bin/streamsets stagelib-cli jks-credentialstore delete \
(-i <cstore ID> | --id <cstore ID>) \
(-n <secret name> | --name <secret name>)
```

| Delete Option                                                         | Description                                                                                                                                                                                    |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>-i &#x3C;cstore ID></p><p>or</p><p>--id &#x3C;cstore ID></p>       | <p>Required. Unique ID for the credential store.<br></p><p>The default ID for a Java keystore is <code>jks</code>.</p>                                                                         |
| <p>-n &#x3C;secret name></p><p>or</p><p>--name &#x3C;secret name></p> | <p>Required. Name of the secret to delete from the Java keystore credential store.<br></p><p>If the name includes non-alphanumeric characters, use single quotation marks around the name.</p> |

For example, the following command deletes a secret named `SQLServerDBPassword` from the `devjks` Java keystore credential store:

```
bin/streamsets stagelib-cli jks-credentialstore delete -i devjks -n SQLServerDBPassword
```

\
**list**\
\
Lists the names of all secrets defined in the Java keystore credential store. The command does not list the values.\
\
Use the command from the $SDC\_DIST directory as follows:

```
bin/streamsets stagelib-cli jks-credentialstore list \
(-i <cstore ID> | --id <cstore ID>)
```

| List Option                                                     | Description                                                                                                            |
| --------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| <p>-i &#x3C;cstore ID></p><p>or</p><p>--id &#x3C;cstore ID></p> | <p>Required. Unique ID for the credential store.</p><p><br>The default ID for a Java keystore is <code>jks</code>.</p> |

For example, the following command lists the names of all secrets defined in the `devjks` Java keystore credential store:

```
bin/streamsets stagelib-cli jks-credentialstore list -i devjks
```
