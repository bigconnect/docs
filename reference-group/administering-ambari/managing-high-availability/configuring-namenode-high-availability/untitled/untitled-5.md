# Modify HDFS configurations

You may need to modify your `hdfs-site` configuration and/or your `core-site` configuration.

**Before you begin**  
Check whether you need to modify your `hdfs-site` configuration, by executing the following command on the Ambari Server host:

`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] get localhost [CLUSTER_NAME] hdfs-site`

If you see any of the following properties, you must delete them from your configuration.

* dfs.nameservices
* dfs.client.failover.proxy.provider.\[NAMESERVICE\_ID\]
* dfs.ha.namenodes.\[NAMESERVICE\_ID\]
* dfs.ha.fencing.methods
* dfs.ha.automatic-failover.enabled
* dfs.namenode.http-address.\[NAMESERVICE\_ID\].nn1
* dfs.namenode.http-address.\[NAMESERVICE\_ID\].nn2
* dfs.namenode.rpc-address.\[NAMESERVICE\_ID\].nn1
* dfs.namenode.rpc-address.\[NAMESERVICE\_ID\].nn2
* dfs.namenode.shared.edits.dir
* dfs.journalnode.edits.dir
* dfs.journalnode.http-address
* dfs.journalnode.kerberos.internal.spnego.principal
* dfs.journalnode.kerberos.principal
* dfs.journalnode.keytab.file

Where \[NAMESERVICE\_ID\] is the NameService ID you created when you ran the **Enable NameNode HA** wizard.

**Steps**

1. Note - For Ambari 2.6.0 and higher, `config.sh` is not supported and will fail. Use `config.py` instead. 
2. On the Ambari Server host, execute the following _for each property_ you found:`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] delete localhost [CLUSTER_NAME] hdfs-site [PROPERTY_NAME]`Replace \[PROPERTY\_NAME\] with the name of _each_ of the properties to be deleted. 
3. Verify that all of the properties have been deleted:`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] get localhost [CLUSTER_NAME] hdfs-site`None of the properties listed above should be present. 
4. Determine whether you must modify your `core-site` configuration:`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] get localhost [CLUSTER_NAME] core-site`

5. If you see the property `ha.zookeeper.quorum`, delete it:`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] delete localhost [CLUSTER_NAME] core-site ha.zookeeper.quorum`

6. If the property `fs.defaultFS` is set to the NameService ID, revert it to its non-HA value:`"fs.defaultFS":"hdfs://[name-service-id]" The property fs.defaultFS needs to be modified as it points to a NameService ID "fs.defaultFS":"hdfs://[nn01.mycompany.com]"`You need not change the property fs.defaultFS, because it points to a specific NameNode, not to a NameService ID. 
7. Revert the property `fs.defaultFS` to the NameNode host value:`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] set localhost [CLUSTER_NAME] core-site fs.defaultFS hdfs://[NAMENODE_HOSTNAME]`

8. Verify that the core-site properties are now properly set:`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] get localhost [CLUSTER_NAME] core-site`The property `fs.defaultFS` should be the NameNode host and the property `ha.zookeeper.quorum` should not appear.

**After you finish**  
Re-create the standby NameNode.

  


