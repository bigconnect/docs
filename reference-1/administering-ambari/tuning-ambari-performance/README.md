# Tuning Ambari performance

Ambari-managed clusters larger that 100 nodes may require tuning to perform optimally.

For clusters larger than 100 nodes, consider the following tuning options:

* Increase available memory by adjusting heap size based on the number of cluster nodes.
* Set a new, larger cache size.
* Adjust the JDBC connection pool settings.
* Increase Wait Timeout and Interacitve Timeout Settings.
* Increase available memory by adjusting heap size based on the number of cluster nodes.
* For clusters larger than 1500 nodes, configure Ambari agents for optimum performance

After performing one or more of these options, restart Ambari server for the option\(s\) to take effect.

If you are using the Ambari Metrics service, you might want to consider switching from the default embedded mode to distributed mode, as well as other tuning options.

{% page-ref page="adjust-ambari-server-heap-size.md" %}

{% page-ref page="increase-ambari-server-cache-size.md" %}

{% page-ref page="adjust-jdbc-pool-settings.md" %}

{% page-ref page="mysql-settings.md" %}

{% page-ref page="agent-performance.md" %}

{% page-ref page="purge-ambari-server-history.md" %}



