# Add a Hive metastore component

**Before you begin**  
If you have ACID enabled in Hive, ensure that the _Run Compactor_ setting is enabled \(set to True\) on only one Hive metastore host.

**Steps**

1. In **Ambari Web**, browse to **Services &gt; Hive**. 
2. In **Service Actions**, click the **+ Add Hive Metastore** option. 
3. Choose the host on which to install the additional Hive Metastore; then click **Confirm Add**.

**Result**  
Ambari installs the component and reconfigures Hive to handle multiple Hive Metastore instances.  
  
**After you finish**  
Review and confirm all recommended configuration changes.

