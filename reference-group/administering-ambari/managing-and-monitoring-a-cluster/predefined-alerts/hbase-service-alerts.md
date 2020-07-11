# HBase service alerts

Descriptions, potential causes and possible remedies for alerts triggered by HBase.

**Table 1 - HBase Service Alerts**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Alert</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Potential Causes</th>
      <th style="text-align:left">Possible Remedies</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Percent RegionServers Available</td>
      <td style="text-align:left">This service-level alert is triggered if the configured percentage of
        Region Server processes cannot be determined to be up and listening on
        the network for the configured critical threshold. The default setting
        is 10% to produce a WARN alert and 30% to produce a CRITICAL alert. It
        aggregates the results of RegionServer process down checks.</td>
      <td style="text-align:left">
        <p>Misconfiguration or less-than-ideal configuration caused the RegionServers
          to crash.</p>
        <p></p>
        <p>Cascading failures brought on by some workload caused the RegionServers
          to crash.</p>
        <p></p>
        <p>The RegionServers shut themselves own because there were problems in the
          dependent services, ZooKeeper or HDFS.</p>
        <p></p>
        <p>GC paused the RegionServer for too long and the RegionServers lost contact
          with ZooKeeper.</p>
      </td>
      <td style="text-align:left">
        <p>Check the dependent services to make sure they are operating correctly.</p>
        <p></p>
        <p>Look at the RegionServer log files (usually <code>/var/log/hbase/*.log</code>)
          for further information.</p>
        <p></p>
        <p>If the failure was associated with a particular workload, try to understand
          the workload better.</p>
        <p></p>
        <p>Restart the RegionServers.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">HBase Master Process</td>
      <td style="text-align:left">This alert is triggered if the HBase master processes cannot be confirmed
        to be up and listening on the network for the configured critical threshold,
        given in seconds.</td>
      <td style="text-align:left">
        <p>The HBase master process is down.</p>
        <p></p>
        <p>The HBase master has shut itself down because there were problems in the
          dependent services, ZooKeeper or HDFS.</p>
      </td>
      <td style="text-align:left">
        <p>Check the dependent services.</p>
        <p></p>
        <p>Look at the master log files (usually <code>/var/log/hbase/*.log</code>)
          for further information.</p>
        <p></p>
        <p>Look at the configuration <code>files /etc/hbase/conf</code>
        </p>
        <p></p>
        <p>Restart the master.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">HBase Master CPU Utilization</td>
      <td style="text-align:left">This host-level alert is triggered if CPU utilization of the HBase Master
        exceeds certain thresholds (200% warning, 250% critical). It checks the
        HBase Master JMX Servlet for the SystemCPULoad property. This information
        is only available if you are running JDK 1.7.</td>
      <td style="text-align:left">Unusually high CPU utilization: Can be caused by a very unusual job/query
        workload, but this is generally the sign of an issue in the daemon.</td>
      <td
      style="text-align:left">
        <p>Use the top command to determine which processes are consuming excess
          CPU</p>
        <p></p>
        <p>Reset the offending process.</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">RegionServers Health Summary</td>
      <td style="text-align:left">This service-level alert is triggered if there are unhealthy RegionServers.</td>
      <td
      style="text-align:left">
        <p>The RegionServer process is down on the host.</p>
        <p></p>
        <p>The RegionServer process is up and running but not listening on the correct
          network port (default 60030).</p>
        </td>
        <td style="text-align:left">Check for dead RegionServer in Ambari Web.</td>
    </tr>
    <tr>
      <td style="text-align:left">HBase RegionServer Process</td>
      <td style="text-align:left">This host-level alert is triggered if the RegionServer processes cannot
        be confirmed to be up and listening on the network for the configured critical
        threshold, given in seconds.</td>
      <td style="text-align:left">
        <p>The RegionServer process is down on the host.</p>
        <p></p>
        <p>The RegionServer process is up and running but not listening on the correct
          network port (default 60030).</p>
      </td>
      <td style="text-align:left">
        <p>Check for any errors in the logs <code>/var/log/hbase/</code> and restart
          the RegionServer process using Ambari Web.</p>
        <p></p>
        <p>Run the netstat-tuplpn command to check if the RegionServer process is
          bound to the correct network port.</p>
      </td>
    </tr>
  </tbody>
</table>

