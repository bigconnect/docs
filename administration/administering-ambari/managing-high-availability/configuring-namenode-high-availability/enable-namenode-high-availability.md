# Enable NameNode high availability

To ensure that another NameNode in your cluster is always available if the active NameNode host fails, you should enable and configure NameNode high availability on your cluster using Ambari Web.

**About this task**  
The **Enable HA wizard** describes the set of automated and manual steps you must take to set up NameNode high availability. HDFS and ZooKeeper must stop and start when enabling NameNode HA. Maintenance Mode will prevent those start and stop operations from occurring. If the HDFS or ZooKeeper services are in Maintenance Mode the NameNode HA wizard will not complete successfully.

**Before you begin**

* Verify that you have at least three hosts in your cluster and are running at least three Apache ZooKeeper servers.
* Verify that the Hadoop Distributed File System \(HDFS\) and ZooKeeper services are not in Maintenance Mode.

**Steps**

1. In **Ambari Web**, browse to **Services &gt; HDFS &gt; Summary**. 
2. Click **Actions**, then click **Enable NameNode HA**. The Enable NameNode HA wizard launches. 
3. On **Get Started**, type in a Nameservice ID and click **Next**.  
   ****You use this Nameservice ID instead of the NameNode FQDN after HA is set up.  
  
   ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_getstartedha_2x.png)  
  
   If you are using Hive, you must manually change the Hive Metastore FS root to point to the Nameservice URI instead of the NameNode URI, as follows:  


   a. Find the current FS root on the Hive host.

   `hive --config /etc/hive/conf/conf.server --service metatool -listFSRoot`

  
   The output should look like: `Listing FS Roots... hdfs://[NAMENODE_HOST]/apps/hive/warehouse.`

  
   b. Change the FS root.

   `$ hive --config /etc/hive/conf/conf.server --service metatool -updateLocation [NEW_LOCATION][OLD_LOCATION]`

  
   For example, if your Nameservice ID is mycluster, you input:  


   `$ hive --config /etc/hive/conf/conf.server --service metatool -updateLocation hdfs://mycluster/apps/hive/warehouse hdfs://c6401.ambari.apache.org/apps/hive/warehouse`

  
   The output looks similar to:`Successfully updated the following locations... Updated X records in SDS table`

{% hint style="danger" %}
**Important**  
The Hive configuration path for a default HDP 2.3.x or later stack is `/etc/hive/conf/conf.server`

The Hive configuration path for a default HDP 2.2.x or earlier stack is `/etc/hive/conf`
{% endhint %}

        c. Adjust the ZooKeeper Failover Controller retries setting for your environment.  
            Browse to Services &gt; HDFS &gt; Configs &gt; Advanced core-site.

        d. Set `ha.failover-controller.active-standby-elector.zk.op.retries=120`.

4. On **Select Hosts**, select a host for the additional NameNode and the JournalNodes, and then click **Next**.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_selecthostha_2x.png)  


5. On **Review**, confirm your host selections and click **Next**:  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_reviewha_2x.png)  


6. Follow the directions on the **Manual Steps Required: Create Checkpoint on NameNode** page.You must log in to your _current_ NameNode host and run the commands to put your NameNode into safe mode and create a checkpoint.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_checkpointsha_2x.png)  
  
When Ambari detects success and the message on the bottom of the window changes to Checkpoint created, click **Next**.

7. On **Configure Components**, monitor the configuration progress bars.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_configurecomponentsha_2x.png)  
  
When all components have been configured successfully, click **Next**.

8. Follow the instructions on **Manual Steps Required: Initialize JournalNodes**.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_initializejournalnodesha_2x.png)  
  
You must log in to your _current_ NameNode host to run the command to initialize the JournalNodes. When JournalNodes initiate successfully, click **Next**.

9. On **Start Components**, monitor the progress bars as the ZooKeeper servers and NameNode start.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_startcomponentsha_2x.png)  
  
When ZooKeeper servers and NameNode have started, click **Next**.  
In a cluster with Ranger enabled, and with Hive configured to use MySQL, Ranger will fail to start if MySQL is stopped. To work around this issue, start the Hive MySQL database and then retry starting components.

10. On **Manual Steps Required: Initialize NameNode HA Metadata**, complete all steps, using the instructions on the page.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_initializemetadataha_2x.png)  
  
For this step, you must log in to both the _current_ NameNode and the _additional_ NameNode. Make sure you are logged in to the correct host for each command. Click **OK** to confirm, after you complete each command. When you have completed the four steps required to intialize metadata, click **Next**.

11. On **Finalize HA Setup**, monitor the progress bars, the wizard completes HA setup.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_finalizeha_2x.png)  
  
When the wizard completes HA setup, click **Done** to finish the wizard.After **Ambari Web** reloads, you may see some alert notifications. Wait a few minutes until all the services restart.

12. Restart any components using **Ambari Web,** if necessary.

**After you finish**  
Review and confirm all recommended configuration changes.

