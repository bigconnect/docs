# Post Upgrade Tasks

In some situations, you must complete tasks within BigConnect Pipelines or your Control Hub on-premises installation after you upgrade.

### Update Control Hub On-Premises <a href="#ariaid-title2" id="ariaid-title2"></a>

By default, BigConnect Pipelines Control Hub on-premises can work with registered BigConnect Pipelines from version 2.1.0.0 to the current version of Control Hub. If you use Control Hub on-premises and you upgrade registered BigConnect Pipelines to a version higher than your current version of Control Hub, you might need to modify the BigConnect Pipelines version range within your Control Hub installation.

For example, if you use Control Hub on-premises version 3.8.0 and you upgrade registered BigConnect Pipelines to version 3.22.0, you must update the maximum BigConnect Pipelines version that can work with Control Hub. As a best practice, configure the maximum BigConnect Pipelines version to 3.99.999 to ensure that BigConnect Pipelines upgrades to later minor versions, such as 3.22.1 or 3.22.2, will continue to work with Control Hub.

**Note**: If you register BigConnect Pipelines version 3.19.x or later with Control Hub on-premises version 3.18.x or earlier, then some stages in the Control Hub Pipeline Designer display a Connection property that is not supported. Do not change the property from the default value of None. If you select Choose Value or use a parameter to define the property, Pipeline Designer hides the remaining connection properties and the pipeline fails to run.

To modify the BigConnect Pipelines version range:

1. Log in to Control Hub as the default system administrator - the admin@admin user account.
2. In the Navigation panel, click **Administration** > **BigConnect Pipelines**.
3. Click the **Component Version Range** icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OrganizationConfig.png).
4. Enter the maximum BigConnect Pipelines version that can work with Control Hub, such as 3.99.999.

### Update Pipelines using Legacy Stage Libraries <a href="#ariaid-title3" id="ariaid-title3"></a>

When you upgrade, review the complete list of legacy stage libraries. If your upgraded pipelines use these legacy stage libraries, the pipelines will not run until you perform one of the following tasks:\
\
**Use a current stage library**\
\
We strongly recommend that you upgrade your system and use a current stage library in the pipeline:

1. Upgrade the system to a more current version.
2. Install the stage library for the upgraded system.
3. In the pipeline, edit the stage and select the appropriate stage library.

**Install the legacy stage library**\
\
Though not recommended, you can install the older stage libraries. For more information, see Legacy Stage Libraries.

### Upgrade Enterprise Stage Libraries <a href="#ariaid-title4" id="ariaid-title4"></a>

When you upgrade BigConnect Pipelines, you must determine whether to upgrade your Enterprise stage libraries. Releases of Enterprise stage libraries occur separately from BigConnect Pipelines releases.

See Enterprise Stage Libraries for a list of available Enterprise stage libraries, the latest available versions, and links to the supported versions and the stage documentation. To view the release notes for Enterprise stage libraries, see the BigConnect Pipelines Documentation page.

1. Uninstall the previous version of the Enterprise stage library.
   1. In Package Manager, select the installed version.
   2. Click the **Uninstall** icon.
   3. Restart BigConnect Pipelines.
2. Follow the stage documentation to install the new version of the Enterprise stage library and restart BigConnect Pipelines.

### Review HTTP Client Processor Pipelines <a href="#ariaid-title5" id="ariaid-title5"></a>

Starting with version 3.22.3, the HTTP Processor performs additional checks against the specified Batch Wait Time property. In certain cases, this change can generate errors. After upgrading from version 3.22.2 or earlier to version 3.22.3 or later, verify that pipelines that include the HTTP Client processor perform as expected.

The Batch Wait Time property defines the maximum amount of time that the processor uses to process all HTTP requests for a single record. When the processing for a record exceeds the specified batch wait time, the output records are passed to the stage for error handling.

In previous releases, the HTTP Client processor only checked the batch wait time before each HTTP request. As a result, the processor did not always notice when the processing time exceeded the batch wait time.

Starting with version 3.22.3, the HTTP Client processor checks the batch wait time before and after every request. As a result, the processor may generate more errors than in previous releases.

Also, in previous releases, the default value for Batch Wait Time was 2,000 milliseconds. Starting with version 3.22.3, the default value is 100,000 milliseconds.

**Important**: When you upgrade from version 3.22.2 or earlier to version 3.22.3 or later, the Batch Wait Time property in the HTTP Client processor is set to the new default of 100,000 milliseconds, unless you changed the property from the default.

