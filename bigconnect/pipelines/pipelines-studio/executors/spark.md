# Spark

The Spark executor starts a Spark application each time it receives an event. You can use the Spark executor with Spark on YARN. The executor is not compatible with Spark on Mesos at this time.&#x20;

Use the Spark executor to start a Spark application as part of an event stream. You can use the executor in any logical way, such as running Spark applications after the Hadoop FS, MapR FS, or Amazon S3 destination closes files. For example, you might use the executor to start a Spark application that converts Avro files to Parquet each time the Hadoop FS destination closes a file.

Note that the Spark executor starts an application in an external system. It does not monitor the application or wait for it to complete. The executor becomes available for additional processing as soon as it successfully submits an application.

The Spark executor can run the application in client or cluster mode. Run the application in client mode only when resource use is not a concern.

Before you use the Spark executor, make sure to perform the prerequisite task.

When you configure the Spark executor, you can specify the number of worker nodes Spark should use, or you can enable dynamic allocation and specify the minimum and maximum number of worker nodes. Dynamic allocation allows Spark to use additional worker nodes as needed, within the specified range.

You can specify additional cluster manager properties to pass to Spark, such as the maximum amount of memory that the application driver and executor can use.

You can also configure additional Spark arguments and environment variables. Any arguments and variables that you enter override any previous definitions, including those in the Spark application, elsewhere in the Spark executor, and the Data Pipelines machine.

You can specify custom Spark and Java home directories, and a Hadoop proxy user. You can also enter Kerberos credentials if needed.

When you configure the application details, you specify the language used to write the application and then define language-specific properties.

You can also configure the executor to generate events for another event stream.&#x20;

### Spark Versions and Stage Libraries <a href="#ariaid-title2" id="ariaid-title2"></a>

The Spark executor supports only Spark version 2.1 or later.

When you use the Spark executor, make sure the Spark version is the same across all related components, as follows:

*   When using the executor to run an application on Spark on YARN, make sure the Spark version used in the selected stage library matches the Spark version used to build the application.

    For example, if you use Spark 2.1 to build the application, use a Spark executor provided in one of the Spark 2.1 stage libraries.
*   When using the executor in a cluster streaming pipeline, the Spark version in the selected stage library must also match the Spark version used by the cluster.

    For example, if your cluster uses Spark 2.2, use a stage library that includes Spark 2.2.

The Spark executor is available in several CDH stage libraries. To verify the Spark version that a stage library includes, see the CDH documentation.&#x20;

### Prerequisite <a href="#ariaid-title3" id="ariaid-title3"></a>

Before you run a Spark executor pipeline that starts applications on YARN, you must enable the Spark executor to submit an application.

You can enable the Spark executor to submit an application in several different ways. Perform one of the following tasks to enable the executor to submit applications:

Configure the YARN Minimum User ID property, min.user.idThe min.user.id property is set to 1000 by default. To allow job submission:

1. Verify the user ID being used by the Data Pipelines user, typically named "sdc".
2.  In Hadoop, configure the YARN min.user.id property.

    Set the property to equal to or lower than the Data Pipelines user ID.

Configure the YARN Allowed System Users property, allowed.system.users

The allowed.system.users property lists allowed user names. To allow job submission:

1.  In Hadoop, configure the YARN allowed.system.users property.

    Add the Data Pipelines user name, typically "sdc", to the list of allowed users.

Configure the Spark executor Proxy User property

In the Spark executor, the Proxy User property allows you to enter a user name for the stage to use when submitting applications. To allow application submission:

1.  In the Spark executor stage, on the Spark tab, configure the Proxy User property.

    Enter a user with an ID that is higher than the min.user.id property, or with a user name that is listed in the allowed.system.users property.

For information about using a Hadoop User, see Using a Proxy Hadoop User.

### Spark Home Requirement <a href="#ariaid-title4" id="ariaid-title4"></a>

When running an application on YARN, the Spark executor requires access to the spark-submit script located in the Spark installation directory.

