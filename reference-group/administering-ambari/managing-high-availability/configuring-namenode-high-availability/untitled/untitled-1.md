# Delete the additional NameNode

You may need to delete your additional NameNode.

**Before you begin**  
Check to see if you need to delete your Additional NameNode, on the Ambari Server host:

`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X GET [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/host_components?HostRoles/component_name=NAMENODE`

If the items array contains two NameNodes, the Additional NameNode must be deleted.

**Steps**

1. On the Ambari Server host, run the following command:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X DELETE [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/hosts/[ADDITIONAL_NAMENODE_HOSTNAME]/host_components/NAMENODE`

2. Verify that the Additional NameNode has been deleted:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X GET [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/host_components?HostRoles/component_name=NAMENODE`This should return an items array that shows only one NameNode.

**After you finish**  
Verify HDFS components.

  


