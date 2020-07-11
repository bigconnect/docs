# Agent Performance

For clusters larger than 1500 nodes, you should configure Ambari agents to optimize performance by decreasing their reporting frequency.

For clusters larger than 1,500 nodes, you should set two properties for optimal performance:

**command\_update\_output**  
When set to 0, the Ambari Agent only reports output of component tasks, such as install/start, after the task completes.

**send\_alert\_changes\_only**  
When set to 1, the Ambari Agent only sends alert status if the status changes. For example, alert status changes from OK to Warning, or changes from Critical to OK. Side Effect: When set to 1, no live text output of the alert displays in the Ambari Web UI.

On each cluster host, in the agent section of the ambari-agent.ini file:

1.Set `command_update_output=0`

2. Set `send_alert_changes_only=1`

3. Restart the Ambari Agent



