# Training checkpoints

The phrase "Saving a TensorFlow model" typically means one of two things:

1. Checkpoints, OR
2. SavedModel.

Checkpoints capture the exact value of all parameters \([`tf.Variable`](https://www.tensorflow.org/api_docs/python/tf/Variable) objects\) used by a model. Checkpoints do not contain any description of the computation defined by the model and thus are typically only useful when source code that will use the saved parameter values is available.

The SavedModel format on the other hand includes a serialized description of the computation defined by the model in addition to the parameter values \(checkpoint\). Models in this format are independent of the source code that created the model. They are thus suitable for deployment via TensorFlow Serving, TensorFlow Lite, TensorFlow.js, or programs in other programming languages \(the C, C++, Java, Go, Rust, C\# etc. TensorFlow APIs\).

This guide covers APIs for writing and reading checkpoints.

## Setup

```python
from __future__ import absolute_import, division, print_function, unicode_literals
import tensorflow as tf

class Net(tf.keras.Model):
  """A simple linear model."""

  def __init__(self):
    super(Net, self).__init__()
    self.l1 = tf.keras.layers.Dense(5)

  def call(self, x):
    return self.l1(x)
    
net = Net()
```

## Saving from tf.keras training APIs

See the [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) guide on saving and restoring.

[`tf.keras.Model.save_weights`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#save_weights) saves a TensorFlow checkpoint.

```python
net.save_weights('easy_checkpoint')
```

## Writing checkpoints

The persistent state of a TensorFlow model is stored in [`tf.Variable`](https://www.tensorflow.org/api_docs/python/tf/Variable) objects. These can be constructed directly, but are often created through high-level APIs like [`tf.keras.layers`](https://www.tensorflow.org/api_docs/python/tf/keras/layers) or [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model).

The easiest way to manage variables is by attaching them to Python objects, then referencing those objects.

Subclasses of [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint), [`tf.keras.layers.Layer`](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Layer), and [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model) automatically track variables assigned to their attributes. The following example constructs a simple linear model, then writes checkpoints which contain values for all of the model's variables.

You can easily save a model-checkpoint with [`Model.save_weights`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#save_weights)

### Manual checkpointing

#### **Setup**

To help demonstrate all the features of [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) define a toy dataset and optimization step:

```python
def toy_dataset():
  inputs = tf.range(10.)[:, None]
  labels = inputs * 5. + tf.range(5.)[None, :]
  return tf.data.Dataset.from_tensor_slices(
    dict(x=inputs, y=labels)).repeat(10).batch(2)
    
def train_step(net, example, optimizer):
  """Trains `net` on `example` using `optimizer`."""
  with tf.GradientTape() as tape:
    output = net(example['x'])
    loss = tf.reduce_mean(tf.abs(output - example['y']))
  variables = net.trainable_variables
  gradients = tape.gradient(loss, variables)
  optimizer.apply_gradients(zip(gradients, variables))
  return loss
```

#### **Create the checkpoint objects**

To manually make a checkpoint you will need a [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) object. Where the objects you want to checkpoint are set as attributes on the object.

A [`tf.train.CheckpointManager`](https://www.tensorflow.org/api_docs/python/tf/train/CheckpointManager) can also be helpful for managing multiple checkpoints.

```python
opt = tf.keras.optimizers.Adam(0.1)
ckpt = tf.train.Checkpoint(step=tf.Variable(1), optimizer=opt, net=net)
manager = tf.train.CheckpointManager(ckpt, './tf_ckpts', max_to_keep=3)
```

#### **Train and checkpoint the model**

The following training loop creates an instance of the model and of an optimizer, then gathers them into a [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) object. It calls the training step in a loop on each batch of data, and periodically writes checkpoints to disk.

```python
def train_and_checkpoint(net, manager):
  ckpt.restore(manager.latest_checkpoint)
  if manager.latest_checkpoint:
    print("Restored from {}".format(manager.latest_checkpoint))
  else:
    print("Initializing from scratch.")

  for example in toy_dataset():
    loss = train_step(net, example, opt)
    ckpt.step.assign_add(1)
    if int(ckpt.step) % 10 == 0:
      save_path = manager.save()
      print("Saved checkpoint for step {}: {}".format(int(ckpt.step), save_path))
      print("loss {:1.2f}".format(loss.numpy()))
      
train_and_checkpoint(net, manager)
```

#### Restore and continue training

After the first you can pass a new model and manager, but pickup training exactly where you left off:

```python
opt = tf.keras.optimizers.Adam(0.1)
net = Net()
ckpt = tf.train.Checkpoint(step=tf.Variable(1), optimizer=opt, net=net)
manager = tf.train.CheckpointManager(ckpt, './tf_ckpts', max_to_keep=3)

train_and_checkpoint(net, manager)
```

The [`tf.train.CheckpointManager`](https://www.tensorflow.org/api_docs/python/tf/train/CheckpointManager) object deletes old checkpoints. Above it's configured to keep only the three most recent checkpoints.

```python
print(manager.checkpoints)  # List the three remaining checkpoints
```

These paths, e.g. `'./tf_ckpts/ckpt-10'`, are not files on disk. Instead they are prefixes for an `index` file and one or more data files which contain the variable values. These prefixes are grouped together in a single `checkpoint` file \(`'./tf_ckpts/checkpoint'`\) where the `CheckpointManager` saves its state.

## Loading mechanics

TensorFlow matches variables to checkpointed values by traversing a directed graph with named edges, starting from the object being loaded. Edge names typically come from attribute names in objects, for example the `"l1"` in `self.l1 = tf.keras.layers.Dense(5)`. `tf.train.Checkpoint` uses its keyword argument names, as in the `"step"` in `tf.train.Checkpoint(step=...)`.The dependency graph from the example above looks like this:

![](../../../../.gitbook/assets/image%20%28426%29.png)

With the optimizer in red, regular variables in blue, and optimizer slot variables in orange. The other nodes, for example representing the [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint), are black.

Slot variables are part of the optimizer's state, but are created for a specific variable. For example the `'m'` edges above correspond to momentum, which the Adam optimizer tracks for each variable. Slot variables are only saved in a checkpoint if the variable and the optimizer would both be saved, thus the dashed edges.

Calling `restore()` on a [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) object queues the requested restorations, restoring variable values as soon as there's a matching path from the `Checkpoint` object. For example we can load just the kernel from the model we defined above by reconstructing one path to it through the network and the layer.

```python
to_restore = tf.Variable(tf.zeros([5]))
print(to_restore.numpy())  # All zeros
fake_layer = tf.train.Checkpoint(bias=to_restore)
fake_net = tf.train.Checkpoint(l1=fake_layer)
new_root = tf.train.Checkpoint(net=fake_net)
status = new_root.restore(tf.train.latest_checkpoint('./tf_ckpts/'))
print(to_restore.numpy())  # We get the restored value now
```

The dependency graph for these new objects is a much smaller subgraph of the larger checkpoint we wrote above. It includes only the bias and a save counter that [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) uses to number checkpoints.

![Visualization of a subgraph for the bias variable](http://tensorflow.org/images/guide/partial_checkpoint.svg)

`restore()` returns a status object, which has optional assertions. All of the objects we've created in our new `Checkpoint` have been restored, so `status.assert_existing_objects_matched()` passes.

```python
status.assert_existing_objects_matched()
```

There are many objects in the checkpoint which haven't matched, including the layer's kernel and the optimizer's variables. `status.assert_consumed()` only passes if the checkpoint and the program match exactly, and would throw an exception here.

### Delayed restorations

`Layer` objects in TensorFlow may delay the creation of variables to their first call, when input shapes are available. For example the shape of a `Dense` layer's kernel depends on both the layer's input and output shapes, and so the output shape required as a constructor argument is not enough information to create the variable on its own. Since calling a `Layer` also reads the variable's value, a restore must happen between the variable's creation and its first use.

To support this idiom, [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) queues restores which don't yet have a matching variable.

```python
delayed_restore = tf.Variable(tf.zeros([1, 5]))
print(delayed_restore.numpy())  # Not restored; still zeros
fake_layer.kernel = delayed_restore
print(delayed_restore.numpy())  # Restored
```

### Manually inspecting checkpoints

[`tf.train.list_variables`](https://www.tensorflow.org/api_docs/python/tf/train/list_variables) lists the checkpoint keys and shapes of variables in a checkpoint. Checkpoint keys are paths in the graph displayed above.

```python
tf.train.list_variables(tf.train.latest_checkpoint('./tf_ckpts/'))
```

### List and dictionary tracking

As with direct attribute assignments like `self.l1 = tf.keras.layers.Dense(5)`, assigning lists and dictionaries to attributes will track their contents.

```python
save = tf.train.Checkpoint()
save.listed = [tf.Variable(1.)]
save.listed.append(tf.Variable(2.))
save.mapped = {'one': save.listed[0]}
save.mapped['two'] = save.listed[1]
save_path = save.save('./tf_list_example')

restore = tf.train.Checkpoint()
v2 = tf.Variable(0.)
assert 0. == v2.numpy()  # Not restored yet
restore.mapped = {'two': v2}
restore.restore(save_path)
assert 2. == v2.numpy()
```

You may notice wrapper objects for lists and dictionaries. These wrappers are checkpointable versions of the underlying data-structures. Just like the attribute based loading, these wrappers restore a variable's value as soon as it's added to the container.

```python
restore.listed = []
print(restore.listed)  # ListWrapper([])
v1 = tf.Variable(0.)
restore.listed.append(v1)  # Restores v1, from restore() in the previous cell
assert 1. == v1.numpy()
```

The same tracking is automatically applied to subclasses of [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model), and may be used for example to track lists of layers.

## Saving object-based checkpoints with Estimator

See the [Estimator guide](https://www.tensorflow.org/guide/estimator).

Estimators by default save checkpoints with variable names rather than the object graph described in the previous sections. [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) will accept name-based checkpoints, but variable names may change when moving parts of a model outside of the Estimator's `model_fn`. Saving object-based checkpoints makes it easier to train a model inside an Estimator and then use it outside of one.

```python
import tensorflow.compat.v1 as tf_compat

def model_fn(features, labels, mode):
  net = Net()
  opt = tf.keras.optimizers.Adam(0.1)
  ckpt = tf.train.Checkpoint(step=tf_compat.train.get_global_step(),
                             optimizer=opt, net=net)
  with tf.GradientTape() as tape:
    output = net(features['x'])
    loss = tf.reduce_mean(tf.abs(output - features['y']))
  variables = net.trainable_variables
  gradients = tape.gradient(loss, variables)
  return tf.estimator.EstimatorSpec(
    mode,
    loss=loss,
    train_op=tf.group(opt.apply_gradients(zip(gradients, variables)),
                      ckpt.step.assign_add(1)),
    # Tell the Estimator to save "ckpt" in an object-based format.
    scaffold=tf_compat.train.Scaffold(saver=ckpt))

tf.keras.backend.clear_session()
est = tf.estimator.Estimator(model_fn, './tf_estimator_example/')
est.train(toy_dataset, steps=10)
```

[`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) can then load the Estimator's checkpoints from its `model_dir`.

```python
opt = tf.keras.optimizers.Adam(0.1)
net = Net()
ckpt = tf.train.Checkpoint(
  step=tf.Variable(1, dtype=tf.int64), optimizer=opt, net=net)
ckpt.restore(tf.train.latest_checkpoint('./tf_estimator_example/'))
ckpt.step.numpy()  # From est.train(..., steps=10)
```

