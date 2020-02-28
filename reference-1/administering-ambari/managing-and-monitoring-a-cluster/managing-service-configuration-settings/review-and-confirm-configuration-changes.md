# Review and confirm configuration changes

When you change a configuration setting, use **Dependent Configurations** to confirm other, related configuration changes.

**Before you begin**  
 Change a configuration property. As prompted, click Show Details.  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_config_change_show_details.png)

**About this task**  
When you change a configuration property value, the Ambari Stack Advisor captures and recommends changes to all related configuration properties affected by your original change. Changing a single property, a Smart Configuration, and other actions; such as adding or deleting a service, host, or ZooKeeper Server, moving a master, or enabling high availability for a component, all require that you review and confirm related configuration changes. For example, if you increase the Minimum Container Size \(Memory\) setting for YARN, **Dependent Configurations** lists all recommended changes that you must review and \(optionally\) accept.  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_dependent_configurations.png)  
  
Types of changes are highlighted in the following colors:  
  
**Value Changes**  
Yellow  
  
**Added Properties**  
Green  
  
**Deleted properties**  
Red  
  
To review and confirm changes to configuration properties:  
  
**Steps**

1. In **Dependent Configurations**, for each listed property review the summary information. 
2. If the change is acceptable, proceed to review the next property in the list. 
3. If the change is not acceptable, click the check mark in the blue box to the right of the listed property change.Clicking the check mark clears the box. Changes for which you clear the box are not confirmed and will not occur. 
4. After reviewing all listed changes, click **OK** to confirm that all marked changes occur.

**After you finish**  
Restart any components marked for restart to utilize the changes you confirmed.  


