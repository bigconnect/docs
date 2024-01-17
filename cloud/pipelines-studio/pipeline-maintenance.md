# Pipeline Maintenance

## Understanding Pipeline States <a href="#ariaid-title2" id="ariaid-title2"></a>

A pipeline state is the current condition of the pipeline, such as "running" or "stopped". The pipeline state can display in the All Pipelines list. The state of a pipeline can also appear in the Data Pipelines log.

The following pipeline states often display in the All Pipelines list:

* EDITED - The pipeline has been created or modified, and has not run since the last modification.
* FINISHED - The pipeline has completed all expected processing and has stopped running.
* RUN\_ERROR - The pipeline encountered an error while running and stopped.
* RUNNING - The pipeline is running.
* STOPPED - The pipeline was manually stopped.
* START\_ERROR - The pipeline encountered an error while starting and failed to start.
* STOP\_ERROR - The pipeline encountered an error while stopping.

The following pipeline states are transient and rarely display in the All Pipelines list. These states can display in the Data Pipelines log when the pipeline logging level is set to Debug:

* CONNECT\_ERROR - When running a cluster-mode pipeline, Data Pipelines cannot connect to the underlying cluster manager, such as Mesos or YARN.
* CONNECTING - The pipeline is preparing to restart after a Data Pipelines restart.
* DISCONNECTED - The pipeline is disconnected from external systems, typically because Data Pipelines is restarting or shutting down.
* FINISHING - The pipeline is in the process of finishing all expected processing.
* RETRY - The pipeline is trying to run after encountering an error while running. This occurs only when the pipeline is configured for a retry upon error.
* RUNNING\_ERROR - The pipeline encounters errors while running.
* STARTING - The pipeline is initializing, but hasn't started yet.
* STARTING\_ERROR - The pipeline encounters errors while starting.
* STOPPING - The pipeline is in the process of stopping after a manual request to stop.
* STOPPING\_ERROR - The pipeline encounters errors while stopping.

### State Transition Examples <a href="#ariaid-title3" id="ariaid-title3"></a>

Here are some examples of how pipelines can move through states:Starting a pipelineWhen you successfully start a pipeline for the first time, a pipeline transitions through the following states:

```
(EDITED)... STARTING... RUNNING
```

When you start a pipeline for the first time but it cannot start, the pipeline transitions through the following states:

```
(EDITED)... STARTING... STARTING_ERROR... START_ERROR
```

Stopping or restarting Data Pipelines

When Data Pipelines shuts down, running pipelines transition through the following states:

```
(RUNNING)... DISCONNECTING... DISCONNECTED
```

When Data Pipelines restarts, any pipelines that were running transition through the following states:

```
DISCONNECTED... CONNECTING... STARTING... RUNNING
```

Retrying a pipelineWhen a pipeline is configured to retry upon error, Data Pipelines performs the specified number of retries when the pipeline encounters errors while running.

When retrying upon error and successfully retrying, a pipeline transitions through the following states:

```
(RUNNING)... RUNNING_ERROR... RETRY... STARTING... RUNNING
```

When retrying upon error and encountering another error, a pipeline transitions through the following states:

```
(RUNNING)... RUNNING_ERROR... RETRY... STARTING... RUNNING... RUNNING_ERROR... 
```

When performing a final retry and unable to return to a Running state, a pipeline transitions through the following states:

```
(RUNNING)... RUNNING_ERROR... RUN_ERROR
```

Stopping a pipelineWhen you successfully stop a pipeline, a pipeline transitions through the following states:

```
(RUNNING)... STOPPING... STOPPED
```

When you stop a pipeline and the pipeline encounters errors, the pipeline transitions through the following states:

```
(RUNNING)... STOPPING... STOPPING_ERROR... STOP_ERROR
```

## Starting Pipelines <a href="#ariaid-title4" id="ariaid-title4"></a>

You can start Data Pipelines pipelines when they are valid. When you start a pipeline, Data Pipelines runs the pipeline until you stop the pipeline or shut down Data Pipelines.

For most origins, when you restart a pipeline, Data Pipelines starts the pipeline from where it last stopped by default. You can reset the origin to read all available data.