For example, if you did not touch the Batch Wait Time property in a 3.22.0 pipeline, then it is increased from 2,000 to 100,000 milliseconds during the upgrade. However, if you set the property to 3000 milliseconds in a 3.22.0 pipeline, then the processor retains the 3000 millisecond batch wait time after the upgrade.

After upgrading from version 3.22.2 or earlier to version 3.22.3 or later, verify that pipelines that include the HTTP Client processor perform as expected. If you want the processor to wait for all HTTP requests to complete, increase the Batch Wait Time as needed.

### Verify Elasticsearch Security <a href="#ariaid-title6" id="ariaid-title6"></a>

Starting with version 3.21.0, Elasticsearch stages include additional security validation. As a result, pipelines with Elasticsearch security issues that previously ran without error might fail to start after you upgrade to version 3.21.0 or later.

When this occurs, check for additional details in the error messages, then correct the security issue or stage configuration, as needed.

For example, in earlier BigConnect Pipelines versions, an Elasticsearch stage configured to use the AWS Signature V4 security mode with SSL/TLS would not generate an error if the certificate was missing from the specified truststore. With version 3.21.0 or later, the pipeline fails to start with the following error:

```
ELASTICSEARCH_43 - Could not connect to the server(s) <SSL/TLS error details>
```

As another example, in earlier versions, if you specify a port in an HTTP URL that doesn’t support the HTTPS protocol when configuring an Elasticsearch stage to use SSL/TLS, the stage used HTTP without raising an error. With version 3.21.0 or later, the pipeline fails to start with an error such as:

```
ELASTICSEARCH_43 - Could not connect to the server(s). 
Unrecognized SSL message, plaintext connection?
```

Note that the details of the message vary based on the originating server.

### Adjust PostgreSQL CDC Pipelines or PostgreSQL Configuration <a href="#ariaid-title7" id="ariaid-title7"></a>

Starting with version 3.21.0, the PostgreSQL CDC Client origin includes a new Status Interval property that helps ensure that the wal2json logical decoder, which helps process changes, does not time out.

The new Status Interval origin property should be set to less than the wal\_sender\_timeout property in the PostgreSQL `postgresql.conf` file. Ideally, the Status Interval property should be half of the value configured for the wal\_sender\_timeout property.

By default, the Status Interval property is 30 seconds. The wal2json `README.md` file previously recommended setting the wal\_sender\_timeout property to 2000 milliseconds, or 2 seconds. If you use these values for both properties, the pipeline can trigger the following error:

```
com.streamsets.pipeline.api.StageException: JDBC_606 - Wal Sender is not active
```

To avoid this issue, update one of the properties so that Status Interval is half of wal\_sender\_timeout.

When possible, use the default Status Interval value and the default wal\_sender\_timeout value of 60000 milliseconds, or 60 seconds.

### Review Processing of MySQL Data (JDBC Processors) <a href="#ariaid-title8" id="ariaid-title8"></a>

Starting with version 3.21.0, JDBC processors convert MySQL unsigned integer data types to different BigConnect PipelinesBigConnect Pipelines types than in earlier BigConnect Pipelines versions. This change occurred for JDBC origins in an earlier version.

When you upgrade to version 3.21.0 or later, review pipelines that use JDBC processors to work with MySQL database data to ensure that downstream expressions provide the expected results.

The following table describes the data type conversion changes:

| MySQL Data Type    | Data Type Conversion Before 3.21.0 | Data Type Conversion with 3.21.0 and Later |
| ------------------ | ---------------------------------- | ------------------------------------------ |
| Bigint Unsigned    | Long                               | Decimal                                    |
| Int Unsigned       | Integer                            | Long                                       |
| Mediumint Unsigned | Integer                            | Long                                       |
| Smallint Unsigned  | Short                              | Short                                      |

### Review Google Pub/Sub Producer Pipelines <a href="#ariaid-title9" id="ariaid-title9"></a>

Starting with version 3.20.0, the Google Pub/Sub Producer destination requires specifying a positive integer value for the Max Outstanding Message Count and Max Outstanding Request Bytes properties.

In earlier BigConnect Pipelines versions, you could set these properties to 0 to opt out of using them. With version 3.20.0 and later, these properties must be set to a positive integer.

Upgraded pipelines with these properties set to positive integers retain the configured values. Upgraded pipelines with these properties set to 0 are updated to use the new default values, as follows:

