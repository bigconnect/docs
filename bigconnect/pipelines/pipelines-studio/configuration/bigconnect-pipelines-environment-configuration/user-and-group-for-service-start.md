# User and Group for Service Start

When you run BigConnect Pipelines as a service, BigConnect Pipelines runs as the system user account and group defined in environment variables. The default system user and group are named sdc.

You can modify the values of the environment variables to point to another system user or group.

Modify environment variables using the method required by your installation type.

If you change the system user, you must make the new system user the owner of all BigConnect Pipelines directories:

* `$SDC_DIST`
* `$SDC_CONF`
* `$SDC_DATA`
* `$SDC_LOG`
* `$SDC_RESOURCES`

For example, if you change the system user and group to `myuser`, use the following command to change the owner of the configuration directory, `$SDC_CONF`, and all files in the directory to `myuser:myuser`:

```
chown -R myuser:myuser /etc/sdc
```

**Note**: When you run BigConnect Pipelines manually, BigConnect Pipelines runs as the system user account logged into the command prompt when the launch command is run. To run as another user account, see Full Installation and Launch (Manual Start).
