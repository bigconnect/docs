# Set Maintenance Mode for a host

How to set Maintenance Mode for a host and what happens when you do.  
  
**About this task**  
This task provides examples of how to use Maintenance Mode in a three-node, Ambari-managed cluster installed using default options and having one data node, on host c7403. The example describes how to explicitly turn on Maintenance Mode for the HDFS service, alternative procedures for explicitly turning on Maintenance Mode for a host, and the implicit effects of turning on Maintenance Mode for a service, a component, and a host.

Steps

1. On the **Hosts** page, select a host FQDN. `c7401.ambari.apache.org` 
2. In **Host Actions**, click **Turn On Maintenance Mode**. 
3. Click **OK** to confirm. Notice, on **Components**, that Maintenance Mode turns on for all components. 
4. To set Maintanence Mode for a muliple hosts, using the **Actions** control. 
5. Using **Hosts**, click `c7401.ambari.apache.org` and `c7403.ambari.apache.org.` 
6. **In Actions &gt; Selected Hosts &gt; Hosts**, click **Turn On Maintenance Mode**.                              ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_set_mm_multiple_hosts.png)
7. Choose **OK.**

**Result**  
Your list of hosts shows that Maintenance Mode is set for hosts c7401 and c7403:  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_mm_hosts_components.png)  
  
**Example**  
If you hover your cursor over each Maintenance Mode icon that appears in the hosts list, you see the following information:

* Hosts c7401 and c7403 are in Maintenance Mode.
* On host c7401, HBaseMaster, HDFS client, NameNode, and ZooKeeper Server are also in Maintenance Mode.
* On host c7403, 15 components are in Maintenance Mode.
* On host c7402, HDFS client and Secondary NameNode are in Maintenance Mode, even though the host is not.

Notice also how the DataNode is affected by setting Maintenance Mode on this host:

* Alerts are suppressed for the DataNode.
* DataNode is omitted from HDFS Start/Stop/Restart All, Rolling Restart.
* DataNode is omitted from all Bulk Operations except Turn Maintenance Mode ON/OFF.
* DataNode is omitted from Start All and / Stop All components.
* DataNode is omitted from a host-level restart/restart all/stop all/start.

**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)

  


