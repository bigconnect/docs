# Delete all JournalNodes

You may need to delete any JournalNodes.

**Before you begin**  
 Check to see if you need to delete JournalNodes, on the Ambari Server host:

`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X GET [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/host_components?HostRoles/component_name=JOURNALNODE`

If this returns an empty items array, you can go on to delete the additional NameNode. Otherwise you must delete the JournalNodes.

**Steps**

1. On the Ambari Server host, run the following command:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X DELETE [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/hosts/[JOURNALNODE1_HOSTNAME]/host_components/JOURNALNODE curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X DELETE [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/hosts/[JOURNALNODE2_HOSTNAME]/host_components/JOURNALNODE     curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X DELETE [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/hosts/[JOURNALNODE3_HOSTNAME]/host_components/JOURNALNODE`

2. Verify that all the JournalNodes have been deleted. On the Ambari Server host:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X GET [AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/host_components?HostRoles/component_name=JOURNALNODE`This should return an `empty items` array.

**After you finish**  
Delete all additional NameNodes

