# Ludwig Basic Principles

Ludwig provides two main functionalities: training models and using them to predict. It is based on datatype abstraction, so that the same data preprocessing and postprocessing will be performed on different datasets that share data types and the same encoding and decoding models developed for one task can be reused for different tasks.

Training a model in Ludwig is pretty straightforward: you provide a CSV dataset and a model definition YAML file.

The model definition contains a list of input features and output features, all you have to do is specify names of the columns in the CSV that are inputs to your model alongside with their datatypes, and names of columns in the CSV that will be outputs, the target variables which the model will learn to predict. Ludwig will compose a deep learning model accordingly and train it for you.

Currently the available datatypes in Ludwig are:

* binary
* numerical
* category
* set
* bag
* sequence
* text
* timeseries
* image
* audio
* date
* h3
* vector

The model definition can contain additional information, in particular how to preprocess each column in the CSV, which encoder and decoder to use for each one, feature hyperparameters and training parameters. This allows ease of use for novices and flexibility for experts.

## Training

For example, given a text classification dataset like the following:

| doc\_text | class |
| :--- | :--- |
| Former president Barack Obama ... | politics |
| Juventus hired Cristiano Ronaldo ... | sport |
| LeBron James joins the Lakers ... | sport |
| ... | ... |

you want to learn a model that uses the content of the `doc_text` column as input to predict the values in the `class` column. You can use the following model definition:

```javascript
{input_features: [{name: doc_text, type: text}], output_features: [{name: class, type: category}]}
```

and start the training typing the following command in your console:

```javascript
ludwig train --data_csv path/to/file.csv --model_definition "{input_features: [{name: doc_text, type: text}], output_features: [{name: class, type: category}]}"
```

where `path/to/file.csv` is the path to a UTF-8 encoded CSV file contaning the dataset in the previous table. Ludwig will perform a random split of the data, preprocess it, build a WordCNN model \(the default for text features\) that decodes output classes through a softmax classifier, train the model on the training set until the accuracy on the validation set stops improving. Training progress will be displayed in the console, but TensorBoard can also be used.

If you prefer to use an RNN encoder and increase the number of epochs you want the model to train for, all you have to do is to change the model definition to:

```javascript
{input_features: [{name: doc_text, type: text, encoder: rnn}], output_features: [{name: class, type: category}], training: {epochs: 50}}
```

Refer to the **User Guide** to find out all the options available to you in the model definition and take a look at the **Examples** to see how you can use Ludwig for several different tasks.

After training, Ludwig will create a directory under `results` containing the trained model with its hyperparameters and summary statistics of the training process. You can visualize them using one of the several visualization options available in the `visualize` tool, for instance:

```javascript
ludwig visualize --visualization learning_curves --training_statistics path/to/training_statistics.json
```

The commands will display a graph that looks like the following, where you can see loss and accuracy as functions of train iteration number:

![](../../../.gitbook/assets/image%20%28484%29.png)

Several visualizations are available, please refer to **Visualizations** for more details.

## Predict

If you have new data and you want your previously trained model to predict target output values, you can type the following command in your console:

```javascript
ludwig predict --data_csv path/to/data.csv --model_path /path/to/model
```

Running this command will return model predictions and some test performance statistics if the dataset contains ground truth information to compare to. Those can be visualized by the `visualize` tool, which can also be used to compare performances and predictions of different models, for instance:

```javascript
ludwig visualize --visualization compare_performance --test_statistics path/to/test_statistics_model_1.json path/to/test_statistics_model_2.json
```

will return a bar plot comparing the models on different measures:

![](../../../.gitbook/assets/image%20%28215%29.png)

A handy `ludwig experiment` command that performs training and prediction one after the other is also available.

## Programmatic API

Ludwig also provides a simple programmatic API that allows you to train or load a model and use it to obtain predictions on new data:

```python
from ludwig.api import LudwigModel

# train a model
model_definition = {...}
model = LudwigModel(model_definition)
train_stats = model.train(training_dataframe)

# or load a model
model = LudwigModel.load(model_path)

# obtain predictions
predictions = model.predict(test_dataframe)

model.close()
```

`model_definition` is a dictionary contaning the same information of the YAML file. More details are provided in the [User Guide](ludwig-user-guide/) and in the [API documentation]().

## Extensibility

Ludwig is built from the ground up with extensibility in mind. It is easy to add an additional datatype that is not currently supported by adding a datatype-specific implementation of abstract classes which contain functions to preprocess the data, encode it, and decode it.

Furthermore, new models, with their own specific hyperparameters, can be easily added by implementing a class that accepts tensors \(of a specific rank, depending of the datatype\) as inputs and provides tensors as output. This encourages reuse and sharing new models with the community. Refer to the [Developer Guide]() for further details.

