# To Error

The To Error destination passes records to the pipeline for error handling. Use the To Error destination to send a stream of records to pipeline error handling.

Typically, error records are generated and dropped from the pipeline individually. With some processors, such as the Stream Selector or JavaScript Evaluator, you can create multiple streams of data, including a stream of records that you consider error records. You can connect an error stream to the To Error destination to pass the records to the pipeline for error handling.

The pipeline performs error handling based on the Error Records properties configured for the pipeline.

As with other destinations, you can monitor the number of records passed to the To Error destination and you can create metric and data alerts. You can also examine the data sent to the destination as part of data preview or a monitor snapshot.

The To Error destination has no configuration options. To use the destination in a pipeline, simply add the stage and connect the pipeline to the stage.
