# Data Preview

## Overview

You can preview data to help build or fine-tune a pipeline.&#x20;

You can use data preview with complete or incomplete pipelines and fragments. And you can choose from several options to provide source data for the preview.

When you preview data, source data passes through the pipeline or fragment, allowing you to review how the data passes and changes through each stage. You can edit stage properties and run the preview again to see how your changes affect the data. You can also edit preview data to test and tune the pipeline logic.

You can preview data for one stage at a time or for a group of stages. You can also view the data in list or table view, and refresh the preview data.

### Data Preview Availability <a href="#ariaid-title3" id="ariaid-title3"></a>

You can preview complete and incomplete pipelines and Control Hub pipeline fragments. The Data Preview icon becomes active when data preview is available.

You can preview data under the following conditions:

* All stages in the pipeline are connected
* All required properties are defined

Tip: Stage configuration does not have to be accurate or complete to preview data. After you connect all stages, you can enable data preview by entering any valid value for required properties.

### Source Data for Data Preview <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use the following types of data for a data preview:

* Data from the origin - Use available data from the origin.
* Data from the test origin - Use data from the test origin configured in the pipeline or fragment properties.
* Data from a snapshot - Use snapshot data from the same pipeline or another pipeline. Available for pipelines only.

### Writing to Destinations and Executors <a href="#ariaid-title5" id="ariaid-title5"></a>

Since data preview is a tool for development, by default, it does not write data to destination systems or pass data to executors connected to destination stages.

Data preview also does not display the data that is written by destinations in the pipeline. You can, however, view the data that is passed to a destination stage, which is typically similar to what is written to destination systems.

If you like, you can configure the preview to write data to destination systems and to trigger executors connected to destination stages. For example, you might enable writing to an executor to verify that it performs the configured task as expected.

To write to destination systems and connected executors, in the Preview Configuration dialog box, select Write to Destinations and Executors.

Important: We advise against writing preview data to production destination systems.

### Notes <a href="#ariaid-title6" id="ariaid-title6"></a>

Keep the following notes in mind when previewing your data:

* Date, datetime, and time data - Data preview displays date, datetime, and time data using the default format of the browser locale. For example, if the browser uses the en\_US locale, preview displays dates using the following format: MMM d, y h:mm:ss a.
* Oracle CDC Client pipelines - When previewing a pipeline that uses the Oracle CDC Client origin, data preview might time out before connecting to the origin system. When this occurs, try increasing the timeout to 120,000 milliseconds to allow the origin time to connect.
* Whole file data format - When previewing a pipeline that processes whole file data, data preview displays only one record.

## Data Pipelines Studio - Preview Mode

You can use Data Pipelines to view how data passes through the pipeline.

The following image shows Data Pipelines in Preview mode:

![](../../../.gitbook/assets/pipelines\_preview.jpg)

| AREA/ICON                                                                                                                | NAME                          | DESCRIPTION                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------ | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1                                                                                                                        | Pipeline canvas               | Displays the pipeline.                                                                                                                                                                                                                                            |
| 2                                                                                                                        | Preview panel                 | Displays the data that enters and exits the selected stage or group of stages. It can also display stage properties and preview configuration.                                                                                                                    |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverHome.png)           | Home icon                     | Displays a home page with a list of pipelines and their statuses, allowing you to perform pipeline maintenance and navigate to individual pipelines.                                                                                                              |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_MonAlert.png)           | Notifications icon            | Displays notifications.                                                                                                                                                                                                                                           |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverAdministration.png) | Administration icon           | Provides access to Data Pipelines configuration properties, directories, and log. Also allows you to shut down Data Pipelines.                                                                                                                                    |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverUser.png)           | User icon                     | Displays the active user and the roles assigned to the user. Also allows you to log out of Data Pipelines.                                                                                                                                                        |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverCHelp.png)          | Help icon                     | <p>Provides context-sensitive help based on the information in the panel. Allows you to configure display settings and to specify whether to use a local or hosted version of the help.</p><p>Provides access to the REST API and the Data Pipelines version.</p> |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PipelineList.png)       | Link to a pipeline list       | Link to a pipeline list on the Home page. Use to view a list of available pipelines, perform pipeline maintenance like starting or sharing a pipeline, and navigate to individual pipelines.                                                                      |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevRecords.png)        | Records icon                  | Displays data preview records.                                                                                                                                                                                                                                    |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevStageConfig.png)    | Stage Configuration icon      | Displays stage properties.                                                                                                                                                                                                                                        |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevPreviewConfig.png)  | Preview Configuration icon    | Displays data preview properties.                                                                                                                                                                                                                                 |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevSingle.png)         | Single button                 | Displays input and output data for a single stage.                                                                                                                                                                                                                |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevMultiple.png)       | Multiple button               | Displays input and output data for a group of stages.                                                                                                                                                                                                             |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevListView.png)       | List View icon                | Displays preview data in a list.                                                                                                                                                                                                                                  |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevTableView.png)      | Table View icon               | Displays preview data in a table.                                                                                                                                                                                                                                 |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevPrevStage.png)      | Previous Stage icon           | Moves the preview to the previous stage.                                                                                                                                                                                                                          |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevNextStage.png)      | Next Stage icon               | Moves the preview to the next stage.                                                                                                                                                                                                                              |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevRefresh.png)        | Refresh Preview icon          | Provides a fresh set of data from the origin.                                                                                                                                                                                                                     |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PreviewRerun.png)       | Run Preview with Changes icon | Runs the data preview using changed data. Use to see how edited data is processed by the pipeline.                                                                                                                                                                |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevRevertChanges.png)  | Revert Changes icon           | Reverts all changes to preview data and returns the preview to the origin.                                                                                                                                                                                        |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevClose.png)          | Close Preview icon            | Closes the data preview.                                                                                                                                                                                                                                          |

