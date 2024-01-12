# Working with Upgraded External Systems

When an external system is upgraded to a new version, you can continue to use existing BigConnect Pipelines pipelines that connected to the previous version of the external system. You simply configure the pipelines to work with the upgraded system.

For example, let's say that you have pipelines that read from Apache Kafka version 0.9. You upgrade Apache Kafka to version 0.10. You can continue to use the existing pipelines after you configure the Kafka stages to use the Kafka version 0.10 stage library.

Or, let's say that you develop a pipeline to write to Cloudera CDH version 5.8 distribution of Hadoop. Then you export and import the pipeline into a BigConnect Pipelines that has the Cloudera CDH version 5.9 stage library installed. You can continue to use the imported pipeline after you configure the appropriate stages to use the Cloudera CDH version 5.9 stage library.

1.  Verify that the new stage library version is installed in BigConnect Pipelines.\
    \
    For a tarball installation, you can use the Package Manager or the command line to view or install stage libraries.

    For an RPM installation, you must use the command line to view or install stage libraries.

    For a Cloudera Manager installation, all available stage libraries are included.
2. Open each pipeline that connects to the upgraded external system.
3. On the **General** tab for each stage that connects to the external system, select the new stage library version.

### Working with Kafka 0.11 or Later <a href="#ariaid-title2" id="ariaid-title2"></a>

When you upgrade to Kafka version 0.11 or later and your pipelines use Apache Kafka stage libraries, you must enable Kerberos authentication for the Apache Kafka stages in the Java Authentication and Authorization Service (JAAS) configuration file.

**Note**: For any version of a Cloudera or Hortonworks Kafka stage library, you enable Kerberos authentication in the JAAS configuration file. As a result, you do not need to update Kerberos authentication for these stages when you upgrade your Kafka system.\
\
For an Apache Kafka stage library version 0.10 or earlier, you can enable Kerberos authentication in one of the following ways:\
\
**BigConnect Pipelines configuration file**\
\
Define the following Kerberos properties in the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`:

* kerberos.client.enabled
* kerberos.client.principal
* kerberos.client.keytab

**JAAS configuration file**\
\
Configure the Kerberos properties in the JAAS configuration file. If BigConnect Pipelines uses LDAP authentication, configure the properties in the `$SDC_CONF/ldap-login.conf` file. If BigConnect Pipelines does not use LDAP authentication, configure the properties in a separate JAAS configuration file on the BigConnect Pipelines machine and modify the SDC\_JAVA\_OPTS environment variable to specify the location of the file.

As of Kafka version 0.11, you can no longer enable Kerberos in the BigConnect Pipelines configuration file for Apache Kafka stage libraries. Instead, you must configure the Kerberos properties in the JAAS configuration file.

If earlier Kafka stages using an Apache Kafka stage library used the BigConnect Pipelines configuration file, you must update each BigConnect Pipelines that runs Kafka pipelines to add the JAAS configuration properties required for Kafka clients.

For instructions about using the JAAS configuration properties to enable Kerberos for Kafka stages, see the "Enabling Security" section in the appropriate stage documentation:

* HTTP to Kafka origin
* Kafka Consumer origin
* Multitopic Kafka Consumer origin
* SDC RPC to Kafka origin
* UDP to Kafka origin
* Kafka Producer destination

### Working with Cloudera CDH 5.11 or Later <a href="#ariaid-title3" id="ariaid-title3"></a>

When you upgrade to Cloudera CDH version 5.11 or later from a previous version, you must update pipelines that set permissions on HDFS or Hive by modifying file mode bits with the minus or equals operators.

Pipelines can modify file mode bits on HDFS or Hive with the following stage properties:

* The HDFS File Metadata executor Set Permissions property.
* The Hadoop FS destination whole file Permissions Expression whole file property.

As of CDH 5.11, Cloudera changed how the minus and equals operators are evaluated as follows:

*   In previous CDH releases, the minus operator (-) grants the specified permissions. In the current release, it removes the specified permissions.

    For example, in previous releases, `a-rw` grants read and write permissions to all users. With CDH 5.11, it removes read and write permissions from all users.
*   In earlier CDH releases, the equals operator (=) removes the specified permissions. In the current release, it grants the specified permissions.

    For example, in previous releases, `a=we` removes write and execute permission from all users. With CDH 5.11, it grants write and execute permission to all users.

To ensure that file permissions are set as expected, update all properties in upgraded pipelines that modify file mode bits with the minus or equals operators.

This behavior change is noted in the [Cloudera documentation](https://www.cloudera.com/documentation/enterprise/release-notes/topics/cdh\_rn\_hdfs\_ic.html) regarding the fix for HADOOP-13508.

### Working with an Upgraded MapR System <a href="#ariaid-title4" id="ariaid-title4"></a>

If you upgrade MapR, you must complete additional steps to continue using existing pipelines that connected to the previous MapR version.

1. Stop BigConnect Pipelines.
2.  In the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`, add the previous MapR version stage library to the `system.stagelibs.blacklist` property.

    You do not need to remove the MapR version that you upgraded to from the list. This is configured automatically when you run the `setup-mapr` command in a later step.

    For example, if you upgraded MapR version 6.0.0 to 6.1.0, simply add MapR version 6.0.0 to the blacklist property so that the property lists all supported MapR versions like so:

    ```
    system.stagelibs.blacklist=streamsets-datacollector-mapr_6_0-lib,
    streamsets-datacollector-mapr_6_0-mep4-lib,streamsets-datacollector-mapr_6_0-mep5-lib,
    streamsets-datacollector-mapr_6_1-lib,streamsets-datacollector-mapr_6_1-mep6-lib
    ```
3.  If the MapR cluster uses username/password login authentication, modify the SDC\_JAVA\_OPTS environment variable in the required file based on how you start BigConnect Pipelines.

    For more information about the required file to edit, see Modifying Environment Variables.

    *   Manual start - Uncomment the following line in the `sdc-env.sh` file:

        ```
        #export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Dmaprlogin.password.enabled=true"
        ```
    *   Service start on operating systems that use the SysV init system - On CentOS 6, Oracle Linux 6, Red Hat Enterprise Linux 6, or Ubuntu 14.04 LTS, uncomment the following line in the `sdcd-env.sh` file:

        ```
        #export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Dmaprlogin.password.enabled=true"
        ```
    *   Service start on operating systems that use the systemd init system - On CentOS 7, Oracle Linux 7, Red Hat Enterprise Linux 7, or Ubuntu 16.04 LTS, add the following line to the file that overrides the default settings in the `sdc.service` file:

        ```
        Environment=SDC_JAVA_OPTS=-Dmaprlogin.password.enabled=true
        ```

        Override the default values in the `sdc.service` file using the same procedure that you use to override unit configuration files on a systemd init system. For an example, see "Example 2. Overriding vendor settings" in this systemd.unit manpage.

        After overriding the default values, use the following command to reload the systemd manager configuration:

        ```
        systemctl daemon-reload
        ```
4.  Run the `setup-mapr` command, as described in Step 3. Run the Command to Set Up MapR.

    The command modifies configuration files and creates the required symbolic links. You can run the command in interactive or non-interactive mode.
