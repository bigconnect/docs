# Open the cluster dashboard

Use the Ambari Web Dashboard to monitor operating status of your cluster.

Ambari Web displays the **Dashboard** page as the home page.

**Dashboard** includes three options:

* **Metrics**
* **Heatmaps**
* **Config History**

The **Metrics** option displays by default. On the **Metrics** page, multiple widgets represent operating status information of services in your cluster. Most widgets display a single metric; for example, **HDFS Disk Usage** represented by a load chart and a percentage figure:

![Ambari Metrics page](../../../../.gitbook/assets/image%20%28331%29.png)

Example Metrics Widgets and Descriptions:

### HDFS metrics

**HDFS Disk Usage**   
The percentage of distributed file system \(DFS\) used, which is a combination of DFS and non-DFS used  
  
**Data Nodes Live**  
The number of DataNodes operating, as reported from the NameNode  
  
**NameNode Heap**  
The percentage of NameNode Java Virtual Machine \(JVM\) heap memory used  
  
**NameNode RPC**  
The average RPC queue latency   
  
**NameNode CPU WIO**   
The percentage of CPU wait I/O   
  
**NameNode Uptime**   
The NameNode uptime calculation

### YARN Metrics

**ResourceManager Heap**  
The percentage of ResourceManager JVM heap memory used  
  
**ResourceManager Uptime**  
The ResourceManager uptime calculation  
  
**NodeManagers Live**   
The number of DataNodes operating, as reported from the ResourceManager  
  
**YARN Memory**  
The percentage of available YARN memory \(used versus. total available\)



