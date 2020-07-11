# Decommission a host

Decommissioning is a process that supports removing components and their hosts from the cluster.

You must decommission a master or slave running on a host before removing it or its host from service. Decommissioning helps you to prevent potential loss of data or disruption of service. Decommissioning is available for the following component types:

* DataNodes
* NodeManagers
* RegionServers

Decommissioning executes the following tasks:  
  
**For DataNodes**  
Safely replicates the HDFS data to other DataNodes in the cluster  
  
**For NodeManagers**  
Stops accepting new job requests from the masters and stops the component  
  
**For RegionServers**  
Turns on drain mode and stops the component  
  
**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)

