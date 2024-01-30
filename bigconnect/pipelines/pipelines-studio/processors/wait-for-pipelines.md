# Wait for Pipelines

The Wait for Pipelines processor waits for one or more pipelines to complete.

The Wait for Pipelines processor is an orchestration stage that you use in orchestration pipelines. Orchestration stages perform tasks, such as schedule and start pipelines, that you can use to create an orchestrated workflow.

Use this processor when you want to wait for pipelines that were started upstream to complete, before performing other orchestration tasks. The Wait for Pipelines processor can wait for pipelines that are running on the specified Data Pipelines engine. To wait for pipelines on multiple engines, use additional processors.

The Wait for Pipelines processor checks the status of all pipelines listed in incoming orchestration records that run on the specified execution engine. When the pipelines complete, the processor updates the pipeline status details in the record and passes a single orchestration record downstream.

When you configure the Wait for Pipelines processor, you define the URL of the execution engine that runs the pipelines.  Then, you specify how long to wait between each pipeline status checks.

You also configure the user name and password to run the pipeline and can optionally configure SSL/TLS properties.

### Stage Processing and Pipeline Implementation <a href="#ariaid-title2" id="ariaid-title2"></a>

Use the Wait for Pipelines processor downstream from a Start Pipelines origin or Start Pipelines processor that starts pipelines that run in the background. When running pipelines in the background, a Start Pipelines stage passes its orchestration record downstream immediately after starting pipelines, rather than waiting for them to complete.

When a Wait for Pipelines processor receives an orchestration record, it checks the status for all of the pipelines that are running on the execution engine specified in the stage. After all of the pipelines complete, the processor updates the pipeline status information in the orchestration record and passes the record downstream.

If you pass orchestration records from multiple stages to the processor, the processor waits until all pipelines associated with those records are complete, then passes a single merged orchestration record downstream.

Note: Using a Wait for Pipelines processor is similar to configuring a Start Pipelines stage to run pipelines in the foreground. In both cases, the orchestration record passes downstream after the pipelines complete. Use the Wait for Pipelines processor when running pipelines in the foreground is not the optimal solution.

For example, instead of using a Wait for Pipelines processor immediately after a Start Pipelines origin that starts a pipeline in the background, you can just configure the origin to run the pipeline in the foreground. Then, the Start Pipelines origin passes its orchestration record downstream after the pipeline completes, with no need for a Wait for Pipelines processor.

When the Wait for Pipelines processor receives the orchestration record, it notes the IDs of all of the pipelines started upstream that are running on the specified Data Pipelines, and waits for them to complete. After the pipelines complete, the Wait for Pipelines processor updates pipeline status information in the orchestration record and passes the record downstream, triggering the start of the pipeline.

### Generated Record <a href="#ariaid-title3" id="ariaid-title3"></a>

When the Wait for Pipelines processor completes its task, it updates the pipeline status and related information in the orchestration record before passing the record downstream.

The processor updates and adds following fields:

| Field Name                           | Description                                                                                                                     |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| \<pipeline ID>/pipelineStatus        | <p>Status of the pipeline. For more information, see Understanding Pipeline States.</p><p>The processor updates this field.</p> |
| \<pipeline ID>/pipelineStatusMessage | <p>Status message for the pipeline.</p><p>The processor updates this field as needed.</p>                                       |
| \<pipeline ID>/finishedSuccessfully  | <p>Boolean field that indicates whether a pipeline completed successfully.</p><p>The processor adds this field.</p>             |
| \<pipeline ID>/pipelineMetrics       | <p>Map field that contains metrics for the pipeline and individual pipeline stages.</p><p>The processor adds these fields.</p>  |
| \<unique task name>/success          | <p>Boolean field that indicates whether all jobs completed successfully.</p><p>The processor adds this field.</p>               |

For example, the following preview shows the fields that a Wait for Pipelines processor adds and updates in comparison to the incoming record:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/WaitPipe-Record.png)

Notice how the processor updated the `pipelineStatus` field, and added the `finishedSuccessfully`, `pipelineMetrics`, and `success` fields. All of the changes indicate that the pipeline completed successfully.

### Configuring a Wait for Pipelines Processor <a href="#ariaid-title4" id="ariaid-title4"></a>

Configure a Wait for Pipelines processor to wait for pipelines to complete before passing an orchestration record downstream. The Wait for Pipelines processor is an orchestration stage that you use in orchestration pipelines.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the Pipeline tab, configure the following properties:

    | Pipeline Property     | Description                                                                                                                                                     |
    | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Execution Engine URL  | URL of the Data Pipelines engine that runs the pipelines.                                                                                                       |
    | Status Check Interval | Milliseconds to wait before checking the specified execution engine URL for the completion status of the pipelines listed in the incoming orchestration record. |
3.  On the Credentials tab, configure the following properties:

    | Credentials Property | Description                                                                                                                                            |
    | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | User Name            | User that runs the pipeline. Enter a user name for the execution engine or enter a Control Hub user name if the engine is registered with Control Hub. |
    | Password             | Password for the user.                                                                                                                                 |
4.  To use SSL/TLS, click the TLS tab and configure the following properties.

    | TLS Property               | Description                                                                                                                                                                                                                                                           |
    | -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Use TLS                    | Enables the use of TLS.                                                                                                                                                                                                                                               |
    | Use Remote Truststore      | Enables loading the contents of the truststore from a remote credential store or from values entered in the stage properties. For more information, see Remote Keystore and Truststore.                                                                               |
    | Trusted Certificates       | Each PEM certificate used in the remote truststore. Enter a credential function that returns the certificate or enter the contents of the certificate.                                                                                                                |
    | Truststore File            | <p>Path to the local truststore file. Enter an absolute path to the file or a path relative to the Data Pipelines resources directory: $SDC_RESOURCES.</p><p>By default, no truststore is used.</p>                                                                   |
    | Truststore Type            | <p>Type of truststore to use. Use one of the following types:</p><ul><li>Java Keystore File (JKS)</li><li>PKCS #12 (p12 file)</li></ul><p>Default is Java Keystore File (JKS).</p>                                                                                    |
    | Truststore Password        | Password to the truststore file. A password is optional, but recommended.                                                                                                                                                                                             |
    | Truststore Trust Algorithm | <p>Algorithm to manage the truststore.</p><p>Default is SunX509.</p>                                                                                                                                                                                                  |
    | Use Default Protocols      | Uses the default TLSv1.2 transport layer security (TLS) protocol. To use a different protocol, clear this option.                                                                                                                                                     |
    | Transport Protocols        | TLS protocols to use. To use a protocol other than the default TLSv1.2, click the Add icon and enter the protocol name. Note: Older protocols are not as secure as TLSv1.2.                                                                                           |
    | Use Default Cipher Suites  | Uses a default cipher suite for the SSL/TLS handshake. To use a different cipher suite, clear this option.                                                                                                                                                            |
    | Cipher Suites              | <p>Cipher suites to use. To use a cipher suite that is not a part of the default set, click the Add icon and enter the name of the cipher suite. </p><p>Enter the Java Secure Socket Extension (JSSE) name for the additional cipher suites that you want to use.</p> |
