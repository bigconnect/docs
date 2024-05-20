# Configuring Partition Backups

By default, Ignite keeps a single copy of each partition (a single copy of the entire data set). In this case, if one or multiple nodes become unavailable, you lose access to partitions stored on these nodes. To avoid this, you can configure Ignite to maintain backup copies of each partition.

Backup copies are configured per cache (table). If you configure 2 backup copies, the cluster maintains 3 copies of each partition. One of the partitions is called the _primary_ partition, and the other two are called _backup_ partitions. By extension, the node that has the primary partition is called the _primary node for the keys stored in the partition_. The node with backup partitions is called the _backup node_.

When a node with the primary partition for some key leaves the cluster, Ignite triggers the partition map exchange (PME) process. PME labels one of the backup partitions (if they are configured) for the key as primary.

Backup partitions increase the availability of your data, and in some cases, the speed of read operations, since Ignite reads data from backed-up partitions if they are available on the local node (this is the default behavior that can be disabled. See Cache Configuration for details.). However, they also increase memory consumption or the size of the persistent storage (if enabled).

### Configuring Backups

To configure the number of backup copies, set the `backups` property in the cache configuration.

```xml
<bean class="org.apache.ignite.configuration.IgniteConfiguration">
    <property name="cacheConfiguration">
        <bean class="org.apache.ignite.configuration.CacheConfiguration">
            <!-- Set the cache name. -->
            <property name="name" value="cacheName"/>
            <!-- Set the cache mode. -->
            <property name="cacheMode" value="PARTITIONED"/>
            <!-- Number of backup copies -->
            <property name="backups" value="1"/>
        </bean>
    </property>
</bean>
```

### Synchronous and Asynchronous Backups

You can configure whether updates of primary and backup copies should be synchronous or asynchronous by specifying a write synchronization mode. You can set the write synchronization mode in the cache configuration:

```xml
<bean class="org.apache.ignite.configuration.IgniteConfiguration">
    <property name="cacheConfiguration">
        <bean class="org.apache.ignite.configuration.CacheConfiguration">
            <!-- Set the cache name. -->
            <property name="name" value="cacheName"/>
            <!-- Number of backup copies -->
            <property name="backups" value="1"/>

            <property name="writeSynchronizationMode" value="FULL_SYNC"/>
        </bean>
    </property>
</bean>
```

The write synchronization mode can be set to the following values:

| Value         | Description                                                                                                                                                                                                                         |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FULL\_SYNC    | Client node will wait for write or commit to complete on all participating remote nodes (primary and backup).                                                                                                                       |
| FULL\_ASYNC   | Client node does not wait for responses from participating nodes, in which case remote nodes may get their state updated slightly after any of the cache write methods complete or after the Transaction.commit() method completes. |
| PRIMARY\_SYNC | This is the default mode. Client node will wait for write or commit to complete on primary node, but will not wait for backups to be updated.                                                                                       |
