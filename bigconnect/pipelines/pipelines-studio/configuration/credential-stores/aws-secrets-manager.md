# AWS Secrets Manager

To use the AWS Secrets Manager credential store system, install the AWS Secrets Manager Credentials Store stage library and define the configuration properties used to connect to Secrets Manager. Then, use credential functions in pipeline stage properties to retrieve stored values.

In Secrets Manager, you must configure an access and secret key pair with correct permission to read the key. To follow best practices, make secrets read-only and limit access. See the Secrets Manager documentation on identity and access management (IAM) policies.

**Note**: This documentation includes Secrets Manager information needed for the configuration process. For more information, see the AWS Secrets Manager documentation.

#### Step 1. Install the Credential Store Stage Library <a href="#ariaid-title5" id="ariaid-title5"></a>

By default, a full BigConnect Pipelines installation includes the AWS Secrets Manager Credentials Store stage library. The core installation does not include the library.

To verify that a BigConnect Pipelines has the AWS Secrets Manager Credentials Store stage library installed, click the Package Manager icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PackageManager.png)) to display the list of installed stage libraries. If the library is not installed, install the library before configuring the Secrets Manager credential store.

#### Step 2. Configure the Credential Store Properties <a href="#ariaid-title6" id="ariaid-title6"></a>

To enable BigConnect Pipelines to connect to the AWS Secrets Manager credential store, configure the Secrets Manager properties in the $SDC\_CONF/credential-stores.properties file.

**Important**: For a Cloudera Manager installation, configure all credential store properties through Cloudera Manager. In Cloudera Manager, select the BigConnect Pipelines service and then click Configuration. Add a line for each credential store property to the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc.properties field as follows:

```
credentialStores=aws
```

1.  Uncomment the **credentialStores** property in the file and specify the credential store ID to use. Use only alphabetic characters for the credential store ID.\


    By default, the property lists a default credential store ID for each type of credential store, `aws` for AWS Secrets Manager, `azure` for Azure Key Vault, and so on. When using one credential store of any type, it's simplest to use the default value.

    \
    To use just a single Secrets Manager, set the value to `aws`.

    \
    To enable multiple credential stores, specify comma-separated list of credential store IDs. For example, to use a Java keystore and a Secrets Manager credential store, set the value to jks,aws. To use multiple Secrets Manager credential stores, simply specify separate IDs for each, such as `awsDev,awsProd`.
2.  Uncomment and configure the following properties as needed.

    If you specified a custom credential store ID, update the names of the following properties, and then configure them as needed. When using the default credential store ID, `aws`, leave the property names intact, and simply configure the properties.\


    To use multiple AWS Secrets Manager credential stores, make a copy of the properties for each credential store. Then, update the credential store ID in each set of property names before defining the properties. For an example, see Enabling Credential Stores.\


    **Important**: Instead of entering sensitive data such as passwords in clear text in the configuration file, you can protect the sensitive data by storing the data in an external location and then using functions to retrieve the data.\


    These properties are grouped in the AWS Secrets Manager section of the file:

    | Secrets Manager Property                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | credentialStore.\<cstore ID>.def                      | <p>Required. Defines the implementation of the AWS Secrets Manager credential store.</p><p><br>Do not change the default value.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | credentialStore.\<cstore ID>.config.nameKey.separator | <p>Optional. Separator to use in the <code>name</code> argument that credential functions use. Use the following format for the <code>name</code> argument:</p><p><code>&#x3C;name>&#x3C;separator>&#x3C;key></code></p><p><br>For example, if you keep the default ampersand (<code>&#x26;</code>), the format for the name argument is: <code>&#x3C;name>&#x26;&#x3C;key></code></p><p><br><strong>Note</strong>: In Secrets Manager, names can contain alphanumeric and the following special characters: <code>/ _ + = . @ -</code> . <br>Therefore, avoid using those characters as separators.</p>                                                                                  |
    | credentialStore.\<cstore ID>.config.region            | Required. AWS region that hosts Secrets Manager. For a list of available regions, see the AWS Region Table.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | credentialStore.\<cstore ID>.config.security.method   | <p>Required. Authentication method used to connect to AWS. Set to one of the following values:</p><ul><li><p><code>instanceProfile</code> - Authenticates using an instance profile associated with BigConnect Pipelines.<br></p><p>Use when BigConnect Pipelines runs on an Amazon EC2 instance that has an associated instance profile. BigConnect Pipelines uses the instance profile credentials to automatically authenticate with AWS.</p></li><li><p><code>accessKeys</code> - Authenticates using an AWS access key pair.<br></p><p>Use when BigConnect Pipelines does not run on an Amazon EC2 instance or when the EC2 instance doesn’t have an instance profile.</p></li></ul> |
    | credentialStore.\<cstore ID>.config.access.key        | Required when using access keys to authenticate with AWS. AWS access key ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | credentialStore.\<cstore ID>.config.secret.key        | Required when using access keys to authenticate with AWS. AWS secret access key.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | credentialStore.\<cstore ID>.config.cache.max.size    | Optional. Maximum number of secrets BigConnect Pipelines can cache locally. Default is 1024.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | credentialStore.\<cstore ID>.config.cache.ttl.millis  | Optional. Number of milliseconds that BigConnect Pipelines considers a cached secret valid before requiring a refresh. Default is 1 hour.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | credentialStore.\<cstore ID>.config.enforceEntryGroup | <p>Optional. Requires BigConnect Pipelines to verify if the user who previews, validates, or starts the pipeline belongs to a group that is permitted to access the secret.<br></p><p>When set to true, each secret must have a corresponding <code>&#x3C;secret name>-groups</code> secret that contains a comma-separated list of groups that is permitted to access the secret.<br></p><p>For more information, see Group Access to Secrets.<br></p><p>Default is false.</p>                                                                                                                                                                                                           |
