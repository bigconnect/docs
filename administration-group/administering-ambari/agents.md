# Agents

Install Ambari agents manually if you do not have SSH for Ambari to install them.

In cases where you do not have SSH for Ambari to automatically configure the agents, you can configure an Ambari agent manually. Perform the actions below on each host in the cluster.

1.Using a text editor, configure the Ambari Agent by editing the ambari-agent.ini file. as shown in the following:

```text
$ vi /etc/ambari-agent/conf/ambari-agent.ini
[server]
hostname=<your.ambari.server.hostname>
url_port=8440
secured_url_port=8441
```

2. Start the agent on every host in your cluster.

```text
$ ambari-agent start
```

The agent registers itself with the Ambari Server on start.

