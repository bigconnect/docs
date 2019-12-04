# Override the global alert check count

Edit the check count in each alert definition to override the global control for how many times Ambari checks an alert before dispatching a notification with a unique value specified per alert.

**About this task**  
If the alert state changes during a check, Ambari attempts to check the condition a number of times. That number is called the _check count._ You can set the value of _check count_ both globally and for each alert. Alert check counts are not applicable to AGGREATE alert types. A state change for an AGGREGATE alert results in a notification dispatch. If your environment experiences transient issues resulting in false alerts, you can increase the check count. In this case, the alert state change is still recorded, but as a _soft state change_. If the alert condition is still triggered after the specified number of checks, the state change is considered a _hard state change_, and notifications dispatch. Generally, you should set the check count value globally for all alerts. You can override the global check count value for any alerts experiencing transient issues by modifying the check count value specifically for each alert.

**Steps**

1. In **Ambari Web**, browse to **Alerts**. 
2. Select the alert for which you want to set a check count value different from the global check count value. 
3. In **Alert Info**, click the Edit ****icon next to the _Check Count_ property. 
4. In **Edit Alert Count**, update the _Check Count_ value. 
5. Click **Save**.

