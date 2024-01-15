# Pipeline Statistics

When you monitor an active job in Control Hub, you can view real-time statistics and metrics about the running pipeline. However, when the job stops, the real-time statistics are no longer visible.

To monitor statistics and metrics for inactive jobs and for previous job runs, you must configure the pipeline to write statistics to Control Hub or to another system. When a pipeline is configured to write statistics, Control Hub saves the pipeline statistics for each job run.

When a job for an edge pipeline runs on SDC Edge or when a job for a standalone pipeline runs on a single BigConnect Pipelines, you can configure the pipeline to write the statistics directly to Control Hub.

When a job for a standalone pipeline runs on multiple BigConnect Pipelines, a remote pipeline instance runs on each BigConnect Pipelines. When a job for a cluster pipeline runs on a single BigConnect Pipelines, remote pipeline instances run on multiple worker nodes in the cluster. To view aggregated statistics for these jobs within Control Hub, you must configure the pipeline to write the statistics to one of the following systems:

* SDC RPC
* Kafka cluster
* Amazon Kinesis Streams
* MapR Streams

When you start a job that includes a pipeline configured to write to Kafka, Kinesis, MapR Streams, or SDC RPC, Control Hub automatically generates and runs a system pipeline for the job. The system pipeline reads the statistics written by each running pipeline instance to Kafka, Kinesis, MapR Streams, or SDC RPC. Then, the system pipeline aggregates and sends the statistics to Control Hub.

{% hint style="info" %}
**Important**: For standalone and cluster pipelines in a production environment, use a Kafka cluster, Amazon Kinesis Streams, or MapR Streams to aggregate statistics. Using SDC RPC to aggregate statistics is not highly available and might cause the loss of some data. It should be used for development purposes only.
{% endhint %}

### Pipeline Execution Mode <a href="#ariaid-title2" id="ariaid-title2"></a>

Pipelines can run in standalone, cluster, or edge execution mode. Some pipeline execution modes do not support all statistics aggregator options.

The following table lists each pipeline execution mode and the statistics aggregator options that it supports:

| Pipeline Execution Mode | Supported Statistics Aggregator Options                                                                                  |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Standalone              | All options are supported.                                                                                               |
| Cluster                 | <ul><li>Write to SDC RPC</li><li>Write to Kafka</li><li>Write to Kinesis Streams</li><li>Write to MapR Streams</li></ul> |
| Edge                    | <ul><li>Write to Directly to Control Hub</li></ul>                                                                       |

{% hint style="info" %}
**Important**: For standalone and cluster pipelines in a production environment, use a Kafka cluster, Amazon Kinesis Streams, or MapR Streams to aggregate statistics. Writing statistics directly to Control Hub is supported only when a job with a standalone pipeline runs on a single BigConnect Pipelines. Writing statistics to SDC RPC is not highly available and might cause the loss of some data. These methods should be used for development purposes only.
{% endhint %}



### Write Statistics Directly to Control Hub <a href="#ariaid-title3" id="ariaid-title3"></a>

When you write statistics directly to Control Hub, Control Hub does not generate a system pipeline for the job. Instead, the BigConnect Pipelines or SDC Edge directly sends the statistics to Control Hub.

Write statistics directly to Control Hub in a development environment when the job for a standalone or edge pipeline runs on a single BigConnect Pipelines or SDC Edge. If the job runs on multiple BigConnect Pipelines or Edge BigConnect Pipelines, Control Hub can display the pipeline statistics for each individual pipeline. However, Control Hub cannot display an aggregated view of the statistics across all running pipeline instances.

When you write statistics directly to Control Hub, Control Hub cannot generate data delivery reports for the job or trigger data SLA alerts for the job.

### Write Statistics to SDC RPC <a href="#ariaid-title4" id="ariaid-title4"></a>

