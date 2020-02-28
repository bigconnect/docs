# Re-enable the standby NameNode

To re-enable the standby NameNode:

**Steps**

1. Run the following commands on the Ambari Server host:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo":{"context":"Enable Secondary NameNode"},"Body":{"HostRoles":{"state":"INSTALLED"}}}'[AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/[CLUSTER_NAME]/hosts/[SECONDARY_NAMENODE_HOSTNAME}/host_components/SECONDARY_NAMENODE`

2. Analyze the output. 
3. If this returns `200`, proceed to delete all JournalNodes. 
4. If this input returns the value 202, wait a few minutes and then run the following command:`curl -u [AMBARI_USER]:[AMBARI_PW] -H "X-Requested-By: ambari" -i -X GET "[AMBARI_PROTO]://localhost:[AMBARI_PORT]/api/v1/clusters/`\[CLUSTER\_NAME\]`/host_components?HostRoles/component_name=SECONDARY_NAMENODE&fields=HostRoles/state"`

5. Wait for the response `"state" : "INSTALLED"` before proceeding.

**After you finish**  
Delete all JournalNodes.

