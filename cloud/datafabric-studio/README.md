# DataFabric Studio

DataFabric is a lightweight, powerful design and execution engine that streams data in real time. Use DataFabric  to route and process data in your data streams.

To define the flow of data, you design a pipeline in DataFabric Studio. A pipeline consists of stages that represent the origin and destination of the pipeline, and any additional processing that you want to perform. After you design the pipeline, you click Start and DataFabric goes to work.

### Logging In and Creating a Pipeline in DataFabric Studio

After you provision a DataFabric Studio service, you can log in and create your first pipeline:

1. To access DataFabric Studio, enter the following URL in the address bar of your browser:    `https://pipelinelab-NNNNNNNNN.cloud.bigconnect.io`where NNNNNNNNN is your Tenant ID.
2. On the **Get Started** page, click **Create New Pipeline**.
3. In the **New Pipeline** window, enter a name for the pipeline, optionally enter a description, and click **Save**.

The pipeline canvas appears. The Properties panel displays pipeline properties. For a description of the icons and areas, see  [DataFabric Studio - Edit Mode](pipeline-configuration/datafabric-ui-edit-mode.md).

### DataFabric Studio UI

DataFabric Studio provides a web-based user interface \(UI\) to configure pipelines, preview data, monitor pipelines, and review snapshots of data.

The UI includes the following general areas and icons:

![](../../.gitbook/assets/image%20%28693%29.png)

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
      <td style="text-align:left">Canvas for configuring, previewing, or monitoring a pipeline.</td>
    </tr>
    <tr>
      <td style="text-align:left">2</td>
      <td style="text-align:left">Properties panel / Preview panel / Monitor panel</td>
      <td style="text-align:left">
        <p>When you configure a pipeline, the Properties panel displays the properties
          of the pipeline or selected stage. You can resize, minimize and maximize
          the panel.
          <br />
        </p>
        <p>When you preview data, the Preview panel displays the data that enters
          and exits the selected stage or group of stages. It can also display stage
          properties and preview configuration.
          <br />
        </p>
        <p>
          <br />
        </p>
        <p>When you monitor a running pipeline, the Monitor panel displays real-time
          metrics and statistics.
          <br />
        </p>
        <p>
          <br />
        </p>
        <p><b>Note</b>: Some icons and options might not display. The items that
          display are based on the task that you are performing and roles assigned
          to your user account.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/image (689).png" alt/>
      </td>
      <td style="text-align:left">Home icon</td>
      <td style="text-align:left">Displays a home page with a list of pipelines and their statuses, allowing
        you to perform pipeline maintenance and navigate to individual pipelines.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/image (690).png" alt/>
      </td>
      <td style="text-align:left">Notifications icon</td>
      <td style="text-align:left">Displays notifications.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/image (686).png" alt/>
      </td>
      <td style="text-align:left">Administration icon</td>
      <td style="text-align:left">Provides access to DataFabric Studio configuration properties, directories,
        and log.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <img src="../../.gitbook/assets/image (692).png" alt/>
      </td>
      <td style="text-align:left">User icon</td>
      <td style="text-align:left">Displays the active user and the roles assigned to the user. Also allows
        you to log out of DataFabric Studio.</td>
    </tr>
  </tbody>
</table>

For information about configuring pipelines, see [Data Fabric Studio - Edit Mode](pipeline-configuration/datafabric-ui-edit-mode.md).

For information about data preview options, see Data Collector UI - Preview Mode.

For information about pipeline monitoring options, see Data Collector UI - Monitor Mode.

