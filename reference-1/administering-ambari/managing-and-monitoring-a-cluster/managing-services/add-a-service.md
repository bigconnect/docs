# Add a service

Use **Add Service** to deploy additional services in your cluster without interrupting operations.

**About this task**  
The Ambari installation wizard installs all available Hadoop services by default. You can choose to deploy only some services initially, and then add other services as you need them. For example, many customers deploy only core Hadoop services initially. The **Add Service** option enables you to deploy additional services without interrupting operations in your Hadoop cluster. When you have deployed all available services, the **Add Service** control display dims, indicating that it is unavailable. To add a service, follow the steps shown in this example.

**Steps**

1. In **Services**, click **... &gt; +Add Service**. The **Add Service** wizard opens. 
2. In the **Add Service** wizard, click **Choose Services**. The Choose Services pane lists those services already added in a shaded background and with checked boxes. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_add_choose_services.png) 
3. In **Choose Services**, click the empty check box next to the service that you want to add. Then, click **Next**. Notice that you can also select all services listed by clicking the checkbox next to the Service table column heading.The Add Services Wizard indicates hosts on which the master components for a chosen service will be installed. A service chosen for addition shows a grey check mark. 
4. In **Assign Masters**, confirm the default host assignment. Alternatively, use the drop-down menu to choose a different host machine to which master components for your selected service will be added. Then, click **Next**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_assign_masters.png) 
5. If you are adding a service that requires slaves and clients, in **Assign Slaves and Clients**, accept the default assignment of slave and client components to hosts. Alternatively, select hosts on which you want to install slave and client components \(at least one host for the slave of each service being added\). Then, click **Next**.  
   ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_assign_slaves_clients.png)  


   | Table 1 | Host Roles Required for Added Services |
   | :--- | :--- |
   | **Service Added** | **Host Role Required** |
   | YARN | NodeManager |
   | HBase | RegionServer |

6. In **Customize Services**, accept the default configuration properties. If necessary, edit any property values indicated. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_customize_services.png)
   * If necessary, click **Override** to create a configuration group for this service.
   * Make sure all configurations have been addressed. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_customize_services_complete.png) Then, click **Next**. 
7. In **Review**, verify that the configuration settings match your intentions. Then, click **Deploy**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_review_service_config.png) 
8. Monitor the progress of installing, starting, and testing the service. When that finishes successfully, click **Next**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_add_service_ist_success.png) 
9. Review the summary display of installation results. Then, click **Complete**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_add_service_summary.png) 
10. Review and confirm recommended configuration changes.

**After you finish**  
Restart any other components that have stale configurations as a result of adding services.  


