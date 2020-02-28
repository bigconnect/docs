# Monitor background operations

Use **Background Operations** to monitor tasks during execution and examime log details, post-completion.  
  
**About this task**  
You can use the Background Operations window to monitor progress and completion of a task that comprises multiple operations, such as a rolling restart of components. The Background Operations window opens by default when you run such a task. This topic shows monitoring the progress of a rolling restart as an example.

**Steps**

1. In **Background Operations**, click the text that describes each operation to show restart operation progress on each host. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_rlgrestart_dnds_ndmgrs.png) 
2. After operations complete, you can click an operation name link to view log files and any error messages generated on the selected host. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_background_ops_hosts.png) ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_background_ops_on_host_1.png) 
3. Optionally, click a host name link to view tasks on that host.![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_background_ops_on_host_2.png) 
4. Optionally,click a task name link to view the task log generated during the operation.You can use the **Copy**, **Open**, or **Host Logs** icons located at the upper-right of the \[HOST\_NAME\] dialog to copy, open, or view log information generated during the operation. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_background_ops_logs.png)  This example demonstrates the task log that contains error and output information for the restart NodeManagers task run on host `c7403.ambari.apache.org.`

**After you finish**  
You can also select the check box at the bottom of the Background Operations window to hide the Background Operations dialog during future operations.  


