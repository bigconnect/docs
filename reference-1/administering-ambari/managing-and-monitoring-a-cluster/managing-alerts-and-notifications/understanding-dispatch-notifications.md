# Understanding dispatch notifications

Use dispatch notifications to communicate alert status changes.

When an alert is enabled and the alert status changes \(for example, from OK to CRITICAL or CRITICAL to OK\), Ambari sends either an email or SNMP notification, depending on how notifications are configured.

For email notifications, Ambari sends an email digest that includes all alert status changes. For example, if two alerts become critical, Ambari sends one email message that `Alert A is CRITICAL` and `Ambari B alert is CRITICAL`. Ambari does not send another email notification until status changes again.

For SNMP notifications, Ambari sends one SNMP trap per alert status change. For example, if two alerts become critical, Ambari sends two SNMP traps, one for each alert, and then sends two more when the two alerts change.

