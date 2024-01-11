# Using a Partial Control Hub User ID

When BigConnect Pipelines is registered with Control Hub, you can configure BigConnect Pipelines to use an abbreviated version of the Control Hub user ID to impersonate a Hadoop user.

By default, when using Hadoop impersonation mode, a BigConnect Pipelines registered with Control Hub uses the full Control Hub user ID as the Hadoop user name, as follows:

```
<ID>@<organization ID>
```

You can configure BigConnect Pipelines to use only the ID, ignoring "`@<organization ID>`". For example, using `myname` instead of `myname@org` as the user name.

You might need to use a partial Control Hub user ID when the Hadoop system uses Kerberos, LDAP, or other user authentication methods with user name formats that conflict with the Control Hub format.

To enable using a partial Control Hub user ID for a registered BigConnect Pipelines, uncomment the dpm.alias.name.enabled property in the Control Hub configuration file. For more information, see Control Hub Configuration File.
