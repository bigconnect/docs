# Pre Upgrade Tasks

In some situations, you must complete tasks before you upgrade.

### Verify Installation Requirements <a href="#ariaid-title2" id="ariaid-title2"></a>

The minimum requirements for BigConnect Pipelines can change with each version. Before you upgrade to a new BigConnect Pipelines, verify that the machine meets the latest minimum requirements as described in Installation Requirements.

### Register and Link with BigConnect Pipelines <a href="#ariaid-title3" id="ariaid-title3"></a>

Before you upgrade to version 3.19.0 or later, users without an enterprise account must create an account with BigConnect Pipelines Accounts. Then, you can download the latest BigConnect Pipelines common installation tarball or run the latest BigConnect Pipelines Docker image.

After you perform the upgrade, link BigConnect Pipelines to your account. BigConnect Pipelines Accounts then provides single sign-on authentication for BigConnect Pipelines. You can change the authentication method as needed.

### Upgrading Full and Core Tarball Installations <a href="#ariaid-title4" id="ariaid-title4"></a>

Starting with version 3.19.0, the full and core tarball installation methods are available only to users with an enterprise account.

If you do not have an enterprise account, when you upgrade from a full or core installation to version 3.19.0 or later, you download the common installation tarball from BigConnect Pipelines Accounts.

After you perform the upgrade, you can install or remove stage libraries as needed.

If you have an enterprise account, you can download the full or core installation packages from the BigConnect Pipelines Support portal.

### Upgrade to Spark 2.1 or Later <a href="#ariaid-title5" id="ariaid-title5"></a>

BigConnect Pipelines version 3.3.0 introduces cluster streaming mode with support for Kafka security features such as SSL/TLS and Kerberos authentication using Spark 2.1 or later and Kafka 0.10.0.0 or later.

However, this means that using Spark 1.x for cluster streaming mode, the Spark Evaluator processor, and the Spark executor was deprecated as of version 3.2.0.0. Support for Spark 1.x is removed in version 3.3.0. If you are using cluster streaming mode, the Spark Evaluator processor, or the Spark executor, you must upgrade to Spark 2.1 or later. In addition, if you are using cluster streaming mode for Kafka, you must also upgrade to Kafka 0.10.0.0 or later.\
\
**Note**: You can continue to use Kafka 0.9.0.0 in standalone pipelines. Or you can continue to use an earlier version of BigConnect Pipelines to use Kafka 0.9.0.0 in cluster pipelines until you can upgrade Kafka.Since Spark 1.x is no longer supported and since Kafka 0.9.0.0 is no longer supported in cluster pipelines, the following stage libraries have changed:

| Category                       | Stage Libraries                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| New stage libraries            | <p>The following new stage libraries include the Kafka Consumer origin for cluster mode pipelines:</p><ul><li>streamsets-datacollector-cdh-spark_2_1-lib</li><li>streamsets-datacollector-cdh-spark_2_2-lib</li><li>streamsets-datacollector-cdh-spark_2_3-lib</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Changed stage libraries        | <p>The following stage library no longer includes the Kafka Consumer origin for cluster mode pipelines:</p><ul><li>streamsets-datacollector-hdp_2_4-lib</li></ul><p>The following stage libraries were upgraded to use Spark 2.1:</p><ul><li>streamsets-datacollector-hdp_2_6-lib</li><li>streamsets-datacollector-mapr_5_2-lib</li><li>streamsets-datacollector-mapr_6_0-mep4-lib</li></ul>                                                                                                                                                                                                                                                                                                             |
| Removed stage libraries        | <p>The following stage libraries are removed:</p><ul><li>streamsets-datacollector-cdh_5_8-cluster-cdh_kafka_2_0-lib</li><li>streamsets-datacollector-cdh_5_9-cluster-cdh_kafka_2_0-lib</li><li>streamsets-datacollector-cdh_5_10-cluster-cdh_kafka_2_1-lib</li><li>streamsets-datacollector-cdh_5_11-cluster-cdh_kafka_2_1-lib</li><li>streamsets-datacollector-cdh_5_12-cluster-cdh_kafka_2_1-lib</li><li>streamsets-datacollector-cdh_5_13-cluster-cdh_kafka_2_1-lib</li><li>streamsets-datacollector-cdh_5_14-cluster-cdh_kafka_2_1-lib</li></ul><p>During the upgrade process, these removed stage libraries are replaced with the new streamsets-datacollector-cdh-spark_2_1-lib stage library.</p> |
| Removed legacy stage libraries | <p>The following legacy stage libraries are removed:</p><ul><li>streamsets-datacollector-cdh_5_4-cluster-cdh_kafka_1_2-lib</li><li>streamsets-datacollector-cdh_5_4-cluster-cdh_kafka_1_3-lib</li><li>streamsets-datacollector-cdh_5_5-cluster-cdh_kafka_1_3-lib</li><li>streamsets-datacollector-cdh_5_7-cluster-cdh_kafka_2_0-lib</li></ul>                                                                                                                                                                                                                                                                                                                                                            |
| Changed legacy stage libraries | <p>The following legacy stage libraries no longer include the Spark Evaluator processor:</p><ul><li>streamsets-datacollector-cdh_5_4-lib</li><li>streamsets-datacollector-cdh_5_5-lib</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

