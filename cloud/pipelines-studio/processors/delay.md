# Delay

The Delay processor delays passing a batch to rest of the pipeline by the specified number of milliseconds. Use the Delay processor to delay pipeline processing.

To limit the volume of data that can be processed at one time, specify the Rate Limit pipeline property.

### Configuring a Delay Processor <a href="#ariaid-title2" id="ariaid-title2"></a>

Configure a Delay processor to delay passing a batch to the next stage in the pipeline.

On the General tab, configure the following properties:

| General Property      | Description                                                                                                                                                                                                                                                          |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name                  | Stage name.                                                                                                                                                                                                                                                          |
| Description           | Optional description.                                                                                                                                                                                                                                                |
| Delay Between Batches | Number of milliseconds to hold each batch before passing it to the rest of the pipeline.                                                                                                                                                                             |
| Required Fields       | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
| Preconditions         | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
| On Record Error       | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