When you write statistics to SDC RPC, BigConnect Pipelines effectively adds an SDC RPC destination to the pipeline that you are configuring. Control Hub automatically generates and runs a system pipeline for the job. The system pipeline is a pipeline with a Dev SDC RPC with Buffering origin that reads the statistics passed from the SDC RPC destination, and then aggregates and sends the statistics to Control Hub.

<img src="https://docs.streamsets.com/datacollector/3.22.x/help/reusable-content/shared-graphics/icon-Edge.png" alt="" data-size="line"> Not valid in BigConnect Pipelines Edge pipelines.

{% hint style="info" %}
**Important**: For standalone and cluster pipelines in a production environment, use a Kafka cluster, Amazon Kinesis Streams, or MapR Streams to aggregate statistics. Using SDC RPC to aggregate statistics is not highly available and might cause the loss of some data. It should be used for development purposes only.
{% endhint %}

\
When you configure a pipeline to write statistics to an SDC RPC destination, you specify the following information:

*   SDC RPC connection - The host and port number of the BigConnect Pipelines machine where Control Hub starts the system pipeline. The host must be a BigConnect Pipelines machine registered with Control Hub that can run a pipeline for the job. A BigConnect Pipelines can run the pipeline when it has all labels associated with the job.

    For example, if you associate the job with the WestCoast label, then the host specified in the RPC connection must be a machine with a registered BigConnect Pipelines that also has the WestCoast label.
* SDC RPC ID - A user-defined identifier that allows SDC RPC stages to recognize each other. To avoid mixing statistics from different jobs, use a unique ID for each job.

You can optionally enable encryption to pass data securely and define retry and timeout properties.

For more information about SDC RPC pipelines, see SDC RPC Pipeline Overview.

#### Best Practices for SDC RPC <a href="#ariaid-title5" id="ariaid-title5"></a>

Consider the following best practices when you configure a pipeline to write statistics to an SDC RPC destination:

* To avoid mixing statistics from different jobs, use a unique SDC RPC ID for each job.
*   Monitor the disk space where the Dev SDC RPC with Buffering origin in the system pipeline temporarily buffers the records to disk before passing the records to the next stage in the pipeline.

    The Dev SDC RPC with Buffering origin in the system pipeline temporarily buffers the statistics to a queue on disk. If the system pipeline slows, the temporary location on disk might become full. The temporary statistics are written to the location specified in the `java.io.tmpdir` system property, to a file with the following name:

    ```
    sdc-fragments<file ID>.queueFile
    ```

### Write Statistics to Kafka <a href="#ariaid-title6" id="ariaid-title6"></a>

When you write statistics to a Kafka cluster, BigConnect Pipelines effectively adds a Kafka Producer destination to the pipeline that you are configuring. Control Hub automatically generates and runs a system pipeline for the job. The system pipeline reads the statistics from Kafka, and then aggregates and sends the statistics to Control Hub.

<img src="https://docs.streamsets.com/datacollector/3.22.x/help/reusable-content/shared-graphics/icon-Edge.png" alt="" data-size="line"> Not valid in BigConnect Pipelines Edge pipelines.

When you write statistics to a Kafka cluster, you define connection information and the topic to write to.

You also configure the partition strategy. The pipeline passes data to partitions in the Kafka topic based on the partition strategy that you choose. You can add additional Kafka configuration properties as needed. You can also configure the pipeline to connect securely to Kafka through SSL/TLS or Kerberos.

#### Partition Strategy <a href="#ariaid-title7" id="ariaid-title7"></a>

The partition strategy determines how to write statistics to Kafka partitions. You can use a partition strategy to balance the work load or to write data semantically.

The pipeline can use one of the following partition strategies:

**Round-Robin**\
\
Writes statistics to a different partition using a cyclical order. Use for load balancing.\
\
**Random**\
\
Writes statistics to a different partition using a random order. Use for load balancing.\
\
**Expression**\
\
Writes statistics to a partition based on the results of the partition expression. Use to perform semantic partitioning.\
\
When you configure the partition expression, define the expression to evaluate to the partition where you want statistics written.\
\
**Default**\
\
Writes statistics using the default partition strategy that Kafka provides.

