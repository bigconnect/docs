# Pipeline Functions

Use pipeline functions to determine information about a pipeline, such as the pipeline title or ID. The expression language provides the following pipeline functions:

### pipeline:id()

Returns the ID of the pipeline. The ID is a UUID automatically generated when the pipeline is created and is used by Data Collector to identify the pipeline. The pipeline ID cannot be changed.

Return type: String.

For example, you can include the function in the directory template for the Hadoop FS destination to include the pipeline ID in the output directory name. The following expression creates output directories based on the pipeline ID and timestamp of a record:

```
/outputfiles/${pipeline:id()}/${YYYY()}-${MM()}-${DD()}-${hh()}
```

### pipeline:name()

Like `pipeline:id`, this function returns the ID of the pipeline. The ID is a UUID automatically generated when the pipeline is created and is used by Data Collector to identify the pipeline. The pipeline ID cannot be changed.In Data Collector versions earlier than 2.3.0.0, this function returned the pipeline name. This function is retained for backward compatibility. Use `pipeline:id` for new development.

Return type: String.

For example, you can include the function in the directory template for the Hadoop FS destination to include the pipeline ID in the output directory name. The following expression creates output directories based on the pipeline ID and timestamp of a record:

```
/outputfiles/${pipeline:name()}/${YYYY()}-${MM()}-${DD()}-${hh()}
```

### pipeline:startTime()

Returns the start time of the pipeline.

Return type: Datetime.

### pipeline:title()

Returns the title or name of the pipeline.

Return type: String.

### pipeline:user()

Returns the user who started the pipeline.

Return type: String.

