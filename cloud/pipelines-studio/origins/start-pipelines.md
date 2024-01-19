# Start Pipelines

The Start Pipelines origin starts one or more pipelines in parallel when the pipeline starts.

The Start Pipelines origin is an orchestration stage that you use in orchestration pipelines. Orchestration stages perform tasks, such as schedule and start pipelines that you can use to create an orchestrated workflow across the platform.

The origin generates a record that contains a list of the started pipelines and information about those pipelines. You can pass the record to an orchestration stage to trigger another task. Or, you can pass it to a non-orchestration stage to perform other processing. For example, you might use a Stream Selector processor to pass the record to different stages based on a pipeline completion status.

When you configure the Start Pipelines origin, you define the URL of the pipelines engine that runs the pipelines. You specify the names or IDs of the pipelines to start along with any runtime parameters to use.&#x20;

You can configure the origin to reset the origins in the pipelines when possible, and to run the pipelines in the background. When running pipelines in the background, the origin immediately passes its generated record downstream instead of waiting for the pipelines to finish.

You also configure the user name and password to run the pipeline and can optionally configure SSL/TLS properties.

### Pipeline Execution and Data Flow <a href="#ariaid-title2" id="ariaid-title2"></a>

The Start Pipelines origin starts the specified pipelines when the pipeline starts. The origin creates a record with task details and passes it downstream based on how the pipelines run:

**Run pipelines in the foreground**

By default, the origin starts pipelines that run in the foreground. When the pipelines run in the foreground, the origin passes the orchestration record downstream after all the started pipelines complete.

Run pipelines in the foreground to ensure that the task defined in the subsequent stage is not performed until all of the pipelines complete.

**Run pipelines in the background**

You can configure the origin to start pipelines that run in the background. When pipelines run in the background, the origin updates and passes the orchestration record downstream immediately after starting the pipelines.The origin does not track or provide information about whether the pipelines finish successfully.

Run pipelines in the background to enable the task defined in the subsequent orchestration stage to occur in parallel with the pipelines started by the origin.

### Generated Record <a href="#ariaid-title3" id="ariaid-title3"></a>

The Start Pipelines origin creates an orchestration record that includes information about the pipelines that it starts.

The record includes the following fields:

| Field Name          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| orchestratorTasks   | <p>List Map field that contains task details for the orchestration pipeline.</p><p>Most orchestration stages add details about their completed tasks within this field.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| \<unique task name> | <p>List Map field within the <code>orchestratorTasks</code> field that contains the following fields:</p><ul><li>pipelineIds - IDs of the pipelines that the stage is configured to start.</li><li>pipelineResults - List Map of pipeline run details for each pipeline.</li><li><p>success - Boolean field that indicates whether all pipelines completed successfully.</p><p>Included only when the pipelines run in the foreground.</p></li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| \<pipeline ID>      | <p>List Map field within the <code>pipelineResults</code> field that provides details about each pipeline. Contains the following fields:</p><ul><li>pipelineId - ID of the pipeline.</li><li>pipelineTitle - Name of the pipeline.</li><li>startedSuccessfully - Boolean field that indicates whether a pipeline was started successfully.</li><li><p>finishedSuccessfully - Boolean field that indicates whether a pipeline completed successfully.</p><p>Included only when the pipeline runs in the foreground.</p></li><li><p>pipelineMetrics - Map field that contains metrics for the pipeline and individual pipeline stages.</p><p>Included only when the pipeline runs in the foreground.</p></li><li>pipelineStatus - Status of the pipeline. For more information, see Understanding Pipeline States.</li><li>pipelineStatusMessage - Status message for the pipeline.</li><li><p>committedOffsetsStr - Offset details for the pipeline.</p><p>Included only when the pipeline runs in the foreground.</p></li></ul> |

For example, the following preview shows information provided by a Start Pipelines origin with the `Load_ADLS` task name. The origin runs one pipeline in the background:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/StartPipe-O-Record.png)

Note that the `startedSuccessfully` and `pipelineStatus` fields indicate that pipeline was started successfully. There is no `finishedSuccessfully` field because the pipeline has not yet completed.
