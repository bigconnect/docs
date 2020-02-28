# Using the SavedModel format

A SavedModel contains a complete TensorFlow program, including weights and computation. It does not require the original model building code to run, which makes it useful for sharing or deploying \(with [TFLite](https://tensorflow.org/lite), [TensorFlow.js](https://js.tensorflow.org/), [TensorFlow Serving](https://www.tensorflow.org/tfx/serving/tutorials/Serving_REST_simple), or [TensorFlow Hub](https://tensorflow.org/hub)\).

If you have code for a model in Python and merely want to load weights into it, see the guide to [training checkpoints](training-checkpoints.md).

## Creating a SavedModel from Keras

For a quick introduction, this section exports a pre-trained Keras model and serves image classification requests with it. The rest of the guide will fill in details and discuss other ways to create SavedModels.

```python
from __future__ import absolute_import, division, print_function, unicode_literals
import tensorflow as tf
from matplotlib import pyplot as plt
import numpy as np

physical_devices = tf.config.experimental.list_physical_devices('GPU')
if physical_devices:
  tf.config.experimental.set_memory_growth(physical_devices[0], True)
  
file = tf.keras.utils.get_file(
    "grace_hopper.jpg",
    "https://storage.googleapis.com/download.tensorflow.org/example_images/grace_hopper.jpg")
img = tf.keras.preprocessing.image.load_img(file, target_size=[224, 224])
plt.imshow(img)
plt.axis('off')
x = tf.keras.preprocessing.image.img_to_array(img)
x = tf.keras.applications.mobilenet.preprocess_input(
    x[tf.newaxis,...])
```

We'll use an image of Grace Hopper as a running example, and a Keras pre-trained image classification model since it's easy to use. Custom models work too, and are covered in detail later.

```python
labels_path = tf.keras.utils.get_file(
    'ImageNetLabels.txt',
    'https://storage.googleapis.com/download.tensorflow.org/data/ImageNetLabels.txt')
imagenet_labels = np.array(open(labels_path).read().splitlines())

pretrained_model = tf.keras.applications.MobileNet()
result_before_save = pretrained_model(x)

decoded = imagenet_labels[np.argsort(result_before_save)[0,::-1][:5]+1]

print("Result before saving:\n", decoded)
```

The top prediction for this image is "military uniform".

```python
tf.saved_model.save(pretrained_model, "/tmp/mobilenet/1/")
```

The save-path follows a convention used by TensorFlow Serving where the last path component \(`1/` here\) is a version number for your model - it allows tools like Tensorflow Serving to reason about the relative freshness.

SavedModels have named functions called signatures. Keras models export their forward pass under the `serving_default` signature key. The [SavedModel command line interface](https://www.tensorflow.org/guide/saved_model#saved_model_cli) is useful for inspecting SavedModels on disk:

```text
!saved_model_cli show --dir /tmp/mobilenet/1 --tag_set serve --signature_def serving_default
```

We can load the SavedModel back into Python with [`tf.saved_model.load`](https://www.tensorflow.org/api_docs/python/tf/saved_model/load) and see how Admiral Hopper's image is classified.

```python
loaded = tf.saved_model.load("/tmp/mobilenet/1/")
print(list(loaded.signatures.keys()))  # ["serving_default"]
```

Imported signatures always return dictionaries.

```python
infer = loaded.signatures["serving_default"]
print(infer.structured_outputs)
```

Running inference from the SavedModel gives the same result as the original model.

```python
labeling = infer(tf.constant(x))[pretrained_model.output_names[0]]

decoded = imagenet_labels[np.argsort(labeling)[0,::-1][:5]+1]

print("Result after saving and loading:\n", decoded)
```

## The SavedModel format on disk

A SavedModel is a directory containing serialized signatures and the state needed to run them, including variable values and vocabularies.

```text
!ls /tmp/mobilenet/1

assets  saved_model.pb  variables
```

The `saved_model.pb` file stores the actual TensorFlow program, or model, and a set of named signatures, each identifying a function that accepts tensor inputs and produces tensor outputs.

SavedModels may contain multiple variants of the model \(multiple `v1.MetaGraphDefs`, identified with the `--tag_set` flag to `saved_model_cli`\), but this is rare. APIs which create multiple variants of a model include [`tf.Estimator.experimental_export_all_saved_models`](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator#experimental_export_all_saved_models) and in TensorFlow 1.x `tf.saved_model.Builder`.

```text
!saved_model_cli show --dir /tmp/mobilenet/1 --tag_set serve

The given SavedModel MetaGraphDef contains SignatureDefs with the following keys:
SignatureDef key: "__saved_model_init_op"
SignatureDef key: "serving_default"
```

The `variables` directory contains a standard training checkpoint \(see the [guide to training checkpoints](training-checkpoints.md)\).

```text
!ls /tmp/mobilenet/1/variables

variables.data-00000-of-00002  variables.data-00001-of-00002  variables.index
```

The `assets` directory contains files used by the TensorFlow graph, for example text files used to initialize vocabulary tables. It is unused in this example.

SavedModels may have an `assets.extra` directory for any files not used by the TensorFlow graph, for example information for consumers about what to do with the SavedModel. TensorFlow itself does not use this directory.

## Reusing SavedModels in Python

Let us look again at class `CustomModule` above, and how `module` objects of this type were saved as SavedModels and loaded back. Did you notice the call to `module(...)` without the use of signatures?

Calling `tf.saved_model.save(obj, '/path')` on objects derived from `tf.keras.Model` or `tf.Module` saves `tf.Variable` attributes as described in the [training checkpoints](https://www.tensorflow.org/guide/checkpoint) tutorial: `obj` and, recursively, the objects referenced by its attributes are traversed and the [`tf.Variable`](https://www.tensorflow.org/api_docs/python/tf/Variable) attributes found on those objects have their current values saved. Likewise, the [`tf.function`](https://www.tensorflow.org/api_docs/python/tf/function)-decorated methods found on those objects have their computation graphs saved. However, the original Python types, Python code of methods, and Python-valued data members are lost.

Calling `obj = tf.saved_model.load('/path')` restores the saved objects \(now reduced to a placeholder type\), their `tf.Variable` attributes with their respective saved values, and their `tf.function`-decorated methods. Those methods can be called as before, as long as a computation graph has been saved that covers the combination of `Tensor` shapes and non-`Tensor` values used in the call. [Retracing the tf.function from Python code](https://www.tensorflow.org/tutorials/customization/performance#tracing) is no longer possible and will raise an exception.

The restored [`tf.function`](https://www.tensorflow.org/api_docs/python/tf/function)s provide a richer, more Pythonic API to the restored model than the concrete functions in the `.signatures` dict. However, this API is inaccessible to the non-Python environments that rely on signatures.

### Basic fine-tuning

Variable objects are available, and we can backprop through imported functions. That is enough to fine-tune \(i.e. retrain\) a SavedModel in simple cases.

```python
optimizer = tf.optimizers.SGD(0.05)

def train_step():
  with tf.GradientTape() as tape:
    loss = (10. - imported(tf.constant(2.))) ** 2
  variables = tape.watched_variables()
  grads = tape.gradient(loss, variables)
  optimizer.apply_gradients(zip(grads, variables))
  return loss
  
for _ in range(10):
  # "v" approaches 5, "loss" approaches 0
  print("loss={:.2f} v={:.2f}".format(train_step(), imported.v.numpy()))
```

### General fine-tuning

A SavedModel from Keras provides [more details](https://github.com/tensorflow/community/blob/master/rfcs/20190509-keras-saved-model.md#serialization-details) than a plain `__call__` to address more advanced cases of fine-tuning. TensorFlow Hub recommends to provide the following of those, if applicable, in SavedModels shared for the purpose of fine-tuning:

* If the model uses dropout or another technique in which the forward pass differs between training and inference \(like batch normalization\), the `__call__` method takes an optional, Python-valued `training=` argument that defaults to `False` but can be set to `True`.
* Next to the `__call__` attribute, there are `.variable` and `.trainable_variable` attributes with the corresponding lists of variables. A variable that was originally trainable but is meant to be frozen during fine-tuning is omitted from `.trainable_variables`.
* For the sake of frameworks like Keras that represent weight regularizers as attributes of layers or sub-models, there can also be a `.regularization_losses` attribute. It holds a list of zero-argument functions whose values are meant for addition to the total loss.

Going back to the initial MobileNet example, we can see some of those in action:

```python
loaded = tf.saved_model.load("/tmp/mobilenet/1/")
print("MobileNet has {} trainable variables: {}, ...".format(
          len(loaded.trainable_variables),
          ", ".join([v.name for v in loaded.trainable_variables[:5]])))
          
trainable_variable_ids = {id(v) for v in loaded.trainable_variables}
non_trainable_variables = [v for v in loaded.variables
                           if id(v) not in trainable_variable_ids]
print("MobileNet also has {} non-trainable variables: {}, ...".format(
          len(non_trainable_variables),
          ", ".join([v.name for v in non_trainable_variables[:3]])))
```

## Control flow in SavedModels

Anything that can go in a [`tf.function`](https://www.tensorflow.org/api_docs/python/tf/function) can go in a SavedModel. With [AutoGraph](https://www.tensorflow.org/guide/function) this includes conditional logic which depends on Tensors, specified with regular Python control flow.

```python
@tf.function(input_signature=[tf.TensorSpec([], tf.int32)])
def control_flow(x):
  if x < 0:
    tf.print("Invalid!")
  else:
    tf.print(x % 3)

to_export = tf.Module()
to_export.control_flow = control_flow
tf.saved_model.save(to_export, "/tmp/control_flow")

imported = tf.saved_model.load("/tmp/control_flow")
imported.control_flow(tf.constant(-1))  # Invalid!
imported.control_flow(tf.constant(2))   # 2
imported.control_flow(tf.constant(3))   # 0
```

## SavedModels from Estimators

Estimators export SavedModels through [`tf.Estimator.export_saved_model`](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator#export_saved_model). See the [guide to Estimator](https://www.tensorflow.org/guide/estimator) for details.

```python
input_column = tf.feature_column.numeric_column("x")
estimator = tf.estimator.LinearClassifier(feature_columns=[input_column])

def input_fn():
  return tf.data.Dataset.from_tensor_slices(
    ({"x": [1., 2., 3., 4.]}, [1, 1, 0, 0])).repeat(200).shuffle(64).batch(16)
estimator.train(input_fn)

serving_input_fn = tf.estimator.export.build_parsing_serving_input_receiver_fn(
  tf.feature_column.make_parse_example_spec([input_column]))
export_path = estimator.export_saved_model(
  "/tmp/from_estimator/", serving_input_fn)
```

This SavedModel accepts serialized `tf.Example` protocol buffers, which are useful for serving. But we can also load it with [`tf.saved_model.load`](https://www.tensorflow.org/api_docs/python/tf/saved_model/load) and run it from Python.

```python
imported = tf.saved_model.load(export_path)

def predict(x):
  example = tf.train.Example()
  example.features.feature["x"].float_list.value.extend([x])
  return imported.signatures["predict"](
    examples=tf.constant([example.SerializeToString()]))
    
print(predict(1.5))
print(predict(3.5))
```

[`tf.estimator.export.build_raw_serving_input_receiver_fn`](https://www.tensorflow.org/api_docs/python/tf/estimator/export/build_raw_serving_input_receiver_fn) allows you to create input functions which take raw tensors rather than [`tf.train.Example`](https://www.tensorflow.org/api_docs/python/tf/train/Example)s.





