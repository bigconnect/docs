# Read audit log files

Read the audit logs to assess details of Ambari-managed operations and configuration changes.

**About this task**  
When you use Ambari to perform an operation, such as user login or logout, stopping or starting a service, and adding or removing a service, Ambari creates an entry in an audit log. By reading the audit logs, you can determine who performed the operation, when the operation occurred, and other, operation-specific information. When you change configuration for a service, Ambari creates an entry in the audit log, and creates a specific log file.

* Fnd the Ambari audit log on your Ambari server host, at: `/var/log/ambari-server/ambari-audit.log`
* Fnd the Ambari config changes log on your Ambari server host, `at: ambari-config-changes.log`

By reading the configuration change log, you can find out even more information about each change. For example:  
`2016-05-25 18:31:26,242 INFO - Cluster 'MyCluster' changed by: 'admin'; service_name='HDFS' config_group='default' config_group_id='-1' version='2'`

