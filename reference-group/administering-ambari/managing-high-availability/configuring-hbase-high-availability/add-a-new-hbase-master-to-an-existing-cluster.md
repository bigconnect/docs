# Add a new HBase master to an existing cluster

**Steps**

1. Log in to the Ambari management interface as a cluster administrator. _admin/admin_ 
2. In **Ambari Web**, browse to **Services &gt; HBase**. 
3. In **Service Actions**, click **+ Add HBase Master**. 
4. Choose the host on which to install the additional HBase master; then click **Confirm Add**.

**Result**  
Ambari installs the new HBase Master and reconfigures HBase to manage multiple Master instances.

**After you finish**  
Add a standby HBase master to the cluster.

