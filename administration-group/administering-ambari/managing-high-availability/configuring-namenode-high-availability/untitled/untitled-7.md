# Restore the HBase configuration

**About this task**  
If you have installed HBase, you might need to restore a configuration to its pre-HA state:

**Steps**

1. From the Ambari server host, determine whether your current HBase configuration must be restored:`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] get localhost [CLUSTER_NAME] hbase-site`Use the environment variables that you set up when preparing the Ambari server host for rollback for the variable names.
   * If `hbase.rootdir` is set to the NameService ID you set up using the **Enable NameNode HA** wizard, you must revert hbase-site to non-HA values. For example, in `"hbase.rootdir":"hdfs://[name-service-id]:8020/apps/hbase/data"`, the `hbase.rootdir` property points to the NameService ID and the value must be rolled back.
   * If hbase.rootdir points instead to a specific NameNode host, it does not need to be rolled back. For example, in `"hbase.rootdir":"hdfs://[nn01.mycompany.com]:8020/apps/hbase/data"`, the `hbase.rootdir` property points to a specific NameNode host and not a NameService ID. This does not need to be rolled back; you can proceed to delete ZooKeeper failover controllers. 
2. If you must roll back the `hbase.rootdir value`, on the Ambari Server host, use the `config.sh` script to make the necessary change: `/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p[AMBARI_PW] -port [AMBARI_PORT] set localhost [CLUSTER_NAME] hbase-site hbase.rootdir hdfs://[NAMENODE_HOSTNAME]:8020/apps/hbase/data`Use the environment variables that you set up when preparing the Ambari server host for rollback for the variable names.For Ambari 2.6.0 and higher, `config.sh` is not supported and will fail. Use `config.py` instead.
3. On the Ambari server host, verify that the `hbase.rootdir` property has been restored properly:`/var/lib/ambari-server/resources/scripts/configs.py -u [AMBARI_USER] -p [AMBARI_PW] -port [AMBARI_PORT] get localhost [CLUSTER_NAME] hbase-site`The `hbase.rootdir` property should now be the same as the NameNode hostname, not the NameService ID.

**After you finish**  
Delete the ZooKeeper failover controllers.

