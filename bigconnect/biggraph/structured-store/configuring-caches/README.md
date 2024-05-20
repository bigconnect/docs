# Configuring Caches

This chapter explains how you can set cache configuration parameters. Once a cache is created, you cannot change its configuration parameters.

### Configuration Example

Below is an example of a cache configuration.

```xml
<bean class="org.apache.ignite.configuration.IgniteConfiguration">
    <property name="cacheConfiguration">
        <bean class="org.apache.ignite.configuration.CacheConfiguration">
            <property name="name" value="myCache"/>
            <property name="cacheMode" value="PARTITIONED"/>
            <property name="backups" value="2"/>
            <property name="rebalanceMode" value="SYNC"/>
            <property name="writeSynchronizationMode" value="FULL_SYNC"/>
            <property name="partitionLossPolicy" value="READ_ONLY_SAFE"/>
            <!-- Other parameters -->
        </bean>
    </property>
</bean>
```

### Cache Templates

A cache template is an instance of `CacheConfiguration` that can be registered in the cluster and used later as a basis for creating new caches or SQL tables. A cache or table created from a template inherits all the properties of the template.

A cache template is an instance of `CacheConfiguration` that can be registered in the cluster and used later as a basis for creating new caches or SQL tables. A cache or table created from a template inherits all the properties of the template.

Templates can be used in many contexts: on servers, clients (thin and thick), in SQL (e.g., CREATE TABLE), in commands such as `createCache` and `getOrCreateCache`, as well as in the REST API calls. For example, thin clients and SQL do not support some cache configuration properties, and cache templates can be used to work around this limitation.

To create a template, define a cache configuration and add it to the `Ignite` instance, as shown below. If you want do define a cache template in the XML configuration file, you must add an asterisk to the template’s name. This is required to indicate that the configuration is a template and not an actual cache.

Once the cache template is registered in the cluster, as shown in the code snippet above, you can use it to create another cache with the same configuration.
