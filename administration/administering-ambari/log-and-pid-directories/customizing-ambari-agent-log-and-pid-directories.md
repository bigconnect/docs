# Customizing Ambari agent log and pid directories

To modify the Ambari agent log and pid locations:

1.Stop the Ambari Agent

2. Edit the Ambari agent properties file: `/opt/bdl/etc/ambari-agent/conf/ambari-agent.ini`

3. In the \[AGENT\] section, modify the piddir and logdir properties:

`[AGENT]  
logdir=/opt/bdl/var/log/ambari-agent  
piddir=/opt/bdl/var/run/ambari-agent`

4. Save the ambari-agent.ini file

5. Create the new directories and be sure to set the directory ownership and permissions to allow the Ambari agent process access.

6. Restart the Ambari agent