Note: Some icons and options might not display. The items that display are based on the task that you are performing and roles assigned to your user account.

## Preview Codes

Data preview displays different colors for different types of data. Preview also uses other codes and formatting to highlight changed fields.

The following table describes the color and asterisk coding:

| PREVIEW CODE            | DESCRIPTION                              |
| ----------------------- | ---------------------------------------- |
| Black values            | Date data                                |
| Blue values             | Numeric data                             |
| Green values            | String data                              |
| Red values              | Boolean data                             |
| Asterisk                | Records that include edited field values |
| Red italic field labels | Fields that contain edited data          |
| Light red background    | Fields removed by a stage                |
| Italic values           | Edited data                              |
| Green stage             | First stage in a multiple-stage preview  |
| Red stage               | Last stage in a multiple-stage preview   |

## Previewing a Single Stage <a href="#ariaid-title9" id="ariaid-title9"></a>

You can preview data for a single stage. In the Preview panel, you can review the values for each record to determine if the stage transforms data as expected.

1. Above the pipeline canvas, click the Preview icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_Preview.png).If the Preview icon is disabled, check the Issues list for unconnected stages and required properties that are not defined.
2.  In the Preview Configuration dialog box, configure the following properties:

    | Preview Property                    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Preview Source                      | <p>Source data for the preview:</p><ul><li>Configured Source - Provides data from the origin system.</li><li>Test Origin - Provides data from the test origin configured for the pipeline.</li><li>Snapshot Data - Uses available snapshot data. Available for pipelines only.</li></ul>                                                                                                                                                                              |
    | Preview Batch Size                  | <p>Number of records to use in the preview. Honors values up to the Data Pipelines preview batch size.</p><p>Default is 10.</p>                                                                                                                                                                                                                                                                                                                                       |
    | Preview Timeout                     | Milliseconds to wait for preview data. Use to limit the time data preview waits for data to arrive at the origin. Relevant for transient origins only.                                                                                                                                                                                                                                                                                                                |
    | Write to Destinations and Executors | <p>Determines whether the preview passes data to destinations or executors.</p><p>By default, does not pass data to destination or executor stages.</p>                                                                                                                                                                                                                                                                                                               |
    | Execute Pipeline Lifecycle Events   | Triggers the generation of any appropriate pipeline events, typically the Start event. If the event is configured to be used, event consumption is also triggered.                                                                                                                                                                                                                                                                                                    |
    | Show Record/Field Header            | Displays record header attributes and field attributes when in List view. Attributes do not display in Table view.                                                                                                                                                                                                                                                                                                                                                    |
    | Show Field Type                     | Displays the data type for fields in List view. Field types do not display in Table view.                                                                                                                                                                                                                                                                                                                                                                             |
    | Snapshot Data                       | When using a snapshot for source data, select the snapshot to use.                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Remember the Configuration          | <p>Stores the current preview configuration for use every time you request a preview for this pipeline.</p><p>After you run data preview, you can change this option in the Preview panel by selecting the Preview Configuration icon (<img src="https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon_PrevPreviewConfig.png" alt="">) and clearing the option. The change takes effect the next time you run data preview.</p> |
3.  Click Run Preview.The Preview panel highlights the origin stage and displays preview data in list view. Since this is the origin of the pipeline, no input data displays.

    To view preview data in table view, click the Table View icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevTableView.png).
4. To delete a record that you do not want to use, click the Delete icon.
5. To view data for the next stage, click the Next Stage icon. Or, to view data for a different stage, select the stage in the pipeline canvas.
6. To refresh the preview, click the Refresh Preview icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevRefresh.png).Refreshing the preview provides a new set of data.
7. To exit data preview, click the Close Preview icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevClose.png).

## Previewing Multiple Stages <a href="#ariaid-title10" id="ariaid-title10"></a>

You can preview data for a group of linked stages within a pipeline.

When you preview multiple stages, you select the first stage and the last stage in the group. The Preview panel then displays the output data of the first stage in the group and the input data of the last stage in the group.

In the Preview panel, you can review the values for each record to determine if the group of stages transforms data as expected.

