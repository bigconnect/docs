# Rebalance HDFS blocks

**About this task**  
HDFS provides a balancer utility to help balance the blocks across DataNodes in the cluster. To initiate a balancing process, follow these steps:

**Steps**

1. In **Ambari Web**, browse to **Services &gt; HDFS &gt; Summary**. 
2. Click **Service Actions &gt; Rebalance HDFS**. 
3. Enter the **Balance Threshold** value as a percentage of disk capacity. 
4. Click **Start**.

**After you finish**  
You can monitor or cancel a rebalance process by opening **Background Operations**.

  


