# Modify an alert

Edit the configuration properties in each alert definition to modify alert behavior.

**About this task**  
General properties for an alert include name, description, check interval, and thresholds. The check interval defines the frequency with which Ambari checks alert status. For example, a 1 minute value means that Ambari checks the alert status every minute. The configuration options for thresholds depend on the alert type. To modify alert behavior, edit the general properties in the alert definition.

**Steps**

1. In **Ambari Web**, click **Alerts**. 
2. In **Alerts**, find an alert definition name, and click it. The alert definition page for that alert opens. 
3. Click the pencil icon next to the alert name to modify the name of the alert. 
4. Click **Edit** to modify configuration properties succh as description, check interval, and thresholds, as necessary. Configuration properties differ for each alert. 
5. Click **Save**. Changes take effect on all alert instances at the next check interval.

