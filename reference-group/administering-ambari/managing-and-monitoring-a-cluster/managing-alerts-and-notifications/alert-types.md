# Alert types

Describes seven types of Ambari alerts: web, port, metric, aggregate, script, server, and recovery.

Alert thresholds and the threshold units depend on the type of the alert. The following list describes alert types, possible status for each type, and to what units thresholds can be configured if the thresholds are configurable.  
  
**WEB Alert Type**  
WEB alerts watch a web URL on a given component; the alert status is determined based on the HTTP response code. Therefore, you cannot change which HTTP response codes determine the thresholds for WEB alerts. You can customize the response text for each threshold and the overall web connection timeout. A connection timeout is considered a CRITICAL alert. Threshold units are based on seconds. The status values and response codes for WEB alerts are:

* OK status if the web URL responds with a code under 400.
* WARNING status if the web URL responds with code 400 and above.
* CRITICAL status if Ambari cannot connect to the web URL.

**PORT Alert Type**  
PORT alerts check the response time to connect to a given a port; the threshold units are based on seconds.  
  
**METRIC Alert Type**  
METRIC alerts check the value of a single or multiple metrics, if a calculation is performed. The metric is accessed from a URL endpoint available on a given component. A connection timeout is considered a CRITICAL alert. The thresholds are adjustable and the units for each threshold depend on the metric. For example, in the case of CPU utilization alerts, the unit is percentage; in the case of RPC latency alerts, the unit is milliseconds.  
  
**AGGREGATE Alert Type**  
AGGREGATE alerts aggregate the alert status as a percentage of the alert instances affected. For example, the Percent DataNode Process alert aggregates the DataNode Process alert.  
  
**SCRIPT Alert Type**  
SCRIPT alerts execute a script that determines status such as OK, WARNING, or CRITICAL. You can customize the response text and values for the properties and thresholds for the SCRIPT alert.  
  
**SERVER Alert Type**  
SERVER alerts execute a server-side runnable class that determines the alert status such as OK, WARNING, or CRITICAL.  
  
**RECOVERY Alert Type**  
RECOVERY alerts are handled by the Ambari Agents that are monitoring for process restarts. Alert status OK, WARNING, and CRITICAL are based on the number of times a process is restarted automatically. This is useful to know when processes are terminating and Ambari is automatically restarting.