#### Best Practices for a Kafka Cluster <a href="#ariaid-title8" id="ariaid-title8"></a>

Consider the following best practices when you configure a pipeline to write statistics to a Kafka cluster:

* To avoid mixing statistics from different jobs, use a unique topic name for each job.
*   Consider the Kafka retention policy.\


    Each running pipeline instance writes statistics to Kafka, and then the system pipeline consumes the statistics from Kafka. If the system pipeline unexpectedly shuts down, Kafka retains the statistics for the amount of time determined by the Kafka retention policy. If the system pipeline is down for longer than Kafka retains data, the statistics are lost.

### Write Statistics to Kinesis Streams <a href="#ariaid-title9" id="ariaid-title9"></a>

When you write statistics to Amazon Kinesis Streams, BigConnect Pipelines effectively adds a Kinesis Producer destination to the pipeline that you are configuring. Control Hub automatically generates and runs a system pipeline for the job. The system pipeline reads the statistics from Kinesis Streams, and then aggregates and sends the statistics to Control Hub.

<img src="https://docs.streamsets.com/datacollector/3.22.x/help/reusable-content/shared-graphics/icon-Edge.png" alt="" data-size="line"> Not valid in BigConnect Pipelines Edge pipelines.

When you write statistics to Kinesis Streams, you define connection information and the stream to write to.

You also configure the partition strategy. The pipeline passes data to partitions in Kinesis shards based on the partition strategy that you choose. You can add additional Kinesis configuration properties as needed.

#### Authentication Method <a href="#ariaid-title10" id="ariaid-title10"></a>

When a pipeline writes aggregated statistics to Amazon Kinesis Streams, you can configure the pipeline to authenticate with Amazon Web Services (AWS) using an instance profile or AWS access keys.

For more information about the authentication methods and details on how to configure each method, see Security in Amazon Stages.

#### Best Practices for Kinesis Streams <a href="#ariaid-title11" id="ariaid-title11"></a>

Consider the following best practices when you configure a pipeline to write statistics to Amazon Kinesis Streams:

* To avoid mixing statistics from different jobs, use a unique stream name for each job.
*   Consider the Kinesis Streams retention policy.

    Each running pipeline instance writes statistics to Kinesis Streams, and then the system pipeline reads the statistics from Kinesis Streams. If the system pipeline unexpectedly shuts down, Kinesis Streams retains the statistics for the amount of time determined by the Kinesis Streams retention policy. If the system pipeline is down for longer than Kinesis Streams retains data, the statistics are lost.

### Write Statistics to MapR Streams <a href="#ariaid-title12" id="ariaid-title12"></a>

When you write statistics to MapR Streams, BigConnect Pipelines effectively adds a MapR Streams Producer destination to the pipeline that you are configuring. Control Hub automatically generates and runs a system pipeline for the job. The system pipeline reads the statistics from MapR Streams, and then aggregates and sends the statistics to Control Hub.

<img src="https://docs.streamsets.com/datacollector/3.22.x/help/reusable-content/shared-graphics/icon-Edge.png" alt="" data-size="line"> Not valid in BigConnect Pipelines Edge pipelines.

When you write statistics to MapR Streams, you define the topic to write to. You also configure the partition strategy. The pipeline passes data to partitions in the MapR Streams topic based on the partition strategy that you choose. You can add additional MapR Streams configuration properties as needed.

Before you can write statistics to MapR Streams, you must perform additional steps to enable BigConnect Pipelines to process MapR data. For more information, see MapR Prerequisites.

#### Partition Strategy <a href="#ariaid-title13" id="ariaid-title13"></a>

The partition strategy determines how to write statistics to MapR Streams partitions. You can use a partition strategy to balance the work load or to write data semantically.

