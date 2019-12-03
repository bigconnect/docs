# Configure a custom host name

Before deploying components to a host, customize the host name, if desired.

Do not customize the host name of an Ambari Agent after components have already been deployed to that host. Customizing the host name for the Ambari Agent will result in a new agent being registered with the custom host name, not the host name of an existing agent being updated.

1.Edit the contents of the `/opt/bdl/var/lib/ambari-agent/host name.sh` script to return the host name that you want the Ambari Agent to register with. Make sure that you chmod the script so it is executable by the user running the Ambari Agent. For most installations, 0755 is the sufficient permission to use. The following script could be used to have the Ambari Agent register with the `'c1r1.bigconnect.local'` host name.

```text
#!/bin/sh
echo ‘c1r1.bigconnect.local’
```

2.Configure the Ambari Agent to use this script, by editing the `/opt/bdl/etc/ambari-agent/conf/ambari-agent.ini` using a text editor.  
Add the following property to the \[agent\] section:  
`host name_script=/var/lib/ambari-agent/host name.sh`  
In this case, Ambari Agent will use the `/opt/bdl/var/lib/ambari-agent/host name.sh` script to determine the host name that it will use to register with the Ambari Server.

3.Restart the agent to ensure that it registers with the custom host name.

