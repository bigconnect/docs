# NameNode high availability alerts

Descriptions, potential causes and possible remedies for alerts related to NameNode high availability.

**Table 1 - NameNode HA Alerts**

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
      <td style="text-align:left">JournalNode Process</td>
      <td style="text-align:left">WEB</td>
      <td style="text-align:left">This host-level alert is triggered if the individual JournalNode process
        cannot be established to be up and listening on the network for the configured
        critical threshold, given in seconds.</td>
      <td style="text-align:left">
        <p>The JournalNode process is down or not responding.</p>
        <p></p>
        <p>The JournalNode is not down but is not listening to the correct network
          port/address.</p>
      </td>
      <td style="text-align:left">Check if the JournalNode process is running.</td>
    </tr>
    <tr>
      <td style="text-align:left">NameNode High Availability Health</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This service-level alert is triggered if either the Active NameNode or
        Standby NameNode are not running.</td>
      <td style="text-align:left">The Active, Standby or both NameNode processes are down.</td>
      <td style="text-align:left">
        <p>On each host running NameNode, check for any errors in the logs /var/log/hadoop/hdfs/
          and restart the NameNode host/process using Ambari Web.</p>
        <p></p>
        <p>On each host running NameNode, run the netstat-tuplpn command to check
          if the NameNode process is bound to the correct network port.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Percent JournalNodes Available</td>
      <td style="text-align:left">AGGREGATE</td>
      <td style="text-align:left">This service-level alert is triggered if the number of down JournalNodes
        in the cluster is greater than the configured critical threshold (33% warn,
        50% crit ). It aggregates the results of JournalNode process checks.</td>
      <td
      style="text-align:left">
        <p>JournalNodes are down.</p>
        <p></p>
        <p>JournalNodes are not down but are not listening to the correct network
          port/address.</p>
        </td>
        <td style="text-align:left">Check for non-operating JournalNodes in Ambari Web.</td>
    </tr>
    <tr>
      <td style="text-align:left">ZooKeeper Failover Controller Process</td>
      <td style="text-align:left">PORT</td>
      <td style="text-align:left">This alert is triggered if the ZooKeeper Failover Controller process cannot
        be confirmed to be up and listening on the network.</td>
      <td style="text-align:left">The ZKFC process is down or not responding.</td>
      <td style="text-align:left">Check if the ZKFC process is running.</td>
    </tr>
  </tbody>
</table>