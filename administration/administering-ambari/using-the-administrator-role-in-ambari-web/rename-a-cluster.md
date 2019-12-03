# Rename a cluster

After you create a cluster, you can give it a new name:

1.On the Ambari Admin page, in Cluster Information &gt; Cluster Name\*, enter up to 80 alphanumeric characters to rename your cluster.

![](../../../.gitbook/assets/image%20%2826%29.png)

2. Click **Save**.

3. Confirm.

5. Restart Ambari server and the Ambari agents.   
After renaming the cluster, alert checks must be re-queued on the agents. Therefore, you must restart Ambari Server and the Ambari Agents for the change to take effect.

6. Adjust any API calls you make to use the new name.  
Changing the name of the cluster changes the name of the Ambari REST API resource for the cluster.



