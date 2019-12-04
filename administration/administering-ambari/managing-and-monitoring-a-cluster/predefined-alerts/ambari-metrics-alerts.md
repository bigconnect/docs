# Ambari metrics alerts

Descriptions, potential causes and possible remedies for alerts triggered by Ambari metrics.  
  
**Table 1 - Ambari metrics alerts**

| Alert | Description | Potential Causes | Possible Remedies |
| :--- | :--- | :--- | :--- |
| Metrics Collector Process | This alert is triggered if the Metrics Collector cannot be confirmed to be up and listening on the configured port for number of seconds equal to threshold. | The Metrics Collector process is not running. | Check the Metrics Collector is running. |
| Metrics Collector – ZooKeeper Server Process | This host-level alert is triggered if the Metrics Collector ZooKeeper Server Process cannot be determined to be up and listening on the network. | The Metrics Collector process is not running. | Check the Metrics Collector is running. |
| Metrics Collector – HBase Master Process | This alert is triggered if the Metrics Collector HBase Master Processes cannot be confirmed to be up and listening on the network for the configured critical threshold, given in seconds. | The Metrics Collector process is not running. | Check the Metrics Collector is running. |
| Metrics Collector – HBase Master CPU Utilization | This host-level alert is triggered if CPU utilization of the Metrics Collector exceeds certain thresholds. | Unusually high CPU utilization generally the sign of an issue in the daemon configuration. | Tune the Ambari Metrics Collector. |
| Metrics Monitor Status | This host-level alert is triggered if the Metrics Monitor process cannot be confirmed to be up and running on the network. | The Metrics Monitor is down. | Check whether the Metrics Monitor is running on the given host. |
| Percent Metrics Monitors Available | This is an AGGREGATE alert of the Metrics Monitor Status. | Metrics Monitors are down. | Check the Metrics Monitors are running. |
| Metrics Collector -Auto-Restart Status | This alert is triggered if the Metrics Collector has been auto-started for number of times equal to start threshold in a 1 hour timeframe. By default if restarted 2 times in an hour, you will receive a Warning alert. If restarted 4 or more times in an hour, you will receive a Critical alert. | The Metrics Collector is running but is unstable and causing restarts. This could be due to improper tuning. | Tune the Ambari Metrics Collector. |
| Percent Metrics Monitors Available | This is an AGGREGATE alert of the Metrics Monitor Status. | Metrics Monitors are down. | Check the Metrics Monitors. |
| Grafana Web UI | This host-level alert is triggered if the AMS Grafana Web UI is unreachable. | Grafana process is not running. | Check whether the Grafana process is running. Restart if it has gone down. |

