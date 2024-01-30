# Retrying the Pipeline

By default, when Data Pipelines Studio encounters a stage-level error that might cause a standalone pipeline to fail, it retries the pipeline. That is, it waits a period of time, and then tries again to run the pipeline.

A stage-level error might include a stage not being able to connect to an external system because the system or network is down.

You can define the maximum number of pipeline retries that Data Pipelines attempts.

With the default value, -1, Data Pipelines retries the pipeline an infinite number of times. This allows the pipeline to continue running as soon as any temporary connection or system failures resolve.

If you want the pipeline to stop after a given number of retries, you can define the maximum number of retries to perform.

The wait time between retries begins at 15 seconds and doubles in length until reaching a maximum of 5 minutes.
