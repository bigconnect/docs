# ZooKeeper alerts

Descriptions, potential causes and possible remedies for alerts triggered by ZooKeeper.

**Table 1 - ZooKeeper alerts**

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
      <td style="text-align:left">Percent ZooKeeper Servers Available</td>
      <td style="text-align:left">AGGREGATE</td>
      <td style="text-align:left">This service-level alert is triggered if the configured percentage of
        ZooKeeper processes cannot be determined to be up and listening on the
        network for the configured critical threshold, given in seconds. It aggregates
        the results of ZooKeeper process checks.</td>
      <td style="text-align:left">The majority of your ZooKeeper servers are down and not responding.</td>
      <td
      style="text-align:left">
        <p>Check the dependent services to make sure they are operating correctly.</p>
        <p></p>
        <p>Check the ZooKeeper logs <code>/var/log/hadoop/zookeeper.log</code> for
          further information.</p>
        <p></p>
        <p>If the failure was associated with a particular workload, try to understand
          the workload better.</p>
        <p></p>
        <p>Restart the ZooKeeper servers from the Ambari UI.</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">ZooKeeper Server Process</td>
      <td style="text-align:left">PORT</td>
      <td style="text-align:left">This host-level alert is triggered if the ZooKeeper server process cannot
        be determined to be up and listening on the network for the configured
        critical threshold, given in seconds.</td>
      <td style="text-align:left">
        <p>The ZooKeeper server process is down on the host.</p>
        <p></p>
        <p>The ZooKeeper server process is up and running but not listening on the
          correct network port (default 2181).</p>
      </td>
      <td style="text-align:left">
        <p>Check for any errors in the ZooKeeper logs <code>/var/log/hbase/</code> and
          restart the ZooKeeper process using Ambari Web.</p>
        <p></p>
        <p>Run the netstat-tuplpn command to check if the ZooKeeper server process
          is bound to the correct network port.</p>
      </td>
    </tr>
  </tbody>
</table>