The pipeline can use one of the following partition strategies:

**Round-Robin**\
\
Writes each record to a different partition using a cyclical order. Use for load balancing.\
\
**Random**\
\
Writes each record to a different partition using a random order. Use for load balancing.\
\
**Expression**\
\
Writes each record to a partition based on the results of the partition expression. Use to perform semantic partitioning.\
\
When you configure the partition expression, define the expression to evaluate to the partition where you want each record written. The expression must return a numeric value.\
\
**Default**\
\
Writes each record using the default partition strategy that MapR Streams provides.

#### Best Practices for MapR Streams <a href="#ariaid-title14" id="ariaid-title14"></a>

Consider the following best practices when you configure a pipeline to write statistics to MapR Streams:

* To avoid mixing statistics from different jobs, use a unique topic name for each job.
*   Consider the MapR Streams retention policy.

    Each running pipeline instance writes statistics to MapR Streams, and then the system pipeline consumes the statistics from MapR Streams. If the system pipeline unexpectedly shuts down, MapR Streams retains the statistics for the amount of time determined by the MapR Streams retention policy. If the system pipeline is down for longer than MapR Streams retains data, the statistics are lost.

### Configuring a Pipeline to Write Statistics <a href="#ariaid-title15" id="ariaid-title15"></a>

You can configure a pipeline to write statistics after BigConnect Pipelines has been registered with Control Hub. Configure a pipeline to write statistics when you want to monitor statistics and metrics for inactive jobs and for previous job runs.

1. Open the pipeline.
2. On the Statistics tab, select the statistics aggregator to use:
   * Discard - Discard the pipeline statistics. Control Hub does not save the statistics. Real-time statistics are only visible when the job is active.
   *   Write Directly to Control Hub - Write the pipeline statistics directly to Control Hub. Use for edge pipelines that run on SDC Edge. Or use for development purposes when a job with a standalone pipeline runs on a single BigConnect Pipelines.

       <img src="https://docs.streamsets.com/datacollector/3.22.x/help/reusable-content/shared-graphics/icon-Edge.png" alt="" data-size="line"> In BigConnect Pipelines Edge pipelines, you must use this option. The other options are not valid.
   * Write to SDC RPC - Write the pipeline statistics to an SDC RPC destination. Use for development purposes only.
   * Write to Kafka - Write the pipeline statistics to a Kafka cluster.
   * Write to Kinesis - Write the pipeline statistics to Amazon Kinesis Streams.
   * Write to MapR Streams - Write the pipeline statistics to MapR Streams.
