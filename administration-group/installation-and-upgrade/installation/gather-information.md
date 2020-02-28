# Gather Information

Before deploying a cluster, you should collect the following information:

1. The fully qualified domain name \(FQDN\) of each host in your system. The Ambari Cluster Install wizard supports using IP addresses. You can use:

```text
hostname -f
```

to check or verify the FQDN of a host.

{% hint style="info" %}
Deploying all components on a single host is possible, but is appropriate only for initial evaluation purposes. Typically, you set up at least three hosts; one master host and two slaves, as a minimum cluster.
{% endhint %}

2. A list of components you want to set up on each host.

3. The base directories you want to use as mount points for storing:

* NameNode data
* DataNodes data
* Secondary NameNode data
* JournalNode data
* YARN data
* ZooKeeper data
* Druid data
* Various log, pid, and db files, depending on your install type

{% hint style="danger" %}
You must use base directories that provide persistent storage locations for your components and your Hadoop data. Installing components in locations that may be removed from a host may result in cluster failure or data loss. For example: Do Not use `/tmp` in a base directory path.
{% endhint %}