* Max Outstanding Message Count is set to 1000 messages
* Max Outstanding Request Bytes is set to 8000 bytes

If upgraded pipelines previously used 0 to opt out of using these properties, review the pipelines to ensure that the new default values are appropriate. Update the properties as needed.

### Review JDBC Multitable Consumer Pipelines <a href="#ariaid-title10" id="ariaid-title10"></a>

Starting with version 3.20.0, the JDBC Multitable Consumer origin behavior while performing multithreaded processing with the Switch Tables batch strategy has changed. This affects multithreaded table and partition processing in a similar manner:\
\
**Multithreaded table processing**\
\
In earlier BigConnect Pipelines versions, when you use the Switch Tables batch strategy with multithreaded table processing, multiple threads can take turns processing data within a single table, caching separate result sets for the table.\
\
With this release, each table can have only a single result set cached at a time.So while a thread switches tables between batches, it now skips tables that already have a result set from another thread. Only one thread can process the data in a table at a time.\
\
**Multithreaded partition processing**\
\
Similarly, in earlier BigConnect Pipelines versions, when you use the Switch Tables batch strategy with multithreaded partition processing, multiple threads can take turns processing data within a single partition, caching separate result sets for the partition.\
\
With this release, each partition can have only a single result set cached at a time.\
\
So while a thread switches partitions between batches, it now skips partitions that already have a result set from another thread. Only one thread can process the data in a partition at a time.

Review upgraded pipelines that use the Switch Table batch strategy. Depending on factors such as the number and size of the tables and partitions being processed, the change might negatively impact performance.

For example, say two threads process four tables in multithreaded table processing, and one table is much larger than the other tables. In earlier versions, using the Switch Tables batch strategy allowed multiple threads to help process the large table. With version 3.20.0 or later, only one thread can process data in one table at a time.

If pipeline performance has been negatively impacted, consider the following options:

*   If multithreaded table processing has slowed, you may have a mix of small and large tables.

    To enable large tables to be processed by more than one thread, consider using multithreaded partition processing for that table.

    To enable threads to cycle through the tables more quickly, you might reduce the number of batches generated from a result set using the Batches from Result Set property.
*   If multithreaded partition processing has slowed, you may have a mix of small and large partitions.

    To enable threads to cycle through the partitions more quickly, you might reduce the number of batches generated from a result set using the Batches from Result Set property.

For information about batch strategies, see Batch Strategy.

### Update Oracle CDC Client Pipelines <a href="#ariaid-title11" id="ariaid-title11"></a>

Consider the following upgrade tasks for pipelines that contain the Oracle CDC Client origin, based on the version that you are upgrading from:

**Upgrade from versions earlier than 3.19.0**\
\
Starting with version 3.19.0, Oracle CDC Client origins with the Parse SQL property enabled no longer generate records for SELECT\_FOR\_UPDATE operations.\
\
If your Oracle CDC Client pipelines do not process SELECT\_FOR\_UPDATE operations or do not need to process SELECT\_FOR\_UPDATE operations, no changes are required.\
\
If you want to capture SELECT\_FOR\_UPDATE statements, you can clear the Parse SQL Query property to write LogMiner SQL statements to generated records. Then, specify SELECT\_FOR\_UPDATE in the Operations property.\
\
**Upgrade from versions earlier than 3.7.0**\
\
Starting with version 3.7.0, pipelines that use the Oracle CDC Client origin can produce some duplicate data.\
\
Due to a change in offset format, when the pipeline restarts, the Oracle CDC Client origin reprocesses all transactions with the commit SCN from the last offset to prevent skipping unread records. This issue occurs only for the last SCN that was processed before the upgrade, and only once, upon upgrading to BigConnect Pipelines version 3.7.0 or later.\
\
When possible, remove the duplicate records from the destination system.

### Update Cluster EMR Batch Pipelines <a href="#ariaid-title12" id="ariaid-title12"></a>

Starting with version 3.19.0, cluster EMR batch pipelines that provision a cluster store the specified EMR version differently than in earlier versions. As a result, the EMR versions defined in earlier pipelines are not retained.

When you upgrade from a version earlier than 3.19.0, you must edit any cluster EMR batch pipeline that provisions a cluster, and define the EMR Version property.

### Review Processing of MySQL Data (JDBC Origins) <a href="#ariaid-title13" id="ariaid-title13"></a>

Starting with version 3.17.0, JDBC origins convert MySQL unsigned integer data types to different BigConnect Pipelines types than in earlier BigConnect Pipelines versions.