3.  To write statistics to an SDC RPC destination, on the **Stats Aggregator** - **Write to SDC RPC** tab, configure the following properties:\


    | SDC RPC Properties                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | SDC RPC Connection                | <p>Host and port where the system pipeline runs. The host must be a machine with a registered BigConnect Pipelines that runs a pipeline instance for the job.<br></p><p>Use the following format: <code>&#x3C;host>:&#x3C;port></code>.</p>                                                                                                                                                                                                                                                                                  |
    | Retries per Batch                 | <p>Number of times the SDC RPC destination tries to write a batch to the Dev SDC RPC with Buffering origin in the system pipeline.<br></p><p>When the SDC RPC destination cannot write the batch within the configured number of retries, it fails the batch.<br></p><p>Default is 3.</p>                                                                                                                                                                                                                                    |
    | Back off Period                   | <p>Milliseconds to wait before retrying writing a batch to the Dev SDC RPC with Buffering origin in the system pipeline.<br></p><p>The value that you enter increases exponentially after each retry. For example, if you set the back off period to 10, the SDC RPC destination attempts the first retry after waiting 10 milliseconds, attempts the second retry after waiting 100 milliseconds, and attempts the third retry after waiting 1,000 milliseconds. Set to 0 to retry immediately.<br></p><p>Default is 0.</p> |
    | SDC RPC ID                        | <p>User-defined ID to allow the SDC RPC destination to pass statistics to the system pipeline. To avoid mixing statistics from different jobs, use a unique ID for each job.</p><p><br>You cannot define an expression that evaluates to the ID.</p>                                                                                                                                                                                                                                                                         |
    | Connection Timeout (ms)           | <p>Milliseconds to establish a connection to the system pipeline.<br></p><p>The SDC RPC destination retries the connection based on the Retries Per Batch property.<br></p><p>Default is 5000 milliseconds.</p>                                                                                                                                                                                                                                                                                                              |
    | TLS Enabled                       | Enables the secure transfer of data using TLS.                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | Truststore File                   | <p>Truststore file for TLS. Required if the keystore file is a self-signed certificate.<br></p><p>Must be stored in the BigConnect Pipelines resources directory, $SDC_RESOURCES, on each BigConnect Pipelines machine that runs a pipeline instance for the job.</p>                                                                                                                                                                                                                                                        |
    | Truststore Password               | <p>Password for the truststore file.<br><br><strong>Tip</strong>: To secure sensitive information such as user names and passwords, you can use runtime resources or credential stores.</p>                                                                                                                                                                                                                                                                                                                                  |
    | Read Timeout (ms)                 | <p>Milliseconds to wait for the Dev SDC RPC with Buffering origin in the system pipeline to read data from a batch.<br></p><p>The SDC RPC destination retries the write based on the Retries Per Batch property.<br></p><p>Default is 2000 milliseconds.</p>                                                                                                                                                                                                                                                                 |
    | Use Compression                   | Enables the SDC RPC destination to use compression to pass data to the Dev SDC RPC with Buffering origin in the system pipeline. Enabled by default.                                                                                                                                                                                                                                                                                                                                                                         |
    | Verify Host in Server Certificate | Verifies the host in the keystore file on the BigConnect Pipelines machine that runs the system pipeline.                                                                                                                                                                                                                                                                                                                                                                                                                    |
