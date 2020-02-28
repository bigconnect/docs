# Find a host in the cluster

Use a filtered search to find only those hosts in the cluster that match specific criteria.

**About this task**  
The Hosts page lists all hosts in the cluster. You can search the full list of hosts, filtering your search by host name, component attribute, and component operating status. You can also search by keyword, simply by typing a word in the search box. The filtered search tool appears left of the Actions menu on the Hosts page.

**Steps**

1. On **Ambari Web &gt; Hosts**, click the search icon. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_search_icon.png) A filtered search field appears above the listed hosts. 
2. Click the filtered search field. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_filtered_search_field.png) Available search types appear, including:  **Search by Host Attribute** Search by host name, IP, host status, and other attributes, including: ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_search_by_host_attribute.png)  **Search by Service** Find hosts that are hosting a component from a given service.  **Search by Component** Find hosts that are hosting a components in a given state, such as started, stopped, maintenance mode, and so on.  **Search by keyword** Type any word that describes what you are looking for in the search box. This becomes a text filter. 
3. Click a Search type. A list of available options appears, depending on your first selection. For example, if you click Service, current services appear: ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_search_by_service.png) 
4. **Click one or multiple options.** In this example, Service: YARN, then Host Status: Master Down.The list of hosts that match your current search criteria display on the Hosts page. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_find_hosts_with_master_down.png) 
5. You can click more options or type keywords to further refine your search.

**Example**  
Examples of searches that you can perform, based on specific criteria, and which interface controls to use:  
  
**Find all hosts with a DataNode**  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_filter_datanode_all.png)  
  
**Find all the hosts with a DataNode that are stopped**  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_filter_datanode_stopped.png)  
  
**Find all the hosts with an HDFS component**  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_filter_service_hdfs.png)  
  
**Find all the hosts with an HDFS or HBase component**  
![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_filter_services_hdfs_hbase.png)  
  
**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)  


  


