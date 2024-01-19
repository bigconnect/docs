# Pipeline Monitoring

When Data Pipelines runs a pipeline, you can view real-time statistics about the pipeline, examine a sample of the data being processed, and create rules and alerts.When you access the Studio while running a pipeline, the UI displays the pipeline in Monitor mode. In Monitor mode, you can perform the following tasks:

* View real-time stage and pipeline statistics
* View stage and pipeline error information, including error records for each stage
* Take and review a snapshot of data
* Configure rules and alerts
* View the pipeline history

## Data Pipelines Studio - Monitor Mode

In Monitor mode, you can use the Studio to view data as it passes through the pipeline.

The following image shows the Studio in Monitor mode:

![](<../../.gitbook/assets/image (251).png>)

| AREA / ICON                                                                                                              | NAME                    | DESCRIPTION                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------ | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1                                                                                                                        | Pipeline canvas         | <p>Displays the pipeline that is running.</p><p>You can click a stage to view statistics about the stage. Click an unused section of the canvas to view pipeline statistics. Or, you can use the stage list to select the information that you want to view.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 2                                                                                                                        | Monitor Panel           | <p>Displays statistics for the pipeline or selected stage by default. Displays the following information on the specified tabs:</p><ul><li>Summary - Summary statistics for the pipeline or selected stage.</li><li>Errors - Summary of pipeline errors or stage error and error records for the selected stage.</li><li>Info - General information about the pipeline or selected stage or link.</li><li>Configuration - Configuration details for the pipeline or selected stage.</li><li>Rules - Metric alert rules, data rules, and email IDs for alerts.</li><li>History - Pipeline history and links to run summaries.</li></ul><p>Can display information about the pipeline or configuration details.</p> |
| 3                                                                                                                        | Stage list              | Lists the stages in the pipeline. Use to select the information that you want to view.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverHome.png)           | Home icon               | Displays a home page with a list of pipelines and their statuses, allowing you to perform pipeline maintenance and navigate to individual pipelines.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonAlert.png)           | Notifications icon      | Displays notifications.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverAdministration.png) | Administration icon     | Provides access to Data Pipelines configuration properties, directories, and log. Also allows you to shut down Data Pipelines.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverUser.png)           | User icon               | Displays the active user and the roles assigned to the user. Also allows you to log out of Data Pipelines.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverCHelp.png)          | Help icon               | <p>Provides context-sensitive help based on the information in the panel. Allows you to configure display settings and to specify whether to use a local or hosted version of the help.</p><p>Provides access to the REST API and the Data Pipelines version.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PipelineList.png)       | Link to a pipeline list | Link to a pipeline list on the Home page. Use to view a list of available pipelines, perform pipeline maintenance like starting or sharing a pipeline, and navigate to individual pipelines.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonMore.png)            | More icon               | Provides additional actions for the pipeline. Use to pause monitoring.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon-ViewLogs.png)            | View Log icon           | Displays the Data Pipelines log. The equivalent to selecting Administration > Logs.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonArrange.png)         | Auto Arrange icon       | Arranges the stages in the pipeline.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonSnapshot.png)        | Snapshot icon           | Captures a snapshot of data passing through the pipeline so you can review the data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonStop.png)            | Stop icon               | Stops the pipeline.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon-Share.png)               | Share icon              | Shares the pipeline with users and groups. Use to configure pipeline permissions.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonDataAlert.png)       | Stream Monitoring icon  | Select to view or configure a data rule or alert.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonInspect.png)         | Inspect Data icon       | <p>Indicates when alerts are configured on the stream:</p><ul><li>Light grey indicates no data rules are defined.</li><li>Medium grey indicates at least one data rule is defined, but none are active.</li><li>Dark grey indicates at least one active data rule.</li><li>Red indicates a data alert has been triggered.</li></ul>                                                                                                                                                                                                                                                                                                                                                                               |

Note: Some icons and options might not display. The items that display are based on the task that you are performing and roles assigned to your user account.

## Viewing Pipeline and Stage Statistics <a href="#ariaid-title4" id="ariaid-title4"></a>

When you monitor a pipeline, you can view real-time summary and error statistics for the pipeline and for stages in the pipeline.

By default, the Data Pipelines UI displays pipeline monitoring information when it runs a pipeline. You can select a stage to view the statistics about the stage. Similarly, you can view error information for the pipeline and its stages.

The Monitor panel displays statistics on the following tabs:Summary

