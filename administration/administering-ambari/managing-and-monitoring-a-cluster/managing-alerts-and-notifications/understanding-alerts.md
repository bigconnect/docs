# Understanding alerts

Use **Ambari Web &gt; Alerts** to scan, enable or disable, or open an alert definition for customization.

Ambari predefines a set of alerts that monitor the cluster components and hosts. Each alert is defined by an _alert definition_, which specifies the _alert type_, check interval, and thresholds. When a cluster is created or modified, Ambari reads the alert definitions and creates _alert instances_ for the specific items to monitor in the cluster. For example, if your cluster includes Hadoop Distributed File System \(HDFS\), there is an alert definition to monitor "DataNode Process". An instance of that alert definition is created for each DataNode in the cluster.

Using **Ambari Web &gt; Alerts**, you can browse the list of alerts defined for your cluster. You can search and filter alert definitions by current status, **alert definition name**, last status change, alert state, and by the service with which the alert definition is associated. You can click alert definition name to view details about that alert, to modify the alert properties \(such as check interval and thresholds\), and to see the list of alert instances associated with that alert definition.

Each alert instance reports an alert status, defined by severity. The most common severity levels are OK, WARNING, and CRITICAL, but there are also severities for UNKNOWN and NONE. Alert notifications are sent when alert status changes \(for example, status changes from OK to CRITICAL\).

