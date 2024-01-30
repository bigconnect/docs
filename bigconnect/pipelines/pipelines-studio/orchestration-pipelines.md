# Orchestration Pipelines

An orchestration pipeline is a pipeline that uses one or more orchestration stages to schedule and perform tasks, such as starting a sequence of pipelines. Use orchestration pipelines to perform tasks in an orchestrated workflow across the StreamSets platform.

For example, you can create an orchestration pipeline that starts several pipelines at 1 am every day.&#x20;

A typical orchestration pipeline generates a single record. As each orchestration stage completes its task, it adds information about the task that it performed to the orchestration record. For example, the Start Pipelines origin lists the pipeline ID and status of each pipeline that it starts. When the orchestration pipeline completes, the resulting orchestration record provides details about all of the tasks performed by the pipeline.

You can use other stages in an orchestration pipeline to create the workflow that you need. For example, you might add a Stream Selector processor that routes the record to different branches based on whether another pipeline succeeded or failed. Or, you might use an Email executor to send an email after the workflow is complete.

An orchestration pipeline can be configured in any logical way. Review the documentation for the orchestration stages to see how you might orchestrate your workflows.

## Orchestration Stages <a href="#ariaid-title3" id="ariaid-title3"></a>

Unlike typical origins, processors, and destinations, orchestration stages do not process batches of data. Instead, when triggered, they perform orchestration tasks such as start pipelines or wait for pipelines to complete.

Origin orchestration stages perform their tasks when the pipeline starts. They also generate an orchestration record that passes downstream. Processors and destination stages perform their tasks when they receive an orchestration record.

Data Pipelines provides the following orchestration stages:

* Cron Scheduler origin - Generates a record at scheduled intervals. Use to schedule orchestration tasks.
* Start Pipelines origin - Starts one or more pipelines in parallel when the orchestration pipeline starts.
* Start Pipelines processor - Starts one or more pipelines in parallel upon receiving a record.
* Wait for Pipelines processor - Waits for pipelines running on the specified execution engine to complete.

## Orchestration Record <a href="#ariaid-title4" id="ariaid-title4"></a>

An orchestration origin generates an orchestration record that contains details about the task that it performed, such as the IDs of the jobs or pipelines that it started and the status of those jobs or pipelines.

As this record passes through the pipeline, each orchestration stage updates the record, adding details about the task that it performed. When the orchestration pipeline completes, the resulting orchestration record provides information about all of the orchestration tasks that the pipeline performed.

For details about how each orchestration stage creates or updates an orchestration record, see "Generated Record" in the stage documentation.

#### Example

Say you have the following orchestration pipeline with a Start Pipelines origin that starts a Transformer pipeline, a Start Pipelines processor that pipeline, and a Hadoop FS destination to store the resulting orchestration record. Both Start Pipelines stages run the pipelines in the foreground, so they wait until their pipelines complete before passing the orchestration record downstream:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/OrchRecord-Pipeline.png)

This orchestration pipeline generates the following orchestration record:

<pre class="language-json"><code class="lang-json">{
  "orchestratorTasks": {
<strong>    "Start_Transformer_Pipeline": {
</strong>      "pipelineIds": [
        "LoadADLS82ea742b-4091-493c-9baf-07341d45564c"
      ],
      "pipelineResults": {
        "LoadTable82ea742b-4091-493c-9baf-07341d45564c": {
          "pipelineId": "LoadADLS82ea742b-4091-493c-9baf-07341d45564c",
          "pipelineTitle": "Load ADLS",
          "startedSuccessfully": true,
          "finishedSuccessfully": true,
          "pipelineMetrics": {
            "pipeline": {
              "outputRecords": 100000500,
              "inputRecords": 100000500
            },
            "stages": {
              "&#x3C;origin1>": {
                "outputRecords": 100000500
              },
              "&#x3C;processor1>": {
                "outputRecords": 100000500,
                "inputRecords": 0
              },
              "&#x3C;processor2>": {
                "outputRecords": 100000500,
                "inputRecords": 100000500
              },
              "&#x3C;destination>": {
                "outputRecords": 100000500,
                "inputRecords": 100000500
              }
            }
          },
          "pipelineStatus": "FINISHED",
          "pipelineStatusMessage": null,
          "committedOffsetsStr": "{&#x3C;offset info>}"
        }
      },
      "success": true
    },
<strong>    "Start_Data_Collector_Pipeline": {
</strong>      "pipelineIds": [
        "SalesProc2a4aa879-ebe2-44f3-be67-f3e95588be6e"
      ],
      "pipelineResults": {
        "SalesProc2a4aa879-ebe2-44f3-be67-f3e95588be6e": {
          "pipelineId": "SalesProc2a4aa879-ebe2-44f3-be67-f3e95588be6e",
          "pipelineTitle": "Sales Processing",
          "startedSuccessfully": true,
          "finishedSuccessfully": true,
          "pipelineMetrics": {
            "pipeline": {
              "outputRecords": 121130,
              "errorMessages": 0,
              "inputRecords": 121130,
              "errorRecords": 0
            },
            "stages": {
              "&#x3C;origin>": {
                "outputRecords": 121130
              },
              "&#x3C;processor>": {
                "outputRecords": 121130,
                "errorMessages": 0,
                "inputRecords": 121130,
                "errorRecords": 0
              },
              "&#x3C;destination>": {
                "outputRecords": 121130,
                "errorMessages": 0,
                "inputRecords": 121130,
                "errorRecords": 0
              }
            }
          },
          "pipelineStatus": "FINISHED",
          "pipelineStatusMessage": null,
          "committedOffsetsStr": "{&#x3C;offset info>}"
        }
      },
      "success": true
    }
  }
}
</code></pre>