For a pipeline, displays the record count for the pipeline, record and batch throughput, and batch processing statistics. For a pipeline started with runtime parameters, displays the parameter values that the pipeline is currently using.

For a stage, displays record and batch throughput and batch processing statistics.Tip: You can hover over different parts of the charts to view exact numbers.

Note that the record and batch throughput graphs are calculated using an exponential moving average, weighing more heavily toward the most recent values and exponentially reducing the effect of old data. For more information, see [https://en.wikipedia.org/wiki/Moving\_average#Exponential\_moving\_average](https://en.wikipedia.org/wiki/Moving\_average#Exponential\_moving\_average).

ErrorFor a pipeline, displays histograms for the number of error records by five minute decay and the number of error records by stage since the pipeline started.For a stage, displays the number of error records and the number of stage errors.

## Monitoring Errors <a href="#ariaid-title5" id="ariaid-title5"></a>

When you monitor a pipeline, you can view error statistics for the pipeline and each stage. You can also view a sampling of the error records.

The Errors tab in the Monitor panel displays pipeline errors by default:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Mon-PipelineErrors.png)

### Stage-Related Errors <a href="#ariaid-title6" id="ariaid-title6"></a>

You can view the errors related to each stage. Stage-related errors include the error records that the stage produces and other errors encountered by the stage.

To view stage-related errors, select the stage from the stage list. Or, click the stage in the canvas. The Errors tab of the Monitor panel displays the following tabs:

Error Records

<figure><img src="https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/mon-StageErrors.png" alt=""><figcaption></figcaption></figure>

Displays a sample of error records with related error messages, as well as the count and an error histogram.

You can expand and review the data in each error record. If the error was produced by an exception, you can click View Stack Trace to view the full stack trace. The number of error records saved in memory is defined in the Data Pipelines configuration file, `$SDC_CONF/sdc.properties`.Stage ErrorsDisplays a list of stage errors as well as the count and an error histogram. Stage errors are operational errors, such as an origin being unable to create a record because of invalid source data.

## Snapshots <a href="#ariaid-title7" id="ariaid-title7"></a>

A snapshot is a set of data captured as it moves through a running pipeline.

You can capture snapshots when you monitor a Data Pipelines pipeline.

View a snapshot to verify how a pipeline processes data. Like data preview, you can view how snapshot data moves through a pipeline stage by stage or across multiple stages. You can drill down to review the values of each record to determine if the stage or group of stages transforms data as expected.

Unlike data preview, you cannot edit data to perform testing when you review a snapshot. Instead, you can use the snapshot as source data for data preview. You might use a snapshot for data preview to test the pipeline with production data.

### Failure Snapshots <a href="#ariaid-title8" id="ariaid-title8"></a>

A failure snapshot is a partial snapshot that occurs automatically when the pipeline stops due to unexpected data. You can view the failure snapshot to troubleshoot the problem.

A failure snapshot captures the data in the pipeline that was in memory when the problem occurred. As a result, it includes the data that caused the problem and might include other unrelated data, but does not include data in each stage like a full snapshot.

Standalone pipelines generate the failure snapshot by default. Pipelines in cluster or edge execution mode do not generate failure snapshots.

You can configure standalone pipelines to skip generating the failure snapshot by clearing the Create Failure Snapshot pipeline property.

**Viewing a Failure Snapshot**

After a standalone pipeline generates a failure snapshot, you can review the snapshot to determine the cause of the error.

To view a failure snapshot, in the stopped pipeline, click the More icon, then select Snapshot. In the Snapshots dialog box, find the failure snapshot and click View.

In the Snapshots dialog box, failure snapshots use the following naming convention: `Failure at <time of failure>`.

When the failure snapshot displays, you can click through the stages. Stages that encountered no errors will typically not display any data. The stage that contains data should be the stage that encountered the errors.

For example, say a pipeline stops with the following error:

```
com.streamsets.pipeline.api.StageException: SCRIPTING_06 - Script error while processing batch: 
javax.script.ScriptException: <error message>
```

You can click through the pipeline starting from the origin, looking for the problematic stage and finding no data. But from the error message, you can tell that there was a problem with a scripting processor. So you can look immediately at the scripting processor. There, you find that the offending data enters the processor but does not exit:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FailureSnapshot.png)

You can then examine the data that caused the errors and edit the pipeline as needed.

### Capturing and Viewing a Snapshot <a href="#ariaid-title10" id="ariaid-title10"></a>

