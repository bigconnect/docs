# Data Preview

You can preview data to help build or fine-tune a pipeline. When using Control Hub, you can also use data preview when developing pipeline fragments.

You can use data preview with complete or incomplete pipelines and fragments. And you can choose from several options to provide source data for the preview.

When you preview data, source data passes through the pipeline or fragment, allowing you to review how the data passes and changes through each stage. You can edit stage properties and run the preview again to see how your changes affect the data. You can also edit preview data to test and tune the pipeline logic.

You can preview data for one stage at a time or for a group of stages. You can also view the data in list or table view, and refresh the preview data.

## Data Collector UI - Preview Mode

You can use Data Collector to view how data passes through the pipeline.

The following image shows Data Collector in Preview mode:

![](../../.gitbook/assets/image%20%28316%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left">AREA/ICON</th>
      <th style="text-align:left">NAME</th>
      <th style="text-align:left">DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">Pipeline canvas</td>
      <td style="text-align:left">Displays the pipeline.</td>
    </tr>
    <tr>
      <td style="text-align:left">2</td>
      <td style="text-align:left">Preview panel</td>
      <td style="text-align:left">Displays the data that enters and exits the selected stage or group of
        stages. It can also display stage properties and preview configuration.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_OverHome.png"
        alt/>
      </td>
      <td style="text-align:left">Home icon</td>
      <td style="text-align:left">Displays a home page with a list of pipelines and their statuses, allowing
        you to perform pipeline maintenance and navigate to individual pipelines.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PackageManager.png"
        alt/>
      </td>
      <td style="text-align:left">Package Manager icon</td>
      <td style="text-align:left">Displays the Package Manager which allows you to install additional stage
        libraries for a core Data Collector installation.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_MonAlert.png"
        alt/>
      </td>
      <td style="text-align:left">Notifications icon</td>
      <td style="text-align:left">Displays notifications.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_OverAdministration.png"
        alt/>
      </td>
      <td style="text-align:left">Administration icon</td>
      <td style="text-align:left">Provides access to Data Collector configuration properties, directories,
        and log. Also allows you to shut down Data Collector.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_OverUser.png"
        alt/>
      </td>
      <td style="text-align:left">User icon</td>
      <td style="text-align:left">Displays the active user and the roles assigned to the user. Also allows
        you to log out of Data Collector.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_OverCHelp.png"
        alt/>
      </td>
      <td style="text-align:left">Help icon</td>
      <td style="text-align:left">
        <p>Provides context-sensitive help based on the information in the panel.
          Allows you to configure display settings and to specify whether to use
          a local or hosted version of the help.</p>
        <p>Provides access to the REST API and the Data Collector version.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PipelineList.png"
        alt/>
      </td>
      <td style="text-align:left">Link to a pipeline list</td>
      <td style="text-align:left">Link to a <a href="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Getting_Started/GettingStarted_Title.html#concept_wc5_ysp_1w">pipeline list on the Home page</a>.
        Use to view a list of available pipelines, perform pipeline maintenance
        like starting or sharing a pipeline, and navigate to individual pipelines.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevRecords.png"
        alt/>
      </td>
      <td style="text-align:left">Records icon</td>
      <td style="text-align:left">Displays data preview records.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevStageConfig.png"
        alt/>
      </td>
      <td style="text-align:left">Stage Configuration icon</td>
      <td style="text-align:left">Displays stage properties.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevPreviewConfig.png"
        alt/>
      </td>
      <td style="text-align:left">Preview Configuration icon</td>
      <td style="text-align:left">Displays data preview properties.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevSingle.png"
        alt/>
      </td>
      <td style="text-align:left">Single button</td>
      <td style="text-align:left">Displays input and output data for a single stage.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevMultiple.png"
        alt/>
      </td>
      <td style="text-align:left">Multiple button</td>
      <td style="text-align:left">Displays input and output data for a group of stages.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevListView.png"
        alt/>
      </td>
      <td style="text-align:left">List View icon</td>
      <td style="text-align:left">Displays preview data in a list.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevTableView.png"
        alt/>
      </td>
      <td style="text-align:left">Table View icon</td>
      <td style="text-align:left">Displays preview data in a table.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevPrevStage.png"
        alt/>
      </td>
      <td style="text-align:left">Previous Stage icon</td>
      <td style="text-align:left">Moves the preview to the previous stage.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevNextStage.png"
        alt/>
      </td>
      <td style="text-align:left">Next Stage icon</td>
      <td style="text-align:left">Moves the preview to the next stage.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevRefresh.png"
        alt/>
      </td>
      <td style="text-align:left">Refresh Preview icon</td>
      <td style="text-align:left">Provides a fresh set of data from the origin.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PreviewRerun.png"
        alt/>
      </td>
      <td style="text-align:left">Run Preview with Changes icon</td>
      <td style="text-align:left">Runs the data preview using changed data. Use to see how edited data is
        processed by the pipeline.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevRevertChanges.png"
        alt/>
      </td>
      <td style="text-align:left">Revert Changes icon</td>
      <td style="text-align:left">Reverts all changes to preview data and returns the preview to the origin.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PrevClose.png"
        alt/>
      </td>
      <td style="text-align:left">Close Preview icon</td>
      <td style="text-align:left">Closes the data preview.</td>
    </tr>
  </tbody>
</table>