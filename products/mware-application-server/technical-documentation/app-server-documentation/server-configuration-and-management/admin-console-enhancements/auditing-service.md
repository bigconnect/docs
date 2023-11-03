---
description: Admin Console Auditing Service
---

# Auditing Service

This feature allows users to log all actions and operations executed via the admin console for auditing purposes.

### Configuring the Auditing Service

All operations executed by users with access to the admin console will be translated to events that correspond to the specific admin user that executed the operation and the `asadmin` command (and its parameters) that is equivalent to the operation being executed in the admin console. For example if the default `admin` user modifies the monitoring level of the _Web Services Container_, the following event will be generated:

```
AUDIT - admin issued command set with parameters
{
    DEFAULT: [configs.config.server-config.monitoring-service.module-monitoring-levels.web-services-container=HIGH]
}
```

The auditing service will relay these events to the [Notification Service](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Logging%20and%20Monitoring/Notification%20Service/Overview.html). In this manner, administrators can be alerted of suspicious activity as soon as possible.

This service can be configured from the admin console and from the command line.

#### From the Admin Console

The auditing service configuration can be found in the web console under `Configurations` → `` <configuration-name>` `` → `Security` → `Admin Audit`:

![Admin Audit Service in Web Console](https://docs.payara.fish/community/docs/\_images/admin-console/admin-audit-menu.png)

Once enabled, actions in the admin console that have a corresponding `asadmin` command will be sent to the [Notification service](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Logging%20and%20Monitoring/Notification%20Service/Overview.html).

Check the **Enabled** option (and the **Dynamic** option too if you don’t want to restart the domain) to switch the auditing service on.

The **Audit Level** options sets the level of operations that will be logged:

**MODIFIERS**

(Default setting) All operations that modify the configuration of the server in some capacity.

**ACCESSORS**

All operations query the configuration of the server in some capacity.

**INTERNAL**

All existing operations, including internal operations of the server.

Aside from this configuration setting, you can also define which notifiers will be used to relay the admin console audit events by moving them to the **Selected Notifiers** box.

![Activate Notifiers in the Admin Console](https://docs.payara.fish/community/docs/\_images/request-tracing/select-notifiers.png)

|   | Keep in mind that for audit events to be relayed to the selected notifiers, both the Notification Service and each selected notifier must be enabled and configured beforehand. |
| - | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

|   | You don’t need to manually add each notifier on this screen. When enabling a notifier on its configuration screen, the server will **automatically** add it to the list of selected notifiers for the Admin audit service. This same result occurs when enabling the notifier using the appropriate _asadmin_ command. |
| - | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

#### From the Command Line

The following is the list of available asadmin commands with their available options to configure the auditing service:

**`set-admin-audit-configuration`**

**Usage**

`asadmin> set-admin-audit-configuration`

**Aim**

This command can be used to set the configuration settings of the service.

**Command Options**

| Option         | Type    | Description                                                                                           | Default   | Mandatory |
| -------------- | ------- | ----------------------------------------------------------------------------------------------------- | --------- | --------- |
| `--enabled`    | Boolean | Enables or disables the service                                                                       | false     | No        |
| `--dynamic`    | Boolean | Whether to apply changes immediately of after a domain restart.                                       | false     | No        |
| `--auditLevel` | String  | <p>Sets the auditing level. One of:</p><ul><li>MODIFIERS</li><li>ACCESSORS</li><li>INTERNAL</li></ul> | MODIFIERS | No        |

**Example**

```shell
asadmin> set-admin-audit-configuration --enabled=true --dynamic=true --auditLevel=ACCESSORS

Command set-admin-audit-configuration executed successfully.
```

**`set-admin-audit-service-notifier-configuration`**

**Usage**

```
asadmin> set-admin-audit-service-notifier-configuration
 --notifier=<string>
 --enabled=true|false
 --dynamic=true|false
 --noisy=true|false
```

**Aim**

This command can be used to enable or disable a specific notifier and the verbosity of the events being relayed.

**Command Options**

| Option       | Type    | Description                                                                                                                                                                                                                                                                                                                                                                              | Default | Mandatory |
| ------------ | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | --------- |
| `--notifier` | String  | <p>The notifier to configure. One of (case-insensitive):</p><ul><li><code>LOG</code></li><li><code>HIPCHAT</code></li><li><code>SLACK</code></li><li><code>JMS</code></li><li><code>EMAIL</code></li><li><code>XMPP</code></li><li><code>SNMP</code></li><li><code>EVENTBUS</code></li><li><code>NEWRELIC</code></li><li><code>DATADOG</code></li><li><code>CDIEVENTBUS</code></li></ul> | -       | yes       |
| `--enable`   | Boolean | Enables or disables the notifier                                                                                                                                                                                                                                                                                                                                                         | false   | Yes       |
| `--noisy`    | Boolean | Enables or disables **noisy mode**. A noisy notifier includes verbose information in the notifiers output.                                                                                                                                                                                                                                                                               | -       | No        |
| `--dynamic`  | Boolean | Whether to apply the changes immediately or after server restart                                                                                                                                                                                                                                                                                                                         | false   | No        |
| `--target`   | String  | The instance or cluster that will be configured                                                                                                                                                                                                                                                                                                                                          | server  | no        |

**Examples**

To configure the auditing service to relay events to the log notifier without having to restart the domain run the following command:

```shell
asadmin> set-admin-audit-service-notifier-configuration --notifier=log --enabled=true --dynamic=true --noisy=false
log.enabled was false set to true
log.noisy was true set to false

Command set-admin-audit-service-notifier-configuration executed successfully.
```

**`get-admin-audit-configuration`**

**Usage**

`asadmin> get-admin-audit-configuration`

**Aim**

This command can be used to list the configuration settings of the auditing service.

**Command Options**

There are no available options for this command.

**Example**

Running the following command will yield the current configuration of the auditing service:

```shell
asadmin> get-admin-audit-configuration

Enabled  Audit Level
false    MODIFIERS
Name     Notifier Enabled
LOG      false

Command get-admin-audit-configuration executed successfully.
```

{% hint style="info" %}
The first row corresponds to the service configuration, second row onwards will detail any notifiers being configured..
{% endhint %}
