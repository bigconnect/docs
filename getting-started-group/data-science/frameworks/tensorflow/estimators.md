# Estimators

This document introduces [`tf.estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator)—a high-level TensorFlow API. Estimators encapsulate the following actions:

* training
* evaluation
* prediction
* export for serving

You may either use the pre-made Estimators we provide or write your own custom Estimators. All Estimators—whether pre-made or custom—are classes based on the [`tf.estimator.Estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator) class.

For a quick example try [Estimator tutorials](https://www.tensorflow.org/tutorials/estimator/linear). For an overview of the API design, see the [white paper](https://arxiv.org/abs/1708.02637).

## Advantages

Similar to a [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model), an `estimator` is a model-level abstraction. The [`tf.estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator) provides some capabilities currently still under development for [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras). These are:

* Parameter server based training
* Full [TFX](http://tensorflow.org/tfx) integration.

## Estimators Capabilities

Estimators provide the following benefits:

* You can run Estimator-based models on a local host or on a distributed multi-server environment without changing your model. Furthermore, you can run Estimator-based models on CPUs, GPUs, or TPUs without recoding your model.
* Estimators provide a safe distributed training loop that controls how and when to:
  * load data
  * handle exceptions
  * create checkpoint files and recover from failures
  * save summaries for TensorBoard

When writing an application with Estimators, you must separate the data input pipeline from the model. This separation simplifies experiments with different data sets.

## Pre-made Estimators

Pre-made Estimators enable you to work at a much higher conceptual level than the base TensorFlow APIs. You no longer have to worry about creating the computational graph or sessions since Estimators handle all the "plumbing" for you. Furthermore, pre-made Estimators let you experiment with different model architectures by making only minimal code changes. [`tf.estimator.DNNClassifier`](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier), for example, is a pre-made Estimator class that trains classification models based on dense, feed-forward neural networks.

### Structure of a pre-made Estimators program

A TensorFlow program relying on a pre-made Estimator typically consists of the following four steps:

**1. Write one or more dataset importing functions.**

For example, you might create one function to import the training set and another function to import the test set. Each dataset importing function must return two objects:

* a dictionary in which the keys are feature names and the values are Tensors \(or SparseTensors\) containing the corresponding feature data
* a Tensor containing one or more labels

For example, the following code illustrates the basic skeleton for an input function:

```python
def input_fn(dataset):
    ...  # manipulate dataset, extracting the feature dict and the label
    return feature_dict, label
```

See [data guide](https://www.tensorflow.org/guide/data) for details.

**2. Define the feature columns.**

Each [`tf.feature_column`](https://www.tensorflow.org/api_docs/python/tf/feature_column) identifies a feature name, its type, and any input pre-processing. For example, the following snippet creates three feature columns that hold integer or floating-point data. The first two feature columns simply identify the feature's name and type. The third feature column also specifies a lambda the program will invoke to scale the raw data:

```python
# Define three numeric feature columns.
population = tf.feature_column.numeric_column('population')
crime_rate = tf.feature_column.numeric_column('crime_rate')
median_education = tf.feature_column.numeric_column(
  'median_education',
  normalizer_fn=lambda x: x - global_education_mean)
```

For further information, see the [feature columns tutorial](https://www.tensorflow.org/tutorials/keras/feature_columns).

**3. Instantiate the relevant pre-made Estimator.**

For example, here's a sample instantiation of a pre-made Estimator named `LinearClassifier`:

```python
# Instantiate an estimator, passing the feature columns.
estimator = tf.estimator.LinearClassifier(
  feature_columns=[population, crime_rate, median_education])
```

For further information, see the [linear classifier tutorial](https://www.tensorflow.org/tutorials/estimator/linear).

**4. Call a training, evaluation, or inference method.**

For example, all Estimators provide a `train` method, which trains a model.

```python
# `input_fn` is the function created in Step 1
estimator.train(input_fn=my_training_set, steps=2000)
```

You can see an example of this below.

### **Benefits of pre-made Estimators**

Pre-made Estimators encode best practices, providing the following benefits:

* Best practices for determining where different parts of the computational graph should run, implementing strategies on a single machine or on a cluster.
* Best practices for event \(summary\) writing and universally useful summaries.

If you don't use pre-made Estimators, you must implement the preceding features yourself.

## Custom Estimators

The heart of every Estimator—whether pre-made or custom—is its _model function_, which is a method that builds graphs for training, evaluation, and prediction. When you are using a pre-made Estimator, someone else has already implemented the model function. When relying on a custom Estimator, you must write the model function yourself.

## Recommended workflow

1. Assuming a suitable pre-made Estimator exists, use it to build your first model and use its results to establish a baseline.
2. Build and test your overall pipeline, including the integrity and reliability of your data with this pre-made Estimator.
3. If suitable alternative pre-made Estimators are available, run experiments to determine which pre-made Estimator produces the best results.
4. Possibly, further improve your model by building your own custom Estimator.

```python
from __future__ import absolute_import, division, print_function, unicode_literals
import tensorflow as tf
import tensorflow_datasets as tfds

tfds.disable_progress_bar()
```

## Create an Estimator from a Keras model

You can convert existing Keras models to Estimators with [`tf.keras.estimator.model_to_estimator`](https://www.tensorflow.org/api_docs/python/tf/keras/estimator/model_to_estimator). Doing so enables your Keras model to access Estimator's strengths, such as distributed training.

Instantiate a Keras MobileNet V2 model and compile the model with the optimizer, loss, and metrics to train with:

```python
keras_mobilenet_v2 = tf.keras.applications.MobileNetV2(
    input_shape=(160, 160, 3), include_top=False)

estimator_model = tf.keras.Sequential([
    keras_mobilenet_v2,
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(1, activation='softmax')
])

# Compile the model
estimator_model.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy'])
```

Create an `Estimator` from the compiled Keras model. The initial model state of the Keras model is preserved in the created `Estimator`:

```python
est_mobilenet_v2 = tf.keras.estimator.model_to_estimator(keras_model=estimator_model)
```

Treat the derived `Estimator` as you would with any other `Estimator`.

```python
IMG_SIZE = 160  # All images will be resized to 160x160

def preprocess(image, label):
  image = tf.cast(image, tf.float32)
  image = (image/127.5) - 1
  image = tf.image.resize(image, (IMG_SIZE, IMG_SIZE))
  return image, label

def train_input_fn(batch_size):
  data = tfds.load('cats_vs_dogs', as_supervised=True)
  train_data = data['train']
  train_data = train_data.map(preprocess).shuffle(500).batch(batch_size)
  return train_data
```

To train, call Estimator's train function:

```python
est_mobilenet_v2.train(input_fn=lambda: train_input_fn(32), steps=500)
```

Similarly, to evaluate, call the Estimator's evaluate function:

```python
est_mobilenet_v2.evaluate(input_fn=lambda: train_input_fn(32), steps=10)
```

For more details, please refer to the documentation for [`tf.keras.estimator.model_to_estimator`](https://www.tensorflow.org/api_docs/python/tf/keras/estimator/model_to_estimator).

