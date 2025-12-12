# TensorFlow Evaluator

The TensorFlow Evaluator processor uses a TensorFlow or PyTorch  machine learning model to generate predictions or classifications of data.&#x20;

Using the TensorFlow Evaluator processor, you can design pipelines that read data and then generate predictions or classifications of the data during the pipeline processing - producing data-driven insights in real time. For example, you can design pipelines that detect fraudulent transactions or that perform natural language processing as data passes through the pipeline.

To use the TensorFlow Evaluator processor, you first build and train the model in TensorFlow. You then save the trained model to file and store the saved model directory on the Data Pipelines machine that runs the pipeline.

When you configure the TensorFlow Evaluator processor, you define the path to the saved model stored on the Data Pipelines machine. You also define the input and output tensor information as configured during the building and training of the model.

You configure whether the processor evaluates each record or evaluates the entire batch at once. When evaluating the entire batch, the processor writes the prediction or classification results to events.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you configure a TensorFlow Evaluator processor, complete the following prerequisites:

Build and train the model in TensorFlowBuild and train the model in TensorFlow. The processor uses version 1.15 of the TensorFlow client library and supports TensorFlow version 1.x.

Save and store the trained model on the Data Pipelines machine.

Save the trained model to file in [SavedModel format](https://www.tensorflow.org/guide/saved_model). When you save a model in SavedModel format, TensorFlow creates a directory consisting of the following subdirectories and files:

```
assets/
assets.extra/
variables/
saved_model.pb
```

Store the complete SavedModel directory on the Data Pipelines machine where you plan to run the pipeline. We recommend storing the model directory in the Data Pipelines resources directory, $SDC\_RESOURCES.&#x20;

### Evaluation Method <a href="#ariaid-title3" id="ariaid-title3"></a>

The TensorFlow Evaluator processor can evaluate each record or evaluate the entire batch at once.

Configure the processor to use one of the following evaluation methods, based on the input that the tensor expects:

Evaluate each recordIf the tensor requires one input to produce one output, configure the TensorFlow Evaluator processor to evaluate each record. By default, the processor evaluates each record, producing one output per record.

The processor receives each record as one input, performs the tensor computations to predict or classify the data, and then produces one output. The output includes all original fields in the record plus an additional output field that includes the prediction or classification result. The output field is a map or list field containing a field for each output that you configure for the processor.

To evaluate each record, ensure that the following processor properties are cleared:

* On the General tab, clear the Produce Events property.
* On the TensorFlow tab, clear the Entire Batch property.

Evaluate the entire batchIf the tensor requires multiple inputs to produce one output, configure the TensorFlow Evaluator processor to evaluate the entire batch.When evaluating a batch, the processor waits until it receives all records in the batch, performs the tensor computations to predict or classify the data, and then produces one output as an event for the entire batch. The processor output includes the original fields in each record. The event output includes the prediction or classification result.To evaluate the entire batch, ensure that the following processor properties are selected:

* On the General tab, select the Produce Events property.
* On the TensorFlow tab, select the Entire Batch property.

Then, connect the event stream from the TensorFlow Evaluator processor to a destination to store the prediction or classification result, as described in Event Generation.

#### Example: Each Record <a href="#ariaid-title4" id="ariaid-title4"></a>

In this example, we'll look at how to configure the TensorFlow Evaluator processor to evaluate each record using the sample Iris classification model.

The model predicts the species of Iris flowers based on the size of their sepals and petals. Each incoming record contains fields with petal and sepal measurements. The processor predicts the species for each record, assigning one of the following long values to an output field added to each record:

* 0 - Represents the Iris setosa species.
* 1 - Represents the Iris versicolor species.
* 2 - Represents the Iris virginica species.

In this example, we'll create the following simple pipeline:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/TensorFlowEachRecord.png)

We just want to preview the TensorFlow Evaluator processor output in our example, so we'll simply use the Trash destination. Feel free to use another destination of your choice.

Complete the following steps to configure the TensorFlow Evaluator processor to predict the species of Iris flowers:

1.  Load the sample Iris tensor model and the store the model in the Data Pipelines resources directory, $SDC\_RESOURCES.For example:

    ```
    /var/lib/sdc-resources/iris_saved_model
    ```

    Because the sample Iris classification model has already been built and trained, we can skip that prerequisite for this example. For more information about how the model was built and trained, see [Premade Estimators](https://www.tensorflow.org/guide/estimator) in the TensorFlow documentation.
2.  Create a Data Pipelines pipeline and add a Dev Raw Data Source origin.Add the following raw data to the origin, and then configure the origin to process the JSON data format:

    ```
    {
      "petalLength": 6.4,
      "petalWidth": 2.8,
      "sepalLength": 5.6,
      "sepalWidth": 2.2
    }
    {
      "petalLength": 5.0,
      "petalWidth": 2.3,
      "sepalLength": 3.3,
      "sepalWidth": 1.0
    }
    {
      "petalLength": 4.9,
      "petalWidth": 2.5,
      "sepalLength": 4.5,
      "sepalWidth": 1.7
    }
    {
      "petalLength": 4.9,
      "petalWidth": 3.1,
      "sepalLength": 1.5,
      "sepalWidth": 0.1
    }
    ```
3. The sample Iris model expects float data as input, so add a Field Type Converter processor to the pipeline.On the Conversions tab of the processor, set the conversion method to By Field Name, and then configure the conversion properties as follows to convert all fields to float:
   * Fields to Convert - /\*
   * Conversion Type - FLOAT
4.  Add a TensorFlow Evaluator processor to the pipeline.

    On the General tab, ensure that Produce Events is cleared because we need the processor to evaluate each record.

    On the TensorFlow tab, define the saved model path and model tags as follows:

    * Saved Model Path - Enter the absolute path where you saved the model on the Data Pipelines machine. For example: `/var/lib/sdc-resources/iris_saved_model`.
    * Model Tags - serve

    Configure four input configurations with the following values:

    | Operation   | Index | Fields to Convert | Shape | Tensor Type |
    | ----------- | ----- | ----------------- | ----- | ----------- |
    | PetalLength | 0     | /petalLength      | 1     | FLOAT       |
    | PetalWidth  | 0     | /petalWidth       | 1     | FLOAT       |
    | SepalLength | 0     | /sepalLength      | 1     | FLOAT       |
    | SepalWidth  | 0     | /sepalWidth       | 1     | FLOAT       |

    For example, the first input configuration should look like this:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/TensorFlowEachRecordInputConfig.png)

    Configure two output configurations with the following values:

    | Operation                          | Index | Tensor Type |
    | ---------------------------------- | ----- | ----------- |
    | dnn/head/predictions/ExpandDims    | 0     | FLOAT       |
    | dnn/head/predictions/probabilities | 0     | FLOAT       |

    Ensure that Entire Batch is cleared, and then leave the default value of /output for the Output Field.
5. Finally, add a Trash destination to the pipeline.

When we run preview and view the output of the TensorFlow Evaluator processor, we can see that each field in the record is passed through the processor untouched. The processor generates an output map field for each record that contains the prediction results in the two fields that we defined in the output configurations. As shown below, the processor predicts the species of the first record to be 2, representing the Iris virginica species:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/TensorFlowEachRecordPreview.png)

