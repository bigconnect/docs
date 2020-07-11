# Refresh YARN Capacity Scheduler

Use Ambari Web to refresh the YARN Capacity Scheduler when you add or modify existing queues.  
  
**About this task**  
After you modify the Capacity Scheduler configuration, YARN enables you to refresh the queues without restarting your ResourceManager, if you have made no destructive changes \(such as completely removing a queue\) to your configuration. The Refresh operation will fail with the following message: `Failed to re-init queues` if you attempt to refresh queues in a case where you performed a destructive change, such as removing a queue. In cases where you have made destructive changes, you must perform a ResourceManager restart for the capacity scheduler change to take effect. To refresh the Capacity Scheduler, follow these steps:

**Steps**

1. In **Ambari Web**, browse to **Services &gt; YARN &gt; Summary**. 
2. Click **Actions &gt; Refresh YARN Capacity Scheduler**. 
3. Confirm that you want to perform this operation. The refresh operation is submitted to the YARN ResourceManager.