When you upgrade to version 3.17.0 or later, review pipelines that use JDBC origins to process MySQL database data to ensure that downstream expressions provide the expected results.

The following table describes the data type conversion changes:

| MySQL Data Type    | Data Type Conversion Before 3.17.0 | Data Type Conversion with 3.17.0 and Later |
| ------------------ | ---------------------------------- | ------------------------------------------ |
| Bigint Unsigned    | Long                               | Decimal                                    |
| Int Unsigned       | Integer                            | Long                                       |
| Mediumint Unsigned | Integer                            | Long                                       |
| Smallint Unsigned  | Short                              | Short                                      |

### Update Elasticsearch Security Properties (Optional) <a href="#ariaid-title14" id="ariaid-title14"></a>

Starting with version 3.17.0, Elasticsearch stages provide a User Name property and a Password property. Elasticsearch stages in previous versions pass the credentials together in a single Security Username/Password property.

When you upgrade to version 3.17.0 or later, any configuration in the Security Username/Password properties is moved to the new User Name property, where the Security Username/Password format, `<username>:<password>`, remains valid.

Though not required, you can update Elasticsearch stages to use the new User Name and Password properties.

### Update Syslog Pipelines <a href="#ariaid-title15" id="ariaid-title15"></a>

Starting with version 3.9.0, the Syslog destination no longer includes the following properties on the Message tab:

* Use Non-Text Message Format
* Message Text

You now configure the destination to use the Text data format on the Data Format tab, just as you do with other destinations.

If pipelines created in a previous version include the Syslog destination configured to use text data, you must configure the Text data format properties on the Data Format tab after the upgrade.

### JDBC Tee and JDBC Producer Cache Change <a href="#ariaid-title16" id="ariaid-title16"></a>

Starting with version 3.9.0, the JDBC Tee processor and the JDBC Producer destination no longer cache prepared statements when performing single-row operations. As a result, the Max Cache Size Per Batch property has been removed from both stages.

In previous versions when you enabled the stage to perform single-row operations, you could configure the Max Cache Size Per Batch property to specify the maximum number of prepared statements to store in the cache.

### Pipeline Export <a href="#ariaid-title17" id="ariaid-title17"></a>

Starting with version 3.8.0, BigConnect Pipelines has changed the behavior of the pipeline **Export** option. BigConnect Pipelines now strips all plain text credentials from exported pipelines. Previously, BigConnect Pipelines included plain text credentials in exported pipelines.

To use the previous behavior and include credentials in the export, choose the new **Export with Plain Text Credentials** option when exporting a pipeline.

### Update TCP Server Pipelines <a href="#ariaid-title18" id="ariaid-title18"></a>

Starting with version 3.7.2, the TCP Server origin has changed the valid values for the Read Timeout property. The property now allows a minimum of 1 second and a maximum of 3,600 seconds.

In previous versions, the Read Timeout property had no maximum value and could be set to 0 to keep the connection open regardless of whether the origin read any data.

If pipelines created in a previous version have the Read Timeout property set to a value less than 1 or greater than 3,600, the upgrade process sets the property to the maximum value of 3,600 seconds. If necessary, update the Read Timeout property as needed after the upgrade.

### Update Cluster Pipelines <a href="#ariaid-title19" id="ariaid-title19"></a>

Starting with version 3.7.0, BigConnect Pipelines now requires that the Java temporary directory on the gateway node in the cluster is writable.

The Java temporary directory is specified by the Java system property `java.io.tmpdir`. On UNIX, the default value of this property is typically /tmp and is writable.

Previous BigConnect Pipelines versions did not have this requirement. Before running upgraded cluster pipelines, verify that the Java temporary directory on the gateway node is writable.

### Update Kafka Consumer or Kafka Multitopic Consumer Pipelines <a href="#ariaid-title20" id="ariaid-title20"></a>

Starting with version 3.7.0, BigConnect Pipelines no longer uses the `auto.offset.reset` value set in the Kafka Configuration property to determine the initial offset for the Kafka Consumer or Kafka Multitopic Consumer origin. Instead, BigConnect Pipelines uses the new Auto Offset Reset property to determine the initial offset. With the default setting of the new property, the origin reads all existing messages in a topic. In previous versions, the origin read only new messages by default. Because running a pipeline sets an offset value, the configuration of the initial offset only affects pipelines that have never run.

