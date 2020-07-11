# Set the rack id for Ambari

**About this task**  
By setting the Rack ID, you can enable Ambari to manage rack information for hosts, including displaying the hosts in heatmaps by Rack ID, enabling users to filter and find hosts on the **Hosts** page, using that Rack ID. If HDFS is installed in your cluster, Ambari passes this Rack ID information to HDFS using a topology script. Ambari generates a topology script at `/etc/hadoop/conf/topology.py` and sets the net.topology.script.file.name property in core-site automatically. This topology script reads a mappings file `/etc/hadoop/conf/topology_mappings.data` that Ambari automatically generates. When you make changes to Rack ID assignment using Ambari, this mappings file will be updated when you push out the HDFS configuration. HDFS uses this topology script to obtain Rack information about the DataNode hosts.

You can set the Rack ID using Ambari Web in two ways.

* Make multiple hosts aware of a rack simultaneously, using **Actions**.
* Make each host aware of a rack individually, using **Host Actions**.

**Steps**

1. To set the Rack ID for multiple hosts, from the **Hosts** page, click **Actions**, then click **Selected Hosts**, **Filtered Hosts**, or **All Hosts**. 
2. Expand the menu, and click **Hosts**. 
3. Then, expand the menu and click **Set Rack**. ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_set_rack.png) 
4. Alteratively, for a specific host, from the **Hosts** page, click **Host Actions &gt; Set Rack.** 
5. In **Set Rack**, type the rack name, then click **OK**.

**After you finish**  
Browse to **Ambari Web &gt; Dashboard**, and restart any services that display the **Restart Required** icon.

  


