# Adjust JDBC pool settings

Add the following properties to adjust the jdbc connection pool settings:

On the Ambari Server host, in `/opt/bdl/etc/ambari-server/conf/ambari-properties`, add the following properties and values:

`server.jdbc.connection-pool.acquisition-size = 5  
server.jdbc.connection-pool.max-age = 0  
server.jdbc.connection-pool.max-idle-time = 14400  
server.jdbc.connection-pool.max-idle-time-excess = 0  
server.jdbc.connection-pool.idle-test-interval = 7200`





