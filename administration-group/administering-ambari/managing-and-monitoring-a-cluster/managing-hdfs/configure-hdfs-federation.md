# Configure HDFS Federation

An HDFS federation allows you to scale a cluster horizontally by configuring multiple namespaces and NameNodes. The DataNodes in the cluster are available as a common block of storage for every NameNode in the federation.

**Before you begin**

* Ensure that you have planned for a cluster maintenance window before configuring the HDFS federation because all the cluster services are restarted during the process of configuration.
* Verify that you have configured HA for all the NameNodes that you want to include in the federation.

**About this task**  
You must associate every NameNode you want to include in a federation with a namespace. You can configure a maximum of four namespaces in a federated environment.

**Steps**

1. In **Ambari Web**, select **Services &gt; HDFS &gt; Summary**. 
2. Click **Actions &gt; Add New HDFS Namespace**. The Add New HDFS Namespace wizard launches. The wizard describes the set of automated and manual steps you must perform to add the new namespace. 
3. On the **Get Started** page, type in a NameService ID and click **Next**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_add_hdfs_namespace_getstarted.png) 
4. On the **Select Hosts** page, select a host for the additional NameNodes and JournalNodes, and click **Next**.![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_add_hdfs_namespace_selecthosts.png) 
5. On the **Review** page, confirm your host selections and click **Next**.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_add_hdfs_namespace_review.png) 
6. On the **Configure Components** page, monitor the progress bars as the wizard completes adding the new namespace, then click **Complete**.  
   ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_add_hdfs_namespace_restart2.png)

   After the Ambari Web UI reloads, you may see some alert notifications. Wait a few minutes until all the services restart.  
  
   You can navigate to **Services &gt; HDFS &gt; Summary** to view details of the newly added namespace.

{% hint style="info" %}
**Note**  
If the process of adding the new namespace does not complete, then you must update the configuration properties of the NameNodes so that the cluster reverts to its earlier functional state.
{% endhint %}

  


  


