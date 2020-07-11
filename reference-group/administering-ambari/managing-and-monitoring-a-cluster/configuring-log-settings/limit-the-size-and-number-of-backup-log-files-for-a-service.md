# Limit the size and number of backup log files for a service

Edit properties in the _log4j property group_ for a service to limit backup log files.

**About this task**  
Log4j properties and values that limit the size and number of backup log files for each service appear above the log4j template file in the log4j property group. To limit backup log files for a service, first access the Log4j property group for the service.

**Steps**

1. In **Ambari Web**, browse to **Services &gt; \[SERVICE\_NAME\] &gt; Configs &gt; Advanced\[SERVICE\_NAME\]-log4j**. 
2. Edit the values for the _\[SERVICE\_NAME\]_ backup file size and _\[SERVICE\_NAME\]_ \# _of backup files_ properties. 
3. In **Configs**, click **Save**.

