# Enable preemption for YARN workflow queues

With Preemption enabled, under-served queues can begin to claim their allocated cluster resources almost immediately, without having to wait for other queues' applications to finish running.

**About this task**  
When using YARN queues, a scenario can occur in which a queue has a guaranteed level of cluster resources, but must wait to run applications because other queues are utilizing all of the available resources. If Preemption is enabled, higher priority applications do not have to wait because lower priority applications have taken up the available capacity.

Steps

1. In **Ambari Web**, browse to **Services &gt; YARN &gt; Configs**. In YARN Features, click **Pre-emption**. The button label changes to indicate that Preemption is enabled.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_enable_preemption.png) 
2. Browse and scroll to **Advanced &gt; Custom yarn-site**. Click **Add Property** and use the **Add Property** pop-up to add a new property in the following format:  
   `yarn.resourcemanager.monitor.capacity.preemption.total_preemption_per_round=<(memory-of-one-NodeManager)/(total-cluster-memory)>`

  
   This is the maximum percentage of resources preempted in a single round. You can use this value to restrict the pace at which Containers are reclaimed from the cluster. After computing the total desired preemption, the policy scales it back to this limit. This should be set to \(memory-of-one-NodeManager\)/\(total-cluster-memory\). For example, if one NodeManager has 32 GB, and the total cluster resource is 100 GB, the total\_preemption\_per\_round should set to 32/100 = 0.32. The default value is 0.1 \(10%\):  
   `yarn.resourcemanager.monitor.capacity.preemption.total_preemption_per_round=0.1`

3. Click **Add Property** again and use the **Add Property** pop-up to add the following **Custom yarn-site** property:`yarn.resourcemanager.monitor.capacity.preemption.natural_termination_factor=1.0`  Similar to total\_preemption\_per\_round, you can apply this factor to slow down resource preemption after the preemption target is computed for each queue \(for example, “give me 5 GB back from queue-A”\). For example, if 5 GB is needed back, in the first cycle preemption takes back 1 GB \(20% of 5GB\), 0.8 GB \(20% of the remaining 4 GB\) in the next, 0.64 GB \(20% of the remaining 3.2 GB\) next, and so on. You can increase this value to speed up resource reclamation. The recommended value for this parameter is 1.0, meaning that 100% of the target capacity is preempted in a cycle. 
4. Click **Save** to save the new configuration settings. On the **Save Configuration** pop-up, type a description of the changes in the **Notes** box, then click **Save**.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_enable_preemption_save_configuration.png) 
5. On the **Dependent Configurations** pop-up, click **OK** to accept the recommended value of `true` for the `yarn.scheduler.capacity.ordering-policy.priority-utilization.underutilized-preemption.enabled` property.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_enable_preemption_dependent_configurations.png) 
6. Click **OK** on the **Save Configuration Changes** pop-up.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_enable_preemption_save_configuration_changes.png) 
7. Select **Restart &gt; Restart All Affected** to restart the YARN service and load the new configuration.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_enable_preemption_yarn_restart.png) 
8. Click **Confirm Restart All** on the confirmation pop-up to confirm the YARN restart. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_enable_preemption_yarn_restart_confirmation.png) 

**Result**  
After YARN restarts, Preemption will be enabled.

**After you finish**  
Other components may also require a restart.

