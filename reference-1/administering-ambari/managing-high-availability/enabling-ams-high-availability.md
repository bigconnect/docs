# Enabling AMS high availability

Make AMS highly available by switching AMS to distributed mode and adding a second, standby Metrics Collector.

**About this task**  
Ambari installs the Ambari Metrics System \(AMS\) , into the cluster with a single Metrics Collector component by default. The Collector is a daemon that runs on a specific host in the cluster and receives data from the registered publishers, the Monitors and Sinks. Depending on your needs, you might require AMS to have two collectors to cover a High Availability scenario. Establishing AMS High Availability means having a standby collector take over in the event the primary collector fails.

**Before you begin**  
You must deploy AMS in distributed, not embedded mode.

**Steps**

1. In **Ambari Web**, browse to the host where you would like to install another collector.
2. On the **Host** page, click **+Add**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_add_metrics_collector.png) 
3. Click **Metrics Collector** from the **+Add** list. Ambari installs the new Metrics Collector and configures Ambari Metrics for HA. The new Collector will be installed in a stopped state. 
4. In **Ambari Web &gt; Components**, start the new Collector component.  
  
   ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_start_metrics_collector.png)  


   If you attempt to add a second Collector to the cluster without first switching AMS to distributed mode, the collector will install but will not be able to be started. To resolve this:  


   1. Delete the newly added Collector.
   2. Enable distributed mode.
   3. Then, re-add the Collector

**Example**

```text
Traceback (most recent call last):
File 
 "/var/lib/ambari-agent/cache/common-services/AMBARI_METRICS/0.1.0/package/scripts/metrics_collector.py", line 150, in <module>
 AmsCollector().execute()
File
 "/usr/lib/python2.6/site-packages/resource_management/libraries/script/script.py", line 313, in execute
 method(env)
File
"/var/lib/ambari-agent/cache/common-services/AMBARI_METRICS/0.1.0/package/scripts/metrics_collector.py", line 48, in start self.configure(env, action = 'start') # for security
File
 "/usr/lib/python2.6/site-packages/resource_management/libraries/script/script.py", line 116, in locking_configure original_configure(obj, *args, **kw)
File
 "/var/lib/ambari-agent/cache/common-services/AMBARI_METRICS/0.1.0/package/scripts/metrics_collector.py", line 42, in configure raise
Fail("AMS in embedded mode cannot have more than 1 instance.
Delete all but 1 instances or switch to Distributed mode ") resource_management.core.exceptions.
Fail: AMS in embedded mode cannot have more than 1 instance.
Delete all but 1 instances or switch to Distributed mode
```

  


