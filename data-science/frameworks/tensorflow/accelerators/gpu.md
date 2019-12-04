# GPU

TensorFlow code, and [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) models will transparently run on a single GPU with no code changes required.

{% hint style="info" %}
Use `tf.config.experimental.list_physical_devices('GPU')` to confirm that TensorFlow is using the GPU.
{% endhint %}

The simplest way to run on multiple GPUs, on one or many machines, is using [Distribution Strategies](distributed-training.md).

This guide is for users who have tried these approaches and found that they need fine-grained control of how TensorFlow uses the GPU.

## Setup

```python
from __future__ import absolute_import, division, print_function, unicode_literals

import tensorflow as tf
print("Num GPUs Available: ", len(tf.config.experimental.list_physical_devices('GPU')))
```

## Overview

TensorFlow supports running computations on a variety of types of devices, including CPU and GPU. They are represented with string identifiers for example:

* `"/device:CPU:0"`: The CPU of your machine.
* `"/GPU:0"`: Short-hand notation for the first GPU of your machine that is visible to TensorFlow.
* `"/job:localhost/replica:0/task:0/device:GPU:1"`: Fully qualified name of the second GPU of your machine that is visible to TensorFlow.

If a TensorFlow operation has both CPU and GPU implementations, by default the GPU devices will be given priority when the operation is assigned to a device. For example, [`tf.matmul`](https://www.tensorflow.org/api_docs/python/tf/linalg/matmul) has both CPU and GPU kernels. On a system with devices `CPU:0` and `GPU:0`, the `GPU:0` device will be selected to run `tf.matmul` unless you explicitly request running it on another device.

## Logging device placement

To find out which devices your operations and tensors are assigned to, put [`tf.debugging.set_log_device_placement(True)`](https://www.tensorflow.org/api_docs/python/tf/debugging/set_log_device_placement) as the first statement of your program. Enabling device placement logging causes any Tensor allocations or operations to be printed.

```python
tf.debugging.set_log_device_placement(True)

# Create some tensors
a = tf.constant([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
b = tf.constant([[1.0, 2.0], [3.0, 4.0], [5.0, 6.0]])
c = tf.matmul(a, b)

print(c)
```

The above code will print an indication the `MatMul` op was executed on `GPU:0`.

## Manual device placement

If you would like a particular operation to run on a device of your choice instead of what's automatically selected for you, you can use `with tf.device` to create a device context, and all the operations within that context will run on the same designated device.

```python
tf.debugging.set_log_device_placement(True)

# Place tensors on the CPU
with tf.device('/CPU:0'):
  a = tf.constant([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
  b = tf.constant([[1.0, 2.0], [3.0, 4.0], [5.0, 6.0]])

c = tf.matmul(a, b)
print(c)
```

You will see that now `a` and `b` are assigned to `CPU:0`. Since a device was not explicitly specified for the `MatMul` operation, the TensorFlow runtime will choose one based on the operation and available devices \(`GPU:0` in this example\) and automatically copy tensors between devices if required.

## Limiting GPU memory growth

By default, TensorFlow maps nearly all of the GPU memory of all GPUs \(subject to [`CUDA_VISIBLE_DEVICES`](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#env-vars)\) visible to the process. This is done to more efficiently use the relatively precious GPU memory resources on the devices by reducing memory fragmentation. To limit TensorFlow to a specific set of GPUs we use the [`tf.config.experimental.set_visible_devices`](https://www.tensorflow.org/api_docs/python/tf/config/experimental/set_visible_devices) method.

```python
gpus = tf.config.experimental.list_physical_devices('GPU')
if gpus:
  # Restrict TensorFlow to only use the first GPU
  try:
    tf.config.experimental.set_visible_devices(gpus[0], 'GPU')
    logical_gpus = tf.config.experimental.list_logical_devices('GPU')
    print(len(gpus), "Physical GPUs,", len(logical_gpus), "Logical GPU")
  except RuntimeError as e:
    # Visible devices must be set before GPUs have been initialized
    print(e)
```

In some cases it is desirable for the process to only allocate a subset of the available memory, or to only grow the memory usage as is needed by the process. TensorFlow provides two methods to control this.

The first option is to turn on memory growth by calling [`tf.config.experimental.set_memory_growth`](https://www.tensorflow.org/api_docs/python/tf/config/experimental/set_memory_growth), which attempts to allocate only as much GPU memory as needed for the runtime allocations: it starts out allocating very little memory, and as the program gets run and more GPU memory is needed, we extend the GPU memory region allocated to the TensorFlow process. Note we do not release memory, since it can lead to memory fragmentation. To turn on memory growth for a specific GPU, use the following code prior to allocating any tensors or executing any ops.

```python
gpus = tf.config.experimental.list_physical_devices('GPU')
if gpus:
  try:
    # Currently, memory growth needs to be the same across GPUs
    for gpu in gpus:
      tf.config.experimental.set_memory_growth(gpu, True)
    logical_gpus = tf.config.experimental.list_logical_devices('GPU')
    print(len(gpus), "Physical GPUs,", len(logical_gpus), "Logical GPUs")
  except RuntimeError as e:
    # Memory growth must be set before GPUs have been initialized
    print(e)
```

Another way to enable this option is to set the environmental variable `TF_FORCE_GPU_ALLOW_GROWTH` to `true`. This configuration is platform specific.

The second method is to configure a virtual GPU device with [`tf.config.experimental.set_virtual_device_configuration`](https://www.tensorflow.org/api_docs/python/tf/config/experimental/set_virtual_device_configuration) and set a hard limit on the total memory to allocate on the GPU.

```python
gpus = tf.config.experimental.list_physical_devices('GPU')
if gpus:
  # Restrict TensorFlow to only allocate 1GB of memory on the first GPU
  try:
    tf.config.experimental.set_virtual_device_configuration(
        gpus[0],
        [tf.config.experimental.VirtualDeviceConfiguration(memory_limit=1024)])
    logical_gpus = tf.config.experimental.list_logical_devices('GPU')
    print(len(gpus), "Physical GPUs,", len(logical_gpus), "Logical GPUs")
  except RuntimeError as e:
    # Virtual devices must be set before GPUs have been initialized
    print(e)
```

This is useful if you want to truly bound the amount of GPU memory available to the TensorFlow process. This is common practice for local development when the GPU is shared with other applications such as a workstation GUI.

## Using a single GPU on a multi-GPU system

If you have more than one GPU in your system, the GPU with the lowest ID will be selected by default. If you would like to run on a different GPU, you will need to specify the preference explicitly:

```python
tf.debugging.set_log_device_placement(True)

try:
  # Specify an invalid GPU device
  with tf.device('/device:GPU:2'):
    a = tf.constant([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
    b = tf.constant([[1.0, 2.0], [3.0, 4.0], [5.0, 6.0]])
    c = tf.matmul(a, b)
except RuntimeError as e:
  print(e)
```

If the device you have specified does not exist, you will get a `RuntimeError`: `.../device:GPU:2 unknown device`.

If you would like TensorFlow to automatically choose an existing and supported device to run the operations in case the specified one doesn't exist, you can call [`tf.config.set_soft_device_placement(True)`](https://www.tensorflow.org/api_docs/python/tf/config/set_soft_device_placement).

```python
tf.config.set_soft_device_placement(True)
tf.debugging.set_log_device_placement(True)

# Creates some tensors
a = tf.constant([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
b = tf.constant([[1.0, 2.0], [3.0, 4.0], [5.0, 6.0]])
c = tf.matmul(a, b)

print(c)
```

## Using multiple GPUs

Developing for multiple GPUs will allow a model to scale with the additional resources. If developing on a system with a single GPU, we can simulate multiple GPUs with virtual devices. This enables easy testing of multi-GPU setups without requiring additional resources.

```python
gpus = tf.config.experimental.list_physical_devices('GPU')
if gpus:
  # Create 2 virtual GPUs with 1GB memory each
  try:
    tf.config.experimental.set_virtual_device_configuration(
        gpus[0],
        [tf.config.experimental.VirtualDeviceConfiguration(memory_limit=1024),
         tf.config.experimental.VirtualDeviceConfiguration(memory_limit=1024)])
    logical_gpus = tf.config.experimental.list_logical_devices('GPU')
    print(len(gpus), "Physical GPU,", len(logical_gpus), "Logical GPUs")
  except RuntimeError as e:
    # Virtual devices must be set before GPUs have been initialized
    print(e)
```

Once we have multiple logical GPUs available to the runtime, we can utilize the multiple GPUs with [`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) or with manual placement.

**With tf.distribute.Strategy**

The best practice for using multiple GPUs is to use [`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy). Here is a simple example:

```python
tf.debugging.set_log_device_placement(True)

strategy = tf.distribute.MirroredStrategy()
with strategy.scope():
  inputs = tf.keras.layers.Input(shape=(1,))
  predictions = tf.keras.layers.Dense(1)(inputs)
  model = tf.keras.models.Model(inputs=inputs, outputs=predictions)
  model.compile(loss='mse',
                optimizer=tf.keras.optimizers.SGD(learning_rate=0.2))
```

This program will run a copy of your model on each GPU, splitting the input data between them, also known as "[data parallelism](https://en.wikipedia.org/wiki/Data_parallelism)".

For more information about distribution strategies, check out the guide [here](https://www.tensorflow.org/guide/distributed_training).

**Manual placement**

[`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) works under the hood by replicating computation across devices. You can manually implement replication by constructing your model on each GPU. For example:

```python
tf.debugging.set_log_device_placement(True)

gpus = tf.config.experimental.list_logical_devices('GPU')
if gpus:
  # Replicate your computation on multiple GPUs
  c = []
  for gpu in gpus:
    with tf.device(gpu.name):
      a = tf.constant([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
      b = tf.constant([[1.0, 2.0], [3.0, 4.0], [5.0, 6.0]])
      c.append(tf.matmul(a, b))

  with tf.device('/CPU:0'):
    matmul_sum = tf.add_n(c)

  print(matmul_sum)
```