3. Restart BigConnect Pipelines to enable the changes.

#### Step 3. Call Secrets from the Pipeline <a href="#ariaid-title7" id="ariaid-title7"></a>

Use the `credential:get()` or `credential:getWithOptions()` function in pipeline stage properties to retrieve secrets from AWS Secrets Manager.

Use the credential functions in any stage property that displays the key icon next to it. For example:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/CredentialStores\_KeyIcon.png)

**Important**: When you use a credential function in a stage property, the function must be the only value defined in the property. For example, you cannot include another function or a literal value along with the credential function.The credential functions use the following arguments:

* cstoreId - Unique ID of the credential store to use. Use the ID specified in the $SDC\_CONF/credential-stores.properties file. For more information, see Enabling Credential Stores.\

*   userGroup - Group that a user must belong to in order to access the secret. Only users that have execute permission on the pipeline and that belong to this group can validate, preview, or run the pipeline that retrieves the secret.\


    If working with Control Hub, specify the group using the required naming convention: `<group ID>@<organization ID>`.\


    To grant access to all users, specify the default `all` group when working only with BigConnect Pipelines. When working with Control Hub and BigConnect Pipelines version 3.16.0 or later, you can specify the default group using `all` or `all@<organization ID>`. BigConnect Pipelines recommends using `all` so that you do not need to modify credential functions when migrating pipelines fromBigConnect Pipelines to Control Hub.\
    \
    **Note**: When working with Control Hub and a BigConnect Pipelines version earlier than 3.16.0, you must use the default `all@<organization ID>` group.\

* name - Name of the secret to retrieve from Secrets Manager. Use the following format: `"<name><separator><key>"`, where:
  * `<name>` is the name of the secret in Secrets Manager to read.
  * `<separator>` is the separator defined, either in the $SDC\_CONF/credential-stores.properties file or in the function call.
  * `<key>` is the key for the value that you want returned.\

*   storeOptions - Used only by the `credential:getWithOptions()` function. Additional options to communicate with the credential store. For Secrets Manager, you can use the following options:

    * `separator` - Specifies the separator for name and key values in the credential functions, overriding the `credentialStore.aws.config.nameKey.separator` property.
    * `alwaysRefresh` - When set to `true`, forces the key to refresh its cached value before BigConnect Pipelines retrieves the value, overriding the `credentialStore.aws.config.cache.ttl.millis` property.

    Use the following format to specify options:

    ```
    "<option1>=<value>,<option2>=<value>"
    ```

    For example, to use the pipe symbol ( | ) as the separator, enter the following for the options argument:

    ```
    "separator=|"
    ```

For example, the following expression returns the value from the key `SQLk1` of the secret `SQLpassword` from the `aws` credential store. The expression allows any user in the `devops` group to access the key when validating, previewing, or running the pipeline:

```
${credential:get("aws", "devops", "SQLpassword&SQLk1")}
```

The following expression returns the same key value, but overrides the separator to use a pipe:

```
${credential:getWithOptions("aws", "devops", "SQLpassword|SQLk1", "separator=|")}
```
