# Working with HDFS Encryption Zones

Hadoop systems use the Hadoop Key Management Server (KMS) to obtain encryption keys. BigConnect Pipelines requires a truststore file to verify the identity of the KMS server.

To enable access to HDFS encryption zones while using proxy users, configure KMS to allow the same user impersonation as you have configured for HDFS.

To create a truststore file, follow the same steps that you do when enabling HTTPS. See Step 2. Create a Truststore File.

To allow BigConnect Pipelines as a proxy user, add the following properties to the KMS configuration file and configure the values for the properties:

* `hadoop.kms.proxyuser.sdc.groups`
* `hadoop.kms.proxyuser.sdc.hosts`

For example, the following properties allows users in the Ops group access to the encryption zones:

```
<property>
<name>hadoop.kms.proxyuser.sdc.groups</name>
<value>Ops</value>
</property>
<property>
<name>hadoop.kms.proxyuser.sdc.hosts</name>
<value>*</value>
</property>
```

Note that the asterisk (\*) indicates no restrictions.

For more information about configuring KMS proxy users, see the KMS documentation for the Hadoop distribution that you are using. For example, for Apache Hadoop, see the [Apache Hadoop documentation](https://hadoop.apache.org/docs/current/hadoop-kms/index.html#KMS\_Proxyuser\_Configuration).