To add to our example, we could add an Expression Evaluator processor following the TensorFlow Evaluator processor to evaluate the prediction value of 0, 1, or 2 stored in the output field and then create a new field with one of the following appropriate values:

* Iris setosa
* Iris versicolor
* Iris virginica

### Event Generation <a href="#ariaid-title5" id="ariaid-title5"></a>

When configured to evaluate the entire batch, the TensorFlow Evaluator processor can generate events. The events contain the results of the prediction or classification made for the batch.

Important: Configure the TensorFlow Evaluator processor to generate events only when the processor is configured to evaluate the entire batch.

TensorFlow Evaluator events can be used in any logical way. For example:

* With a destination to store event information.
*   With a data alert to send notifications.

    You can configure data alerts on the event stream to notify upon reaching specified thresholds.

#### Event Records <a href="#ariaid-title6" id="ariaid-title6"></a>

Event records generated by the TensorFlow Evaluator processor have the following event-related record header attributes. Record header attributes are stored as String values:

| Record Header Attribute       | Description                                                                                                                                                  |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| sdc.event.type                | <p>Event type. Uses the following event type:</p><ul><li>tensorflow-event - Contains the results of the prediction or classification of the batch.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                 |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                            |

The TensorFlow Evaluator processor generates a tensorflow-event record when the processor completes processing all records in the batch. The event record is a Map field containing a field for each output configuration that you define for the processor.

