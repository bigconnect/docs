# Manage JournalNodes

After enabling NameNode high availability, use the Manage JournalNodes wizards to maintain at least three active JournalNodes.

**About this task**  
NameNode high availability requires that you must maintain at least three, active JournalNodes in your cluster. You can use the **Manage JournalNode** wizard to assign, add, or remove JournalNodes on hosts in your cluster. The **Manage JournalNode** wizard enables you to assign JournalNodes, review and confirm required configuration changes, and will restart all components in the cluster to take advantage of the changes made to JournalNode placement and configuration. Completing the **Manage JournalNode** wizard restarts all cluster services.  
  
**Before you begin**  
NameNode high availability must be enabled in your cluster.  
  
**Steps**

1.  In **Ambari Web**, browse to **Services &gt; HDFS &gt; Summary**. 
2. Click **Service Actions**, then click **Manage JournalNodes**.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_1-open.png) 
3. On **Assign JournalNodes**, make assignments by clicking the **+** and **-** icons and selecting host names in the drop-down menus.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_2-assignJNs.png)  The **Assign JournalNodes** page enables you to maintain three, current JournalNodes by updating each time you make an assignment. 
4. On **Review**, verify the summary of your JournalNode host assignments and the related configuration changes.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_3-review_assignments.png)  When you are satisfied that all assignments match your intentions, click **Next**. 
5. Using a remote shell, complete the steps on **Save Namespace**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_4-save_namespace.png) When you have successfully created a checkpoint, click **Next**. 
6. On **Add/Remove JournalNodes**, monitor the progress bars.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_5-monitor_add-removejns.png)  When progress completes, click **Next**. 
7. Follow the instructions on **Manual Steps Required: Format JournalNodes**.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_6-manual_reformat_jns.png)  When JournalNodes have initialized, click **Next**. 
8. In the remote shell, confirm that you want to initialize JournalNodes, by entering Y, at the following prompt:`Re-format filesystem in QJM to [host.ip.address.1, host.ip.address.2, host.ip.address.3,] ? (Y or N) Y` 
9. On **Start Active NameNodes**, monitor the progress bars.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_7-monitor_start_nns.png)  When all services have re-started, click **Next**. 
10. On **Manual Steps Required: Bootstrap Standby NameNode**, complete each step, using the instructions on the page.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_8-bootstrap_stanadby_nn.png)  When you complete the bootstrap steps, click **Next**. 
11. In the remote shell, confirm that you want to bootstrap the standby NameNode, by entering Y, at the following prompt:`RE-format filesystem in Storage Directory /grid/0/hadoop/hdfs/namenode ? (Y or N) Y` 
12. On **Start All Services**, monitor the progress bars.  ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-high-availability/how-to/images/amb_manage_jn_9-start_services.png)  When the wizard has started all services, click **Done**. After **Ambari Web** reloads, you may see some alert notifications. Wait a few minutes until all the services restart and alerts clear. 
13. Restart any components using **Ambari Web**, if necessary.

**After you finish**  
Review and confirm all recommended configuration changes.

