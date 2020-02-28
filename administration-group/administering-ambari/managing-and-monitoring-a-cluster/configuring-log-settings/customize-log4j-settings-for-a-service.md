# Customize log4j settings for a service

Edit the _log4j property template_ in which default log4j properties reside, and use those customizations to overwrite the custom log4j properties for each service to control logging activities for that service.

**About this task**  
Ambari uses sets of properties called Log4j properties to control logging activities for each service running in your Hadoop cluster. Initially, default values for each property reside in the log4j property group. To customize log settings, first access the Log4j property group for the service. Then, edit the log4j template, and save a service-specific version of property values appropriate to control log activities for the service, in **custom \[SERVICE\_NAME\]log4j properties**.

**Steps**

1. In **Ambari Web**, browse to **Services &gt; \[SERVICE\_NAME\] &gt; Configs &gt; Advanced\[SERVICE\_NAME\]-log4j**. 
2. Edit values of any properties in the **\[SERVICE\_NAME\] log4j template**. 
3. Copy the content of the log4j template file. 
4. Browse to the **custom \[SERVICE\_NAME\]log4j** properties group. 
5. Paste the copied content into **custom \[SERVICE\_NAME\]log4j properties**, overwriting, the default content. 
6. In **Configs**, click **Save**. 
7. Review and confirm any recommended, related configuration changes, as prompted. 
8. Restart affected components and services, as prompted. Restarting components in the service pushes the configuration properties displayed in **Custom log4j.properites** to each host running components for that service.

**After you finish**  
 If you have customized logging properties that define how activities for each service are logged, you see refresh indicators next to each service name. Ensure that logging properties displayed in **Custom logj4.properties** include any customization.  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_edit_log4jprops.png)  
Optionally, you can create configuration groups that include custom logging properties.

  


