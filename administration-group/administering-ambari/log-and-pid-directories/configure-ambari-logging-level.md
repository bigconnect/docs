# Configure Ambari logging level

You can set the level at which Amabari Server and Ambari Agents create log files that record cluster operations activity.

1. Using a command line editor on the Ambari Server host, browse to `/opt/bdl/etc/ambari-server/conf/log4j.properties`
2. Modify the logging level for Ambari Server by editing `ambari-server.log`
3. Using a command line editor on the Ambari Agent host, browse to `/opt/bdl/etc/ambari-agent/conf/ambari-agent.ini`
4. Modify the logging level for each Agent by editing loglevel

   You can also modify these locations. Generally, you should also consider setting log-rotate policies for your systems. Refer to your operating system documentation for more information on setting up [log-rotate](https://linuxconfig.org/logrotate-8-manual-page) in your environment.



