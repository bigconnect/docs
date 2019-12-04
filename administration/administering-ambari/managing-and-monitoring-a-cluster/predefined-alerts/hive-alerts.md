# Hive alerts

Descriptions, potential causes and possible remedies for alerts triggered by Hive.

**Table 1 - Hive Alerts**

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
      <td style="text-align:left">HiveServer2 Process</td>
      <td style="text-align:left">This host-level alert is triggered if the HiveServer cannot be determined
        to be up and responding to client requests.</td>
      <td style="text-align:left">
        <p>HiveServer2 process is not running.</p>
        <p></p>
        <p>HiveServer2 process is not responding.</p>
      </td>
      <td style="text-align:left">Using Ambari Web, check status of HiveServer2 component. Stop and then
        restart.</td>
    </tr>
    <tr>
      <td style="text-align:left">Hive Metastore Process</td>
      <td style="text-align:left">This host-level alert is triggered if the Hive Metastore process cannot
        be determined to be up and listening on the network for the configured
        critical threshold, given in seconds.</td>
      <td style="text-align:left">
        <p>The Hive Metastore service is down.</p>
        <p></p>
        <p>The database used by the Hive Metastore is down.</p>
        <p></p>
        <p>The Hive Metastore host is not reachable over the network.</p>
      </td>
      <td style="text-align:left">Using Ambari Web, stop the Hive service and then restart it.</td>
    </tr>
    <tr>
      <td style="text-align:left">WebHCat Server Status</td>
      <td style="text-align:left">This host-level alert is triggered if the WebHCat server cannot be determined
        to be up and responding to client requests.</td>
      <td style="text-align:left">
        <p>The WebHCat server is down.</p>
        <p></p>
        <p>The WebHCat server is hung and not responding.</p>
        <p></p>
        <p>The WebHCat server is not reachable over the network.</p>
      </td>
      <td style="text-align:left">Restart the WebHCat server using Ambari Web.</td>
    </tr>
  </tbody>
</table>