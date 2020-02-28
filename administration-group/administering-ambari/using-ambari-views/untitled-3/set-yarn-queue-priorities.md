# Set YARN queue priorities

Set YARN queue priorities to ensure that applications can access cluster resources.  
  
**Before you begin**  
In order for YARN Queue Priorities to be applied, you must enable preemption.  
  
**About this task**  
Even with preemption enabled, there are some use cases where applications might not have access to cluster resources without setting priorities.  
  
**Long-running applications**  
Without setting priorities, long-running applications in queues that are under capacity and with lower relative resource usage may not release cluster resources until they finish running.   
  
**Applications that require large containers**   
The issue with long-running applications is exacerbated for applications that require large containers. With short-running applications, previous containers may eventually finish running and free cluster resources for applications with large containers. But with long-running services in the cluster, the large containers may never get sufficiently large resources on any nodes.   
  
**Hive LLAP**   
Hive LLAP \(Low-Latency Analytical Processing\) enables you to run Hive queries with low-latency in near real-time. To ensure low-latency, you should set the priority of the queue used for LLAP to a higher priority, especially if your cluster includes long-running applications.

**Steps**

1. On the **YARN Queue Manager** view, select a queue, then enter a priority in the **Priority** box under **Resources**.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_queue_set_priority.png)  All queues are set to a priority of 0 by default. Higher numbers indicate higher priority. 
2. Select **Actions &gt; Save and Refresh Queues** to save the priority setting.![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_actions_menu_refresh_queues.png)

**Example**  
To set the queue used for Hive LLAP, in **Ambari Web**, browse to **Hive &gt; Config &gt; Settings**, then select a queue using the **Interactive Query Queue** drop-down. For example, the following figure shows a 3-node cluster with long-running 20 GB containers. The LLAP daemons require 90 GB of cluster resources, but preemption does not occur because the available queues are under capacity with lower relative resource usage. With only 80 GB available on any of the nodes, LLAP must wait for the long-running applications to finish before it can access cluster resources.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_queue_llap_example.png)

