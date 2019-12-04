# Perform host level actions

Use **Hosts &gt; Actions** options to start bulk operations across multiple hosts in your cluster.  
  
**About this task**  
Use the Actions UI control to act on hosts in your cluster. Actions that you perform that comprise more than one operation, possibly on multiple hosts, are also known as _bulk operations_. The **Actions** control comprises a workflow that uses a sequence of three menus to refine your search: a hosts menu, a menu of objects based on your host choice, and a menu of actions based on your object choice. For example, if you want to restart the RegionServers on any host in your cluster on which a RegionServer exists:

**Steps**

1. In the **Hosts** page, select or search for hosts running a RegionServer: 
2. Using the **Actions** control, click **Filtered Hosts &gt; RegionServers &gt; Restart &gt;**

![](../../../../.gitbook/assets/image%20%28249%29.png)

3. Click **OK** to start the selected operation.

**After you finish**  
Optionally, monitor background operations to follow, diagnose, or troubleshoot the restart operation.  
  
**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)