To continue to use cluster streaming mode, you must upgrade to a newer Cloudera CDH or Hortonworks Hadoop distribution and to Kafka 0.10.0.0 or later. The major Hadoop distribution vendors provide a means for Spark 1.x and Spark 2.x to coexist on the same cluster, so you can use both versions in your clusters. BigConnect Pipelines supports the following Spark 2.x versions for the Hadoop distribution vendors:

* **Cloudera** - Cloudera Distribution of Spark 2.1 release 1 or later is supported. For more information, see [Spark 2 Requirements](https://www.cloudera.com/documentation/spark2/latest/topics/spark2\_requirements.html).
* **Hortonworks** - Hortonworks Data Platform (HDP) 2.6 or later includes Spark 2.2.0. For more information, see the [HDP 2.6 Release Notes](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.4/bk\_release-notes/content/comp\_versions.html).
* **MapR** - MapR with MapR Expansion Pack 3.0 or later is supported. For more information, see [MEP Support by MapR Core Version](https://maprdocs.mapr.com/home/InteropMatrix/r\_mep\_support\_core\_version.html#reference\_jqk\_b2m\_zz).

Then, you must configure upgraded pipelines to work with the upgraded system, as described in Working with Upgraded External Systems.

In addition to selecting the upgraded stage library version for each stage that connects to the upgraded CDH, HDP, or Kafka system, you might need to perform additional tasks for the following stages:

* **Spark Evaluator processor** - If the Spark application was previously built with Spark 2.0 or earlier, you must rebuild it with Spark 2.1. Or if you used Scala to write the custom Spark class, and the application was compiled with Scala 2.10, you must recompile it with Scala 2.11.
* **Spark executor** - If the Spark application was previously built with Spark 2.0 or earlier, you must rebuild it with Spark 2.1 and Scala 2.11.

### Migrate to Java 8 <a href="#ariaid-title6" id="ariaid-title6"></a>

BigConnect Pipelines version 2.5.0.0 requires Java 8. If your previous BigConnect Pipelines version ran on Java 7, you must migrate to Java 8 before upgrading to the latest BigConnect Pipelines version.

All services that use BigConnect Pipelines JAR files also must run on Java 8. This means that your Hadoop cluster must run on Java 8 if you are using cluster pipelines, the Spark Executor, or the MapReduce Executor.

To migrate to Java 8, complete the following steps before upgrading to the latest BigConnect Pipelines version:

1. Shut down BigConnect Pipelines.
2. Install Java 8 on the BigConnect Pipelines machine.
3. If you customized Java configuration options in the SDC\_JAVA7\_OPTS environment variable and if those options are valid in Java 8, migrate those customizations to the SDC\_JAVA8\_OPTS environment variable.
4. Restart BigConnect Pipelines and verify that it works as expected.
5. If any pipelines include the JavaScript Evaluator processor, open the pipelines and validate the scripts on Java 8.

### Upgrade Cluster Streaming Pipelines <a href="#ariaid-title7" id="ariaid-title7"></a>

If you use cluster pipelines that run in cluster streaming mode and you are upgrading from a version earlier than 2.3.0.0, you must upgrade to BigConnect Pipelines version 2.3.0.0 before upgrading to the latest version.

Prior to 2.3.0.0, BigConnect Pipelines used the Spark checkpoint mechanism to recover cluster pipelines after a failure. Starting in version 2.3.0.0, BigConnect Pipelines maintains the state of cluster pipelines without relying on Spark checkpoints.\
\
<mark style="color:red;">**Warning**</mark>: If you upgrade from a version earlier than 2.3.0.0 directly to the latest version - without first upgrading to version 2.3.0.0 - cluster pipelines fail when starting.\
\
Before you upgrade to the latest version, complete the following general tasks:

1. Upgrade to BigConnect Pipelines version 2.3.0.0.
2. Start the upgraded BigConnect Pipelines version 2.3.0.0 and run the cluster pipelines so that they process some data.

After verifying that the upgrade to BigConnect Pipelines version 2.3.0.0 was successful, upgrade to the latest version.
