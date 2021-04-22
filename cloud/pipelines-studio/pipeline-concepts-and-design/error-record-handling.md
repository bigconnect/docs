# Error Record Handling

You can configure error record handling at a stage level and at a pipeline level. You can also specify the version of the record to use as the basis for the error record.

When an error occurs as a stage processes a record, Data Pipelines handles the record based on the stage configuration. One of the stage options is to pass the record to the pipeline for error handling. For this option, Data Pipelines  processes the record based on the pipeline error record handling configuration.

When you configure a pipeline, be aware that stage error handling takes precedence over pipeline error handling. That is, a pipeline might be configured to write error records to file, but if a stage is configured to discard error records those records are discarded. You might use this functionality to reduce the types of error records that are saved for review and reprocessing.

Note that records missing required fields do not enter the stage. They are passed directly to the pipeline for error handling.

## Pipeline Error Record Handling

Pipeline error record handling determines how Data Pipelines processes error records that stages send to the pipeline for error handling. It also handles records deliberately dropped from the pipeline such as records without required fields.

The pipeline handles error records based on the Error Records property on the Error Records tab. When Data Pipelines encounters an unexpected error, it stops the pipeline and logs the error.Pipelines provide the following error record handling options:

### Discard

The pipeline discards the record. Data Pipelines includes the records in error record counts and metrics.

### Send Response to Origin

The pipeline passes error records back to the microservice origin to be included in a response to the originating REST API client. Data Pipelines includes the records in error record counts and metrics. Use in [microservice pipelines](../microservice-pipelines.md) only.

### Write to BigConnect Object Store / S3

The pipeline writes error records and related details to BigConnect Object Store or any S3-compatible sotre. Data Pipelines includes the records in error record counts and metrics.

You define the Amazon S3 configuration properties.

### Write to Another Pipeline

The pipeline writes error records to an DC RPC pipeline. Data Pipelines includes the records in error record counts and metrics.

When you write to another pipeline, Data Pipelines effectively creates an DC RPC origin pipeline to pass the error records to another pipeline.

You need to create an DC RPC destination pipeline to process the error records. 

The pipeline must include an DC RPC origin configured to read error records from this pipeline.

### Write to Azure Event Hub

The pipeline writes error records and related details to Microsoft Azure Event Hub. Data Pipelines includes the records in error record counts and metrics. 

You define the configuration properties for the Azure Event Hub to use.

### Write to Elasticsearch

The pipeline writes error records and related details to Elasticsearch. Data Pipelines includes the records in error record counts and metrics.

You define the configuration properties for the Elasticsearch cluster to use.

### Write to File

The pipeline writes error records and related details to a local directory. Data Pipelines includes the records in error record counts and metrics.

You define the directory to use and the maximum file size. Error files are named based on the File Prefix pipeline property.

Write to file is not supported for cluster pipelines at this time.

### Write to Google Cloud Storage

The pipeline writes error records and related details to Google Cloud Storage. Data Pipelines includes the records in error record counts and metrics.

You define the Google Cloud Storage configuration properties.

### Write to Google Sub/Pub

The pipeline writes error records and related details to Google Sub/Pub. Data Pipelines includes the records in error record counts and metrics.

You define the Google Sub/Pub configuration properties.

### Write to Kafka

The pipeline writes error records and related details to Kafka. Data Pipelines includes the records in error record counts and metrics.

You define the configuration properties for the Kafka cluster to use.

### Write to Kinesis

The pipeline writes error records and related details to Amazon Kinesis Streams. Data Pipelines includes the records in error record counts and metrics.

You define the configuration properties for the Kinesis stream to use.

### Write to MQTT

The pipeline writes error records and related details to an MQTT broker. DataFabric includes the records in error record counts and metrics.

You define the configuration properties for the MQTT broker to use.

## Stage Error Record Handling

Most stages include error record handling options. When an error occurs when processing a record, Data Pipelines processes records based on the On Record Error property on the General tab of the stage.

Stages include the following error handling options:

### Discard

The stage silently discards the record. Data Pipelines does not log information about the error or note the specific record that encountered an error. The discarded record is not included in Monitor mode error record counts or metrics.

### Send to Error

The stage sends the record to the pipeline for error handling. The pipeline processes the record based on the pipeline error handling configuration.

When you monitor the pipeline, you can view the most recent error records and the issues they encountered on the Error Records tab for the stage. This information becomes unavailable after you stop the pipeline.

### Stop Pipeline

Data Pipelines stops the pipeline and logs information about the error. The error that stopped the pipeline displays as an alert in Monitor mode and as an error in the pipeline history.

Stop pipeline is not supported for cluster mode pipelines at this time.

## Error Records and Version

When Data Pipelines creates an error record, it preserves the data and attributes from the record that triggered the error, and then adds error related information as record header attributes.When you configure a pipeline, you can specify the version of the record that you want to use:

* The original record - The record as originally generated by the origin. Use this record when you want the original record without any additional pipeline processing.
* The current record - The record in the stage that generated the error. Depending on the type of error that occurred, this record can be unprocessed or partially processed by the error-generating stage.

  
  Use this record when you want to preserve any processing that the pipeline completed before the record caused an error.

