# Increase Ambari server cache size

Increase the Amabari Server cache size to accommodate a large number of hosts.

1.Determine the cache value using the formula:

`ecCacheSizeValue=60*[CLUSTER_SIZE]`

where `[CLUSTER_SIZE]` is the number of nodes in the cluster.

2. On the Ambari Server host, in `/opt/bdl/etc/ambari-server/conf/ambari-properties`, add the following property and value:

`server.ecCacheSize=[EC_CACHE_SIZE_VALUE]`

where `[EC_CACHE_SIZE_VALUE]` is the value calculated in the previous step, based on the number of nodes in the cluster.





