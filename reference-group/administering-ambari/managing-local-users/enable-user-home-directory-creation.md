# Enable user home directory creation

You can enable automated creation of a `/user/[USER_NAME]` HDFS home directory for each user that you create.

A common requirement to initialize user accounts to run Hadoop components is the existence of a unique, `/user/[USER_NAME]` HDFS home directory. You can enable automated creation of a `/user/[USER_NAME]` HDFS home directory for each user that you create. Home directory creation occurs for users created either manually using the Ambari Admin page, or through LDAP synchronization.

1. On your Ambari Server host, edit the ambari-properties file. using a command line editor \(vi, in this example\):

```text
$ vi /opt/bdl/etc/ambari-server/conf/ambari.properties
```

2. Add the following property: `ambari.post.user.creation.hook.enabled=true`

3. Add the script path to the ambari properties file:   
`ambari.post.user.creation.hook=/var/lib/ambari-server/resources/scripts/post-user-creation-hook.sh`

{% hint style="danger" %}
In a Kerberized environment, you must modify the kinit file path in the default user creation hook script.  
`/var/lib/ambari-server/resources/scripts/post-user-creation-hook.sh`
{% endhint %}

4. Restart Ambari server

After enabling the post-user creation script, Ambari executes the script whenever a user is created and logs a message each time the script is invoked. If the script has a non-zero exit code, an ERROR is logged, otherwise an INFO-level message that includes the script path and parameters is logged.