After upgrading from a version earlier than 3.7.0, update any pipelines that have not run and use the Kafka Consumer or Kafka Multitopic Consumer origins.

1. On the **Kafka** tab for the origin, set the value of the **Auto Offset Reset** property:
   * **Earliest** - Select to have the origin read messages starting with the first message in the topic (same behavior as configuring **auto.offset.reset** to **earliest** in previous versions of BigConnect Pipelines).
   * **Latest** - Select to have the origin read messages starting with the last message in the topic (same behavior as not configuring **auto.offset.reset** in previous versions of BigConnect Pipelines).
   * **Timestamp** - Select to have the origin read messages starting with messages at a particular timestamp, which you specify in the **Auto Offset Reset Timestamp** property.
2. If configured in the **Kafka Configuration** property, delete the **auto.offset.reset** property.

### Update JDBC Pipelines <a href="#ariaid-title21" id="ariaid-title21"></a>

Starting with version 3.5.0, BigConnect Pipelines requires the maximum lifetime for a connection to be at least 30 minutes in stages that use a JDBC connection. BigConnect Pipelines does not validate stages with lower non-zero values configured.

If you upgrade pipelines that include a stage that uses a JDBC connection, update the stage to set the maximum lifetime for a connection to be at least 30 minutes.

On the **Advanced tab**, set the **Max Connection Lifetime** property to be at least 30 minutes or 1800 seconds.

### Update Spark Executor with Databricks Pipelines <a href="#ariaid-title22" id="ariaid-title22"></a>

Starting with version 3.5.0, BigConnect Pipelines introduces a new Databricks Job Launcher executor and has removed the ability to use the Spark executor with Databricks.

If you upgrade pipelines that include the Spark executor with Databricks, you must update the pipeline to use the Databricks Job Launcher executor after you upgrade.

### Update Pipelines to Use Spark 2.1 or Later <a href="#ariaid-title23" id="ariaid-title23"></a>

Starting with version 3.3.0, BigConnect Pipelines removes support for Spark 1.x and introduces cluster streaming mode with support for Kafka security features such as SSL/TLS and Kerberos authentication using Spark 2.1 or later and Kafka 0.10.0.0 or later. For more information about these changes, see Upgrade to Spark 2.1 or Later.

After upgrading the Cloudera CDH distribution, Hortonworks Hadoop distribution, or Kafka system to the required version and then upgrading BigConnect Pipelines, you must update pipelines to use Spark 2.1 or later. Pipelines that use the earlier systems will not run until you perform these tasks:

1. Install the stage library for the upgraded system.
2. In the pipeline, edit the stage and select the appropriate stage library.
3.  If the pipeline includes a Spark Evaluator processor and the Spark application was previously built with Spark 2.0 or earlier, rebuild it with Spark 2.1.

    Or if you used Scala to write the custom Spark class, and the application was compiled with Scala 2.10, recompile it with Scala 2.11.
4. If the pipeline includes a Spark executor and the Spark application was previously built with Spark 2.0 or earlier, rebuild it with Spark 2.1 and Scala 2.11.

### Update Value Replacer Pipelines <a href="#ariaid-title24" id="ariaid-title24"></a>

Starting with version 3.1.0.0, BigConnect Pipelines introduces a new Field Replacer processor and has deprecated the Value Replacer processor.

The Field Replacer processor lets you define more complex conditions to replace values. For example, unlike the Value Replacer, the Field Replacer can replace values that fall within a specified range.

You can continue to use the deprecated Value Replacer processor in pipelines. However, the processor will be removed in a future release - so we recommend that you update pipelines to use the Field Replacer as soon as possible.

To update your pipelines, replace the Value Replacer processor with the Field Replacer processor. The Field Replacer replaces values in fields with nulls or with new values. In the Field Replacer, use field path expressions to replace values based on a condition.

For example, let's say that your Value Replacer processor is configured to replace null values in the product\_id field with "NA" and to replace the "0289" store ID with "0132" as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/UpgradeValueReplacer.png)

In the Field Replacer processor, you can configure the same replacements using field path expressions as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/UpgradeValueReplacerWithFieldReplacer.png)

### Update Einstein Analytics Pipelines <a href="#ariaid-title25" id="ariaid-title25"></a>

Starting with version 3.1.0.0, the Einstein Analytics destination introduces a new append operation that lets you combine data into a single dataset. Configuring the destination to use dataflows to combine data into a single dataset has been deprecated.

