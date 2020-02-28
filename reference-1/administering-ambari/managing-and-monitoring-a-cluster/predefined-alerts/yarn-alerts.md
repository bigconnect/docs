# YARN alerts

Descriptions, potential causes and possible remedies for alerts triggered by YARN.

**Table 1 - YARN Alerts**

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
      <td style="text-align:left">App Timeline Web UI</td>
      <td style="text-align:left">WEB</td>
      <td style="text-align:left">This host-level alert is triggered if the App Timeline Server Web UI is
        unreachable.</td>
      <td style="text-align:left">
        <p>The App Timeline Server is down.</p>
        <p></p>
        <p>App Timeline Service is not down but is not listening to the correct network
          port/address.</p>
      </td>
      <td style="text-align:left">Check for non-operating App Timeline Server in Ambari Web.</td>
    </tr>
    <tr>
      <td style="text-align:left">Percent NodeManagers Available</td>
      <td style="text-align:left">AGGREGATE</td>
      <td style="text-align:left">This alert is triggered if the number of down NodeManagers in the cluster
        is greater than the configured critical threshold. It aggregates the results
        of DataNode process alert checks.</td>
      <td style="text-align:left">
        <p>NodeManagers are down.</p>
        <p></p>
        <p>NodeManagers are not down but are not listening to the correct network
          port/address.</p>
      </td>
      <td style="text-align:left">
        <p>Check for non-operating NodeManagers.</p>
        <p></p>
        <p>Check for any errors in the NodeManager logs <code>/var/log/hadoop/yarn</code>and
          restart the NodeManagers hosts/processes, as necessary.</p>
        <p></p>
        <p>Run the netstat-tuplpn command to check if the NodeManager process is
          bound to the correct network port.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ResourceManager Web UI</td>
      <td style="text-align:left">WEB</td>
      <td style="text-align:left">This host-level alert is triggered if the ResourceManager Web UI is unreachable.</td>
      <td
      style="text-align:left">The ResourceManager process is not running.</td>
        <td style="text-align:left">Check if the ResourceManager process is running.</td>
    </tr>
    <tr>
      <td style="text-align:left">ResourceManager RPC Latency</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This host-level alert is triggered if the ResourceManager operations RPC
        latency exceeds the configured critical threshold. Typically an increase
        in the RPC processing time increases the RPC queue length, causing the
        average queue wait time to increase for ResourceManager operations.</td>
      <td
      style="text-align:left">A job or an application is performing too many ResourceManager operations.</td>
        <td
        style="text-align:left">Review the job or the application for potential bugs causing it to perform
          too many ResourceManager operations.</td>
    </tr>
    <tr>
      <td style="text-align:left">ResourceManager CPU Utilization</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This host-level alert is triggered if CPU utilization of the ResourceManager
        exceeds certain thresholds (200% warning, 250% critical). It checks the
        ResourceManager JMX Servlet for the SystemCPULoad property. This information
        is only available if you are running JDK 1.7.</td>
      <td style="text-align:left">Unusually high CPU utilization: Can be caused by a very unusual job/query
        workload, but this is generally the sign of an issue in the daemon.</td>
      <td
      style="text-align:left">
        <p>Use the top command to determine which processes are consuming excess
          CPU.</p>
        <p></p>
        <p>Reset the offending process.</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">NodeManager Web UI</td>
      <td style="text-align:left">WEB</td>
      <td style="text-align:left">This host-level alert is triggered if the NodeManager process cannot be
        established to be up and listening on the network for the configured critical
        threshold, given in seconds.</td>
      <td style="text-align:left">
        <p>NodeManager process is down or not responding.</p>
        <p></p>
        <p>NodeManager is not down but is not listening to the correct network port/address.</p>
      </td>
      <td style="text-align:left">
        <p>Check if the NodeManager is running.</p>
        <p></p>
        <p>Check for any errors in the NodeManager logs <code>/var/log/hadoop/yarn</code>and
          restart the NodeManager, if necessary.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">NodeManager Health Summary</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This host-level alert checks the node health property available from the
        NodeManager component.</td>
      <td style="text-align:left">NodeManager Health Check script reports issues or is not configured.</td>
      <td
      style="text-align:left">
        <p>Check in the NodeManager logs <code>/var/log/hadoop/yarn</code> for health
          check errors and restart the NodeManager, and restart if necessary.</p>
        <p></p>
        <p>Check in the ResourceManager UI logs <code>/var/log/hadoop/yarn</code> for
          health check errors.</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">NodeManager Health</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This host-level alert checks the nodeHealthy property available from the
        NodeManager component.</td>
      <td style="text-align:left">The NodeManager process is down or not responding.</td>
      <td style="text-align:left">Check in the NodeManager logs <code>/var/log/hadoop/yarn</code> for health
        check errors and restart the NodeManager, and restart if necessary.</td>
    </tr>
  </tbody>
</table>