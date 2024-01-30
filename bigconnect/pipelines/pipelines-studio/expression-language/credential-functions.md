# Credential Functions

Use credential functions to retrieve sensitive information stored as secrets from supported credential stores.

Before you use a credential function, you must configure the credential store.

You can use credential functions in stages that require sensitive information, such as user names or passwords, to securely access data in external systems. Credential functions allow pipelines to access external systems without exposing sensitive details.

You cannot use credential functions in all stages. For example, we intentionally do not allow the use of credential functions in the Expression Evaluator processor. If credential functions were allowed in stages such as the Expression Evaluator, any user with access to the pipeline could access or print sensitive values, compromising the security of the external system.

Important: When you use a credential function in a stage property, the function must be the only value defined in the property. For example, you cannot include another function or a literal value along with the credential function.

String literals must be enclosed in single or double quotation marks.

The expression language provides the following credential functions:

### credential:get(\<cstoreId>, \<userGroup>, \<name>)

Returns the secret from the credential store. Uses the following arguments:

* cstoreId - Unique ID of the credential store to use. Use the ID specified in the $SDC\_CONF/credential-stores.properties file. For more information, see Enabling Credential Stores.
*   userGroup - Group that a user must belong to in order to access the secret. Only users that have execute permission on the pipeline and that belong to this group can validate, preview, or run the pipeline that retrieves the secret.

    To grant access to all users, specify the default `all` group when working only with Data Collector.&#x20;
* name - Name of the secret to retrieve from the credential store. Use the required format for the credential store:
  *   AWS Secrets Manager - Enter the secret name using the following format:

      `"<name><separator><key>"`

      Where `<name>` is the name of the secret to read, `<separator>` is the separator defined in the $SDC\_CONF/credential-stores.properties file, and `<key>` is the key for the value that you want returned.
  *   CyberArk - Enter the secret name using the following format:

      `"<safe><separator><folder><separator><object name>[<separator><element name>]"`

      Where `<safe>` is the CyberArk safe to read, `<separator>` is the separator defined in the $SDC\_CONF/credential-stores.properties file, `<folder>` is the CyberArk folder to read, `<object name>` is the CyberArk object or secret to read, and `<element name>` is an optional name for the value that you want returned.

      If you do not specify `<element name>`, Data Collector uses `Content`.
  *   Hashicorp Vault - Enter the secret name using the following format:

      `"<path><separator><key>"`

      Where `<path>` is the path in Vault to read, `<separator>` is the separator defined in the $SDC\_CONF/credential-stores.properties file, and `<key>` is the key for the value that you want returned.
  * Java keystore - Enter the name of the secret added to the Java keystore file using the `jks-cs add` command.
  * Azure Key Vault - Enter the name of the key or secret to retrieve from Azure Key Vault.

Return type: String.For example, the following expression returns the value of the `OracleDBPassword` credential defined in the `devjks` Java keystore credential store and allows any user belonging to the `devops` group access to the credential when validating, previewing, or running the pipeline:

```
${credential:get("devjks", "devops", "OracleDBPassword")}
```

### credential:getWithOptions(\<cstoreId>, \<userGroup>, \<name>, \<storeOptions>)

Returns the secret from the credential store using additional options to communicate with the credential store. Not applicable for the Java keystore credential store.

For example, use this function with the CyberArk credential store to define the separator for the secret name within the function. Or use this function with the Hashicorp Vault credential store when you might need a delay of 10 seconds or more before the credentials can be used successfully.

Uses the following arguments:

* cstoreId - Unique ID of the credential store to use. Use the ID specified in the $SDC\_CONF/credential-stores.properties file. For more information, see Enabling Credential Stores.
*   userGroup - Group that a user must belong to in order to access the secret. Only users that have execute permission on the pipeline and that belong to this group can validate, preview, or run the pipeline that retrieves the secret.

    To grant access to all users, specify the default `all` group when working only with Data Collector.&#x20;