### Serving a TensorFlow Model <a href="#ariaid-title7" id="ariaid-title7"></a>

If you include a TensorFlow Evaluator processor in a microservice pipeline, you can serve the TensorFlow model in the running pipeline.

When you serve a TensorFlow model, external clients can use the model to perform computations. In a microservice pipeline, a client makes a REST API call to the origin. The microservice pipeline performs all processing - which can include the predictions or classifications made by the TensorFlow Evaluator processor. The records with the TensorFlow prediction or classification result are sent back to the microservice pipeline origin. The origin then transmits JSON-formatted responses back to the originating REST API client.

### Configuring a TensorFlow Evaluator <a href="#ariaid-title8" id="ariaid-title8"></a>

Configure a TensorFlow Evaluator processor to use a TensorFlow machine learning model to generate predictions or classifications of data.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                                                                            |
    | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                                                                            |
    | Description      | Optional description.                                                                                                                                                                                                                                                  |
    | Produce Events   | Generates event records when events occur. Use for event handling.                                                                                                                                                                                                     |
    | Required Fields  | <p>Fields that must include data for the record to be passed into the stage.</p><p>Tip: You might include fields that the stage uses.</p><p>Records that do not include all required fields are processed based on the error handling configured for the pipeline.</p> |
    | Preconditions    | <p>Conditions that must evaluate to TRUE to allow a record to enter the stage for processing. Click Add to create additional preconditions.</p><p>Records that do not meet all preconditions are processed based on the error handling configured for the stage.</p>   |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline. Not valid for cluster pipelines.</li></ul>                  |
2.  On the TensorFlow tab, configure the following properties:

    | TensorFlow Property   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Saved Model Path      | <p>Path to the saved TensorFlow model on the Data Pipelines machine. Specify either an absolute path or the path relative to the Data Pipelines resources directory.For example, if you saved a model named my_saved_model to the Data Pipelines resources directory /var/lib/sdc-resources, then enter either of the following paths:</p><ul><li>/var/lib/sdc-resources/my_saved_model</li><li>my_saved_model</li></ul>                                                                                                                                                                                                                                                                                                                         |
    | Model Tags            | Tags applied to the TensorFlow model when the model was built and trained.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Input Configurations  | <p>Tensor input information configured during the building and training of the model.Define one or more input configurations, configuring the following properties for each:</p><ul><li>Operation - Operation to perform on the inputs.</li><li>Index - Position of this input in the matrix of inputs.</li><li><p>Fields to Convert - Fields in the record to convert to tensor fields as required by the input operation.</p><p>Specify one or more fields, or configure a field path expression to define a set of fields.</p></li><li>Shape - Number of elements in each dimension.</li><li>Tensor Type - Data type of the tensor.</li></ul><p>Using simple or bulk edit mode, click the Add icon to define another input configuration.</p> |
    | Output Configurations | <p>Tensor output information configured during the building and training of the model.Define one or more output configurations, configuring the following properties for each:</p><ul><li>Operation - Operation to perform on the outputs.</li><li>Index - Position of this output in the matrix of outputs.</li><li>Tensor Type - Data type of the tensor.</li></ul><p>Using simple or bulk edit mode, click the Add icon to define another output configuration.</p>                                                                                                                                                                                                                                                                           |
    | Entire Batch          | <p>Evaluates the entire batch at once. Select when the TensorFlow model requires many inputs to generate one output. Clear when the TensorFlow model requires one input to generate one output.</p><p>Default is cleared.</p><p>If selected, you must also configure the processor to generate events so that the processor produces one output as an event for the entire batch. The event output includes the prediction or classification result.</p>                                                                                                                                                                                                                                                                                         |
    | Output Field          | If evaluating each record, the output field for the prediction or classification result. The processor adds the output field to each record.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