You can capture a snapshot of data when you monitor a pipeline.

After you capture a snapshot, you can view the snapshot data stage by stage or through a group of stages, like data preview. You can also delete snapshot data or use it as source data for data preview.

1. From the pipeline canvas of a running pipeline, click the Snapshots icon.
2. In the Snapshots dialog box, click Capture Snapshot to capture a set of data.The Data Pipelines captures a snapshot of the next batch that passes through the pipeline and displays it in the list.\
   You can take additional snapshots, view a snapshot, delete a snapshot, or close the dialog box and use the snapshot later.
3. To view a snapshot, click View for the snapshot that you want to use.The canvas highlights the origin stage of the pipeline. The Monitor panel displays snapshot data in the Output Data column. Since this is the origin of the pipeline, no input data displays.
4. To view data for the next stage, click the Next Stage icon. Or, to view data for a different stage, select the stage in the pipeline canvas.
5. To view the snapshot for multiple stages, click Multiple.The Preview panel displays two lists of stages:
   1. From the list on the left, select the first stage to include.
   2. In the list on the right, select the last stage to include.
6. To review data from a different snapshot, on the upper left side of the monitor panel, select a different snapshot name
7. To exit the snapshot review, click Close Snapshot.

### Downloading a Snapshot <a href="#ariaid-title11" id="ariaid-title11"></a>

When needed, you can download a snapshot. You might download a snapshot from a production Data Pipelines so you can review it on a development Data Pipelines. Or you might download a snapshot to use the Dev Snapshot Replaying origin to read records from the downloaded file.

When you download a snapshot, it downloads to the default download location on the Data Pipelines machine.

Downloaded snapshots use the following naming convention: `<pipeline id>_<snapshot name>.json`.

The pipeline ID is the original title for the pipeline followed by a UUID.

For standard snapshots, the snapshot name is "snapshot" followed by the epoch timestamp of when the snapshot was taken. For example, a standard snapshot downloaded for the Oracle to Google Cloud pipeline might have the following name:

```
OracletoGoogleCloud_f116d713-372c-4105-ad03-e042c47dc72b_snapshot1513289740553.json
```

For failure snapshots, the snapshot name is "Failure\_" followed by a UUID. For example, a failure snapshot downloaded for the AWS pipeline might have the following name:

```
AWSa81167ba-be03-4f74-8028-2f5b0439e6a9_Failure_6c7912ee-c4f7-4aee-b67f-0b0e587bc91e.json
```

1. From the pipeline canvas of a running pipeline, click the Snapshots icon. Or, from the canvas of a stopped pipeline, click the More icon and then click Snapshots.The Snapshots window displays all available snapshots for the pipeline.
2. In the Snapshots window, click Download for the snapshot that you want to download.

### Deleting a Snapshot <a href="#ariaid-title12" id="ariaid-title12"></a>

Data Pipelines retains all snapshots for a pipeline by default. You can delete snapshots when they are no longer needed. For example, after taking a snapshot on a production Data Pipelines, you might download the snapshot for review on a development Data Pipelines, then delete the snapshot from the production machine.

Note: When you delete a snapshot, the information is irrevocably removed. You cannot retrieve a deleted snapshot.

1. From the pipeline canvas of a running pipeline, click the Snapshots icon. Or, from the canvas of a stopped pipeline, click the More icon and then click Snapshots.The Snapshots window displays all available snapshots for the pipeline.
2. In the Snapshots window, click Delete for the snapshot that you want to delete.

## Viewing the Run History <a href="#ariaid-title13" id="ariaid-title13"></a>

You can view the run history of a pipeline and a summary of each run when you configure or monitor a pipeline.

The pipeline history shows the following information:

* The pipeline status
* The time the pipeline started or stopped
* Related messages
* Access to each run summary

Click the History tab in the pipeline properties or monitor panel to view the run history. The following image shows a sample run history:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/RunHistory.png)

### Viewing a Run Summary <a href="#ariaid-title14" id="ariaid-title14"></a>

You can view a run summary for each run of the pipeline when you view the pipeline history.

You can view run summaries for completed runs. A run summary includes the following information:

* Input, output, and error record count for the pipeline.
* Input, output, and error record count for each stage.
* Runtime statistics for the pipeline, including the number of batches processed, the time the last record was received, and the source offset when available.

To view a run summary, on the History tab of the pipeline, click View Summary.
