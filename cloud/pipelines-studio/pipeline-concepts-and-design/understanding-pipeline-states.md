# Understanding Pipeline States

A pipeline state is the current condition of the pipeline, such as "running" or "stopped". The pipeline state can display in the All Pipelines list. The state of a pipeline can also appear in the Data Pipelines log.The following pipeline states often display in the All Pipelines list:

* EDITED - The pipeline has been created or modified, and has not run since the last modification.
* FINISHED - The pipeline has completed all expected processing and has stopped running.
* RUN\_ERROR - The pipeline encountered an error while running and stopped.
* RUNNING - The pipeline is running.
* STOPPED - The pipeline was manually stopped.
* START\_ERROR - The pipeline encountered an error while starting and failed to start.
* STOP\_ERROR - The pipeline encountered an error while stopping.

The following pipeline states are transient and rarely display in the All Pipelines list. These states can display in the Data Pipelines  log when the pipeline logging level is set to Debug:

* CONNECT\_ERROR - When running a cluster-mode pipeline, Data Pipelines  cannot connect to the underlying cluster manager, such as Mesos or YARN.
* CONNECTING - The pipeline is preparing to restart after a Data Pipelines  restart.
* DISCONNECTED - The pipeline is disconnected from external systems, typically because Data Pipelines is restarting or shutting down.
* FINISHING - The pipeline is in the process of finishing all expected processing.
* RETRY - The pipeline is trying to run after encountering an error while running. This occurs only when the pipeline is configured for a retry upon error.
* RUNNING\_ERROR - The pipeline encounters errors while running.
* STARTING - The pipeline is initializing, but hasn't started yet.
* STARTING\_ERROR - The pipeline encounters errors while starting.
* STOPPING - The pipeline is in the process of stopping after a manual request to stop.
* STOPPING\_ERROR - The pipeline encounters errors while stopping.

## State Transition Examples

Here are some examples of how pipelines can move through states:

### Starting a pipeline

When you successfully start a pipeline for the first time, a pipeline transitions through the following states:

`(EDITED)... STARTING... RUNNING`

When you start a pipeline for the first time but it cannot start, the pipeline transitions through the following states:

`(EDITED)... STARTING... STARTING_ERROR... START_ERROR`

### Stopping or restarting Data Pipelines Studio

When Data Pipelines shuts down, running pipelines transition through the following states:

`(RUNNING)... DISCONNECTING... DISCONNECTED`

When Data Pipelines restarts, any pipelines that were running transition through the following states:

`DISCONNECTED... CONNECTING... STARTING... RUNNING`

### Retrying a pipeline

When a pipeline is configured to retry upon error, Data Pipelines performs the specified number of retries when the pipeline encounters errors while running.

When retrying upon error and successfully retrying, a pipeline transitions through the following states:

`(RUNNING)... RUNNING_ERROR... RETRY... STARTING... RUNNING`

When retrying upon error and encountering another error, a pipeline transitions through the following states:

`(RUNNING)... RUNNING_ERROR... RETRY... STARTING... RUNNING... RUNNING_ERROR...` 

When performing a final retry and unable to return to a Running state, a pipeline transitions through the following states:

`(RUNNING)... RUNNING_ERROR... RUN_ERROR`

### Stopping a pipeline

When you successfully stop a pipeline, a pipeline transitions through the following states:

`(RUNNING)... STOPPING... STOPPED`

When you stop a pipeline and the pipeline encounters errors, the pipeline transitions through the following states:

`(RUNNING)... STOPPING... STOPPING_ERROR... STOP_ERROR`

