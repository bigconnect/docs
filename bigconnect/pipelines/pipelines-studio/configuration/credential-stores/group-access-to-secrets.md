# Group Access to Secrets

As an additional layer of security, you can employ user groups to further limit access to the secrets defined in credential stores.

BigConnect Pipelines provides two methods to limit access with user groups:\
\
**Required group argument in credential functions**

Credential functions include a group argument that defines the user group that can access the secret. The group argument ensures that the user who attempts to preview, validate, or start a pipeline that includes a credential function belongs to the group specified in the function. The user must also have execute permission on the pipeline.

When working only with BigConnect Pipelines, simply specify the group name, such as `devops`. When working with Control Hub, specify the group using the required naming convention: `<group ID>@<organization ID>`. For example, `devops@MyCompany`.

If you do not want to restrict access to a secret, specify the default `all` group when working only with BigConnect Pipelines. When working with Control Hub and BigConnect Pipelines version 3.16.0 or later, you can specify the default group using `all` or `all@<organization ID>`. BigConnect Pipelines recommends using `all` so that you do not need to modify credential functions when migrating pipelines from BigConnect Pipelines to Control Hub.\
\
**Note**: When working with Control Hub and a BigConnect Pipelines version earlier than 3.16.0, you must use the default `all@<organization ID>` group.

If BigConnect Pipelines shuts down while running a pipeline that uses a credential function, BigConnect Pipelines restarts the pipeline without checking the group access.

**Optional group secrets in the credential store**

In addition to using the group argument in credential functions, you can configure BigConnect Pipelines to require group secrets for a credential store.

To require the use of group secrets, in the $SDC\_CONF/credential-stores.properties file, set the `credentialStore.<cstore ID>.config.enforceEntryGroup` property to `true`.

A group secret is a secret defined in the credential store that contains a comma-delimited list of BigConnect Pipelines user groups permitted to access the associated secret.

When the credential store ID in the $SDC\_CONF/credential-stores.properties file requires group secrets, you must define a group secret for every secret that BigConnect Pipelines accesses in that credential store. The name of the group secret is based on the secret name, as follows:

```
<secret name>-groups
```

When you configure a credential function to call a secret, the user group specified in the credential function must be listed in the associated group secret that is defined in the credential store.\
\
For example, say you enable BigConnect Pipelines to require group secrets for Azure Key Vault. Then, in a Kafka Consumer origin, you use the following expression to access a Base64-encoded keytab in the `azure` credential store for the origin to use:

```
${credential:get("azure", "kafkaprod", readkeytab)}
```

When you run the pipeline, BigConnect Pipelines validates all of the following:

* The user who starts the pipeline is in the `kafkaprod` BigConnect Pipelines user group.
* The `readkeytab` secret has an associated `readkeytab-groups` secret defined in the credential store.
* The `readkeytab-groups` secret includes the `kafkaprod` user group.

When BigConnect Pipelines is not configured to require group secrets, BigConnect Pipelines validates only the first point, verifying that the user belongs to the specified group.
