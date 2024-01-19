# MapReduce

The MapReduce executor starts a MapReduce job in HDFS each time it receives an event record. Use the MapReduce executor as part of an event stream.&#x20;

You can use the MapReduce executor to start a custom job, such as a validation job that compares the number of records in files. You can use a custom job by configuring it in the executor or using a predefined configuration object. You can also use the MapReduce executor to start a predefined job. The MapReduce executor includes two predefined jobs: one that converts Avro files to ORC files, and one that converts Avro files to Parquet.

You can use the executor in any logical way, such as running MapReduce jobs after the Hadoop FS or MapR FS destination closes files. For example, you can use the Avro to ORC job to convert Avro files to ORC files after a MapR FS destination closes a file. Or, you might use the Avro to Parquet job to convert Avro files to Parquet after the Hadoop FS destination closes a file as part of the Drift Synchronization Solution for Hive.

Note: The MapReduce executor starts jobs in an external system. It does not monitor jobs or wait for the job to complete. The executor becomes available for additional processing as soon as it successfully submits the job.

When you configure the MapReduce executor, you specify connection information and job details. For predefined jobs, you specify Avro conversion details, such as the input and output file location, as well as ORC- or Parquet-specific details. For other types of jobs, you specify a job creator or configuration object, and the job configuration properties to use.

When necessary, you can enable Kerberos authentication and specify a MapReduce user. You can also use MapReduce configuration files and add other MapReduce configuration properties as needed.

You can also configure the executor to generate events for another event stream. For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

For a solution that describes how to use the MapReduce executor, see Converting Data to the Parquet Data Format.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you run a pipeline that includes MapReduce executor, you must enable MapReduce executor to submit a job.

You can enable MapReduce executor to submit a job in several different ways. Perform one of the following tasks to enable the executor to submit jobs:

Configure the YARN Minimum User ID property, min.user.idThe min.user.id property is set to 1000 by default. To allow job submission:

1. Verify the user ID being used by the Data Pipelines user, typically named "sdc".
2.  In Hadoop, configure the YARN min.user.id property.

    Set the property to equal to or lower than the Data Pipelines user ID.

Configure the YARN Allowed System Users property, allowed.system.usersThe allowed.system.users property lists allowed user names. To allow job submission:

1.  In Hadoop, configure the YARN allowed.system.users property.

    Add the Data Pipelines user name, typically "sdc", to the list of allowed users.

Configure the MapReduce executor MapReduce User propertyIn the MapReduce executor, the MapReduce User property allows you to enter a user name for the stage to use when submitting jobs. To allow job submission:

1.  In the MapReduce executor stage, configure the MapReduce User property.

    Enter a user with an ID that is higher than the min.user.id property, or with a user name that is listed in the allowed.system.users property.

For information about the MapReduce User property, see Using a MapReduce User.

### Related Event Generating Stages <a href="#ariaid-title3" id="ariaid-title3"></a>

Use the MapReduce executor in the event stream of a pipeline. The MapReduce executor is meant to start MapReduce jobs after output files are written.

Use the MapReduce executor to perform post-processing for files written by the following destinations:

* Hadoop FS destination
* MapR FS destination

### MapReduce Jobs and Job Configuration Properties <a href="#ariaid-title4" id="ariaid-title4"></a>

The MapReduce executor can run a custom job that you must configure or a predefined job that is provided by the executor.

When configuring a custom job, you can either specify a job creator and job configuration properties, or use a custom configuration object and specify job configuration properties.

When using a predefined job, you specify the job, Avro conversion details, and job-related properties.

When configuring job configuration properties, you specify key-value pairs. You can use expressions in the key-value pairs.

#### Predefined Jobs for Parquet and ORC <a href="#ariaid-title5" id="ariaid-title5"></a>

The MapReduce executor includes two predefined jobs: Avro to ORC and Avro to Parquet.

The Avro to ORC job converts Avro files to ORC files. The Avro to Parquet job converts Avro files to Parquet. Both jobs process Avro files after they are written. That is, a destination finishes writing an Avro file and generates an event record. The event record contains information about the file, including the name and location of the file. And when the MapReduce executor receives the event record, it starts the selected predefined MapReduce job.

When using a predefined job, you configure input file information and the output directory, whether to keep the input file, and whether to overwrite temporary files.

By default, for the input file, the MapReduce executor uses the file name and location in the "filepath" field of the event record, as follows:

```
${record:value('/filepath')}
```

The executor writes output files in the specified output directory. The executor uses the name of the input file that was processed as the basis for the output file name and adds `.parquet` or `.orc`, depending on the job type.

When using the Avro to ORC job, you specify the ORC batch size on the Avro to ORC tab. To specify additional job information, add job configuration properties on the Job tab. For information about the properties you might want to use, see the [Hive documentation](https://orc.apache.org/docs/hive-config.html).

When using the Avro to Parquet job, you specify job-specific properties on the Avro to Parquet tab. You can specify additional job information by adding job configuration properties on the Job tab.

### Event Generation <a href="#ariaid-title6" id="ariaid-title6"></a>

The MapReduce executor can generate events that you can use in an event stream. When you enable event generation, the executor generates events each time it starts a MapReduce job.

MapReduce executor events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title7" id="ariaid-title7"></a>

Event records generated by the MapReduce executor have the following event-related record header attributes. Record header attributes are stored as String values:

| Record Header Attribute       | Description                                                                                                                                        |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>job-created - Generated when the executor creates and starts a MapReduce job.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                       |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                  |

Event records generated by the MapReduce executor have the following fields:

| Event Field Name | Description                         |
| ---------------- | ----------------------------------- |
| tracking-url     | Tracking URL for the MapReduce job. |
| job-id           | Job ID of the MapReduce job.        |

### Kerberos Authentication <a href="#ariaid-title8" id="ariaid-title8"></a>

You can use Kerberos authentication to connect to Hadoop services such as HDFS or YARN. When you use Kerberos authentication, Data Pipelines uses the Kerberos principal and keytab to authenticate. By default, Data Pipelines uses the user account who started it to connect.

The Kerberos principal and keytab are defined in the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`. To use Kerberos authentication, configure all Kerberos properties in the Data Pipelines configuration file, and then enable Kerberos in the MapReduce executor.

For more information about enabling Kerberos authentication for Data Pipelines, see Kerberos Authentication.

### Using a MapReduce User <a href="#ariaid-title9" id="ariaid-title9"></a>

Data Pipelines can either use the currently logged in Data Pipelines user or a user configured in the executor to submit jobs.

A Data Pipelines configuration property can be set that requires using the currently logged in Data Pipelines user. When this property is not set, you can specify a user in the origin. For more information about Hadoop impersonation and the Data Pipelines property, see Hadoop Impersonation Mode.

Note that the executor uses a different user account to connect. By default, Data Pipelines uses the user account who started it to connect to external systems. When using Kerberos, Data Pipelines uses the Kerberos principal.

To configure a user in the executor, perform the following tasks:

1.  On the external system, configure the user as a proxy user and authorize the user to impersonate the MapReduce user.

    For more information, see the MapReduce documentation.
2. In the MapReduce executor, on the MapReduce tab, configure the MapReduce User property.
