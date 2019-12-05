# Disable ResourceManager high availability

Using the Ambari API, delete the ResourceManager. Then, use the ZooKeeper client to update the znode permissions.

**About this task**  
To disable ResourceManager high availability, you must delete one ResourceManager and keep one ResourceManager. This requires using the Ambari API to modify the cluster configuration to delete the ResourceManager and using the ZooKeeper client to update the znode permissions.

**Before you begin**  
Because these steps involve using the Ambari REST API, you should test and verify them in a test environment prior to executing against a production environment.

**Steps**

1. In **Ambari Web**, stop YARN and ZooKeeper services.  


2. On the Ambari server host, use the Ambari API to retrieve the YARN configurations into a JSON file.`/var/lib/ambari-server/resources/scripts/configs.py get [AMBARI_SERVER] [CLUSTER_NAME] yarn-site yarn-site.json`In this example, \[AMBARI\_SERVER\] is the hostname of your Ambari Server and \[CLUSTER\_NAME\] is the name of your cluster.

For Ambari 2.6.0 and higher, `config.sh` is not supported and will fail. Use `config.py` instead.

3. In the yarn-site.json file, change _yarn.resourcemanager.ha.enabled_ to _false_ and delete the following properties:

* yarn.resourcemanager.ha.rm-ids
* yarn.resourcemanager.hostname.rm1
* yarn.resourcemanager.hostname.rm2
* yarn.resourcemanager.webapp.address.rm1
* yarn.resourcemanager.webapp.address.rm2
* yarn.resourcemanager.webapp.https.address.rm1
* yarn.resourcemanager.webapp.https.address.rm2
* yarn.resourcemanager.cluster-id
* yarn.resourcemanager.ha.automatic-failover.zk-base-path

4. Verify that the following properties in the `yarn-site.json` file are set to the ResourceManager         hostname you are keeping:

* yarn.resourcemanager.hostname
* yarn.resourcemanager.admin.address
* yarn.resourcemanager.webapp.address
* yarn.resourcemanager.resource-tracker.address
* yarn.resourcemanager.scheduler.address
* yarn.resourcemanager.webapp.https.address
* yarn.timeline-service.webapp.address
* yarn.timeline-service.webapp.https.address
* yarn.timeline-service.address
* yarn.log.server.url

5. Search the `yarn-site.json` file and remove any references to the ResourceManager hostname that you are removing.  
  
6. Search the `yarn-site.json` file and remove any properties that might still be set for ResourceManager IDs.For example, rm1 and rm2.

7. Save the `yarn-site.json` file and set that configuration against the Ambari Server.`/var/lib/ambari-server/resources/scripts/configs.py set [AMBARI_SERVER] [CLUSTER_NAME] yarn-site yarn-site.json`

8. Using the Ambari API, delete the ResourceManager host component for the host that you are deleting.`curl --user admin:admin -i -H "X-Requested-By: ambari" -X DELETE http://ambari.server:8080/api/v1/clusters/cluster.name/hosts/hostname/host_components/RESOURCEMANAGER`

9. In **Ambari Web**, start the ZooKeeper service.

10. On a host that has the ZooKeeper client installed, use the ZooKeeper client to change znode permissions.

```text
/usr/hdp/current/zookeeper-client/bin/zkCli.sh
getAcl /rmstore/ZKRMStateRoot
setAcl /rmstore/ZKRMStateRoot world:anyone:rwcda
```

11. In **Ambari Web**, restart ZooKeeper service and start YARN service.

**After you finish**  
Review and confirm all recommended configuration changes.