You can continue to configure the destination to use dataflows. However, dataflows will be removed in a future release - so we recommend that you update pipelines to use the append operation as soon as possible.

### Disable Cloudera Navigator Integration <a href="#ariaid-title26" id="ariaid-title26"></a>

Starting with version 3.0.0.0, the beta version of Cloudera Navigator integration is no longer available with BigConnect Pipelines. Cloudera Navigator integration now requires a paid subscription. For more information about purchasing Cloudera Navigator integration, contact BigConnect Pipelines.

When upgrading from a BigConnect Pipelines version with Cloudera Navigator integration enabled to version 3.0.0.0 without a paid subscription, perform the following post-upgrade task:

Do not include the Cloudera Navigator properties when you configure the 3.0.0.0 BigConnect Pipelines configuration file, sdc.properties. The properties to omit are:

* lineage.publishers
* lineage.publisher.navigator.def
* All other properties with the lineage.publisher.navigator prefix

### JDBC Multitable Consumer Query Interval Change <a href="#ariaid-title27" id="ariaid-title27"></a>

Starting with version 3.0.0.0, the Query Interval property is replaced by the new Queries per Second property.

Upgraded pipelines with the Query Interval specified using a constant or the default format and unit of time, ${10 \* SECONDS}, have the new Queries per Second property calculated and defined as follows:

```
Queries per Second = Number of Threads / Query Interval (in seconds)
```

For example, say the origin uses three threads and Query Interval is configured for ${15 \* SECONDS}. Then, the upgraded origin sets Queries per Seconds to 3 divided by 15, which is .2. This means the origin will run a maximum of two queries every 10 seconds.

The upgrade would occur the same way if Query Interval were set to 15.

Pipelines with a Query Interval configured to use other units of time, such as ${.1 \*MINUTES}, or configured with a different expression format, such as ${SECONDS \* 5}, are upgraded to use the default for Queries per Second, which is 10. This means the pipeline will run a maximum of 10 queries per second. The fact that these expressions are not upgraded correctly is noted in the BigConnect Pipelines log.

If necessary, update the Queries per Second property as needed after the upgrade.

### Update JDBC Query Consumer Pipelines used for SQL Server CDC Data <a href="#ariaid-title28" id="ariaid-title28"></a>

Starting with version 3.0.0.0, the Microsoft SQL Server CDC functionality in the JDBC Query Consumer origin has been deprecated and will be removed in a future release.

For pipelines that use the JDBC Query Consumer to process Microsoft SQL Server CDC data, replace the JDBC Query Consumer origin with another origin:

* To read data from Microsoft SQL Server CDC tables, use the SQL Server CDC Client origin.
* To read data from Microsoft SQL Server change tracking tables, use the SQL Server Change Tracking origin.

### Update MongoDB Destination Upsert Pipelines <a href="#ariaid-title29" id="ariaid-title29"></a>

Starting with version 3.0.0.0, the MongoDB destination supports the replace and update operation codes, and no longer supports the upsert operation code. You can use a new Upsert flag in conjunction with Replace and Update.

After upgrading from a version earlier than 3.0.0.0, update the pipeline as needed to ensure that records passed to the destination do not use the upsert operation code (sdc.operation.type = 4). Records that use the upsert operation code will be sent to error.

In previous releases, records flagged for upsert were treated in the MongoDB system as Replace operations with the Upsert flag set.

If you want to replicate the upsert behavior from earlier releases, perform the following steps:

1.  Configure the pipeline to use the Replace operation code.

    Make sure that the sdc.operation.type is set to 7 for Replace instead of 4 for Upsert.
2. In the MongoDB destination, enable the new Upsert property.

### Time Zones in Stages <a href="#ariaid-title30" id="ariaid-title30"></a>

Starting with version 3.0.0.0, time zones have been organized and updated to use JDK 8 names. This should make it easier to select time zones in stage properties.

In the rare case that an upgraded pipeline uses a format not supported by JDK 8, edit the pipeline to select a compatible time zone.

### Update Kudu Pipelines <a href="#ariaid-title31" id="ariaid-title31"></a>

Consider the following upgrade tasks for Kudu pipelines, based on the version that you are upgrading from:

