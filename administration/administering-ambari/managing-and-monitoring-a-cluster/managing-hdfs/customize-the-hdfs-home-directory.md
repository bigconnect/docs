# Customize the HDFS home directory

**About this task**  
 By default, the HDFS home directory is set to `/user/[USER_NAME]`. You can use the `dfs.user.home.base.dir` property to customize the HDFS home directory.

**Steps**

1. In **Ambari Web**, browse to **Services &gt; HDFS &gt; Configs &gt; Advanced**. 
2. Click **Custom hdfs-site**, then click **Add Property**. 
3. On the **Add Property** pop-up, add the following property:`dfs.user.home.base.dir=[HOME_DIRECTORY]` Where `[HOME_DIRECTORY]` is the path to the new home directory. 
4. Click **Add**, then save the new configuration and restart, as prompted.

