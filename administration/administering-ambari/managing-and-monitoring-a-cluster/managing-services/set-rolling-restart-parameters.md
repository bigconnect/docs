# Set rolling restart parameters

You can adjust the default parameter values before starting a rolling restart operation.

**About this task**  
When you choose to restart slave components, you should use parameters to control how restarts of components roll. Parameter values based on ten percent of the total number of components in your cluster are set as default values. For example, default settings for a rolling restart of components in a three-node cluster restarts one component at a time, waits two minutes between restarts, proceeds if only one failure occurs, and restarts all existing components that run this service.

**Before you begin**  
Initiate a rolling restart.

* In the **Restart** dialog, enter integer, non-zero values for all parameters.  
  
  **Restart \(Batch Size\)**  
  Number of components to include in each restart batch.  
  
  **Wait \(Time\)**  
  Time \(in seconds\) to wait between queuing each batch of components.  
  
  **Tolerate up to x failures**  
  Total number of restart failures to tolerate, across all batches, before halting the restarts and not queuing batches.  
  
  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_rllng_restart_datanodes.png)  


  If you trigger a rolling restart of _components_, the default value of **Only restart components with stale configs** is `true`.

  If you trigger a rolling restart of _services_, the default value of **Only restart services with stale configs** is `false`.  

* Change the default value of **Only restart components with stale configs**, if necessary.  **After you finish** Click **Trigger Rolling Restart**.

