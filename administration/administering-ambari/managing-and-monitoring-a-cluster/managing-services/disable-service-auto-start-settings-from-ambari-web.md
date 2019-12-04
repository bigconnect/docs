# Disable service auto start settings from Ambari Web

Disable Auto Start Settings to prevent components in a stopped state in your cluster from re-starting automatically.  
  
**About this task**  
As a Cluster Administrator or Cluster Operator, you can enable each component in your cluster to re-start automatically. Enabling auto-start for a service causes the ambari-agent to attempt re-starting service components in a stopped state without manual effort by a user. Auto-start attempts do not display as background operations. To disable Auto-Start Services:

**Steps**

1. In **Ambari Web &gt; Cluster Admin**, click **Service Auto Start**.![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_auto_start_services_defaults.png) Auto-Start Settings is enabled by default, but _only_ the Ambari Metrics Collector component is set to auto-start by default. 
2. In **Auto-Start Settings**, click the green Enabled button.![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_service_auto_start_setting_on.png) The Auto Start settings button toggles to Disabled. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_service_auto_start_setting_off.png) 
3. To clear all pending status changes before saving them, click **Cancel**. 
4. When you complete changes to your auto-start settings, click **Save**.

  


