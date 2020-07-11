# Checkpoint the active NameNode

**About this task**  
If HDFS is used _after_ you enable NameNode HA, but you want to revert to a non-HA state, you must checkpoint the HDFS state before proceeding with the rollback. If the **Enable NameNode HA wizard** failed and you need to revert, you can omit this step and proceed to stop all services. Checkpointing the HDFS state requires different syntax, depending on whether Kerberos security is enabled on the cluster or not:

* If Kerberos security has _not_ been enabled on the cluster, use the following command on the active NameNode host and as the HDFS service user, to save the namespace:`sudo su -l [HDFS_USER] -c 'hdfs dfsadmin -safemode enter' sudo su -l [HDFS_USER] -c 'hdfs dfsadmin -saveNamespace'`
* If Kerberos security _has_ been enabled on the cluster, use the following commands to save the namespace:`sudo su -l [HDFS_USER] -c 'kinit -kt /etc/security/keytabs/nn.service.keytab nn/[HOSTNAME]@[REALM];hdfs dfsadmin -safemode enter' sudo su -l [HDFS_USER] -c 'kinit -kt /etc/security/keytabs/nn.service.keytab nn/[HOSTNAME]@[REALM];hdfs dfsadmin -saveNamespace'`In this example, \[HDFS\_USER\] is the HDFS service user \(for example, hdfs\), \[HOSTNAME\] is the Active NameNode hostname, and \[REALM\] is your Kerberos realm.

**After you finish**  
Stop all services.

