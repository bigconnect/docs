# Register a remote cluster

Ambari-managed clusters not local to your Ambari server host must be registered as remote clusters with your Ambari Server.

You might work with clusters that are managed by Ambari but are not local to your Ambari server. These clusters are considered remote with respect to your local Ambari server. They are managed by a remote Ambari server. If you plan to run a standalone server to host views, including accessing clusters managed by a different Ambari server, you can register the cluster managed by the standalone Ambari server as a remote cluster. After you register a remote cluster, use it to configure view instances.

1. In **Ambari Admin &gt; Remote Clusters**, click **Register Remote Cluster**.
2. In **Remote Clusters/Register**, enter a name for the remote cluster, the Ambari cluster URL, and a cluster user name and associated password.
3. Click **Save**.

The remote cluster is now available for configuring View instances.

