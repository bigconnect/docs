# Changing host names

A non-trivial change in which you must update the Ambari and all hosts of the new names.

Circumstances may require that you change the names of the hosts in your existing cluster. Beyond any infrastructure and environment changes you need to make, you must also change the host names that Ambari uses to manage the cluster.

**Before you begin**

* Make a backup of your Ambari database.
* Disable Kerberos. Using Ambari Web, browse to **Admin &gt; Kerberos** and click **Disable Kerberos**.

1.In Ambari Web &gt; Background Operations, stop all pending commands and jobs.

2.In Ambari Web &gt; Dashboard, stop all services

3.Stop ambari-server and ambari-agents on all hosts

4.Create \*.json file with host names changes.

`host_names_changes.json`

```javascript
{
  "cluster1" : {
      "c6400.ambari.apache.org" : "c6410.ambari.apache.org",
      "c6401.ambari.apache.org" : "c6411.ambari.apache.org",
     ....
  }
}
```

where cluster1 is cluster name and "c6400.ambari.apache.org" : "c6410.ambari.apache.org"is the host names pair in the format:

`[CURRENT_HOST_NAME] : [NEW_HOST_NAME]`

5.Execute the following command on the ambari-server host:

`ambari-server update-host-names host_names_changes.json`

6.After successful end of this action, update host names for all nodes, according to changes that you added to \*.json file.

7.If you changed the host name for the node on which the ambari server resides, then you must update that name for every ambari-agent.  
In `/opt/bdl/etc/ambari-agent/conf/ambari-agent.ini`, update the hostname field to the new host name for node on which the ambari-server resides.

8.Start ambari-server and ambari-agents on all hosts

9.If you have NameNode HA enabled, after starting the ZooKeeper service, you must:

* Start all ZooKeeper components.
* Execute the following command on one of the NameNode hosts: `hdfs zkfc -formatZK -force`

10.Start all services, using Ambari Web. For each, browse to **Services &gt; Service.name &gt; Service Actions &gt; Start**.

**After you finish**  
If you disabled Kerberos before starting this procedure, you must enable Kerberos security by working through either the automated or manual setup procedure. If you enable Kerberos with the manual option, you must generate and deploy new keytabs that contain the new host names.