A Kafka Consumer origin starts processing data based on the offset passed from the Kafka ZooKeeper.

You can start pipelines from the following locations:

* From the Home page, select pipelines in the list and then click the Start icon.
*   From the pipeline canvas, click the Start icon.

    If the Start icon is not enabled, the pipeline is not valid.

### Starting Pipelines with Parameters <a href="#ariaid-title5" id="ariaid-title5"></a>

If you defined runtime parameters for a pipeline, you can specify the parameter values to use when you start the pipeline.

Note: If you want to use the default parameter values, you can simply click the Start icon to start the pipeline.

For more information, see Using Runtime Parameters.

1.  From the pipeline canvas, click the More icon, and then click Start with Parameters.If Start with Parameters is not enabled, the pipeline is not valid.

    The Start with Parameters dialog box lists all parameters defined for the pipeline and their default values.
2. Override any default values with the values you want to use for this pipeline run.
3. Click Start.

### Resetting the Origin <a href="#ariaid-title6" id="ariaid-title6"></a>

You can reset the origin when you want the Data Pipelines to process all available data instead of processing data from the last-saved offset. Reset the origin when the pipeline is not running.

You can reset the origin for the following origin stages:

* Amazon S3
* Azure Data Lake Storage Gen1
* Azure Data Lake Storage Gen2
* Directory
* Elasticsearch
* File Tail
* Google Cloud Storage
* Groovy Scripting
* Hadoop FS Standalone
* HTTP Client
* JavaScript Scripting
* JDBC Multitable Consumer
* JDBC Query Consumer
* Jython Scripting
* Kinesis Consumer
* MongoDB
* MongoDB Oplog
* MySQL Binary Log
* Salesforce
* SAP HANA Query Consumer
* SFTP/FTP/FTPS Client
* SQL Server 2019 BDC Multitable Consumer
* SQL Server CDC Client
* SQL Server Change Tracking
* Teradata Consumer
* Windows Event Log

For these origins, when you stop the pipeline, the Data Pipelines notes where it stopped processing data. When you restart the pipeline, it continues from where it left off by default. When you want the Data Pipelines to process all available data instead of continuing from where it stopped, reset the origin. For unique details about resetting the Kinesis Consumer origin, see Resetting the Kinesis Consumer Origin.

You can reset the Azure IoT/Event Hub Consumer origin by deleting offset details in the Microsoft Azure portal. The remaining origin stages process transient data where resetting the origin has no effect.

You can reset the origin for multiple pipelines at the same time from the Home page. Or, you can reset the origin for a single pipeline from the pipeline canvas.

To reset the origin:

1. Select multiple pipelines from the Home page, or view a single pipeline in the pipeline canvas.
2. Click the More icon, and then click Reset Origin.
3. In the Reset Origin Confirmation dialog box, click Yes to reset the origin.

## Stopping Pipelines <a href="#ariaid-title7" id="ariaid-title7"></a>

Stop pipelines when you want Data Pipelines to stop processing data for the pipelines.

When stopping a pipeline, Data Pipelines waits for the pipeline to gracefully complete all tasks for the in-progress batch. In some situations, this can take several minutes.

For example, if a scripting processor includes code with a timed wait, Data Pipelines waits for the scripting processor to complete its task. Then, Data Pipelines waits for the rest of the pipeline to complete all tasks before stopping the pipeline.

When Data Pipelines runs a pipeline, it displays in the Data Pipelines UI in Monitor mode by default.

1. From the Home page, select the pipelines in the list and then click the Stop icon. Or to stop a pipeline in the pipeline canvas, click the Stop icon.The Stop Pipeline Confirmation dialog box appears.
2.  To stop the pipelines, click Yes.Depending on the pipeline complexity, the pipeline might take some time to stop.

    When a pipeline remains in a Stopping state for an unexpectedly long period of time, you can force the pipeline to stop.

### Forcing a Pipeline to Stop <a href="#ariaid-title8" id="ariaid-title8"></a>

When necessary, you can force Data Pipelines to stop a pipeline.

When forcing a pipeline to stop, Data Pipelines often stops processes before they complete, which can lead to unexpected results.

