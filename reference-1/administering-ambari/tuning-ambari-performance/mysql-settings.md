# MySQL settings

Adjust the following properties to increase the timeout values for a MySQL Ambari backend database:

{% hint style="danger" %}
It is critical that the Ambari configuration for `server.jdbc.connection-pool.max-idle-time` and `server.jdbc.connection-pool.idle-test-interval` are lower than the MySQL `wait_timeout` and `interactive_timeout` set on the MySQL side.
{% endhint %}

1.In your MSQL configuration, increase the `wait_timeout`, `interactive_timeout` and `max.connections` settings:

| Setting | Default | Recommended |
| :--- | :--- | :--- |
| wait\_timeout | n/a | 8 hrs \(28800\) |
| interactive\_timeout | n/a | 8 hrs \(28800\) |
| max.connections | 32 | 128 |

2. If you choose to decrease these timeout values, adjust connection pool settings in the Ambari configuration so that they are less than the adjusted `wait_timeout` and `interactive_timeout` values.

| Connection Pool Setting | Value must be | Timout Setting |
| :--- | :---: | :--- |
| downserver.jdbc.connection-pool.max-idle-time	 | &lt; | wait\_timeout |
| server.jdbc.connection-pool.idle-test-interval	 | &lt; | interactive\_timeout |

