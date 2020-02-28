# HDFS alerts

Descriptions, potential causes and possible remedies for alerts triggered by HDFS.

**Table 1 - HDFS Service Alerts**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Alert</th>
      <th style="text-align:left">Alert Type</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Potential Causes</th>
      <th style="text-align:left">Possible Remedies</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">NameNode Blocks Health</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This service-level alert is triggered if the number of corrupt or missing
        blocks exceeds the configured critical threshold.</td>
      <td style="text-align:left">
        <p>Some DataNodes are down and the replicas that are missing blocks are only
          on those DataNodes.</p>
        <p></p>
        <p>The corrupt or missing blocks are from files with a replication factor
          of 1. New replicas cannot be created because the only replica of the block
          is missing.</p>
      </td>
      <td style="text-align:left">
        <p>For critical data, use a replication factor of 3.</p>
        <p></p>
        <p>Bring up the failed DataNodes with missing or corrupt blocks.</p>
        <p></p>
        <p>Identify the files associated with the missing or corrupt blocks by running
          the Hadoop fsck command.</p>
        <p></p>
        <p>Delete the corrupt files and recover them from backup, if one exists.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">NFS Gateway Process</td>
      <td style="text-align:left">PORT</td>
      <td style="text-align:left">This host-level alert is triggered if the NFS Gateway process cannot be
        confirmed as active.</td>
      <td style="text-align:left">NFS Gateway is down.</td>
      <td style="text-align:left">Check for a non-operating NFS Gateway in Ambari Web.</td>
    </tr>
    <tr>
      <td style="text-align:left">DataNode Storage</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This host-level alert is triggered if storage capacity is full on the
        DataNode (90% critical). It checks the DataNode JMX Servlet for the Capacity
        and Remaining properties.</td>
      <td style="text-align:left">
        <p>Cluster storage is full.</p>
        <p></p>
        <p>If cluster storage is not full, DataNode is full.</p>
      </td>
      <td style="text-align:left">
        <p>If the cluster still has storage, use the load balancer to distribute
          the data to relatively less-used DataNodes.</p>
        <p></p>
        <p>If the cluster is full, delete unnecessary data or add additional storage
          by adding either more DataNodes or more or larger disks to the DataNodes.</p>
        <p></p>
        <p>After adding more storage, run the load balancer.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">DataNode Process</td>
      <td style="text-align:left">PORT</td>
      <td style="text-align:left">This host-level alert is triggered if the individual DataNode processes
        cannot be established to be up and listening on the network for the configured
        critical threshold, in seconds.</td>
      <td style="text-align:left">
        <p>DataNode process is down or not responding.</p>
        <p></p>
        <p>DataNode are not down but is not listening to the correct network port/address.</p>
      </td>
      <td style="text-align:left">
        <p>Check for non-operating DataNodes in Ambari Web.</p>
        <p></p>
        <p>Check for any errors in the DataNode logs <code>/var/log/hadoop/hdfs</code> and
          restart the DataNode, if necessary.</p>
        <p></p>
        <p>Run the netstat-tuplpn command to check if the DataNode process is bound
          to the correct network port.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">DataNode Web UI</td>
      <td style="text-align:left">WEB</td>
      <td style="text-align:left">This host-level alert is triggered if the DataNode web UI is unreachable.</td>
      <td
      style="text-align:left">The DataNode process is not running.</td>
        <td style="text-align:left">Check whether the DataNode process is running.</td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Host CPU Utilization</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This host-level alert is triggered if CPU utilization of the NameNode
        exceeds certain thresholds (200% warning, 250% critical). It checks the
        NameNode JMX Servlet for the SystemCPULoad property. This information is
        available only if you are running JDK 1.7.</td>
      <td style="text-align:left">Unusually high CPU utilization might be caused by a very unusual job or
        query workload, but this is generally the sign of an issue in the daemon.</td>
      <td
      style="text-align:left">
        <p>Use the top command to determine which processes are consuming excess
          CPU.</p>
        <p></p>
        <p>Reset the offending process.</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Web UI</td>
      <td style="text-align:left">WEB</td>
      <td style="text-align:left">This host-level alert is triggered if the NameNode web UI is unreachable.</td>
      <td
      style="text-align:left">The NameNode process is not running.</td>
        <td style="text-align:left">Check whether the NameNode process is running.</td>
    </tr>
    <tr>
      <td style="text-align:left">Percent DataNodes with Available Space</td>
      <td style="text-align:left">AGGREGATE</td>
      <td style="text-align:left">This service-level alert is triggered if the storage is full on a certain
        percentage of DataNodes (10% warn, 30% critical).</td>
      <td style="text-align:left">
        <p>Cluster storage is full.</p>
        <p></p>
        <p>If cluster storage is not full, DataNode is full.</p>
      </td>
      <td style="text-align:left">
        <p>If the cluster still has storage, use the load balancer to distribute
          the data to relatively less-used DataNodes.</p>
        <p></p>
        <p>If the cluster is full, delete unnecessary data or increase storage by
          adding either more DataNodes or more or larger disks to the DataNodes.
          After adding more storage, run the load balancer.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Percent DataNodes Available</td>
      <td style="text-align:left">AGGREGATE</td>
      <td style="text-align:left">This alert is triggered if the number of non-operating DataNodes in the
        cluster is greater than the configured critical threshold. This aggregates
        the DataNode process alert.</td>
      <td style="text-align:left">
        <p>DataNodes are down.</p>
        <p></p>
        <p>DataNodes are not down but are not listening to the correct network port/address.</p>
      </td>
      <td style="text-align:left">
        <p>Check for non-operating DataNodes in Ambari Web.</p>
        <p></p>
        <p>Check for any errors in the DataNode logs <code>/var/log/hadoop/hdfs</code> and
          restart the DataNode hosts/processes.</p>
        <p></p>
        <p>Run the netstat-tuplpn command to check if the DataNode process is bound
          to the correct network port.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode RPC Latency</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This host-level alert is triggered if the NameNode operations RPC latency
        exceeds the configured critical threshold. Typically an increase in the
        RPC processing time increases the RPC queue length, causing the average
        queue wait time to increase for NameNode operations.</td>
      <td style="text-align:left">A job or an application is performing too many NameNode operations.</td>
      <td
      style="text-align:left">Review the job or the application for potential bugs causing it to perform
        too many NameNode operations.</td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Last Checkpoint</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This alert will trigger if the last time that the NameNode performed a
        checkpoint was too long ago or if the number of uncommitted transactions
        is beyond a certain threshold.</td>
      <td style="text-align:left">
        <p>Too much time elapsed since last NameNode checkpoint.</p>
        <p></p>
        <p>Uncommitted transactions beyond threshold.</p>
      </td>
      <td style="text-align:left">
        <p>Set NameNode checkpoint.</p>
        <p></p>
        <p>Review threshold for uncommitted transactions.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Secondary NameNode Process</td>
      <td style="text-align:left">WEB</td>
      <td style="text-align:left">If the Secondary NameNode process cannot be confirmed to be up and listening
        on the network. This alert is not applicable when NameNode HA is configured.</td>
      <td
      style="text-align:left">The Secondary NameNode is not running.</td>
        <td style="text-align:left">Check that the Secondary DataNode process is running.</td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Directory Status</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This alert checks if the NameNode NameDirStatus metric reports a failed
        directory.</td>
      <td style="text-align:left">One or more of the directories are reporting as not healthy.</td>
      <td style="text-align:left">Check the NameNode UI for information about unhealthy directories.</td>
    </tr>
    <tr>
      <td style="text-align:left">HDFS Capacity Utilization</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This service-level alert is triggered if the HDFS capacity utilization
        exceeds the configured critical threshold (80% warn, 90% critical). It
        checks the NameNode JMX Servlet for the CapacityUsed and CapacityRemaining
        properties.</td>
      <td style="text-align:left">Cluster storage is full.</td>
      <td style="text-align:left">
        <p>Delete unnecessary data.</p>
        <p></p>
        <p>Archive unused data.</p>
        <p></p>
        <p>Add more DataNodes.</p>
        <p></p>
        <p>Add more or larger disks to the DataNodes.</p>
        <p></p>
        <p>After adding more storage, run the load balancer.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">DataNode Health Summary</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This service-level alert is triggered if there are unhealthy DataNodes.</td>
      <td
      style="text-align:left">A DataNode is in an unhealthy state.</td>
        <td style="text-align:left">Check the NameNode UI for the list of non-operating DataNodes.</td>
    </tr>
    <tr>
      <td style="text-align:left">HDFS Pending Deletion Blocks</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This service-level alert is triggered if the number of blocks pending
        deletion in HDFS exceeds the configured warning and critical thresholds.
        It checks the NameNode JMX Servlet for the PendingDeletionBlock property.</td>
      <td
      style="text-align:left">Large number of blocks are pending deletion.</td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">HDFS Upgrade Finalized State</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This service-level alert is triggered if HDFS is not in the finalized
        state.</td>
      <td style="text-align:left">The HDFS upgrade is not finalized.</td>
      <td style="text-align:left">Finalize any upgrade you have in process.</td>
    </tr>
    <tr>
      <td style="text-align:left">DataNode Unmounted Data Dir</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This host-level alert is triggered if one of the data directories on a
        host was previously on a mount point and became unmounted.</td>
      <td style="text-align:left">If the mount history file does not exist, then report an error if a host
        has one or more mounted data directories as well as one or more unmounted
        data directories on the root partition. This may indicate that a data directory
        is writing to the root partition, which is undesirable.</td>
      <td style="text-align:left">Check the data directories to confirm they are mounted as expected.</td>
    </tr>
    <tr>
      <td style="text-align:left">DataNode Heap Usage</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This host-level alert is triggered if heap usage goes past thresholds
        on the DataNode. It checks the DataNode JMXServlet for the MemHeapUsedM
        and MemHeapMaxM properties. The threshold values are percentages.</td>
      <td
      style="text-align:left"></td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Client RPC Queue Latency</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This service-level alert is triggered if the deviation of RPC queue latency
        on client port has grown beyond the specified threshold within an given
        period. This alert will monitor Hourly and Daily periods.</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Client RPC Processing Latency</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This service-level alert is triggered if the deviation of RPC latency
        on client port has grown beyond the specified threshold within a given
        period. This alert will monitor Hourly and Daily periods.</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Service RPC Queue Latency</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This service-level alert is triggered if the deviation of RPC latency
        on the DataNode port has grown beyond the specified threshold within a
        given period. This alert will monitor Hourly and Daily periods.</td>
      <td
      style="text-align:left"></td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Service RPC Processing Latency</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This service-level alert is triggered if the deviation of RPC latency
        on the DataNode port has grown beyond the specified threshold within a
        given period. This alert will monitor Hourly and Daily periods.</td>
      <td
      style="text-align:left"></td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">HDFS Storage Capacity Usage</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This service-level alert is triggered if the increase in storage capacity
        usage deviation has grown beyond the specified threshold within a given
        period. This alert will monitor Daily and Weekly periods.</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode Heap Usage</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This service-level alert is triggered if the NameNode heap usage deviation
        has grown beyond the specified threshold within a given period. This alert
        will monitor Daily and Weekly periods.</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>