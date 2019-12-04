# Enable Service Auto Start from Ambari Web

Use **Cluster Admin &gt; Service Auto Start** to control whether components in a stopped state re-start automatically.

**About this task**  
As a Cluster Administrator or Cluster Operator, you can enable each service in your stack to re-start automatically. Enabling auto-start for a service causes the ambari-agent to attempt re-starting service components in a stopped state without manual effort by a user. As a first step, you should enable auto-start for the worker nodes in the core Hadoop services, the DataNode and NameNode components in YARN and HDFS, for example. You should also enable auto-start for all components in the SmartSense service. After enabling auto-start, monitor the operating status of your services on the Ambari Web dashboard. Auto-start attempts do not display as background operations.   
To manage the auto-start status for components in a service:

**Steps**

1. In **Ambari Web &gt; Cluster Admin**, click **Service Auto Start**.![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_auto_start_services_defaults.png) Auto-Start Settings is enabled by default, but _only_ the Ambari Metrics Collector component is set to auto-start by default. 
2. To toggle auto-start settings from Enabled to Disabled and back, click the Auto-Start settings button. 
3. To set a componet to restart automatically, click the **Auto-Start** checkbox for a component. 
4. To set all components to auto-start, click the **Auto Start** checkbox.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_auto_start_services_all_cmpnts.png) 
5. To clear all pending status changes before saving them, click **Cancel**. 
6. When you complete changes to your auto-start settings, click **Save**.

**After you finish**  
To diagnose issues with service components that fail to start, check the ambari agent logs, located at: `/var/log/ambari-agent.log` on the component host.