**Upgrade from versions earlier than 3.0.0.0**\
\
Starting with version 3.0.0.0, if the destination receives a change data capture log from the following source systems, you must specify the source system so that the destination can determine the format of the log: Microsoft SQL Server, Oracle CDC Client, MySQL Binary Log, or MongoDB Oplog.\
\
Previously, the Kudu destination could not directly receive changed data from these source systems. You either had to include a scripting processor in the pipeline to modify the field paths in the record to a format that the destination could read. Or, you had to add multiple Kudu destinations to the pipeline - one per operation type - and include a Stream Selector processor to send records to the destination by operation type.\
\
If you implemented one of these workarounds, then after upgrading, modify the pipeline to remove the scripting processor or the Stream Selector processor and the multiple destinations. In the Kudu destination, set the Change Log Format to the appropriate format of the log: Microsoft SQL Server, Oracle CDC Client, MySQL Binary Log, or MongoDB Oplog.\
\
**Upgrade from versions earlier than 2.2.0.0**\
\
Starting with version 2.2.0.0, BigConnect Pipelines provides support for Apache Kudu version 1.0.x and no longer supports earlier Kudu versions. To upgrade pipelines that contain a Kudu destination from BigConnect Pipelines versions earlier than 2.2.0.0, upgrade your Kudu cluster and then add a stage alias for the earlier Kudu version to the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`.

The configuration file includes stage aliases to enable backward compatibility for pipelines created with earlier versions of BigConnect Pipelines.

To update Kudu pipelines:

1.  Upgrade your Kudu cluster to version 1.0.x.

    For instructions, see the Apache Kudu documentation.
2.  Open the `$SDC_CONF/sdc.properties` file and locate the following comment:

    ```
    # Stage aliases for mapping to keep backward compatibility on pipelines when stages move libraries
    ```
3.  Below the comment, add a stage alias for the earlier Kudu version as follows:

    ```
    stage.alias.streamsets-datacollector-apache-kudu-<version>-lib, com_streamsets_pipeline_stage_destination_kudu_KuduDTarget = streamsets-datacollector-apache-kudu_1_0-lib, com_streamsets_pipeline_stage_destination_kudu_KuduDTarget
    ```

    Where \<version> is the earlier Kudu version: 0\_7, 0\_8, or 0\_9. For example, if you previously used Kudu version 0.9, add the following stage alias:

    ```
    stage.alias.streamsets-datacollector-apache-kudu-0_9-lib, com_streamsets_pipeline_stage_destination_kudu_KuduDTarget = streamsets-datacollector-apache-kudu_1_0-lib, com_streamsets_pipeline_stage_destination_kudu_KuduDTarget
    ```
4. Restart BigConnect Pipelines to enable the changes.

### Update JDBC Multitable Consumer Pipelines <a href="#ariaid-title32" id="ariaid-title32"></a>

Starting with version 2.7.1.1, the JDBC Multitable Consumer origin can now read from views in addition to tables. The origin now reads from all tables and all views that are included in the defined table configurations.

When upgrading pipelines that contain a JDBC Multitable Consumer origin from BigConnect Pipelines versions earlier than 2.7.1.1, review the table configurations to determine if any views are included. If a table configuration includes views that you do not want to read, simply exclude them from the configuration.

### Update Vault Pipelines <a href="#ariaid-title33" id="ariaid-title33"></a>

Starting with version 2.7.0.0, BigConnect Pipelines introduces a credential store API and credential expression language functions to access Hashicorp Vault secrets.

In addition, the BigConnect Pipelines Vault integration now relies on Vault's App Role authentication backend.

Previously, BigConnect Pipelines used Vault functions to access Vault secrets and relied on Vault's App ID authentication backend. BigConnect Pipelines has deprecated the Vault functions, and Hashicorp has deprecated the App ID authentication backend.

After upgrading, update pipelines that use Vault functions in one of the following ways:

**Use the new credential store expression language functions (recommended)**\
\
To use the new credential functions, install the Vault credential store stage library and define the configuration properties used to connect to Vault. Then, update each upgraded pipeline that includes stages using Vault functions to use the new credential functions to retrieve the credential values.\
\
For details on using the Vault credential store system, see Hashicorp Vault.\
\
**Continue to use the deprecated Vault functions**\
\
You can continue to use the deprecated Vault functions in pipelines. However, the functions will be removed in a future release - so we recommend that you use the credential functions as soon as possible.\
\
To continue to use the Vault functions, make the following changes after upgrading:

* Uncomment the single Vault EL property in the `$SDC_CONF/vault.properties` file.
* The remaining Vault configuration properties have been moved to the $SDC\_CONF/credential-stores.properties file. The properties use the same name, with an added "credentialStore.vault.config" prefix. Copy any values that you customized in the previous `vault.properties` file into the same property names in the `credential-stores.properties` file.
* Define the Vault Role ID and Secret ID that BigConnect Pipelines uses to authenticate with Vault in the `credential-stores.properties` file. Defining an App ID for BigConnect Pipelines is deprecated and will be removed in a future release.

For details on using the Vault functions, see Accessing Hashicorp Vault Secrets with Vault Functions (Deprecated).

### Configure JDBC Producer Schema Names <a href="#ariaid-title34" id="ariaid-title34"></a>

Starting with BigConnect Pipelines version 2.5.0.0, you can use a Schema Name property to specify the database or schema name. In previous releases, you specified the database or schema name in the Table Name property.

Upgrading from a previous release does not require changing any existing configuration at this time. But we recommend using the new Schema Name property, since the ability to specify a database or schema name with the table name might be deprecated in the future.

### Evaluate Precondition Error Handling <a href="#ariaid-title35" id="ariaid-title35"></a>

Starting with BigConnect Pipelines version 2.5.0.0, precondition error handling has changed.

The Precondition stage property allows you to define conditions that must be met for a record to enter the stage. Previously, records that did not meet all specified preconditions were passed to the pipeline for error handling. That is, the records were processed based on the Error Records pipeline property.

With version 2.5.0.0, records that do not meet the specified preconditions are handled by the error handling configured for the stage. Stage error handling occurs based on the On Record Error property on the General tab of the stage.

Review pipelines that use preconditions to verify that this change does not adversely affect the behavior of the pipelines.

### Authentication for Docker Image <a href="#ariaid-title36" id="ariaid-title36"></a>

Starting with BigConnect Pipelines version 2.4.1.0, the Docker image now uses the form type of file-based authentication by default. As a result, you must use a BigConnect Pipelines user account to log in to the BigConnect Pipelines. If you haven't set up custom user accounts, you can use the admin account shipped with the BigConnect Pipelines. The default login is: `admin / admin`.

Earlier versions of the Docker image used no authentication.

### Configure Pipeline Permissions <a href="#ariaid-title37" id="ariaid-title37"></a>

BigConnect Pipelines version 2.4.0.0 is designed for multitenancy and enables you to share and grant permissions on pipelines. Permissions determine the access level that users and groups have on pipelines.

In earlier versions of BigConnect Pipelines without pipeline permissions, pipeline access is determined by roles. For example, any user with the Creator role could edit any pipeline.

In version 2.4.0.0, roles are augmented with pipeline permissions. In addition to having the necessary role, users must also have the appropriate permissions to perform pipeline tasks.

For example, to edit a pipeline in 2.4.0.0, a user with the Creator role must also have read and write permission on the pipeline. Without write permission, the user cannot edit the pipeline. Without read permission, the user cannot see the pipeline at all. It does not display in the list of available pipelines.

**Note**: With pipeline permissions enabled, all upgraded pipelines are initially visible only to users with the Admin role and the pipeline owner - the user who created the pipeline. To enable other users to work with pipelines, have an Admin user configure the appropriate permissions for each pipeline.

In BigConnect Pipelines version 2.5.0.0, pipeline permissions are disabled by default. To enable pipeline permissions, set the pipeline.access.control.enabled property to true in the BigConnect Pipelines configuration file.

<mark style="color:green;">**Tip**</mark>: You can configure pipeline permissions when permissions are disabled. Then, you can enable the pipeline permissions property after pipeline permissions are properly configured.

For more information about roles and permissions, see Roles and Permissions. For details about configuring pipeline permissions, see Sharing Pipelines.

### Update Elasticsearch Pipelines <a href="#ariaid-title38" id="ariaid-title38"></a>

BigConnect Pipelines version 2.3.0.0 includes an enhanced Elasticsearch destination that uses the Elasticsearch HTTP API. To upgrade pipelines that use the Elasticsearch destination from BigConnect Pipelines versions earlier than 2.3.0.0, you must review the value of the Default Operation property.

Review all upgraded Elasticsearch destinations to ensure that the **Default Operation** property is set to the correct operation. Upgraded Elasticsearch destinations have the Default Operation property set based on the configuration for the Enable Upsert property:

* With upsert enabled, the default operation is set to INDEX.
* With upsert not enabled, the default operation is set to CREATE which requires a DocumentId.

**Note**: The Elasticsearch version 5 stage library is compatible with all versions of Elasticsearch. Earlier stage library versions have been removed.
