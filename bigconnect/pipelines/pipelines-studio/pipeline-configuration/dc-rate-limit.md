# Rate Limit

You can limit the rate at which a Data Pipelines pipeline processes records by defining the maximum number of records that the pipeline can read in a second

By default, a Data Pipelines pipeline has no rate limit. You might want to limit the rate for the following reasons:

**The pipeline reads records faster than it can write them to the destination system**

Because a pipeline processes one batch at a time, pipeline performance slows when a pipeline reads records faster than it can process them or write them to the destination system. The pipeline must wait until a batch is committed to the destination system before reading the next batch, causing a delay before the next batch is read and preventing the pipeline from reading at a steady rate. Reading data at a steady rate provides better performance than reading sporadically.

You can limit the rate at which the pipeline reads records to decrease the delay between reads from the origin system.

**The origin system requires that the data be read at a slower rate**

If the origin system is being used for other purposes, it might not be able to handle the rate at which the pipeline reads records. You can limit the rate to meet the origin system requirements.

Use the **Rate Limit** pipeline property to define the maximum number of records that the pipeline can read in a second.
