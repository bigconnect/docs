# Download client configuration files for a cluster

Use the **Ambari Web &gt; Services &gt; ... &gt; Download All Client Configs** option to download all client configurations for your cluster.

**About this task**  
Client configuration files include; .xml files, env-sh scripts, and log4j properties used to configure Hadoop services. For services that include client components \(most services _except_ SmartSense and Ambari Metrics Service\), you can download the client configuration files associated with that service. You can also download the client configuration files for your entire cluster as a single archive. To download client configuration files for your entire cluster:  
  
**Steps**

1. In **Ambari Web &gt; Services &gt; ...**, click **Download All Client Configs**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_download_all_client_configs.png) Your browser downloads a tarball archive containing all client configuration files for your cluster to your default, local downloads directory. 
2. If prompted to save or open the cluster configs bundle, click **Save File**, then click **OK**.

