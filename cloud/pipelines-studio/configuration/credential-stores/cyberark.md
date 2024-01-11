# CyberArk

To use the CyberArk credential store system, install the CyberArk credential store stage library and define the configuration properties used to connect to CyberArk Application Identity Manager. Then, use credential functions in pipeline stage properties to retrieve stored values.

At this time, CyberArk integration is only supported using web services to the CyberArk Central Credential Provider.\
\
**Note**: This documentation includes details about CyberArk to simplify the configuration process. For more information, see the CyberArk documentation.

#### Step 1. Install the Credential Store Stage Library <a href="#ariaid-title9" id="ariaid-title9"></a>

By default, a full BigConnect Pipelines installation includes the CyberArk credential store stage library. The core installation does not include the library.

To verify that a BigConnect Pipelines has the CyberArk credential store stage library installed, click the Package Manager icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PackageManager.png)) to display the list of installed stage libraries. If the library is not installed, install the library before configuring the CyberArk credential store.

#### Step 2. Configure the Credential Store Properties <a href="#ariaid-title10" id="ariaid-title10"></a>

To enable BigConnect Pipelines to connect to the CyberArk credential store, configure the CyberArk properties in the $SDC\_CONF/credential-stores.properties file.

**Important**: For a Cloudera Manager installation, configure all credential store properties through Cloudera Manager. In Cloudera Manager, select the BigConnect Pipelines service and then click Configuration. Add a line for each credential store property to the BigConnect Pipelines Advanced Configuration Snippet (Safety Valve) for sdc.properties field as follows:

```
credentialStores=cyberark
```

1.  Uncomment the **credentialStores** property in the file and specify the credential store ID to use. Use only alphabetic characters for the credential store ID.

    \
    By default, the property lists a default credential store ID for each type of credential store, `aws` for AWS Secrets Manager, `azure` for Azure Key Vault, and so on. When using one credential store of any type, it's simplest to use the default value.

    \
    To use just a single CyberArk credential store, set the value to `cyberark`.

    \
    To enable multiple credential stores, specify comma-separated list of credential store IDs. For example, to use a Java keystore and a CyberArk credential store, set the value to jks,cyberark. To use multiple CyberArk credential stores, simply specify separate IDs for each, such as `cyberarkDev,cyberarkProd`.
