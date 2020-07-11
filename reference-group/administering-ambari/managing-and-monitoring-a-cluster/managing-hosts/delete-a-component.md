# Delete a component

Use **Component &gt; Action** to delete a decommissioned component:

**Before you begin:**  
Decommision the component that you intend to delete.

**Steps**

1. Using **Ambari Web**, browse the **Hosts** page. 
2. Find and click the FQDN of the host on which the component resides. 
3. In **Components**, find a decommissioned component. 
4. If the component status is Started, stop it. A decommissioned slave component may restart in the decommissioned state. 
5. Click **Delete** from the **Action** drop-down menu. Deleting a slave component, such as a DataNode does not automatically inform a master component, such as a NameNode, to remove the slave component from its exclusion list.Adding a deleted slave component back into the cluster presents the following issue: The added slave remains decommissioned from the master's perspective. Restart the master component, as a work-around. 
6. Restart services.

**Result**  
Ambari recognizes and monitors only the remaining components.  
  
**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)



