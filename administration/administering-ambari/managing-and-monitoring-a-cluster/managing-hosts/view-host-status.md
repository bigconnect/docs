# View host status

You can view the individual hosts in your cluster on **Ambari Web &gt; Hosts.**

**About this task  
Hosts** lists cluster hosts by fully qualified domain name \(FDQN\) and accompanied by a colored icon that indicates the host's operating status.  
  
Observe the icon beside each listed host name.

**Example  
  
Red Triangle**   
At least one master component on that host is down. You can hover your cursor over the host name to see a tooltip that lists affected components.  
  
**Orange**   
At least one slave component on that host is down. Hover to see a tooltip that lists affected components.

**Yellow**  
Ambari Server has not received a heartbeat from that host for more than 3 minutes.  
  
**Green**  
Normal running state.  
  
**Maintenance Mode**  
Black, medical-bag icon indicates a host in maintenance mode.  
  
**Alert**  
Red square with white number indicates the number of alerts generated on a host.  
  
**Example**  
A red icon overrides an orange icon, which overrides a yellow icon. In other words, a host that has a master component down is accompanied by a red icon, even though it might have slave component or connection issues as well. Hosts in maintenance mode or are experiencing alerts, are accompanied by an icon to the right of the host name.

The following example **Hosts** page shows three hosts, one having a master component down, one having a slave component down, one running normally, and two with alerts:

![](../../../../.gitbook/assets/image%20%2858%29.png)

**Parent topic:** [Managing Hosts](https://app.gitbook.com/@bigconnect-1/s/bigconnect-data-lake/~/drafts/-LvBHp9a6uIdEM8gIjtB/administration/administering-ambari/managing-and-monitoring-a-cluster/managing-hosts)