2.  Uncomment and configure the following properties as needed.

    If you specified a custom credential store ID, update the names of the following properties, and then configure them as needed. When using the default credential store ID, `cyberark`, leave the property names intact, and simply configure the properties.

    To use multiple CyberArk credential stores, make a copy of the properties for each credential store. Then, update the credential store ID in each set of property names before defining the properties. For an example, see Enabling Credential Stores.\
    \
    **Important**: Instead of entering sensitive data such as passwords in clear text in the configuration file, you can protect the sensitive data by storing the data in an external location and then using functions to retrieve the data.\


    These properties are grouped in the CyberArk section of the file:\


    | CyberArk Property                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | credentialStore.\<cstore ID>.def                                      | <p>Required. Defines the implementation of the CyberArk credential store.</p><p><br>Do not change the default value.</p>                                                                                                                                                                                                                                                                                                                                                                                         |
    | credentialStore.\<cstore ID>.config.credential.refresh.millis         | Optional. Number of milliseconds that BigConnect Pipelines locally caches a credential. When the time expires, BigConnect Pipelines retrieves the credential from CyberArk.                                                                                                                                                                                                                                                                                                                                      |
    | credentialStore.\<cstore ID>.config.credential.retry.millis           | Optional. Number of milliseconds that BigConnect Pipelines waits before attempting to retry a retrieval of a credential from CyberArk, in the case of an error.                                                                                                                                                                                                                                                                                                                                                  |
    | credentialStore.\<cstore ID>.config.connector                         | Optional. Connector type to CyberArk. Leave the default, `webservices`, since only web services is currently supported.                                                                                                                                                                                                                                                                                                                                                                                          |
    | credentialStore.\<cstore ID>.config.ws.url                            | <p>Required. CyberArk Central Credential Provider web service URL.</p><p><br>Use the following format:</p><pre><code>https://&#x3C;host name>:&#x3C;port>/AIMWebService/api/Accounts
    </code></pre>                                                                                                                                                                                                                                                                                                               |
    | credentialStore.\<cstore ID>.config.ws.appId                          | Required. CyberArk application ID for this BigConnect Pipelines. You must create the application ID in CyberArk.                                                                                                                                                                                                                                                                                                                                                                                                 |
    | credentialStore.\<cstore ID>.config.ws.maxConcurrentConnections       | Optional. Maximum number of concurrent web service calls that BigConnect Pipelines can make to CyberArk.                                                                                                                                                                                                                                                                                                                                                                                                         |
    | credentialStore.\<cstore ID>.config.ws.validateAfterInactivity.millis | Optional. Number of milliseconds of inactivity before BigConnect Pipelines validates the HTTP connection to CyberArk.                                                                                                                                                                                                                                                                                                                                                                                            |
    | credentialStore.\<cstore ID>.config.ws.connectionTimeout.millis       | Optional. Number of milliseconds to wait for a connection to CyberArk.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | credentialStore.\<cstore ID>.config.ws.nameSeparator                  | <p>Optional. Separator to use in the <code>name</code> argument that credential functions use.Use the following format for the <code>name</code> argument:</p><pre><code>&#x3C;safe>&#x3C;separator>&#x3C;folder>&#x3C;separator>&#x3C;object name>&#x3C;separator>&#x3C;element name>
    </code></pre><p>For example, if you keep the default ampersand (&#x26;), the format for the name argument is:</p><pre><code>&#x3C;safe>&#x26;&#x3C;folder>&#x26;&#x3C;object name>&#x26;&#x3C;element name>
    </code></pre> |
    | credentialStore.\<cstore ID>.config.ws.http.authentication            | <p>Optional. Authentication type used by the CyberArk Central Credential Provider web services: none, basic, or digest.</p><p><br>Default is none.</p>                                                                                                                                                                                                                                                                                                                                                           |
    | credentialStore.\<cstore ID>.config.ws.http.authentication.user       | Optional. Username if using basic or digest authentication.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | credentialStore.\<cstore ID>.config.ws.http.authentication.password   | <p>Optional. Password if using basic or digest authentication.</p><p><br>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                               |
    | credentialStore.\<cstore ID>.config.ws.truststoreFile                 | <p>Optional. Path to the truststore file if using HTTPS and the server certificate is using a private CA or is not trusted by the Java default truststore file.</p><p><br>Enter a path relative to the BigConnect Pipelines configuration directory, $SDC_CONF, or enter an absolute path.</p>                                                                                                                                                                                                                   |
    | credentialStore.\<cstore ID>.config.ws.truststorePassword             | <p>Optional. Password for the truststore file.</p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                                                   |
    | credentialStore.\<cstore ID>.config.ws.supportedProtocols             | Optional. SSL/TLS-enabled protocols. Versions TLSv1.2 or later are recommended.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | credentialStore.\<cstore ID>.config.ws.hostnameVerifier.skip          | <p>Optional. Determines whether the host name of the CyberArk Central Credential Provider web services should be verified against the domain defined in the HTTPS certificate.<br></p><p>By default, the host name is verified.</p>                                                                                                                                                                                                                                                                              |
    | credentialStore.\<cstore ID>.config.ws.keystoreFile                   | <p>Optional. If using HTTPS and the CyberArk Central Credential Provider web services requires client side certificates, the path to the keystore file that contains the client certificate.<br></p><p>Enter a path relative to the BigConnect Pipelines configuration directory, $SDC_CONF, or enter an absolute path.</p>                                                                                                                                                                                      |
    | credentialStore.\<cstore ID>.config.ws.keystorePassword               | <p>Optional. Password for the keystore file.</p><p>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                                                     |
    | credentialStore.\<cstore ID>.config.ws.keyPassword                    | <p>Optional. Password to access the certificate within the keystore file.</p><p><br>To protect the password, store the password in an external location and then use a function to retrieve the password.</p>                                                                                                                                                                                                                                                                                                    |
    | credentialStore.\<cstore ID>.config.enforceEntryGroup                 | <p>Optional. Requires BigConnect Pipelines to verify if the user who previews, validates, or starts the pipeline belongs to a group that is permitted to access the secret.<br></p><p>When set to true, each secret must have a corresponding <code>&#x3C;secret name>-groups</code> secret that contains a comma-separated list of groups that is permitted to access the secret.<br></p><p>For more information, see Group Access to Secrets.<br></p><p>Default is false.</p>                                  |
3. Restart BigConnect Pipelines to enable the changes.

#### Step 3. Call Secrets from the Pipeline <a href="#ariaid-title11" id="ariaid-title11"></a>

Use the `credential:get()` or `credential:getWithOptions()` function in pipeline stage properties to retrieve secrets from CyberArk.

Use the credential functions in any stage property that displays the key icon next to it. For example:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/CredentialStores\_KeyIcon.png)

