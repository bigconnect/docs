# Delete a host from a cluster

Use **Host Actions &gt; Delete Host** to remove a host from the cluster.

**About this task**  
Deleting a host removes the host from the cluster.

**Before you begin**  
Before deleting a host, you must:

* Stop all components running on the host.
* Decommission any DataNodes running on the host.
* Move from the host any master components, such as NameNode or ResourceManager, running on the host.
* Turn off host Maintenance Mode, if it is on.

**Steps**

1. Using **Ambari Web**, browse the hosts page to find and click the FQDN of the host that you want to delete. 
2. On the **Host** page, click **Host Actions**. 
3. Click **Delete Host**.

![](../../../../.gitbook/assets/image%20%28162%29.png)

The host is removed from the cluster**.**

**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)