1. Above the pipeline canvas, click the Preview icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_Preview.png).If the Preview icon is disabled, check the Issues list for unconnected stages and required properties that are not defined.
2.  In the Preview Configuration dialog box, configure the following properties:

    | Preview Property                    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Preview Source                      | <p>Source data for the preview:</p><ul><li>Configured Source - Provides data from the origin system.</li><li>Test Origin - Provides data from the test origin configured for the pipeline.</li><li>Snapshot Data - Uses available snapshot data. Available for pipelines only.</li></ul>                                                                                                                                                                              |
    | Preview Batch Size                  | <p>Number of records to use in the preview. Honors values up to the Data Pipelines preview batch size.</p><p>Default is 10. </p>                                                                                                                                                                                                                                                                                                                                      |
    | Preview Timeout                     | Milliseconds to wait for preview data. Use to limit the time data preview waits for data to arrive at the origin. Relevant for transient origins only.                                                                                                                                                                                                                                                                                                                |
    | Write to Destinations and Executors | <p>Determines whether the preview passes data to destinations or executors.</p><p>By default, does not pass data to destination or executor stages.</p>                                                                                                                                                                                                                                                                                                               |
    | Execute Pipeline Lifecycle Events   | Triggers the generation of any appropriate pipeline events, typically the Start event. If the event is configured to be used, event consumption is also triggered.                                                                                                                                                                                                                                                                                                    |
    | Show Record/Field Header            | Displays record header attributes and field attributes when in List view. Attributes do not display in Table view.                                                                                                                                                                                                                                                                                                                                                    |
    | Show Field Type                     | Displays the data type for fields in List view. Field types do not display in Table view.                                                                                                                                                                                                                                                                                                                                                                             |
    | Snapshot Data                       | When using a snapshot for source data, select the snapshot to use.                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Remember the Configuration          | <p>Stores the current preview configuration for use every time you request a preview for this pipeline.</p><p>After you run data preview, you can change this option in the Preview panel by selecting the Preview Configuration icon (<img src="https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon_PrevPreviewConfig.png" alt="">) and clearing the option. The change takes effect the next time you run data preview.</p> |
3.  Click Run Preview.The Preview panel highlights the origin stage and displays preview data in list view. Since this is the origin of the pipeline, no input data displays.

    To view preview data in table view, click the Table View icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevTableView.png).
4. To preview multiple stages, click Multiple.The Preview panel displays two lists of stages.
5. From the list on the left, select the first stage to use.
6. In the list on the right, select the last stage to use.The Preview panel displays the input and output data for the group of stages. You can review the details of each record.
7. To refresh the preview, click the Refresh Preview icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevRefresh.png).Refreshing the preview provides a new set of data.
8. To exit data preview, click the Close Preview icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevClose.png).

## Editing Preview Data <a href="#ariaid-title11" id="ariaid-title11"></a>

You can edit preview data to view how a stage or group of stages processes the changed data. Edit preview data to test for data conditions that might not appear in the preview data set.

For example, when the stage filters integer data based on an expression, you might change the input data to test positive and negative integer values, as well as zero.

You can edit preview data in the following locations:

* The output data column for an origin.
* The input data column for processors.

When you edit preview data, you can pass the changed data through the pipeline, or you can revert your changes to return to the original data.

1. To change field values, in the Output Data column of an origin or the Input Data column for all other stages, click the value that you want to change and enter a new value.You can edit values for any input data.
2. To process changed data, click the Run With Changes icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PreviewRerun.png).This runs the data preview with the current set of data and stage configuration.In the Input Data column, records with changed values display with an asterisk and the changed values are highlighted. The Output Data column displays the results of processing. You can change and process preview data as often as necessary.
3. To refresh the preview, click the Refresh Preview icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevRefresh.png).Refreshing the preview provides a new set of data.
4. To revert changes to data, click the Revert Data Changes icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevRevertChanges.png).

## Editing Properties <a href="#ariaid-title12" id="ariaid-title12"></a>

In data preview, you can edit stage properties to see how the changes affect preview data. For example, you might edit the expression in an Expression Evaluator to see how the expression alters data.

When you edit properties, you can test the change with the existing preview data or you can refresh the preview data.

When changing properties in the origin, refresh the preview data to test your changes. Refreshing the preview data allows Data Pipelines to use the latest origin properties to process preview data instead of using the cached data.

Note: Unlike changes to data, you cannot automatically revert property changes. Manually revert any changes that you do not want to preserve.

1. To edit stage properties while in data preview, select the stage you want to edit and click the Stage Configuration icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevStageConfig.png).
2. Change properties as needed.
3. To test properties changed in the origin, click the Refresh Preview icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PrevRefresh.png).This refreshes the preview data. Based on the origin type, it might use the same data or a new set of data with the updated origin properties.To test properties in any non-origin stage using the same set of data, click the Run With Changes icon: ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_PreviewRerun.png).
4. If you want to revert your change, manually change the property back.
