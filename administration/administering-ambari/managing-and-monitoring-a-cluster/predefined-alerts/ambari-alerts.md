# Ambari alerts

Descriptions, potential causes and possible remedies for alerts triggered by Ambari.

**Table 1 - Ambari alerts**

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
      <td style="text-align:left">Host Disk Usage</td>
      <td style="text-align:left">SCRIPT</td>
      <td style="text-align:left">This host-level alert is triggered if the amount of disk space used on
        a host goes above specific thresholds (50% warn, 80% crit ).</td>
      <td style="text-align:left">The amount of free disk space left is low.</td>
      <td style="text-align:left">Check host for disk space to free or add more storage.</td>
    </tr>
    <tr>
      <td style="text-align:left">Ambari Agent Heartbeat</td>
      <td style="text-align:left">SERVER</td>
      <td style="text-align:left">This alert is triggered if the server has lost contact with an agent.</td>
      <td
      style="text-align:left">
        <p>Ambari Server host is unreachable from Agent host.</p>
        <p></p>
        <p>Ambari Agent is not running.</p>
        </td>
        <td style="text-align:left">
          <p>Check connection from Agent host to Ambari Server.</p>
          <p></p>
          <p>Check Agent is running.</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Ambari Server Alerts</td>
      <td style="text-align:left">SERVER</td>
      <td style="text-align:left">This alert is triggered if the server detects that there are alerts which
        have not run in a timely manner.</td>
      <td style="text-align:left">
        <p>Agents are not reporting alert status.</p>
        <p></p>
        <p>Agents are not running.</p>
      </td>
      <td style="text-align:left">Check that all Agents are running and heartbeating.</td>
    </tr>
    <tr>
      <td style="text-align:left">Ambari Server Performance</td>
      <td style="text-align:left">SERVER</td>
      <td style="text-align:left">This alert is triggered if the Ambari Server detects that there is a potential
        performance problem with Ambari.</td>
      <td style="text-align:left">This type of issue can arise for many reasons, but is typically attributed
        to slow database queries and host resource exhaustion.</td>
      <td style="text-align:left">Check your Ambari Server database connection and database activity. Check
        your Ambari Server host for resource exhaustion such as memory.</td>
    </tr>
  </tbody>
</table>