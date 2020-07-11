# Understanding notifications

Use alerts, notifications, and groups to inform unique sets of users about only those issues important to them, using an appropriate channel.

Using alert groups and notifications enables you to create groups of alerts and set up notification targets for each group in such a way that you can notify different parties interested in certain sets of alerts by using different methods. For example, you might want your Hadoop Operations team to receive all alerts by email, regardless of status, while at the same time you want your System Administration team to receive only RPC- and CPU-related alerts that are in Critical state, and only by simple network management protocol \(SNMP\).

To achieve these different results, you can have one alert notification that manages email for all alert groups for all severity levels, and a different alert notification group that manages SNMP on critical-severity alerts for an alert group that contains the RPC and CPU alerts.