For the Start Pipelines stages, task details are grouped based on the Task Name property defined in each stage. In this case, the task names match the stage names:

*   The Start\_Transformer\_Pipeline task performed by the Start Pipelines origin starts the `Load ADLS` Transformer pipeline.

    From the `startedSuccessfully` and `finishedSuccessfully` fields. you can see that the pipeline started and completed successfully before the record was passed downstream.

    You can also see the pipeline and stage metrics provided by the origin.
*   The Start\_Data\_Collector\_Pipeline task performed by the Start Pipelines processor starts the `Sales Processing` pipeline.

    This pipeline also started and completed successfully, and the pipeline metrics are included in the orchestration record.

### Using the Orchestration Record <a href="#ariaid-title5" id="ariaid-title5"></a>

As with most records, you can use information in an orchestration record to perform pipeline processing,

For example, say you want to start different obs depending on whether several pipelines complete successfully. The orchestration pipeline might look like this:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Orch-UseRecord.png)

In the orchestration record generated by the Start Pipelines origin, the `success` field indicates that the origin has completed all of its configured tasks. To start a job after all pipelines started by the origin successfully complete, you might set the following precondition for the Start Job if Pipeline Succeeds processor:

```
${record:value('/orchestratorTasks/startJobs/success') == true}
```

In the field path, `startJobs` is the task name defined in the origin.

If you only needed a particular pipeline to complete, you could use the following precondition instead:

```
${record:value('/orchestratorTasks/startJobs/pipelineResults/<pipelineID>/finishedSuccessfully') == true}
```

For the Start Job if Pipeline Fails processor, you can use the following precondition to ensure that the job starts if any of the pipelines failed to complete:

```
${record:value('/orchestratorTasks/startJobs/success') == false}
```

If you wanted to start the job only if the first pipeline started by the origin has more than 100 error records, you could use the following precondition:

```
${record:value('/orchestratorTasks/startJobs/pipelineResults[0]/pipelineMetrics/pipeline/errorRecords') > 100}
```

For details about the fields that each orchestration stage creates or updates, see "Generated Record" in the stage documentation.

## Sample Pipeline <a href="#ariaid-title6" id="ariaid-title6"></a>

You can develop orchestration pipelines that perform a variety of tasks in an orchestrated workflow, such as scheduling pipelines and jobs and triggering subsequent tasks.

#### Recurring Task that Runs Multiple Dependent Pipelines

With the orchestration stages, you can design an orchestration pipeline that triggers a set of tasks daily and starts dependent pipelines based on the success of an earlier pipeline.

For example, an orchestration pipeline might run three separate but dependent pipelines, P1, P2, and P3. The orchestration pipeline runs two of the pipelines, P1 and P2, in parallel, and then starts the third pipeline only after both P1 and P2 finish, and P2 succeeds. The following image shows the orchestration pipeline:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Orchestrator-DependentPipelines.png)

The Cron Scheduler origin generates an orchestration record at the specified time each day. When the Start Pipelines processor receives the record, it starts the P1 and P2 pipelines in parallel, running them in the foreground. After both pipelines finish, the processor updates and passes the record to a Stream Selector processor, which checks for the success of P2.

If P2 succeeds, the Stream Selector processor routes the record to start P3. If P2 fails to complete, the Stream Selector processor routes the record to error handling. When the Start P3 Start Pipelines processor receives a record, it runs P3 in the background. So, after it starts the pipeline, the processor updates and passes the orchestration record immediately to the Email executor, which sends an email announcing the start of the P3 pipeline.

If you wanted to run the orchestration pipeline manually rather than on a schedule, you could replace the Cron Scheduler origin and the Start Pipelines processor with a Start Pipelines origin.

