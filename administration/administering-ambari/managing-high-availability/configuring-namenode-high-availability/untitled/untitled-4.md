# Re-create the standby NameNode

You may need to recreate your standby NameNode.

**Before you begin**  
Check whether you need to recreate the standby NameNode, on the Ambari Server host:

`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X GET [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/host_components?HostRoles/component_name=SECONDARY_NAMENODE`

If this returns an empty items array, you must _recreate_ your standby NameNode. Otherwise you can proceed to _re-enable_ your standby NameNode.

**Steps**

1. On the Ambari Server host, run the following command:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X POST -d '{"host_components" : [{"HostRoles":{"component_name":"SECONDARY_NAMENODE"}}] }' [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/hosts?Hosts/host_name=[SECONDARY_NAMENODE_HOSTNAME]`

2. Verify that the standby NameNode now exists. On the Ambari Server host, run the following command:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X GET [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/host_components?HostRoles/component_name=SECONDARY_NAMENODE`This should return a non-empty items array containing the standby NameNode.

**After you finish**  
Re-enable the standby NameNode.

