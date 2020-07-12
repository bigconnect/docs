# Event Generation

The event framework generates pipeline events for Data Collector standalone pipelines when the pipeline starts and stops.

In a Data Collector standalone pipeline, you can pass pipeline events to an executor or to another pipeline for additional processing. By default, these events are discarded.

## Pipeline Event Records

Pipeline event records have the following event-related record header attributes. Record header attributes are stored as String values:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Record Header Attribute</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">sdc.event.type</td>
      <td style="text-align:left">
        <p>Event type. Uses one of the following types:</p>
        <ul>
          <li>pipeline-start - Generated as the pipeline starts.</li>
          <li>pipeline-stop - Generated as the pipeline stops.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">sdc.event.version</td>
      <td style="text-align:left">Integer that indicates the version of the event record type.</td>
    </tr>
    <tr>
      <td style="text-align:left">sdc.event.creation_timestamp</td>
      <td style="text-align:left">Epoch timestamp when the stage created the event.</td>
    </tr>
  </tbody>
</table>

The event framework generates the following types of pipeline events:

**pipeline-start**

The event framework generates start events as the pipeline initializes, immediately after it starts and before individual stages are initialized.

The start event record has `sdc.event.type` set to `pipeline-start`, and the following fields:

| Pipeline Start Event Field | Description |
| :--- | :--- |
| pipelineId | The ID of the pipeline that started. |
| pipelineTitle | The user-defined name of the pipeline that started. |
| user | The user who started the pipeline. |
| parameters | Any parameters used when starting the pipeline. |

**pipeline-stop**

The event framework generates stop events as the pipeline stops, either manually, programmatically, or due to a failure. The stop event is generated after all stages have completed processing and cleaning up temporary resources, such as removing temporary files.

The stop event record has `sdc.event.type` set to `pipeline-stop`, and the following fields:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Pipeline Stop Event Field</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">pipelineId</td>
      <td style="text-align:left">The ID of the pipeline that stopped.</td>
    </tr>
    <tr>
      <td style="text-align:left">pipelineTitle</td>
      <td style="text-align:left">The user-defined name of the pipeline that stopped.</td>
    </tr>
    <tr>
      <td style="text-align:left">reason</td>
      <td style="text-align:left">
        <p>The reason why the pipeline stopped. Can be set to the following reasons:</p>
        <ul>
          <li>Error - An error occurred as the pipeline was running.</li>
          <li>Finished - The pipeline finished all expected processing.</li>
          <li>User - A user stopped the pipeline.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">user</td>
      <td style="text-align:left">The user who stopped the pipeline, when relevant.</td>
    </tr>
  </tbody>
</table>

