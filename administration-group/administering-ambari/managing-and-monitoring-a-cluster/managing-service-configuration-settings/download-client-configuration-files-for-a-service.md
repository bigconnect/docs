# Download client configuration files for a service

Use the **Services &gt; Actions &gt; Download Client Configs** option to download client configurations for a single service.

**About this task**  
Client configuration files include; .xml files, env-sh scripts, and log4j properties used to configure Hadoop services. For services that include client components \(most services _except_ SmartSense and Ambari Metrics Service\), you can download the client configuration files associated with that service. You can also download the client configuration files for your entire cluster as a single archive. To download client configuration files for a single service:

**Steps**

1. In **Ambari Web &gt; Services**, click the service for which you want to download configurations. 
2. Click **Actions**. 
3. Click **Download Client Configs**.Your browser downloads a tarball archive containing only the client configuration files for that service to your default, local downloads directory. 
4. If prompted to save or open the client configs bundle, ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_dwnld_clnt_cnfgs.png) Click **Save File**, then click **OK**.

  


