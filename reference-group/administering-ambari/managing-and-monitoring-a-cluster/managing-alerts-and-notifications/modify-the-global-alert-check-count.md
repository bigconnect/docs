# Modify the global alert check count

Use **Alerts &gt; Actions &gt; Manage Alert Settings** to set a global control for how many times Ambari checks an alert before dispatching a notification.

**About this task**  
If the alert state changes during a check, Ambari attempts to check the condition a number of times. That number is called the _check count._ You can set the value of _check count_. Alert check counts are not applicable to AGGREATE alert types. A state change for an AGGREGATE alert results in a notification dispatch. If your environment experiences transient issues resulting in false alerts, you can increase the check count. In this case, the alert state change is still recorded, but as a _soft state change_. If the alert condition is still triggered after the specified number of checks, the state change is considered a _hard state change_, and notifications dispatch. Generally, you should set the check count value globally for all alerts. You can override the global check count value for any alerts experiencing transient issues.

**Steps**

1. In **Ambari Web**, browse to **Alerts &gt; Actions &gt; Manage Alert Settings**.
2. In **Check Count**, edit the value, as necessary.
3. Click **Save**.

**After you finish**  
Wait several seconds for changes made to the global alert check count to appear for individual alerts in **Ambari Web**.