* name - Name of the secret to retrieve from the credential store. Use the required format for the credential store:
  *   AWS Secrets Manager - Enter the secret name using the following format:

      `"<name><separator><key>"`

      Where `<name>` is the name of the secret to read, `<separator>` is the separator defined in the $SDC\_CONF/credential-stores.properties file or in the store options passed with the function, and `<key>` is the key for the value that you want returned.
  *   CyberArk - Enter the secret name using the following format:

      `"<safe><separator><folder><separator><object name>`_`<separator><element name>`_`"`

      Where `<safe>` is the CyberArk safe to read, `<separator>` is the separator defined in the $SDC\_CONF/credential-stores.properties file, `<folder>` is the CyberArk folder to read, `<object name>` is the CyberArk object to read, and `<element name>` is an optional name for the value that you want returned.

      If you do not specify `<element name>`, Data Collector uses `Content`.
  *   Hashicorp Vault - Enter the secret name using the following format:

      `"<path><separator><key>"`

      Where `<path>` is the path in Vault to read, `<separator>` is the separator defined in the $SDC\_CONF/credential-stores.properties file, and `<key>` is the key for the value that you want returned.
  * Azure Key Vault - Enter the name of the key or secret to retrieve from Azure Key Vault.
*   storeOptions - Additional options to communicate with the credential store.For AWS Secrets Manager, you can use the following options:

    * `separator` - Specifies the separator for name and key values in the credential functions, overriding the `credentialStore.aws.config.nameKey.separator` property.
    * `alwaysRefresh` - When set to true, forces the key to refresh its cached value before Data Collector retrieves the value, overriding the `credentialStore.aws.config.nameKey.separator` property.

    For CyberArk, you can use the following options:

    * `separator` - Separator to use for the secret name.
    * `ConnectionTimeout` - Connection timeout value.
    * `FailRequestOnPasswordChange` - Whether to fail the request on a password change, set to true or false.

    For Hashicorp Vault, you can use the `delay` option to enter a delay in milliseconds to allow time for external processing. Use the delay option when using the Vault AWS secret backend to generate AWS access credentials based on IAM policies. According to Vault documentation, you might need a delay of 10 seconds or more before the credentials can be used successfully.

    For Azure Key Vault, you can use the following options:

    * `url` - Overrides the `credentialStore.azure.config.vault.url` property in the $SDC\_CONF/credential-stores.properties file.
    * `retry` - Overrides the `credentialStore.azure.config.credential.retry.millis` property in the $SDC\_CONF/credential-stores.properties file.
    * `refresh` - Overrides the `credentialStore.azure.config.credential.refresh.millis` property in the $SDC\_CONF/credential-stores.properties file.
    * `credentialType=certificate` - Instructs Azure Key Vault to retrieve the stored PEM certificate as a certificate rather than as a secret. Use to retrieve a PEM certificate stored in Azure Key Vault when you configure a stage to use a [remote keystore or truststore](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Pipeline\_Configuration/SSL-TLS.html#concept\_q2c\_hj2\_1nb) for SSL/TLS encryption.

    Use the following format to specify options:

    ```
    "<option1>=<value>,<option2>=<value>"
    ```

    For example, to use the pipe symbol (|) as the CyberArk separator, enter the following for the options argument:

    ```
    "separator=|"
    ```

    Or to set the Vault delay to 1,000 milliseconds, enter the following for the options argument:

    ```
    "delay=1000"
    ```

Return type: String.

For example, the following expression returns the value of the key password stored in the Vault path /secret/databases/oracle from the `vault` credential store after waiting for a delay of 1,000 milliseconds. The name argument uses the default ampersand (&) as the separator. The expression allows any user belonging to the `devops` group access to the secret when validating, previewing, or running the pipeline:

```
${credential:getWithOptions("vault", "devops", "/secret/databases/oracle&password", "delay=1000")}
```
