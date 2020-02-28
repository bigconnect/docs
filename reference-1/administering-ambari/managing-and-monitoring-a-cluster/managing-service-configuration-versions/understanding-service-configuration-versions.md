# Understanding service configuration versions

Ambari creates and stores a unique configuration version when you change any property value in a config type.

It is important to understand how service configurations are organized and stored in Ambari. Properties are grouped into configuration types. A set of _config types_ composes the set of configurations for a service.

For example, the Hadoop Distributed File System \(HDFS\) service includes the hdfs-site, core-site, hdfs-log4j, hadoop-env, and hadoop-policy config types. Using **Ambari Web**, browse to **Services &gt; HDFS &gt; Configs**, to edit the configuration properties for these config types.

Ambari performs configuration versioning at the service level. Therefore, when you modify a configuration property in a service, Ambari creates a _service config version_. The following figure shows version 1 \(V1\) and version 2 \(V2\) of a service configuration with a change to a property in Config Type A. When you changie a property value in Config Type A in V1, Ambari creates V2.  
  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/concepts/images/amb_svc_cnfg_vrsn_cncpt.png)

