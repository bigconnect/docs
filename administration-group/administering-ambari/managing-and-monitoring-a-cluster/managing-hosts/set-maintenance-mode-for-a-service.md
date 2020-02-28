# Set Maintenance Mode for a service

How to set Maintenance Mode for a service and what happens when you do.

**About this task**  
Setting Maintenance Mode enables you to suppress alerts and omit bulk operations for specific services, components, and hosts in an Ambari-managed cluster. Explicitly setting Maintenance Mode for a service implicitly sets Maintenance Mode for components and hosts that run the service. While Maintenance Mode prevents bulk operations being performed on the service, component, or host, you may explicitly start and stop a service, component, or host while in Maintenance Mode.

**Steps**

1. Using **Services**, click a service name. For example, **HDFS.** 
2. Click **Actions**, then click **Turn On Maintenance Mode**. 
3. Click **OK** to confirm. Notice, on **Services Summary** that Maintenance Mode turns on for the NameNode and SNameNode components.

**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)

