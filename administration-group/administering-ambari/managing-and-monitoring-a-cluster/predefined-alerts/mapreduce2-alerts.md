# MapReduce2 alerts

Descriptions, potential causes and possible remedies for alerts triggered by MapReduce2.

**Table 1 - MapReduce2 Alerts**

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
      <td style="text-align:left">History Server Web UI</td>
      <td style="text-align:left">WEB</td>
      <td style="text-align:left">This host-level alert is triggered if the HistoryServer Web UI is unreachable.</td>
      <td
      style="text-align:left">The HistoryServer process is not running.</td>
        <td style="text-align:left">Check if the HistoryServer process is running.</td>
    </tr>
    <tr>
      <td style="text-align:left">History Server RPC latency</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This host-level alert is triggered if the HistoryServer operations RPC
        latency exceeds the configured critical threshold. Typically an increase
        in the RPC processing time increases the RPC queue length, causing the
        average queue wait time to increase for NameNode operations.</td>
      <td style="text-align:left">A job or an application is performing too many HistoryServer operations.</td>
      <td
      style="text-align:left">Review the job or the application for potential bugs causing it to perform
        too many HistoryServer operations.</td>
    </tr>
    <tr>
      <td style="text-align:left">History Server CPU Utilization</td>
      <td style="text-align:left">METRIC</td>
      <td style="text-align:left">This host-level alert is triggered if the percent of CPU utilization on
        the HistoryServer exceeds the configured critical threshold.</td>
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
      <td style="text-align:left">History Server Process</td>
      <td style="text-align:left">PORT</td>
      <td style="text-align:left">This host-level alert is triggered if the HistoryServer process cannot
        be established to be up and listening on the network for the configured
        critical threshold, given in seconds.</td>
      <td style="text-align:left">
        <p>HistoryServer process is down or not responding.</p>
        <p>HistoryServer is not down but is not listening to the correct network
          port/address.</p>
      </td>
      <td style="text-align:left">
        <p>Check the HistoryServer is running.</p>
        <p></p>
        <p>Check for any errors in the HistoryServer logs <code>/var/log/hadoop/mapred</code>and
          restart the HistoryServer, if necessary.</p>
      </td>
    </tr>
  </tbody>
</table>