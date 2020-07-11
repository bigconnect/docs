# Restart multiple components

Use a rolling restart to restart multiple components.

**About this task**  
When you restart multiple components, use _rolling restarts_ to distribute the task. A rolling restart stops and then starts multiple running slave components, such as DataNodes, NodeManagers, RegionServers, or Supervisors, using a batch sequence. You set rolling restart parameter values to control the number, time between, tolerance for failures, and limits for restarts of many components across large clusters.

{% hint style="danger" %}
**Important**  
Rolling restarts of DataNodes should be performed only during cluster maintenance.
{% endhint %}

**Steps**

1. In **Services**, click a service name. 
2. In **Summary**, click a link, such as DataNodes or RegionServers, of any components that you want to restart.The **Hosts** page lists any host names in your cluster on which that component resides. 
3. Using the host-level **Actions** menu, click the name of a slave component option. Then, click **Restart**. 
4. Review and set values for **Rolling Restart Parameters**. 
5. Optionally, reset the flag to restart only components with changed configurations. 
6. Click **Trigger Rolling Restart**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_rllng_restart_datanodes.png)

**After you finish**  
Monitor the progress of the background operations.

