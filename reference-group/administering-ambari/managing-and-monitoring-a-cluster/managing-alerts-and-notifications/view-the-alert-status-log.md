# View the alert status log

Using a command line editor, view the alert status log on the Ambari server host.

**About this task**  
Whether you have configured Ambari to send alert notifications or not, it writes alert status changes to a log on the Ambari Server host. You can view the alert status log using a command line editor.

**Steps**

1. On the Ambari server host, browse to the log directory.`cd /var/log/ambari-server/`
2. View the ambari-alerts.log file.Log entries include the time of the status change, the alert status, the alert definition name, and the response text.

```text
2015-08-10 22:47:37,120 [OK] [HARD] [STORM] (Storm Server Process) TCP OK - 0.000s response on port 8744
2015-08-11 11:06:18,479 [CRITICAL] [HARD] [AMBARI] [ambari_server_agent_heartbeat] (Ambari Agent Heartbeat) c6401.ambari.apache.org is not sending heartbeats
2015-08-11 11:08:18,481 [OK] [HARD] [AMBARI] [ambari_server_agent_heartbeat] (Ambari Agent Heartbeat) c6401.ambari.apache.org is healthy 
```