Important: Pipelines can take a long time to stop gracefully, depending on the processing logic. Use this option only after waiting an appropriate amount of time for the pipeline to come to a graceful stop.

1. To force a pipeline to stop from the Home page, click the More icon for the pipeline, and then click Force Stop. Or to force a pipeline to stop from the pipeline canvas, click Force Stop.The Force Stop Pipeline Confirmation dialog box appears.
2. To force the pipelines to stop, click Yes.

## Importing Pipelines <a href="#ariaid-title9" id="ariaid-title9"></a>

Import pipelines to use pipelines developed on a different Data Pipelines or to restore backup files.

You can import pipelines that were developed on the same version of Data Pipelines or on an earlier version of Data Pipelines. Data Pipelines does not support importing a pipeline developed on a later version of Data Pipelines.

You can import pipelines from individual pipeline files, from a ZIP file containing multiple pipeline files, or from an external HTTP URL. Pipeline files are JSON files exported from a Data Pipelines.

### Importing a Pipeline <a href="#ariaid-title10" id="ariaid-title10"></a>

You can import a single pipeline from a pipeline JSON file exported from a Data Pipelines. When you import a single pipeline, you can rename the pipeline during the import.

1. To import a single pipeline, from the Home page, click Create New Pipeline > Import Pipeline.
2. In the Import Pipeline dialog box, enter a pipeline title and optional description.
3. Browse and select the pipeline file, and then click Open.
4. Click Import.

### Importing a Set of Pipelines from an Archive File <a href="#ariaid-title11" id="ariaid-title11"></a>

You can import a set of pipelines from a ZIP file that contains multiple pipeline JSON files. When you import a set of pipelines, Data Pipelines imports the existing pipeline names. If necessary, you can rename the pipelines after the import.

1. To import a set of pipelines, from the Home page, click Create New Pipeline > Import Pipelines from Archive.
2. In the Import Pipelines from Archive dialog box, browse and select the ZIP file that contains the pipeline files, and then click Open.
3. To import all pipelines in the file, click Import.

### Importing a Pipeline from an HTTP URL <a href="#ariaid-title12" id="ariaid-title12"></a>

You can import a single pipeline from an external HTTP URL.

When you import a pipeline from an HTTP URL, you can rename the pipeline during the import.

1. To import a pipeline from an HTTP URL, from the Home page, click Create New Pipeline > Import Pipeline from HTTP URL.
2. In the Import Pipeline from HTTP URL dialog box, enter a pipeline title and optional description.
3. Enter the HTTP URL for the pipeline.
4. Click Import.

## Sharing Pipelines <a href="#ariaid-title13" id="ariaid-title13"></a>

When you create a pipeline, you become the owner of the pipeline. As the owner of a pipeline, you have all permissions for the pipeline, you can configure pipeline sharing, and you can change the owner of the pipeline. A pipeline can have a single user as the owner.

Like the pipeline owner, a user with the Admin role also has all permissions for all pipelines, can configure pipeline sharing and can change the pipeline owner.

By default, all other users have no access to pipelines. To allow other users to work with a pipeline, you must share the pipeline with the users or their groups, and configure pipeline permissions.

When you share a pipeline, you can configure the following permissions for each user and group:

| Permission | Description                                                                 |
| ---------- | --------------------------------------------------------------------------- |
| Read       | View and monitor the pipeline, and see alerts. View existing snapshot data. |
| Write      | Edit the pipeline and alerts.                                               |
| Execute    | Start and stop the pipeline. Preview data and take a snapshot.              |

When someone shares a pipeline with you, it displays in the Pipeline library under the Shared With Me label in the pipeline library.

For more information about roles and permissions, see Roles and Permissions.

### Sharing a Pipeline <a href="#ariaid-title14" id="ariaid-title14"></a>

Share a pipeline to allow users to perform pipeline-related tasks. You can share a pipeline with individual users or with groups.

You can share a pipeline if you are the owner of the pipeline or a user with the Admin role.

You can configure pipeline sharing at any time, but pipeline permissions are only enforced when Data Pipelines is enabled to use pipeline access controls. The sharing configuration goes into effect when sharing is enabled.

