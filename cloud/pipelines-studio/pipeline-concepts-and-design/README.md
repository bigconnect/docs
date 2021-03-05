# Pipeline Concepts and Design

A pipeline describes the flow of data from the origin system to destination systems and defines how to transform the data along the way.

You can use a single [origin](../origins.md) stage to represent the origin system, multiple [processor](../processors.md) stages to transform data, and multiple [destination](../destinations.md) stages to represent destination systems.

When you develop a pipeline, you can use development stages to provide sample data and generate errors to test error handling. And you can use data preview to determine how stages alter the data through the pipeline.

You can use [executor](../executors.md) stages to perform event-triggered task execution or to save event information. To process large volumes of data, you can use multithreaded pipelines or cluster mode pipelines.

In pipelines that write to Hive or parquet or to PostgreSQL, you can implement a data drift solution that detects drift in incoming data and updates tables in destination systems.

When you start a pipeline, Data Pipelines runs the pipeline until you stop the pipeline or shut down Data Collector. You can use Data Collector to run multiple pipelines.

While the pipeline runs, you can [monitor](../pipeline-monitoring.md) the pipeline to verify that the pipeline performs as expected. You can also define metric and data rules and alerts to let you know when certain thresholds are reached.