4.  To write statistics to Kafka, on the **Stats Aggregator** - **Write to Kafka tab**, configure the following properties:\


    | Kafka Properties                                                                                                                                             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Broker URI                                                                                                                                                   | <p>Comma-separated list of connection strings for the Kafka brokers. Use the following format for each broker: <code>&#x3C;host>:&#x3C;port></code>.</p><p>T<br>o ensure a pipeline can connect to Kafka in case a specified broker goes down, list as many brokers as possible.</p>                                                                                                                                                                                                                                                                                                                                                                        |
    | Runtime Topic Resolution                                                                                                                                     | Do not use at this time.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Topic                                                                                                                                                        | <p>Topic to use. To avoid mixing statistics from different jobs, use a unique topic name for each job.<br></p><p>You cannot define an expression that evaluates to the topic name.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | [Partition Strategy](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Destinations/KProducer.html#concept\_qpm\_xp4\_4r) | <p>Strategy to use to write to partitions:</p><ul><li>Round Robin - Takes turns writing to different partitions.</li><li>Random - Writes to partitions randomly.</li><li>Expression - Uses an expression to write data to different partitions. Writes records to the partitions specified by the results of the expression.</li><li>Default - Uses an expression to extract a partition key from the record. Writes records to partitions based on a hash of the partition key.</li></ul>                                                                                                                                                                  |
    | Partition Expression                                                                                                                                         | <p>Expression to use when using the expression partition strategy.<br></p><p>Define the expression to evaluate to the partition where you want statistics written. Partition numbers start with 0.<br></p><p>Optionally, click <strong>Ctrl + Space Bar</strong> for help with creating the expression.</p>                                                                                                                                                                                                                                                                                                                                                 |
    | Kafka Configuration                                                                                                                                          | <p>Additional Kafka properties to use. Using simple or bulk edit mode, click the <strong>Add</strong> icon and define the Kafka property name and value.<br></p><p>Use the property names and values as expected by Kafka. Do not use the broker.list property.</p>                                                                                                                                                                                                                                                                                                                                                                                         |
    | ZooKeeper URI                                                                                                                                                | <p>Connection string for the ZooKeeper of the Kafka cluster. Use the following format: <code>&#x3C;host>:&#x3C;port></code>.<br></p><p>To use a ZooKeeper quorum, enter a comma-separated list.<br></p><p>To use a ZooKeeper chroot path, add the path at the end of the list as follows:</p><pre><code>&#x3C;host>:&#x3C;port>, &#x3C;host2>:&#x3C;port2>, .../&#x3C;chroot_path>
    </code></pre>                                                                                                                                                                                                                                                            |
    | Kafka Message Key                                                                                                                                            | <p>Passes message key values stored in a record header attribute to Kafka as message keys.<br></p><p>Enter an expression that specifies the attribute where the message keys are stored.<br></p><p>To pass string message keys stored in an attribute, use:</p><pre><code>${record:attribute('&#x3C;message key attribute name>'}
    </code></pre><p>To pass Avro message keys stored in an attribute, use:</p><pre><code>${avro:decode(record:attribute('avroKeySchema'),base64:decodeBytes(record:attribute('&#x3C;messsage key attribute name')))}
    </code></pre><p>For more information, about working with Kafka message keys, see Kafka Message Keys.</p> |
    | Security Option                                                                                                                                              | <p>Authentication and encryption option used to connect to the Kafka brokers:</p><ul><li>None (Security Protocol=PLAINTEXT) - Uses no authentication or encryption.</li><li>SSL/TLS Encryption (Security Protocol=SSL)</li><li>SSL/TLS Encryption and Authentication (Security Protocol=SSL)</li><li>Kerberos Authentication (Security Protocol=SASL_PLAINTEXT)</li><li>Kerberos Authentication on SSL/TLS (Security Protocol=SASL_SSL)</li></ul><p>Enabling security requires completing several prerequisite tasks and configuring additional security properties, as described in Security in Kafka Stages.</p>                                          |
    | Message Key Format                                                                                                                                           | <p>Data format of the message key values to pass to Kafka. Ignore this property when not passing message key values to Kafka.<br></p><p>For more information about working with Kafka message key values, see Kafka Message Keys.</p>                                                                                                                                                                                                                                                                                                                                                                                                                       |
5.  To write statistics to Amazon Kinesis Streams, on the Stats Aggregator - Write to Kinesis tab, configure the following properties:\


    | Kinesis Properties                                                                                                                                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Authentication Method                                                                                                                                                  | <p>Authentication method used to connect to Amazon Web Services (AWS):</p><ul><li>AWS Keys - Authenticates using an AWS access key pair.</li><li>Instance Profile - Authenticates using an instance profile associated with the BigConnect Pipelines EC2 instance.</li></ul>                                                                                                                                                                                                                                                                                                                 |
    | Access Key ID                                                                                                                                                          | AWS access key ID. Required when using AWS keys to authenticate with AWS.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Secret Access Key                                                                                                                                                      | <p>AWS secret access key. Required when using AWS keys to authenticate with AWS.<br><br><strong>Tip</strong>: To secure sensitive information such as access key pairs, you can use runtime resources or credential stores.</p>                                                                                                                                                                                                                                                                                                                                                              |
    | [Assume Role](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Pipeline\_Configuration/AmazonSecurity.html#concept\_inb\_tdj\_3nb) | Temporarily assumes another role to authenticate with AWS.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Role ARN                                                                                                                                                               | <p>Amazon resource name (ARN) of the role to assume, entered in the following format:</p><p><code>arn:aws:iam::&#x3C;account_id>:role/&#x3C;role_name></code><br></p><p>Where <code>&#x3C;account_id></code> is the ID of your AWS account and <code>&#x3C;role_name></code> is the name of the role to assume. You must create and attach an IAM trust policy to this role that allows the role to be assumed.</p><p><br>Available when assuming another role.</p>                                                                                                                          |
    | Role Session Name                                                                                                                                                      | <p>Optional name for the session created by assuming a role. Overrides the default unique identifier for the session.<br></p><p>Available when assuming another role.</p>                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Session Timeout                                                                                                                                                        | <p>Maximum number of seconds for each session created by assuming a role. The session is refreshed if the pipeline continues to run for longer than this amount of time.</p><p><br>Set to a value between 3,600 seconds and 43,200 seconds.<br></p><p>Available when assuming another role.</p>                                                                                                                                                                                                                                                                                              |
    | Set Session Tags                                                                                                                                                       | <p>Sets a session tag to record the name of the currently logged in BigConnect Pipelines or Transformer user that starts the pipeline or the Control Hub user that starts the job for the pipeline. AWS IAM verifies that the user account set in the session tag can assume the specified role.<br></p><p>Select only when the IAM trust policy attached to the role to be assumed uses session tags and restricts the session tag values to specific user accounts.<br></p><p>When cleared, the connection does not set a session tag.<br></p><p>Available when assuming another role.</p> |
    | Region                                                                                                                                                                 | AWS region to connect to. Select one of the available regions. If the region is not listed, select Other.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Endpoint                                                                                                                                                               | Endpoint to connect to when you select Other for the region. Enter the endpoint name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Stream Name                                                                                                                                                            | <p>Kinesis stream name. To avoid mixing statistics from different jobs, use a unique stream name for each job.<br></p><p>You cannot define an expression that evaluates to the stream name.</p>                                                                                                                                                                                                                                                                                                                                                                                              |
    | Partitioning Strategy                                                                                                                                                  | <p>Strategy to write data to Kinesis shards:</p><ul><li>Random - Generates a random partition key.</li><li>Expression - Uses the result of an expression as the partition key.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Partition Expression                                                                                                                                                   | <p>Expression to generate the partition key used to pass data to different shards.<br></p><p>Use for the expression partition strategy.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Kinesis Producer Configuration                                                                                                                                         | <p>Additional Kinesis properties.<br></p><p>When you add a configuration property, enter the exact property name and the value. The pipeline does not validate the property names or values.</p>                                                                                                                                                                                                                                                                                                                                                                                             |
6.  To write statistics to MapR Streams, on the **Stats Aggregator - Write to MapR Streams** tab, configure the following properties:\


    | MapR Streams Properties    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Runtime Topic Resolution   | Do not use at this time.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Topic                      | <p>Topic to use. To avoid mixing statistics from different jobs, use a unique topic name for each job.<br></p><p>You cannot define an expression that evaluates to the topic name.</p>                                                                                                                                                                                                                                                                                                     |
    | Partition Strategy         | <p>Strategy to use to write to partitions:</p><ul><li>Round Robin - Takes turns writing to different partitions.</li><li>Random - Writes to partitions randomly.</li><li>Expression - Uses an expression to write data to different partitions. Writes records to the partitions specified by the results of the expression.</li><li>Default - Uses an expression to extract a partition key from the record. Writes records to partitions based on a hash of the partition key.</li></ul> |
    | Partition Expression       | <p>Expression to use when using the expression partition strategy.<br></p><p>Define the expression to evaluate to the partition where you want statistics written. Partition numbers start with 0.<br></p><p>Optionally, click Ctrl + Space Bar for help with creating the expression.</p>                                                                                                                                                                                                 |
    | MapR Streams Configuration | <p>Additional configuration properties to use. Using simple or bulk edit mode, click the Add icon and define the MapR Streams property name and value.<br></p><p>Use the property names and values as expected by MapR Streams. You can use MapR Streams properties and the set of Kafka properties supported by MapR Streams</p>                                                                                                                                                          |