**Important**: When you use a credential function in a stage property, the function must be the only value defined in the property. For example, you cannot include another function or a literal value along with the credential function.The credential functions use the following arguments:

* cstoreId - Unique ID of the credential store to use. Use the ID specified in the $SDC\_CONF/credential-stores.properties file. For more information, see [Enabling Credential Stores](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Configuration/CredentialStores.html#concept\_em2\_xfk\_xlb).
*   userGroup - Group that a user must belong to in order to access the secret. Only users that have execute permission on the pipeline and that belong to this group can validate, preview, or run the pipeline that retrieves the secret.\


    If working with Control Hub, specify the group using the required naming convention: `<group ID>@<organization ID>`.

    \
    To grant access to all users, specify the default `all` group when working only with BigConnect Pipelines. When working with Control Hub and BigConnect Pipelines version 3.16.0 or later, you can specify the default group using `all` or `all@<organization ID>`. BigConnect Pipelines recommends using `all` so that you do not need to modify credential functions when migrating pipelines from BigConnect Pipelines to Control Hub.\
    \
    **Note**: When working with Control Hub and a BigConnect Pipelines version earlier than 3.16.0, you must use the default `all@<organization ID>` group.\

* name - Name of the secret to retrieve from CyberArk. Use the following format: `"<safe><separator><folder><separator><object name>[<separator><element name>]"`, where:
  * `<safe>` is the CyberArk safe to read. For example, `production`.
  * `<separator>` is the separator defined for the safe, folder, object name, and element name values in the $SDC\_CONF/credential-stores.properties file. Or if you use the `credential:getWithOptions()` function, you can define the separator in the options argument.
  * `<folder>` is the folder in CyberArk to read. For example, `Root\\sqldatabases`.
  * `<object name>` is the object or secret in CyberArk to read. For example, `payroll`.
  * `<element name>` is an optional name for the value in the secret that you want returned. For example, enter `Content` to return the password or `Username` to return an optional user name value. If you do not specify `<element name>`, BigConnect Pipelines uses `Content`.\

*   storeOptions - Used only by the `credential:getWithOptions()` function. Additional options to communicate with the credential store. For CyberArk, you can use the following options:

    * `separator` - Separator to use in the `name` argument.
    * `ConnectionTimeout` - Connection timeout value in milliseconds.
    * `FailRequestOnPasswordChange` - Whether to fail the request on a password change, set to true or false. See the CyberArk documentation for details on this option.

    Use the following format to specify options:

    ```
    "<option1>=<value>,<option2>=<value>"
    ```

    For example, to use the pipe symbol (|) as the separator, enter the following for the options argument:

    ```
    "separator=|"
    ```

For example, the following expression returns the password for the payroll secret stored in the Root\\\sqldatabases folder in the production safe of the `cyberark` credential store. The `name` argument uses the default ampersand (&) as the separator. The expression allows any user belonging to the `devops` group access to the secret when validating, previewing, or running the pipeline:

```
${credential:get("cyberark", "devops", "production&Root\\sqldatabases&payroll&Content")}
```

The following expression returns the same password, but specifies the pipe symbol (|) as the separator:

```
${credential:getWithOptions("cyberark", "devops", "production|Root\\sqldatabases|payroll|Content", "separator=|")}
```
