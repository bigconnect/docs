# Delete ZooKeeper failover controllers

**Before you begin**  
If the following command on the Ambari server host returns an empty items array then you must delete ZooKeeper \(ZK\) Failover Controllers:

`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/host_components?HostRoles/component_name=ZKFC`

**Steps**

1. On the Ambari Server host, issue the following DELETE commands:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X DELETE [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/hosts/[NAMENODE_HOSTNAME]/host_components/ZKFC curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X DELETE [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/hosts/[ADDITIONAL_NAMENODE_HOSTNAME]/host_components/ZKFC`
2. Verify that the controllers are gone:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/host_components?HostRoles/component_name=ZKFC`This command should return an empty items array.

**After you finish**  
Modify HDFS configurations.

