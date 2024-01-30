# PMML Evaluator

The PMML Evaluator processor uses a machine learning model stored in the Predictive Model Markup Language (PMML) format to generate predictions or classifications of data.

With the PMML Evaluator processor, you can create pipelines that predict the existence of known patterns in your data and gain real-time insights. For example, you can design pipelines that detect fraud or needed maintenance as data passes through the pipeline.

To use the PMML Evaluator processor, you first build and train the model with your preferred machine learning technology. You then export the trained model to a PMML document and save that file on the Data Pipelines machine that runs the pipeline.

When you configure the PMML Evaluator processor, you define the path to the saved PMML document stored on the Data Pipelines machine. You also define mappings between fields in the record and input fields in the model, and you define the model fields to output and the record field to store the model output.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before configuring the PMML Evaluator processor, you must complete the following prerequisites:

1. Build and train a machine learning model with your preferred machine learning technology.
2. Export the trained model as a PMML document. For more information, see the [Data Mining Group website](http://dmg.org/).
3. Save the PMML document on the Data Pipelines machine that runs the pipeline. StreamSets recommends storing the document in the Data Pipelines resources directory, $SDC\_RESOURCES.
4. Install the PMML stage library on the Data Pipelines machine.

#### Installing the PMML Stage Library <a href="#ariaid-title3" id="ariaid-title3"></a>

To use the PMML Evaluator processor, you must have a paid subscription and install the PMML stage library.

1. Use the link in the email that you received from StreamSets to download the tarball that contains the PMML stage library.
2.  Extract the tarball.The tarball is extracted into the following directory:

    ```
    streamsets-datacollector-pmml-lib
    ```
3. Store the extracted PMML stage library as a custom stage library.

### PMML Model as a Microservice <a href="#ariaid-title4" id="ariaid-title4"></a>

External clients can use a model saved as a PMML document to perform computations when you include a PMML Evaluator processor in a microservice pipeline.

For example, in the following microservice pipeline, a REST API client sends a request with input data to the REST Service origin, labeled PMML Model Serving Service. The PMML Evaluator processor uses a machine learning model to generate predictions from the data. The processor passes records that contain the model's predictions to the Send Response to Origin destination, labeled Send Predictions, which sends the records back to the REST Service origin. The origin then transmits JSON-formatted responses back to the originating REST API client.

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PMML-MicroservicePipeline.png)

### Example: Iris Classification <a href="#ariaid-title5" id="ariaid-title5"></a>

For example, suppose you build and train an Iris classification model and save the model in PMML format. The model predicts the species of Iris based on length and width measurements from a flower's petal and sepal.

You can give the model the following inputs:

```
{
  "petalLength": 6.4,
  "petalWidth": 2.8,
  "sepalLength": 5.6,
  "sepalWidth": 2.2
}
```

And the model generates the following four outputs, which give the predicted species along with the probability of each of the three species:

| Output field            | Value     |
| ----------------------- | --------- |
| Predicted\_Species      | virginica |
| Probability\_setosa     | 0.0       |
| Probability\_versicolor | 0.12      |
| Probability\_virginica  | 0.88      |

To include this model in a pipeline, save the model document on the Data Pipelines machine, add the PMML Evaluator processor to the pipeline, and then configure the processor to use the PMML document and to map the required input fields and generated output fields to fields in the record. The following image shows the processor configuration:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/PMML-ExampleFieldSettings.png)

### Configuring a PMML Evaluator Processor <a href="#ariaid-title6" id="ariaid-title6"></a>

Configure a PMML Evaluator processor to generate predictions or classifications of data with a machine learning model saved as a PMML document.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                                                                |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p>      |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p> |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                |
2.  On the PMML tab, configure the following properties:

    | PMML Property         | Description                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Saved Model File Path | <p>Path to the saved PMML document on the Data Pipelines machine. Specify either an absolute path or the path relative to the Data Pipelines resources directory.For example, if you saved a PMML document named maint.model.pmml in the Data Pipelines resources directory /var/lib/sdc-resources, then enter either of the following paths:</p><ul><li>/var/lib/sdc-resources/maint.model.pmml</li><li>maint.model.pmml</li></ul> |
    | Input Configs         | <p>Mapping of input fields in the machine learning model to fields in the record. For each mapping, enter:</p><ul><li>PMML Input Field – An input field in the PMML model.</li><li>Field to Convert – Corresponding field in the record, specified as a path.</li></ul>                                                                                                                                                             |
    | Model Output Fields   | Output fields in the model to return to the pipeline.                                                                                                                                                                                                                                                                                                                                                                               |
    | Output Field          | List-map field that stores model output in the record. Specify as a path.                                                                                                                                                                                                                                                                                                                                                           |
