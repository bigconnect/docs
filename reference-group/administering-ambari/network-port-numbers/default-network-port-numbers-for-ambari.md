# Default network port numbers for Ambari

Reference information related to Ambari port default settings.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Service</th>
      <th style="text-align:left">Servers</th>
      <th style="text-align:left">Default Ports</th>
      <th style="text-align:left">Protocol</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Ambari Server</td>
      <td style="text-align:left">Ambari Server host</td>
      <td style="text-align:left">8080</td>
      <td style="text-align:left">http</td>
      <td style="text-align:left">Interface to Ambari Web and Ambari REST API</td>
    </tr>
    <tr>
      <td style="text-align:left">Ambari Server</td>
      <td style="text-align:left">Ambari Server host</td>
      <td style="text-align:left">8440</td>
      <td style="text-align:left">https</td>
      <td style="text-align:left">Handshake Port for Ambari Agents to Ambari Server</td>
    </tr>
    <tr>
      <td style="text-align:left">Ambari Server</td>
      <td style="text-align:left">Ambari Server host</td>
      <td style="text-align:left">8441</td>
      <td style="text-align:left">https</td>
      <td style="text-align:left">Registration and Heartbeat Port for Ambari Agents to Ambari Server</td>
    </tr>
    <tr>
      <td style="text-align:left">Ambari Agent</td>
      <td style="text-align:left">All hosts running Ambari Agents</td>
      <td style="text-align:left">
        <p>8670</p>
        <p>You can change the Ambari Agent ping port in the Ambari Agent configuration.</p>
      </td>
      <td style="text-align:left">tcp</td>
      <td style="text-align:left">Ping port used for alerts to check the health of the Ambari Agent</td>
    </tr>
  </tbody>
</table>

