# Keras

This guide gives you the basics to get started with Keras. It's a 10-minute read.

## Import tf.keras

[`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) is TensorFlow's implementation of the [Keras API specification](https://keras.io/). This is a high-level API to build and train models that includes first-class support for TensorFlow-specific functionality, such as [eager execution](https://www.tensorflow.org/guide/eager), [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) pipelines, and [Estimators](https://www.tensorflow.org/guide/estimator). [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) makes TensorFlow easier to use without sacrificing flexibility and performance.

To get started, import [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) as part of your TensorFlow program setup:

```python
from __future__ import absolute_import, division, print_function, unicode_literals

import tensorflow as tf

from tensorflow import keras
```

[`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) can run any Keras-compatible code, but keep in mind:

* The [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) version in the latest TensorFlow release might not be the same as the latest `keras` version from PyPI. Check [`tf.keras.`**`version`**](https://www.tensorflow.org/api_docs/python/tf/keras#__version__).
* When [saving a model's weights](https://www.tensorflow.org/guide/keras/save_and_serialize), [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) defaults to the [checkpoint format](https://www.tensorflow.org/guide/checkpoint). Pass `save_format='h5'` to use HDF5 \(or pass a filename that ends in `.h5`\).

## Build a simple model

### Sequential model

In Keras, you assemble _layers_ to build _models_. A model is \(usually\) a graph of layers. The most common type of model is a stack of layers: the [`tf.keras.Sequential`](https://www.tensorflow.org/api_docs/python/tf/keras/Sequential) model.

To build a simple, fully-connected network \(i.e. multi-layer perceptron\):

```python
from tensorflow.keras import layers

model = tf.keras.Sequential()
# Adds a densely-connected layer with 64 units to the model:
model.add(layers.Dense(64, activation='relu'))
# Add another:
model.add(layers.Dense(64, activation='relu'))
# Add a softmax layer with 10 output units:
model.add(layers.Dense(10, activation='softmax'))
```

You can find a complete, short example of how to use Sequential models [here](https://www.tensorflow.org/tutorials/quickstart/beginner).

To learn about building more advanced models than Sequential models, see: - [Guide to the Keras Functional API](https://www.tensorflow.org/guide/keras/functional) - [Guide to writing layers and models from scratch with subclassing](https://www.tensorflow.org/guide/keras/custom_layers_and_models)

### Configure the layers

There are many [`tf.keras.layers`](https://www.tensorflow.org/api_docs/python/tf/keras/layers) available. Most of them share some common constructor arguments:

* `activation`: Set the activation function for the layer. This parameter is specified by the name of a built-in function or as a callable object. By default, no activation is applied.
* `kernel_initializer` and `bias_initializer`: The initialization schemes that create the layer's weights \(kernel and bias\). This parameter is a name or a callable object. This defaults to the `"Glorot uniform"` initializer.
* `kernel_regularizer` and `bias_regularizer`: The regularization schemes that apply the layer's weights \(kernel and bias\), such as L1 or L2 regularization. By default, no regularization is applied.

The following instantiates [`tf.keras.layers.Dense`](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense) layers using constructor arguments:

```python
# Create a sigmoid layer:
layers.Dense(64, activation='sigmoid')
# Or:
layers.Dense(64, activation=tf.keras.activations.sigmoid)

# A linear layer with L1 regularization of factor 0.01 applied to the kernel matrix:
layers.Dense(64, kernel_regularizer=tf.keras.regularizers.l1(0.01))

# A linear layer with L2 regularization of factor 0.01 applied to the bias vector:
layers.Dense(64, bias_regularizer=tf.keras.regularizers.l2(0.01))

# A linear layer with a kernel initialized to a random orthogonal matrix:
layers.Dense(64, kernel_initializer='orthogonal')

# A linear layer with a bias vector initialized to 2.0s:
layers.Dense(64, bias_initializer=tf.keras.initializers.Constant(2.0))
```

## Train and evaluate

### Set up training

After the model is constructed, configure its learning process by calling the `compile` method:

```python
model = tf.keras.Sequential([
# Adds a densely-connected layer with 64 units to the model:
layers.Dense(64, activation='relu', input_shape=(32,)),
# Add another:
layers.Dense(64, activation='relu'),
# Add a softmax layer with 10 output units:
layers.Dense(10, activation='softmax')])

model.compile(optimizer=tf.keras.optimizers.Adam(0.01),
              loss='categorical_crossentropy',
              metrics=['accuracy'])
```

[`tf.keras.Model.compile`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#compile) takes three important arguments:

* `optimizer`: This object specifies the training procedure. Pass it optimizer instances from the [`tf.keras.optimizers`](https://www.tensorflow.org/api_docs/python/tf/keras/optimizers) module, such as [`tf.keras.optimizers.Adam`](https://www.tensorflow.org/api_docs/python/tf/keras/optimizers/Adam) or [`tf.keras.optimizers.SGD`](https://www.tensorflow.org/api_docs/python/tf/keras/optimizers/SGD). If you just want to use the default parameters, you can also specify optimizers via strings, such as `'adam'` or `'sgd'`.
* `loss`: The function to minimize during optimization. Common choices include mean square error \(`mse`\), `categorical_crossentropy`, and `binary_crossentropy`. Loss functions are specified by name or by passing a callable object from the [`tf.keras.losses`](https://www.tensorflow.org/api_docs/python/tf/keras/losses) module.
* `metrics`: Used to monitor training. These are string names or callables from the [`tf.keras.metrics`](https://www.tensorflow.org/api_docs/python/tf/keras/metrics) module.
* Additionally, to make sure the model trains and evaluates eagerly, you can make sure to pass `run_eagerly=True` as a parameter to compile.

The following shows a few examples of configuring a model for training:

```python
# Configure a model for mean-squared error regression.
model.compile(optimizer=tf.keras.optimizers.Adam(0.01),
              loss='mse',       # mean squared error
              metrics=['mae'])  # mean absolute error

# Configure a model for categorical classification.
model.compile(optimizer=tf.keras.optimizers.RMSprop(0.01),
              loss=tf.keras.losses.CategoricalCrossentropy(),
              metrics=[tf.keras.metrics.CategoricalAccuracy()])
```

### Train from NumPy data

For small datasets, use in-memory [NumPy](https://www.numpy.org/) arrays to train and evaluate a model. The model is "fit" to the training data using the `fit` method:

```python
import numpy as np

data = np.random.random((1000, 32))
labels = np.random.random((1000, 10))

model.fit(data, labels, epochs=10, batch_size=32)
```

[`tf.keras.Model.fit`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#fit) takes three important arguments:

* `epochs`: Training is structured into _epochs_. An epoch is one iteration over the entire input data \(this is done in smaller batches\).
* `batch_size`: When passed NumPy data, the model slices the data into smaller batches and iterates over these batches during training. This integer specifies the size of each batch. Be aware that the last batch may be smaller if the total number of samples is not divisible by the batch size.
* `validation_data`: When prototyping a model, you want to easily monitor its performance on some validation data. Passing this argument—a tuple of inputs and labels—allows the model to display the loss and metrics in inference mode for the passed data, at the end of each epoch.

Here's an example using `validation_data`:

```python
import numpy as np

data = np.random.random((1000, 32))
labels = np.random.random((1000, 10))

val_data = np.random.random((100, 32))
val_labels = np.random.random((100, 10))

model.fit(data, labels, epochs=10, batch_size=32,
          validation_data=(val_data, val_labels))
```

### Train from tf.data datasets

Use the [Datasets API](https://www.tensorflow.org/guide/data) to scale to large datasets or multi-device training. Pass a [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) instance to the `fit` method:

```python
# Instantiates a toy dataset instance:
dataset = tf.data.Dataset.from_tensor_slices((data, labels))
dataset = dataset.batch(32)

model.fit(dataset, epochs=10)
```

Since the `Dataset` yields batches of data, this snippet does not require a `batch_size`.

Datasets can also be used for validation:

```python
dataset = tf.data.Dataset.from_tensor_slices((data, labels))
dataset = dataset.batch(32)

val_dataset = tf.data.Dataset.from_tensor_slices((val_data, val_labels))
val_dataset = val_dataset.batch(32)

model.fit(dataset, epochs=10,
          validation_data=val_dataset)
```

### Evaluate and predict

The [`tf.keras.Model.evaluate`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#evaluate) and [`tf.keras.Model.predict`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#predict) methods can use NumPy data and a [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset).

Here's how to _evaluate_ the inference-mode loss and metrics for the data provided:

```python
# With Numpy arrays
data = np.random.random((1000, 32))
labels = np.random.random((1000, 10))

model.evaluate(data, labels, batch_size=32)

# With a Dataset
dataset = tf.data.Dataset.from_tensor_slices((data, labels))
dataset = dataset.batch(32)

model.evaluate(dataset)
```

And here's how to _predict_ the output of the last layer in inference for the data provided, as a NumPy array:

```python
result = model.predict(data, batch_size=32)
print(result.shape)
```

For a complete guide on training and evaluation, including how to write custom training loops from scratch, see the [guide to training and evaluation](https://www.tensorflow.org/guide/keras/train_and_evaluate).

## Build complex models

### The Functional API

The [`tf.keras.Sequential`](https://www.tensorflow.org/api_docs/python/tf/keras/Sequential) model is a simple stack of layers that cannot represent arbitrary models. Use the [Keras functional API](https://www.tensorflow.org/guide/keras/functional) to build complex model topologies such as:

* Multi-input models,
* Multi-output models,
* Models with shared layers \(the same layer called several times\),
* Models with non-sequential data flows \(e.g. residual connections\).

Building a model with the functional API works like this:

1. A layer instance is callable and returns a tensor.
2. Input tensors and output tensors are used to define a [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model) instance.
3. This model is trained just like the `Sequential` model.

The following example uses the functional API to build a simple, fully-connected network:

```python
inputs = tf.keras.Input(shape=(32,))  # Returns an input placeholder

# A layer instance is callable on a tensor, and returns a tensor.
x = layers.Dense(64, activation='relu')(inputs)
x = layers.Dense(64, activation='relu')(x)
predictions = layers.Dense(10, activation='softmax')(x)
```

Instantiate the model given inputs and outputs.

```python
model = tf.keras.Model(inputs=inputs, outputs=predictions)

# The compile step specifies the training configuration.
model.compile(optimizer=tf.keras.optimizers.RMSprop(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# Trains for 5 epochs
model.fit(data, labels, batch_size=32, epochs=5)
```

### Model subclassing

Build a fully-customizable model by subclassing [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model) and defining your own forward pass. Create layers in the `__init__` method and set them as attributes of the class instance. Define the forward pass in the `call` method.

Model subclassing is particularly useful when [eager execution](https://www.tensorflow.org/guide/eager) is enabled, because it allows the forward pass to be written imperatively.

{% hint style="info" %}
If you need your model to always run imperatively, you can set dynamic=True when calling the super constructor.
{% endhint %}

{% hint style="info" %}
Use the right API for the job. While model subclassing offers flexibility, it comes at a cost of greater complexity and more opportunities for user errors. If possible, prefer the functional API.
{% endhint %}

The following example shows a subclassed [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model) using a custom forward pass that does not have to be run imperatively:

```python
class MyModel(tf.keras.Model):

  def __init__(self, num_classes=10):
    super(MyModel, self).__init__(name='my_model')
    self.num_classes = num_classes
    # Define your layers here.
    self.dense_1 = layers.Dense(32, activation='relu')
    self.dense_2 = layers.Dense(num_classes, activation='sigmoid')

  def call(self, inputs):
    # Define your forward pass here,
    # using layers you previously defined (in `__init__`).
    x = self.dense_1(inputs)
    return self.dense_2(x)
```

Instantiate the new model class:

```python
model = MyModel(num_classes=10)

# The compile step specifies the training configuration.
model.compile(optimizer=tf.keras.optimizers.RMSprop(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# Trains for 5 epochs.
model.fit(data, labels, batch_size=32, epochs=5)
```

### Custom layers

Create a custom layer by subclassing [`tf.keras.layers.Layer`](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Layer) and implementing the following methods:

* `__init__`: Optionally define sublayers to be used by this layer.
* `build`: Create the weights of the layer. Add weights with the `add_weight` method.
* `call`: Define the forward pass.
* Optionally, a layer can be serialized by implementing the `get_config` method and the `from_config` class method.

Here's an example of a custom layer that implements a `matmul` of an input with a kernel matrix:

```python
class MyLayer(layers.Layer):

  def __init__(self, output_dim, **kwargs):
    self.output_dim = output_dim
    super(MyLayer, self).__init__(**kwargs)

  def build(self, input_shape):
    # Create a trainable weight variable for this layer.
    self.kernel = self.add_weight(name='kernel',
                                  shape=(input_shape[1], self.output_dim),
                                  initializer='uniform',
                                  trainable=True)

  def call(self, inputs):
    return tf.matmul(inputs, self.kernel)

  def get_config(self):
    base_config = super(MyLayer, self).get_config()
    base_config['output_dim'] = self.output_dim
    return base_config

  @classmethod
  def from_config(cls, config):
    return cls(**config)
```

Create a model using your custom layer:

```python
model = tf.keras.Sequential([
    MyLayer(10),
    layers.Activation('softmax')])

# The compile step specifies the training configuration
model.compile(optimizer=tf.keras.optimizers.RMSprop(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# Trains for 5 epochs.
model.fit(data, labels, batch_size=32, epochs=5)
```

Learn more about creating new layers and models from scratch with subclassing in the [Guide to writing layers and models from scratch](https://www.tensorflow.org/guide/keras/custom_layers_and_models).

## Callbacks

A callback is an object passed to a model to customize and extend its behavior during training. You can write your own custom callback, or use the built-in [`tf.keras.callbacks`](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks) that include:

* [`tf.keras.callbacks.ModelCheckpoint`](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ModelCheckpoint): Save checkpoints of your model at regular intervals.
* [`tf.keras.callbacks.LearningRateScheduler`](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/LearningRateScheduler): Dynamically change the learning rate.
* [`tf.keras.callbacks.EarlyStopping`](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/EarlyStopping): Interrupt training when validation performance has stopped improving.
* [`tf.keras.callbacks.TensorBoard`](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/TensorBoard): Monitor the model's behavior using [TensorBoard](https://tensorflow.org/tensorboard).

To use a [`tf.keras.callbacks.Callback`](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/Callback), pass it to the model's `fit` method:

```python
callbacks = [
  # Interrupt training if `val_loss` stops improving for over 2 epochs
  tf.keras.callbacks.EarlyStopping(patience=2, monitor='val_loss'),
  # Write TensorBoard logs to `./logs` directory
  tf.keras.callbacks.TensorBoard(log_dir='./logs')
]
model.fit(data, labels, batch_size=32, epochs=5, callbacks=callbacks,
          validation_data=(val_data, val_labels))
```

## Save and restore

### Save just the weights values

Save and load the weights of a model using [`tf.keras.Model.save_weights`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#save_weights):

```python
model = tf.keras.Sequential([
layers.Dense(64, activation='relu', input_shape=(32,)),
layers.Dense(10, activation='softmax')])

model.compile(optimizer=tf.keras.optimizers.Adam(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])
```

```python
# Save weights to a TensorFlow Checkpoint file
model.save_weights('./weights/my_model')

# Restore the model's state,
# this requires a model with the same architecture.
model.load_weights('./weights/my_model')
```

By default, this saves the model's weights in the [TensorFlow checkpoint](https://www.tensorflow.org/guide/checkpoint) file format. Weights can also be saved to the Keras HDF5 format \(the default for the multi-backend implementation of Keras\):

```python
# Save weights to a HDF5 file
model.save_weights('my_model.h5', save_format='h5')

# Restore the model's state
model.load_weights('my_model.h5')
```

### Save just the model configuration

A model's configuration can be saved—this serializes the model architecture without any weights. A saved configuration can recreate and initialize the same model, even without the code that defined the original model. Keras supports JSON and YAML serialization formats:

```python
# Serialize a model to JSON format
json_string = model.to_json()
```

```python
import json
import pprint
pprint.pprint(json.loads(json_string))
```

Recreate the model \(newly initialized\) from the JSON:

```python
fresh_model = tf.keras.models.model_from_json(json_string)
```

Serializing a model to YAML format requires that you install `pyyaml` _before you import TensorFlow_:

```python
yaml_string = model.to_yaml()
print(yaml_string)
```

Recreate the model from the YAML:

```python
fresh_model = tf.keras.models.model_from_yaml(yaml_string)
```

{% hint style="warning" %}
Caution: Subclassed models are not serializable because their architecture is defined by the Python code in the body of the call method.
{% endhint %}

### Save the entire model in one file

The entire model can be saved to a file that contains the weight values, the model's configuration, and even the optimizer's configuration. This allows you to checkpoint a model and resume training later—from the exact same state—without access to the original code.

```python
# Create a simple model
model = tf.keras.Sequential([
  layers.Dense(10, activation='softmax', input_shape=(32,)),
  layers.Dense(10, activation='softmax')
])
model.compile(optimizer='rmsprop',
              loss='categorical_crossentropy',
              metrics=['accuracy'])
model.fit(data, labels, batch_size=32, epochs=5)


# Save entire model to a HDF5 file
model.save('my_model.h5')

# Recreate the exact same model, including weights and optimizer.
model = tf.keras.models.load_model('my_model.h5')
```

Learn more about saving and serialization for Keras models in the guide to [save and serialize models](https://www.tensorflow.org/guide/keras/save_and_serialize).

## Eager execution

[Eager execution](https://www.tensorflow.org/guide/eager) is an imperative programming environment that evaluates operations immediately. This is not required for Keras, but is supported by [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) and useful for inspecting your program and debugging.

All of the [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) model-building APIs are compatible with eager execution. And while the `Sequential` and functional APIs can be used, eager execution especially benefits _model subclassing_ and building _custom layers_—the APIs that require you to write the forward pass as code \(instead of the APIs that create models by assembling existing layers\).

See the [eager execution guide](https://www.tensorflow.org/guide/eager) for examples of using Keras models with custom training loops and [`tf.GradientTape`](https://www.tensorflow.org/api_docs/python/tf/GradientTape). You can also find a complete, short example [here](https://www.tensorflow.org/tutorials/quickstart/advanced).

## Distribution

### Multiple GPUs

[`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) models can run on multiple GPUs using [`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy). This API provides distributed training on multiple GPUs with almost no changes to existing code.

Currently, [`tf.distribute.MirroredStrategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/MirroredStrategy) is the only supported distribution strategy. `MirroredStrategy` does in-graph replication with synchronous training using all-reduce on a single machine. To use [`distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy)s , nest the optimizer instantiation and model construction and compilation in a `Strategy`'s `.scope()`, then train the model.

The following example distributes a [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model) across multiple GPUs on a single machine.

First, define a model inside the distributed strategy scope:

```python
strategy = tf.distribute.MirroredStrategy()

with strategy.scope():
  model = tf.keras.Sequential()
  model.add(layers.Dense(16, activation='relu', input_shape=(10,)))
  model.add(layers.Dense(1, activation='sigmoid'))

  optimizer = tf.keras.optimizers.SGD(0.2)

  model.compile(loss='binary_crossentropy', optimizer=optimizer)

model.summary()
```

Next, train the model on data as usual:

```python
x = np.random.random((1024, 10))
y = np.random.randint(2, size=(1024, 1))
x = tf.cast(x, tf.float32)
dataset = tf.data.Dataset.from_tensor_slices((x, y))
dataset = dataset.shuffle(buffer_size=1024).batch(32)

model.fit(dataset, epochs=1)
```

For more information, see the [full guide on Distributed Training in TensorFlow](https://www.tensorflow.org/guide/distributed_training).