1. You can share a pipeline from either of the following locations:
   * From the Home page, select the pipeline, click the More icon, and click Share.
   * From the pipeline canvas, click the Share icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon-Share.png).
2. In the Sharing Settings dialog box, click in the Select Users and Groups window, select the users and groups that you want to share with, and click Add.
3. Configure the permissions that you want each user and group to have and click Save.

### Changing the Pipeline Owner <a href="#ariaid-title15" id="ariaid-title15"></a>

As the owner of a pipeline or a user with the Admin role, you can specify a user as the pipeline owner.

The pipeline owner has all permissions for the pipeline and can configure sharing for other users and groups. There can only be one pipeline owner.

1. You can configure pipeline permissions from the following locations:
   * From the Home page, select the pipeline, click the More icon, and click Share.
   * From the pipeline canvas, click the Share icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon-Share.png).
2. In the Sharing Settings dialog box, if necessary, add the user that you want to use as the owner.
3. To select a new owner, click the More icon for the user and click Is Owner.
4. Click Save to save the change.

## Adding Labels to Pipelines <a href="#ariaid-title16" id="ariaid-title16"></a>

You can add labels to pipelines to group similar pipelines. For example, you might want to group pipelines by database schema or by the test or production environment.

You can use nested labels to create a hierarchy of pipeline groupings. Enter nested labels using the following format:

```
<label1>/<label2>/<label3>
```

For example, to group pipelines in the test environment by the origin system, you might add the labels `Test/HDFS` and `Test/Elasticsearch` to the appropriate pipelines.

You can add labels to pipelines from the following locations:

* From the Home page, select pipelines in the list, click the More icon, and then click Add Labels. Enter labels and then click Save.Note: Existing labels that have already been added to the pipeline are ignored.
* From the pipeline canvas, click the General tab and then enter labels for the Labels property.

## Exporting Pipelines <a href="#ariaid-title17" id="ariaid-title17"></a>

Export pipelines to create backups or to use the pipelines with another Data Pipelines. You can export pipelines with or without plain text credentials configured in the pipeline. You can export a single pipeline or a set of pipelines.

1. From the Home page, select one or more pipelines to export.Alternatively, to export a single pipeline, you can open the pipeline.
2.  Click the More icon, and choose whether to export the pipeline with or without configured plain text credentials:

    * Export - Data Pipelines removes any configured plain text credentials from the exported pipelines.
    * Export with Plain Text Credentials - Data Pipelines includes any configured plain text credentials in the exported pipelines.

    Data Pipelines writes a file containing the exported pipelines to your default downloads directory:

    * When you export a single pipeline, Data Pipelines generates a JSON file named after the pipeline, as follows: `<pipeline name>.json`.
    * When you export a set of pipelines, Data Pipelines creates a ZIP file named `pipelines.zip`.

## Duplicating a Pipeline <a href="#ariaid-title19" id="ariaid-title19"></a>

Duplicate a pipeline when you want to keep the existing version of a pipeline while continuing to configure a duplicate version. A duplicate is an exact copy of the original pipeline.

When you duplicate a pipeline, you can rename the pipeline and specify the number of copies to make.

1. From the Home page, select a pipeline in the list view and then click the Duplicate icon. Or to duplicate a pipeline in the pipeline canvas, click the More icon for the pipeline and then click Duplicate.
2. In the Duplicate Pipeline Definition dialog box, enter a name for the duplicate pipeline and the number of copies to make.When you create multiple copies, Data Pipelines appends an integer after the pipeline name. For example, if you enter the name "test" and create two copies of the pipeline, Data Pipelines names the duplicate pipelines "test1" and "test2".
3. Click Duplicate.The duplicate pipelines display.

## Deleting Pipelines <a href="#ariaid-title20" id="ariaid-title20"></a>

You can delete pipelines when you no longer need them. Deleting pipelines is permanent. To keep backups, export the pipelines before you delete them.

1. From the Home page, select pipelines in the list and then click the Delete icon. Or to delete a pipeline in the pipeline canvas, click the More icon for the pipeline and then click Delete.A confirmation window appears.
2. To delete the pipelines, click Yes.
