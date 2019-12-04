# Configure ViewFs

Configure ViewFs to create common views for file and directory paths corresponding to locations on different NameNodes of a federated HDFS cluster by specifying mount table entries. You can use Ambari Web UI to specify ViewFs as the default file system, and specify mount table entries for the file and directory paths.

**Before you begin**  
Verify that you have configured HDFS federation for the cluster.

**About this task**

* You can either specify the ViewFs mount table entries as key-value pairs in `core-site.xml`, or add a separate configuration file containing the mount paths.
* ViewFs is _not_ supported on Hive clusters.

**Steps**  


1.In **Ambari Web**, select **Services &gt; HDFS &gt; Configs**, and navigate to **Advanced core-site**. 

2. Update the value of the **fs.defaultFS** property to `viewfs://<clustername>`  
For example, if your cluster is named `Clusterx`, set the value to `viewfs://ClusterX.`   
  
This sets ViewFs as the default file system.

{% hint style="info" %}
**Note**  
Updating fs.defaultFS to ViewFs also updates the default root to point to the ViewFs root. Therefore, you must remap any directory paths mounted earlier to point to the namespaces that contain those directories.
{% endhint %}

3. Use either of the following options to specify the mount table entries for the file and directory paths.   
  
**Key-value pairs in core-site.xml**:   
  
a. Navigate to **Custom core-site**.   
  
b. Click **Add property**.   
  
c. Add a path name property as the key, its corresponding mount point as the value, and click **Add**.   
Add similar key-value pairs for all the file and directory paths you want to mount.

 For example, if `ClusterX` has a directory named `/tmp` in the namespace `ns1` and another directory named `/foo` in the namespace `ns2`, you can define the following key-value pairs:

|  | Key | Value |
| :--- | :--- | :--- |
|  | `fs.viewfs.mounttable.ClusterX.link./tmp` | `hdfs://ns1/tmp` |
|  | `fs.viewfs.mounttable.ClusterX.link./foo` | `hdfs://ns2/foo` |

  
d. Click **Save**.   
e. Add a description of the configuration changes, and click **Save**.   
f. Restart HDFS and other services as applicable.   
  
**Mount table configuration file:**   
  
a.Navigate to **Advanced viewfs-mount-table**.   
  
b. Enter the mount table entries to the various file and directory paths as properties in the text box.   
  
For the previous example, you can specify mount table entries for the directories `/foo` and `/tmp` as follows:

```text
<configuration>
  <property>
    <name>fs.viewfs.mounttable.ClusterX.link./tmp</name>
    <value>hdfs://ns1/tmp</value>
  </property>
  <property>
    <name>fs.viewfs.mounttable.ClusterX.link./foo</name>
    <value>hdfs://ns2/foo</value>
  </property>
</configuration>
```

c. Click **Save**.

d. Add a description of the configuration changes, and click **Save**.

e. Restart HDFS and other services as applicable.

