# Change the default Ambari server-agent port

To change the deafault port number, 8187, that Ambari Server uses to access Ambari Agents, edit the Ambari properties file.

By default, Ambari Server uses port 8187 to communicate with Ambari Agents. To change the port number, you must edit the Ambari properties file.

{% hint style="warning" %}
Ambari Server should not be running when you change port numbers. Edit `ambari.properties` before you start Ambari Server the first time or stop Ambari Server before editing properties.
{% endhint %}

1.On the Ambari Server host, open /etc/ambari-server/conf/ambari.properties with a text editor.

2. Add the following properties and set them to your desired port values:

`security.server.two_way_ssl.port=5222  
security.server.one_way_ssl.port=5223`

3. On every Ambari Agent host, open `/opt/bdl/etc/ambari-agent/conf/ambari-agent.ini` with a text editor.

4. Add the following properties and set them to your desired port values:

`url_port=5223  
secured_url_port=5222`

5. Start or re-start the Ambari Server.

Ambari Server now accesses Ambari Agents via the newly configured port.

