# MLeap Evaluator

The MLeap Evaluator processor uses a machine learning model stored in an MLeap bundle to generate evaluations, scoring, or classifications of data.

With the MLeap Evaluator processor, you can create pipelines that produce data-driven insights in real time. For example, you can design pipelines that detect fraudulent transactions or that perform natural language processing as data passes through the pipeline.

To use the MLeap Evaluator processor, you first build and train the model with your preferred machine learning technology. You then export the trained model to an MLeap bundle and save that file on the Data Pipelines machine that runs the pipeline.

When you configure the MLeap Evaluator processor, you define the path to the saved MLeap bundle stored on the Data Pipelines machine. You also define mappings between fields in the record and input fields in the model, and you define the model fields to output and the record field to store the model output.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before configuring the MLeap Evaluator processor, you must complete the following prerequisites:

1. Build and train a machine learning model with your preferred machine learning technology.
2. Export the trained model to an MLeap bundle. For more information, see the [MLeap documentation](https://github.com/combust/mleap-docs/blob/master/core-concepts/mleap-bundles.md).
3. Save the bundle file on the Data Pipelines machine that runs the pipeline. We recommend storing the model directory in the Data Pipelines resources directory, $SDC\_RESOURCES.

### MLeap Model as a Microservice <a href="#ariaid-title3" id="ariaid-title3"></a>

External clients can use a model in an MLeap bundle to perform computations when you include an MLeap Evaluator processor in a microservice pipeline.

For example, in the following microservice pipeline, a REST API client sends a request with input data to the REST Service origin. The MLeap Evaluator processor uses a machine learning model to generate predictions and passes records that contain those predictions from the model to the Send Response to Origin destination, labeled Send Predictions, which sends the records back to the REST Service origin. The origin then transmits JSON-formatted responses back to the originating REST API client.

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/MLeap-MicroservicePipeline.png)

### Configuring an MLeap Evaluator Processor <a href="#ariaid-title5" id="ariaid-title5"></a>

Configure an MLeap Evaluator processor to generate predictions or classifications of data with a machine learning model stored in an MLeap bundle.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the MLeap tab, configure the following properties:

    | MLeap Property        | Description                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Saved Model File Path | <p>Path to the saved MLeap bundle on the Data Pipelines machine. Specify either an absolute path or the path relative to the Data Pipelines resources directory.For example, if you saved a bundle named pricing.model.zip in the Data Pipelines resources directory /var/lib/sdc-resources, then enter either of the following paths:</p><ul><li>/var/lib/sdc-resources/pricing.model.zip</li><li>pricing.model.zip</li></ul> |
    | Input Configs         | <p>Mapping of input fields in the machine learning model to fields in the record. For each mapping, enter:</p><ul><li>MLeap Input Field – An input field in the model stored in the MLeap bundle.</li><li>Field to Convert – The corresponding field in the record, specified as a path.</li></ul>                                                                                                                             |
    | Model Output Fields   | Output fields in the model to return to the pipeline.                                                                                                                                                                                                                                                                                                                                                                          |
    | Output Field          | List-map field that stores model output in the record. Specify as a path.                                                                                                                                                                                                                                                                                                                                                      |
