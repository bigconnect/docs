# Eager execution

TensorFlow's eager execution is an imperative programming environment that evaluates operations immediately, without building graphs: operations return concrete values instead of constructing a computational graph to run later. This makes it easy to get started with TensorFlow and debug models, and it reduces boilerplate as well. To follow along with this guide, run the code samples below in an interactive `python` interpreter.

Eager execution is a flexible machine learning platform for research and experimentation, providing:

* _An intuitive interface_—Structure your code naturally and use Python data structures. Quickly iterate on small models and small data.
* _Easier debugging_—Call ops directly to inspect running models and test changes. Use standard Python debugging tools for immediate error reporting.
* _Natural control flow_—Use Python control flow instead of graph control flow, simplifying the specification of dynamic models.

Eager execution supports most TensorFlow operations and GPU acceleration.

{% hint style="info" %}
Note: Some models may experience increased overhead with eager execution enabled. Performance improvements are ongoing, but please [file a bug](https://github.com/tensorflow/tensorflow/issues) if you find a problem and share your benchmarks.
{% endhint %}

## Setup and basic usage

```python
from __future__ import absolute_import, division, print_function, unicode_literals
import os

import tensorflow as tf

import cProfile
```

Now you can run TensorFlow operations and the results will return immediately:

```python
x = [[2.]]
m = tf.matmul(x, x)
print("hello, {}".format(m))
```

Enabling eager execution changes how TensorFlow operations behave—now they immediately evaluate and return their values to Python. [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor) objects reference concrete values instead of symbolic handles to nodes in a computational graph. Since there isn't a computational graph to build and run later in a session, it's easy to inspect results using `print()` or a debugger. Evaluating, printing, and checking tensor values does not break the flow for computing gradients.

Eager execution works nicely with [NumPy](http://www.numpy.org/). NumPy operations accept [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor) arguments. TensorFlow [math operations](https://www.tensorflow.org/api_guides/python/math_ops) convert Python objects and NumPy arrays to [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor) objects. The `tf.Tensor.numpy` method returns the object's value as a NumPy `ndarray`.

```python
a = tf.constant([[1, 2],
                 [3, 4]])
print(a)

# Broadcasting support
b = tf.add(a, 1)
print(b)

# Operator overloading is supported
print(a * b)

# Use NumPy values
import numpy as np

c = np.multiply(a, b)
print(c)

# Obtain numpy value from a tensor:
print(a.numpy())
# => [[1 2]
#     [3 4]]
```

## Dynamic control flow

A major benefit of eager execution is that all the functionality of the host language is available while your model is executing. So, for example, it is easy to write [fizzbuzz](https://en.wikipedia.org/wiki/Fizz_buzz):

```python
def fizzbuzz(max_num):
  counter = tf.constant(0)
  max_num = tf.convert_to_tensor(max_num)
  for num in range(1, max_num.numpy()+1):
    num = tf.constant(num)
    if int(num % 3) == 0 and int(num % 5) == 0:
      print('FizzBuzz')
    elif int(num % 3) == 0:
      print('Fizz')
    elif int(num % 5) == 0:
      print('Buzz')
    else:
      print(num.numpy())
    counter += 1

fizzbuzz(15)
```

This has conditionals that depend on tensor values and it prints these values at runtime.

## Eager training

### Computing gradients

[Automatic differentiation](https://en.wikipedia.org/wiki/Automatic_differentiation) is useful for implementing machine learning algorithms such as [backpropagation](https://en.wikipedia.org/wiki/Backpropagation) for training neural networks. During eager execution, use [`tf.GradientTape`](https://www.tensorflow.org/api_docs/python/tf/GradientTape) to trace operations for computing gradients later.

You can use [`tf.GradientTape`](https://www.tensorflow.org/api_docs/python/tf/GradientTape) to train and/or compute gradients in eager. It is especially useful for complicated training loops.

Since different operations can occur during each call, all forward-pass operations get recorded to a "tape". To compute the gradient, play the tape backwards and then discard. A particular [`tf.GradientTape`](https://www.tensorflow.org/api_docs/python/tf/GradientTape) can only compute one gradient; subsequent calls throw a runtime error.

```python
w = tf.Variable([[1.0]])
with tf.GradientTape() as tape:
  loss = w * w

grad = tape.gradient(loss, w)
print(grad)  # => tf.Tensor([[ 2.]], shape=(1, 1), dtype=float32)
```

### Train a model

The following example creates a multi-layer model that classifies the standard MNIST handwritten digits. It demonstrates the optimizer and layer APIs to build trainable graphs in an eager execution environment.

```python
# Fetch and format the mnist data
(mnist_images, mnist_labels), _ = tf.keras.datasets.mnist.load_data()

dataset = tf.data.Dataset.from_tensor_slices(
  (tf.cast(mnist_images[...,tf.newaxis]/255, tf.float32),
   tf.cast(mnist_labels,tf.int64)))
dataset = dataset.shuffle(1000).batch(32)

# Build the model
mnist_model = tf.keras.Sequential([
  tf.keras.layers.Conv2D(16,[3,3], activation='relu',
                         input_shape=(None, None, 1)),
  tf.keras.layers.Conv2D(16,[3,3], activation='relu'),
  tf.keras.layers.GlobalAveragePooling2D(),
  tf.keras.layers.Dense(10)
])
```

Even without training, call the model and inspect the output in eager execution:

```python
for images,labels in dataset.take(1):
  print("Logits: ", mnist_model(images[0:1]).numpy())
```

While keras models have a builtin training loop \(using the `fit` method\), sometimes you need more customization. Here's an example, of a training loop implemented with eager:

```python
optimizer = tf.keras.optimizers.Adam()
loss_object = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)

loss_history = []
```

{% hint style="info" %}
Use the assert functions in [tf.debugging to check](https://www.tensorflow.org/api_docs/python/tf/debugging) if a condition holds up. This works in eager and graph execution.
{% endhint %}

```python
def train_step(images, labels):
  with tf.GradientTape() as tape:
    logits = mnist_model(images, training=True)
    
    # Add asserts to check the shape of the output.
    tf.debugging.assert_equal(logits.shape, (32, 10))
    
    loss_value = loss_object(labels, logits)

  loss_history.append(loss_value.numpy().mean())
  grads = tape.gradient(loss_value, mnist_model.trainable_variables)
  optimizer.apply_gradients(zip(grads, mnist_model.trainable_variables))


def train(epochs):
  for epoch in range(epochs):
    for (batch, (images, labels)) in enumerate(dataset):
      train_step(images, labels)
    print ('Epoch {} finished'.format(epoch))
    

train(epochs = 3)
```

```python
import matplotlib.pyplot as plt

plt.plot(loss_history)
plt.xlabel('Batch #')
plt.ylabel('Loss [entropy]')
```

### Variables and optimizers

[`tf.Variable`](https://www.tensorflow.org/api_docs/python/tf/Variable) objects store mutable [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor)-like values accessed during training to make automatic differentiation easier.

The collections of variables can be encapsulated into layers or models, along with methods that operate on them. See [Custom Keras layers and models](https://www.tensorflow.org/keras/custom_layers_and_models) for details. The main difference between layers and models is that models add methods like [`Model.fit`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#fit), [`Model.evaluate`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#evaluate), and [`Model.save`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#save).

For example, the automatic differentiation example above can be rewritten:

```python
class Linear(tf.keras.Model):
  def __init__(self):
    super(Linear, self).__init__()
    self.W = tf.Variable(5., name='weight')
    self.B = tf.Variable(10., name='bias')
  def call(self, inputs):
    return inputs * self.W + self.B
    
# A toy dataset of points around 3 * x + 2
NUM_EXAMPLES = 2000
training_inputs = tf.random.normal([NUM_EXAMPLES])
noise = tf.random.normal([NUM_EXAMPLES])
training_outputs = training_inputs * 3 + 2 + noise

# The loss function to be optimized
def loss(model, inputs, targets):
  error = model(inputs) - targets
  return tf.reduce_mean(tf.square(error))

def grad(model, inputs, targets):
  with tf.GradientTape() as tape:
    loss_value = loss(model, inputs, targets)
  return tape.gradient(loss_value, [model.W, model.B])
```

Next:

1. Create the model.
2. The Derivatives of a loss function with respect to model parameters.
3. A strategy for updating the variables based on the derivatives.

```python
model = Linear()
optimizer = tf.keras.optimizers.SGD(learning_rate=0.01)

print("Initial loss: {:.3f}".format(loss(model, training_inputs, training_outputs)))

steps = 300
for i in range(steps):
  grads = grad(model, training_inputs, training_outputs)
  optimizer.apply_gradients(zip(grads, [model.W, model.B]))
  if i % 20 == 0:
    print("Loss at step {:03d}: {:.3f}".format(i, loss(model, training_inputs, training_outputs)))
    
print("Final loss: {:.3f}".format(loss(model, training_inputs, training_outputs)))
print("W = {}, B = {}".format(model.W.numpy(), model.B.numpy()))
```

{% hint style="info" %}
Variables persist until the last reference to the python object is removed, and is the variable is deleted.
{% endhint %}

### Object-based saving

A [`tf.keras.Model`](https://www.tensorflow.org/api_docs/python/tf/keras/Model) includes a covienient `save_weights` method allowing you to easily create a checkpoint:

```python
model.save_weights('weights')
status = model.load_weights('weights')
```

Using [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) you can take full control over this process.

This section is an abbreviated version of the [guide to training checkpoints](https://www.tensorflow.org/guide/checkpoint).

```python
x = tf.Variable(10.)
checkpoint = tf.train.Checkpoint(x=x)

x.assign(2.)   # Assign a new value to the variables and save.
checkpoint_path = './ckpt/'
checkpoint.save('./ckpt/')

x.assign(11.)  # Change the variable after saving.

# Restore values from the checkpoint
checkpoint.restore(tf.train.latest_checkpoint(checkpoint_path))

print(x)  # => 2.0
```

To save and load models, [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint) stores the internal state of objects, without requiring hidden variables. To record the state of a `model`, an `optimizer`, and a global step, pass them to a [`tf.train.Checkpoint`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint):

```python
model = tf.keras.Sequential([
  tf.keras.layers.Conv2D(16,[3,3], activation='relu'),
  tf.keras.layers.GlobalAveragePooling2D(),
  tf.keras.layers.Dense(10)
])
optimizer = tf.keras.optimizers.Adam(learning_rate=0.001)
checkpoint_dir = 'path/to/model_dir'
if not os.path.exists(checkpoint_dir):
  os.makedirs(checkpoint_dir)
checkpoint_prefix = os.path.join(checkpoint_dir, "ckpt")
root = tf.train.Checkpoint(optimizer=optimizer,
                           model=model)

root.save(checkpoint_prefix)
root.restore(tf.train.latest_checkpoint(checkpoint_dir))
```

{% hint style="info" %}
In many training loops, variables are created after [`tf.train.Checkpoint.restore`](https://www.tensorflow.org/api_docs/python/tf/train/Checkpoint#restore) is called. These variables will be restored as soon as they are created, and assertions are available to ensure that a checkpoint has been fully loaded. See the [guide to training checkpoints](https://www.tensorflow.org/guide/checkpoint) for details.
{% endhint %}

### Object-oriented metrics

[`tf.keras.metrics`](https://www.tensorflow.org/api_docs/python/tf/keras/metrics) are stored as objects. Update a metric by passing the new data to the callable, and retrieve the result using the `tf.keras.metrics.result` method, for example:

```python
m = tf.keras.metrics.Mean("loss")
m(0)
m(5)
m.result()  # => 2.5
m([8, 9])
m.result()  # => 5.5
```

### Summaries and TensorBoard

[TensorBoard](https://tensorflow.org/tensorboard) is a visualization tool for understanding, debugging and optimizing the model training process. It uses summary events that are written while executing the program.

You can use [`tf.summary`](https://www.tensorflow.org/api_docs/python/tf/summary) to record summaries of variable in eager execution. For example, to record summaries of `loss` once every 100 training steps:

```python
logdir = "./tb/"
writer = tf.summary.create_file_writer(logdir)

steps = 1000
with writer.as_default():  # or call writer.set_as_default() before the loop.
  for i in range(steps):
    step = i + 1
    # Calculate loss with your real train function.
    loss = 1 - 0.001 * step
    if step % 100 == 0:
      tf.summary.scalar('loss', loss, step=step)
```

```python
!ls tb/
```

## Advanced automatic differentiation topics

### Dynamic models

[`tf.GradientTape`](https://www.tensorflow.org/api_docs/python/tf/GradientTape) can also be used in dynamic models. This example for a [backtracking line search](https://wikipedia.org/wiki/Backtracking_line_search) algorithm looks like normal NumPy code, except there are gradients and is differentiable, despite the complex control flow:

```python
def line_search_step(fn, init_x, rate=1.0):
  with tf.GradientTape() as tape:
    # Variables are automatically tracked.
    # But to calculate a gradient from a tensor, you must `watch` it.
    tape.watch(init_x)
    value = fn(init_x)
  grad = tape.gradient(value, init_x)
  grad_norm = tf.reduce_sum(grad * grad)
  init_value = value
  while value > init_value - rate * grad_norm:
    x = init_x - rate * grad
    value = fn(x)
    rate /= 2.0
  return x, value
```

### Custom gradients

Custom gradients are an easy way to override gradients. Within the forward function, define the gradient with respect to the inputs, outputs, or intermediate results. For example, here's an easy way to clip the norm of the gradients in the backward pass:

```python
@tf.custom_gradient
def clip_gradient_by_norm(x, norm):
  y = tf.identity(x)
  def grad_fn(dresult):
    return [tf.clip_by_norm(dresult, norm), None]
  return y, grad_fn
```

Custom gradients are commonly used to provide a numerically stable gradient for a sequence of operations:

```python
def log1pexp(x):
  return tf.math.log(1 + tf.exp(x))

def grad_log1pexp(x):
  with tf.GradientTape() as tape:
    tape.watch(x)
    value = log1pexp(x)
  return tape.gradient(value, x)
  
# The gradient computation works fine at x = 0.
grad_log1pexp(tf.constant(0.)).numpy()

# However, x = 100 fails because of numerical instability.
grad_log1pexp(tf.constant(100.)).numpy()
```

Here, the `log1pexp` function can be analytically simplified with a custom gradient. The implementation below reuses the value for [`tf.exp(x)`](https://www.tensorflow.org/api_docs/python/tf/math/exp) that is computed during the forward pass—making it more efficient by eliminating redundant calculations:

```python
@tf.custom_gradient
def log1pexp(x):
  e = tf.exp(x)
  def grad(dy):
    return dy * (1 - 1 / (1 + e))
  return tf.math.log(1 + e), grad

def grad_log1pexp(x):
  with tf.GradientTape() as tape:
    tape.watch(x)
    value = log1pexp(x)
  return tape.gradient(value, x)
  
# As before, the gradient computation works fine at x = 0.
grad_log1pexp(tf.constant(0.)).numpy()

# And the gradient computation also works at x = 100.
grad_log1pexp(tf.constant(100.)).numpy()
```

## Performance

Computation is automatically offloaded to GPUs during eager execution. If you want control over where a computation runs you can enclose it in a `tf.device('/gpu:0')` block \(or the CPU equivalent\):

```python
import time

def measure(x, steps):
  # TensorFlow initializes a GPU the first time it's used, exclude from timing.
  tf.matmul(x, x)
  start = time.time()
  for i in range(steps):
    x = tf.matmul(x, x)
  # tf.matmul can return before completing the matrix multiplication
  # (e.g., can return after enqueing the operation on a CUDA stream).
  # The x.numpy() call below will ensure that all enqueued operations
  # have completed (and will also copy the result to host memory,
  # so we're including a little more than just the matmul operation
  # time).
  _ = x.numpy()
  end = time.time()
  return end - start

shape = (1000, 1000)
steps = 200
print("Time to multiply a {} matrix by itself {} times:".format(shape, steps))

# Run on CPU:
with tf.device("/cpu:0"):
  print("CPU: {} secs".format(measure(tf.random.normal(shape), steps)))

# Run on GPU, if available:
if tf.config.experimental.list_physical_devices("GPU"):
  with tf.device("/gpu:0"):
    print("GPU: {} secs".format(measure(tf.random.normal(shape), steps)))
else:
  print("GPU: not found")
```

A [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor) object can be copied to a different device to execute its operations:

```python
if tf.config.experimental.list_physical_devices("GPU"):
  x = tf.random.normal([10, 10])

  x_gpu0 = x.gpu()
  x_cpu = x.cpu()

  _ = tf.matmul(x_cpu, x_cpu)    # Runs on CPU
  _ = tf.matmul(x_gpu0, x_gpu0)  # Runs on GPU:0
```

### Benchmarks

For compute-heavy models, such as [ResNet50](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/eager/python/examples/resnet50) training on a GPU, eager execution performance is comparable to [`tf.function`](https://www.tensorflow.org/api_docs/python/tf/function) execution. But this gap grows larger for models with less computation and there is work to be done for optimizing hot code paths for models with lots of small operations.

