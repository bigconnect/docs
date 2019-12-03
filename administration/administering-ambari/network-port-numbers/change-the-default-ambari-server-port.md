# Change the default Ambari server port

To change the deafault port number, 8080, that Ambari Server uses to access Ambari Web, edit the Ambari properties file.

By default, Ambari Server uses port 8080 to access Ambari Web and the REST API. To change the port number, you must edit the Ambari properties file.

{% hint style="warning" %}
Ambari Server should not be running when you change port numbers. Edit `ambari.properties` before you start Ambari Server the first time or stop Ambari Server before editing properties.
{% endhint %}

1. On the Ambari Server host, open `/opt/bdl/etc/ambari-server/conf/ambari.properties` with a text editor.
2. Add the client API port property and set it to your desired port value: `client.api.port=[PORT_NUMBER]`
3. Start or re-start the Ambari Server

Ambari Server is now accessible via the newly configured port.

