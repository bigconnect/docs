# Bulk add or delete hosts

Use the **Hosts &gt; Actions &gt; Delete Hosts** option to delete multiple hosts in your cluster.

**Before you begin**  
Decommission and stop all components on the target hosts. There are guardrails in place to prevent the accidental deletion of hosts containing singleton master instances. To delete such hosts, first move all the masters to a different host.

**About this task**  
Use the **Actions** UI control to act on hosts in your cluster. Actions that you perform that comprise more than one operation, possibly on multiple hosts, are also known as _bulk operations_. After you request that Ambari perform a bulk operation, Ambari validates whether or not it can successfully perform the action . There are two possible scenarios where the operation cannot be performed:

* The state of the component makes it impossible to perform the operation. For example, asking to delete service components not in the stopped state. Expand each host to see the details on why the operation cannot be performed. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_bulk_component_delete_fail1.png) 
* Number of components remaining after the operation completes is less than the minimum number of components required by the stack. For example, asking to delete all the nodemanagers, where the minimum number of nodemanagers required by the service is 1. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_bulk_component_delete_fail2.png)

**Steps**

1. the **Hosts page,** select or search for multiple hosts.
2. Using the **Actions control**, click **Selected Hosts &gt; Hosts &gt; Delete Hosts.** ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_host_bulk_delete.png) 
3. Click **OK** to confirm the bulk operation.

**After you finish**  
Optionally, monitor background operations to follow, diagnose, or troubleshoot the bulk operation.  
  
**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)

