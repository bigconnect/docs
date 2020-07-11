# Configure a public host name

Edit the host name script on each host to use a public host name.

1.Edit the contents of the `/opt/bdl/var/lib/ambari-agent/public_host name.sh` script to return the public host name that you want the Ambari Agent to be configured with. Make sure that you chmod the script so it is executable by the user running the Ambari Agent. For most installations, 0755 is the sufficient permission to use. The following script could be used to configure the Ambari Agent to use `'nn1.bigconnect.local'` as the public host name.

```text
#!/bin/sh
echo ‘nn1.bigconnect.local’
```

2.Configure the Ambari Agent to use this script by editing the `/opt/bdl/etc/ambari-agent/conf/ambari-agent.ini` using a text editor. Add the following property to the \[agent\] section:   
`public_host name_script=/var/lib/ambari-agent/public_host name.sh`   
In this case, Ambari Agent will use the `/opt/bdl/var/lib/ambari-agent/public_host name.sh` script to determine the host name that it will use as the public host name in the Ambari Server.

3.Restart the agent for these changes to take effect

