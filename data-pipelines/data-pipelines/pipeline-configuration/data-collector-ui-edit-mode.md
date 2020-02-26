# Data Collector UI - Edit Mode

The following image shows the Data Collector UI when you configure a pipeline:

![](../../../.gitbook/assets/image%20%28227%29.png)

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
      <td style="text-align:left">Displays the pipeline. Use to configure the pipeline data flow.</td>
    </tr>
    <tr>
      <td style="text-align:left">2</td>
      <td style="text-align:left">Pipeline Creation Help Bar</td>
      <td style="text-align:left">
        <p>Offers lists of stages to help complete the pipeline.</p>
        <p>You can use the help bar to connect a stage to an open node. You can also
          add a stage between linked stages by clicking the link.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">3</td>
      <td style="text-align:left">Stage list</td>
      <td style="text-align:left">Lists the stages in the pipeline. Use to select a stage to configure.
        You can also select the stage in the canvas.</td>
    </tr>
    <tr>
      <td style="text-align:left">4</td>
      <td style="text-align:left">Properties panel</td>
      <td style="text-align:left">Displays the properties of the pipeline or selected stage when you configure
        a pipeline.</td>
    </tr>
    <tr>
      <td style="text-align:left">5</td>
      <td style="text-align:left">Stage library panel</td>
      <td style="text-align:left">
        <p>List of available stages. Use to add stages to the pipeline. You can drag
          a stage to a location on canvas or click a stage to add it to the end of
          the pipeline.</p>
        <p>You can view all stages, stages by type, or stages by library. You can
          also search for a stage by name.</p>
      </td>
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
      <td style="text-align:left">Link to a pipeline list on the Home page. Use to view a list of available
        pipelines, perform pipeline maintenance like starting or sharing a pipeline,
        and navigate to individual pipelines.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigMore.png"
        alt/>
      </td>
      <td style="text-align:left">More icon</td>
      <td style="text-align:left">Provides additional actions for the pipeline. Use to reset the origin
        for the pipeline. Or to view or delete snapshot data.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_Issues.png"
        alt/>
      </td>
      <td style="text-align:left">Issues icon</td>
      <td style="text-align:left">Displays the number of implicit validation issues in the pipeline. Click
        to view a detailed list of issues found during implicit validation.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon-ViewLogs.png"
        alt/>
      </td>
      <td style="text-align:left">View Log icon</td>
      <td style="text-align:left">Displays the Data Collector log. The equivalent to selecting Administration
        &gt; Logs.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigUndo.png"
        alt/>
      </td>
      <td style="text-align:left">Undo icon</td>
      <td style="text-align:left">Reverts recent changes. On Mac, you can also use Command+Z. On Windows,
        you can use Ctrl+Z.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigRedo.png"
        alt/>
      </td>
      <td style="text-align:left">Redo icon</td>
      <td style="text-align:left">Restores changes that were reverted. On Mac, you can also use Command+Shift+Z.
        On Windows, you can use Ctrl+Y.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigDelete.png"
        alt/>
      </td>
      <td style="text-align:left">Delete icon</td>
      <td style="text-align:left">Deletes the selected stage or link.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigCopy.png"
        alt/>
      </td>
      <td style="text-align:left">Duplicate Stage icon</td>
      <td style="text-align:left">
        <p>Duplicates the selected stage. You can duplicate processors and destinations.
          Because a pipeline can include only one origin, you cannot duplicate an
          origin stage.</p>
        <p>You can also use keyboard shortcuts to copy and paste the selected stage,
          i.e. Ctrl+C and Ctrl+V for Windows and Command+C and Command+V for Macintosh.</p>
        <p>Stages can be copied and pasted to other pipelines only if the pipelines
          are in the same browser tab.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigArrange.png"
        alt/>
      </td>
      <td style="text-align:left">Auto Arrange icon</td>
      <td style="text-align:left">Arranges the stages in the pipeline.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigDataPreview.png"
        alt/>
      </td>
      <td style="text-align:left">Preview icon</td>
      <td style="text-align:left">Starts a data preview.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigValidate.png"
        alt/>
      </td>
      <td style="text-align:left">Validate icon</td>
      <td style="text-align:left">Validates the pipeline. Performs explicit validation.</td>
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
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigStartPipeline.png"
        alt/>
      </td>
      <td style="text-align:left">Start icon</td>
      <td style="text-align:left">Starts the pipeline.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigStageLibrary.png"
        alt/>
      </td>
      <td style="text-align:left">Stage Library icon</td>
      <td style="text-align:left">Toggles the display of the Stage Library panel.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigLink.png"
        alt/>
      </td>
      <td style="text-align:left">Stream Link icon</td>
      <td style="text-align:left">Indicates the flow of data through the pipeline. Select to configure data
        rules and alerts.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigStageIssue.png"
        alt/>
      </td>
      <td style="text-align:left">Error icon</td>
      <td style="text-align:left">
        <p>Indicates that one or more required properties are not defined. Can display
          on a stage for stage properties or in the canvas for pipeline properties.</p>
        <p>Related error messages display when you hover over the icon. You can also
          view the messages in the Issues list.</p>
        <p>The icon can also display on tabs in the properties panel to indicate
          the location of missing properties.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigMaximize.png"
        alt/>
      </td>
      <td style="text-align:left">Maximize Pane icon</td>
      <td style="text-align:left">Maximizes the configuration panel.</td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
Some icons and options might not display. The items that display are based on the task that you are performing and roles assigned to your user account.

For example, the Copy icon displays only when you select stage that can be copied, it does not display when you select a link or an origin. Or, if you log in with the Manager role, most configuration-related icons are not available.
{% endhint %}

