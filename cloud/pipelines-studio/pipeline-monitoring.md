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

![](../../.gitbook/assets/image%20%28694%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left">AREA / ICON</th>
      <th style="text-align:left">NAME</th>
      <th style="text-align:left">DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">Pipeline canvas</td>
      <td style="text-align:left">
        <p>Displays the pipeline that Data Pipelines is running.</p>
        <p>You can click a stage to view statistics about the stage. Click an unused
          section of the canvas to view pipeline statistics. Or, you can use the
          stage list to select the information that you want to view.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">2</td>
      <td style="text-align:left">Monitor Panel</td>
      <td style="text-align:left">
        <p>Displays statistics for the pipeline or selected stage by default. Displays
          the following information on the specified tabs:</p>
        <ul>
          <li>Summary - Summary statistics for the pipeline or selected stage.</li>
          <li>Errors - Summary of pipeline errors or stage error and error records for
            the selected stage.</li>
          <li>Info - General information about the pipeline or selected stage or link.</li>
          <li>Configuration - Configuration details for the pipeline or selected stage.</li>
          <li>Rules - Metric alert rules, data rules, and email IDs for alerts.</li>
          <li>History - Pipeline history and links to run summaries.</li>
        </ul>
        <p>Can display information about the pipeline or configuration details.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">3</td>
      <td style="text-align:left">Stage list</td>
      <td style="text-align:left">Lists the stages in the pipeline. Use to select the information that you
        want to view.</td>
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
      <td style="text-align:left">Provides access to the Studio&apos;s configuration properties, directories,
        and log. Also allows you to shut down the Studio.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_OverUser.png"
        alt/>
      </td>
      <td style="text-align:left">User icon</td>
      <td style="text-align:left">Displays the active user and the roles assigned to the user. Also allows
        you to log out of the Studio.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_PipelineList.png"
        alt/>
      </td>
      <td style="text-align:left">Link to a pipeline list</td>
      <td style="text-align:left">Link to a pipeline list on the Home page. Use to view a list of available
        pipelines, perform pipeline maintenance like starting or sharing a pipeline,
        and navigate to individual pipelines.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_MonMore.png"
        alt/>
      </td>
      <td style="text-align:left">More icon</td>
      <td style="text-align:left">Provides additional actions for the pipeline. Use to pause monitoring.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon-ViewLogs.png"
        alt/>
      </td>
      <td style="text-align:left">View Log icon</td>
      <td style="text-align:left">Displays the Data Pipelines Studio log. The equivalent to selecting Administration
        &gt; Logs.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_MonArrange.png"
        alt/>
      </td>
      <td style="text-align:left">Auto Arrange icon</td>
      <td style="text-align:left">Arranges the stages in the pipeline.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_MonSnapshot.png"
        alt/>
      </td>
      <td style="text-align:left">Snapshot icon</td>
      <td style="text-align:left">Captures a snapshot of data passing through the pipeline so you can review
        the data.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_MonStop.png"
        alt/>
      </td>
      <td style="text-align:left">Stop icon</td>
      <td style="text-align:left">Stops the pipeline.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon-Share.png"
        alt/>
      </td>
      <td style="text-align:left">Share icon</td>
      <td style="text-align:left">Shares the pipeline with users and groups. Use to configure pipeline permissions.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_MonDataAlert.png"
        alt/>
      </td>
      <td style="text-align:left">Stream Monitoring icon</td>
      <td style="text-align:left">Select to view or configure a data rule or alert.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_MonInspect.png"
        alt/>
      </td>
      <td style="text-align:left">Inspect Data icon</td>
      <td style="text-align:left">
        <p>Indicates when alerts are configured on the stream:</p>
        <ul>
          <li>Light grey indicates no data rules are defined.</li>
          <li>Medium grey indicates at least one data rule is defined, but none are
            active.</li>
          <li>Dark grey indicates at least one active data rule.</li>
          <li>Red indicates a data alert has been triggered.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

