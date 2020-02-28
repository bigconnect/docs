# Apply capacity scheduler queue hierarchy changes

Apply capacity scheduler queue hierarchy using the Actions ****menu, to ensure that all components pick up applied configuration changes.

**About this task**  
You can use the Actions menu to apply configuration changes made to the queue hierarchy. Depending on the configuration changes made, the Actions menu will guide you to the options available to apply the changes.

* For changes that are not valid and cannot be applied, the **Actions** button will turn red, and the menu will not appear.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_actions_menu_red.png) 
* For configuration changes that can be applied dynamically \(without restarting the YARN ResourceManager\), the Actions Menu will guide you to **Save and Refresh Queues**.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_actions_menu_refresh_queues.png) 
* For configuration changes that require a restart of the YARN ResourceManager, the Actions Menu will guide you to **Save and Restart ResourceManager**.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_actions_menu_restart_rm.png)