By default, the Spark executor uses the directory defined in the SPARK\_HOME environment variable on the Data Pipelines machine. The SPARK\_HOME environment variable must be set before you start Data Pipelines.

Note: When Spark 2 is installed on a Cloudera CDH cluster, set the SPARK\_HOME environment variable for Data Pipelines as follows:

```
export SPARK_HOME=/opt/cloudera/parcels/SPARK2/lib/spark2
```

You can override the environment variable as needed by configuring the Custom Spark Home property in the executor stage properties. Use the Custom Spark Home property when the SPARK\_HOME environment variable is not set, or when it points to a conflicting version of Spark.

For example, if you are using a Spark 2.1 stage library for the Spark executor and SPARK\_HOME points to an earlier version of Spark, use the Custom Spark Home property to specify the location of the Spark 2.1 spark-submit script.

### Application Properties <a href="#ariaid-title5" id="ariaid-title5"></a>

When using the Spark executor, you specify an application name. The application name displays in the cluster manager and Spark server logs, so use a distinctive name to enable distinguishing the Spark application from others. For example, SDC\_\<pipeline name>\_\<app\_type>.

In the executor, you can enable verbose logging to help test the pipeline and debug the application.

Configure additional application details based on the language used to write the application:

**Java or Scala**

For applications written in Java or Scala, you specify the main class and application resource - the full path to the primary JAR or file.You can specify additional arguments and JARs to use. You can also pass additional files to the application using the `--files` protocol.

**Python**

For applications written in Python, you specify the application resource - the full path to the primary Python file - and any required dependencies. You can define application arguments and pass additional files to the application using the `--files` protocol.

Note: Make sure the user that runs Data Pipelines - or the Hadoop proxy user, if configured - has read permission on all required paths.

### Using a Proxy Hadoop User <a href="#ariaid-title6" id="ariaid-title6"></a>

You can configure the Spark executor to use a Hadoop user as a proxy user to submit applications to Spark on YARN.

By default, the Data Pipelines uses the user account who started it to connect to external systems. When using Kerberos, the Data Pipelines can use the Kerberos principal specified in the executor.

To use a Hadoop user, perform the following tasks:

1. On the external system, configure the Data Pipelines user as a proxy user and authorize the Data Pipelines user to impersonate the Hadoop user.
2. In the Spark executor, on the Spark tab, configure the Proxy User property to use the Hadoop user name.

### Kerberos Authentication <a href="#ariaid-title7" id="ariaid-title7"></a>

You can use Kerberos authentication to connect to the destination system where output files are written. To enable this, on the Credentials tab of the Spark executor, enter the Kerberos principal and keytab for the YARN cluster where the application runs.

### Event Generation <a href="#ariaid-title8" id="ariaid-title8"></a>

The Spark executor can generate events that you can use in an event stream. When you enable event generation, the executor generates events each time it starts a Spark application.

Spark executor events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

Since Spark executor events include the application ID for each application that it starts, you might generate events to keep a log of the application IDs.

#### Event Records <a href="#ariaid-title9" id="ariaid-title9"></a>

Event records generated by the Spark executor have the following event-related record header attributes. Record header attributes are stored as String values:

| Record Header Attribute       | Description                                                                                                                              |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following type:</p><ul><li>AppSubmittedEvent - Generated when the executor starts a Spark application.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                             |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                        |

Event records generated by the Spark executor have the following fields:

| Event Field Name | Description                                    |
| ---------------- | ---------------------------------------------- |
| app\_id          | YARN application ID for the Spark application. |

### Monitoring <a href="#ariaid-title10" id="ariaid-title10"></a>

Data Pipelines does not monitor Spark applications. Use your regular cluster monitor application to view the status of applications.

Applications started by the Spark executor display using the application name specified in the stage. The application name is the same for all instances of the application. You can find the application ID for a particular instance in the Data Pipelines log.

The Spark executor also writes the application ID to the event record. To keep a record of all application IDs, enable event generation for the stage.
