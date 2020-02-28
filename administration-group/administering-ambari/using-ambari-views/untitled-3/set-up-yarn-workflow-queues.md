# Set up YARN workflow queues

To set up Capacity Scheduler queues on a view instance.  
  
**Steps**

1. On the YARN Queue Manager view instance configuration page, click **Add Queu**e.  
   To return to a _previously created_ YARN Queue Manager view instance:  


   a. Click **Views** on the Manage Ambari page.  


   b. Click **CAPACITY-SCHEDULER**.  


   c. Click the applicable YARN Queue Manager view instance, then click **Go to instance** at the top of the page.The queue will be added under the top-level, or root queue. A default queue already exists under the root queue.  
  
   ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/installation-upgrade/images/amb_views_cs_add_queue_button.png)  

2. Type in a name for the new queue, then click the green check mark to create the queue. In the following example, we're creating a queue named Engineering.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/installation-upgrade/images/amb_views_cs_add_queue_engineering.png)  The Engineering queue is added, and its configuration page appears.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/installation-upgrade/images/amb_views_cs_queue_engineering.png)
3. Set the capacity for the Engineering queue to 60%.The sum of queue capacities at any level in the YARN Queue Manager configuration must total 100%. Here the default queue is already set to 100%. Therefore, if we try to set the Engineering queue capacity to 60%, error messages appear warning that the total at this level is 160%  .![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/installation-upgrade/images/amb_views_cs_queue_engineering_error.png)
4. If we click the **Default** queue and set its capacity to 0%, the **Level Total** bar at the top of the page lists the total queue capacity at this level as 60%.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/installation-upgrade/images/amb_views_cs_queue_default_zero.png) 
5. To add more queues at the root level, click the **root** queue, then click **Add Queue**. In the following example, we have added a Support queue set to 10% of the level capacity, and a Marketing queue set to 30%. The root-level queue capacities now total 100%.   ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/installation-upgrade/images/amb_views_cs_root_queues.png) 
6. To save your configuration, click **Actions &gt; Save Only**. 
7. On the **Notes** pop-up, enter an optional description of your changes, then click **Save**. Each version is retained and listed in the Versions box.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/installation-upgrade/images/amb_views_cs_save_queues.png) 
8. To build a queue hierarchy, click a top-level queue, then click **Add Queue**. In the following example, the qa and development queues have been added under the Engineering queue.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/installation-upgrade/images/amb_views_cs_engineering_subqueues.png)

