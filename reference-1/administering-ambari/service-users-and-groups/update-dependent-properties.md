# Update dependent properties

You must configure Service properties to match customized user and group names for each service.

Some property values must be set to match specific service or service group user names. You set such property values using Ambari Web, on the Advanced Configurations tab for the service.

1. Browse the **Ambari Web UI &gt; Services &gt; Service.Name &gt; Configs &gt; Advanced &gt;** tabs.
2. Set the property value.If you have set up non-default, customized service user names for the **HDFS** service, you must edit the following properties in **HDFS Settings: Advanced**:   **dfs.permissions.superusergroup** The same as the HDFS username. The default is hdfs.  **dfs.cluster.administrators** A single space followed by the HDFS username.  If you have set up non-default, customized service user names for the HBase service, you must edit the following properties in **HDFS Settings: Advanced:**   **dfs.block.local-path-access.user** The HBase username. The default is hbase.  If you have set up non-default, customized service user names for the Hadoop group name, you must edit the following properties, in **MapReduce Settings: Advanced:   mapreduce.cluster.administrators** A single space followed by the Hadoop group name.

