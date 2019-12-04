# Distributed training

[`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) is a TensorFlow API to distribute training across multiple GPUs, multiple machines or TPUs. Using this API, you can distribute your existing models and training code with minimal code changes.

[`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) has been designed with these key goals in mind:

* Easy to use and support multiple user segments, including researchers, ML engineers, etc.
* Provide good performance out of the box.
* Easy switching between strategies.

[`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) can be used with a high-level API like [Keras](https://www.tensorflow.org/guide/keras), and can also be used to distribute custom training loops \(and, in general, any computation using TensorFlow\).

In TensorFlow 2.0, you can execute your programs eagerly, or in a graph using [`tf.function`](https://www.tensorflow.org/guide/function). [`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) intends to support both these modes of execution. Although we discuss training most of the time in this guide, this API can also be used for distributing evaluation and prediction on different platforms.

You can use [`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) with very few changes to your code, because we have changed the underlying components of TensorFlow to become strategy-aware. This includes variables, layers, models, optimizers, metrics, summaries, and checkpoints.

In this guide, we explain various types of strategies and how you can use them in different situations.

{% hint style="info" %}
For a deeper understanding of the concepts, please watch [this deep-dive presentation](https://youtu.be/jKV53r9-H14). This is especially recommended if you plan to write your own training loop.
{% endhint %}

```python
# Import TensorFlow
from __future__ import absolute_import, division, print_function, unicode_literals
import tensorflow as tf
```

## Types of strategies

[`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) intends to cover a number of use cases along different axes. Some of these combinations are currently supported and others will be added in the future. Some of these axes are:

* _Synchronous vs asynchronous training:_ These are two common ways of distributing training with data parallelism. In sync training, all workers train over different slices of input data in sync, and aggregating gradients at each step. In async training, all workers are independently training over the input data and updating variables asynchronously. Typically sync training is supported via all-reduce and async through parameter server architecture.
* _Hardware platform:_ You may want to scale your training onto multiple GPUs on one machine, or multiple machines in a network \(with 0 or more GPUs each\), or on Cloud TPUs.

In order to support these use cases, there are six strategies available. Here is a quick overview:

| Training API | MirroredStrategy | TPUStrategy | MultiWorkerMirroredStrategy | CentralStorageStrategy | ParameterServerStrategy | OneDeviceStrategy |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Keras API** | Supported | Experimental support | Experimental support | Experimental support | Supported planned post 2.0 | Supported |
| **Custom training loop** | Experimental support | Experimental support | Support planned post 2.0 | Support planned post 2.0 | No support yet | Supported |
| **Estimator API** | Limited Support | Not supported | Limited Support | Limited Support | Limited Support | Limited Support |

{% hint style="info" %}
Estimator support is limited. Basic training and evaluation are experimental, and advanced features—such as scaffold—are not implemented. We recommend using Keras or custom training loops if a use case is not covered.
{% endhint %}

### MirroredStrategy

[`tf.distribute.MirroredStrategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/MirroredStrategy) supports synchronous distributed training on multiple GPUs on one machine. It creates one replica per GPU device. Each variable in the model is mirrored across all the replicas. Together, these variables form a single conceptual variable called `MirroredVariable`. These variables are kept in sync with each other by applying identical updates.

Efficient all-reduce algorithms are used to communicate the variable updates across the devices. All-reduce aggregates tensors across all the devices by adding them up, and makes them available on each device. It’s a fused algorithm that is very efficient and can reduce the overhead of synchronization significantly. There are many all-reduce algorithms and implementations available, depending on the type of communication available between devices. By default, it uses NVIDIA NCCL as the all-reduce implementation. You can choose from a few other options we provide, or write your own.

### CentralStorageStrategy

[`tf.distribute.experimental.CentralStorageStrategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/experimental/CentralStorageStrategy) does synchronous training as well. Variables are not mirrored, instead they are placed on the CPU and operations are replicated across all local GPUs. If there is only one GPU, all variables and operations will be placed on that GPU.

### MultiWorkerMirroredStrategy

[`tf.distribute.experimental.MultiWorkerMirroredStrategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/experimental/MultiWorkerMirroredStrategy) is very similar to `MirroredStrategy`. It implements synchronous distributed training across multiple workers, each with potentially multiple GPUs. Similar to `MirroredStrategy`, it creates copies of all variables in the model on each device across all workers.

It uses [CollectiveOps](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/ops/collective_ops.py) as the multi-worker all-reduce communication method used to keep variables in sync. A collective op is a single op in the TensorFlow graph which can automatically choose an all-reduce algorithm in the TensorFlow runtime according to hardware, network topology and tensor sizes.

It also implements additional performance optimizations. For example, it includes a static optimization that converts multiple all-reductions on small tensors into fewer all-reductions on larger tensors. In addition, we are designing it to have a plugin architecture - so that in the future, you will be able to plugin algorithms that are better tuned for your hardware. Note that collective ops also implement other collective operations such as broadcast and all-gather.

### TPUStrategy

[`tf.distribute.experimental.TPUStrategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/experimental/TPUStrategy) lets you run your TensorFlow training on Tensor Processing Units \(TPUs\). TPUs are Google's specialized ASICs designed to dramatically accelerate machine learning workloads. They are available on Google Colab, the [TensorFlow Research Cloud](https://www.tensorflow.org/tfrc) and [Cloud TPU](https://cloud.google.com/tpu).

In terms of distributed training architecture, `TPUStrategy` is the same `MirroredStrategy` - it implements synchronous distributed training. TPUs provide their own implementation of efficient all-reduce and other collective operations across multiple TPU cores, which are used in `TPUStrategy`.

### ParameterServerStrategy

[`tf.distribute.experimental.ParameterServerStrategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/experimental/ParameterServerStrategy) supports parameter servers training on multiple machines. In this setup, some machines are designated as workers and some as parameter servers. Each variable of the model is placed on one parameter server. Computation is replicated across all GPUs of all the workers.

### OneDeviceStrategy

[`tf.distribute.OneDeviceStrategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/OneDeviceStrategy) runs on a single device. This strategy will place any variables created in its scope on the specified device. Input distributed through this strategy will be prefetched to the specified device. Moreover, any functions called via `strategy.experimental_run_v2` will also be placed on the specified device.

You can use this strategy to test your code before switching to other strategies which actually distributes to multiple devices/machines.

## Using tf.distribute.Strategy with Keras

We've integrated [`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy) into [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) which is TensorFlow's implementation of the [Keras API specification](https://keras.io/). [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) is a high-level API to build and train models. By integrating into [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras) backend, we've made it seamless for you to distribute your training written in the Keras training framework.

Here's what you need to change in your code:

1. Create an instance of the appropriate [`tf.distribute.Strategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/Strategy)
2. Move the creation and compiling of Keras model inside `strategy.scope`.

We support all types of Keras models - sequential, functional and subclassed.

## Examples and Tutorials

Here is a list of tutorials and examples that illustrate the above integration end to end with Keras:

1. Tutorial to train [MNIST](https://www.tensorflow.org/tutorials/distribute/keras) with `MirroredStrategy`.
2. Official [ResNet50](https://github.com/tensorflow/models/blob/master/official/vision/image_classification/resnet_imagenet_main.py) training with ImageNet data using `MirroredStrategy`.
3. [ResNet50](https://github.com/tensorflow/tpu/blob/master/models/experimental/resnet50_keras/resnet50_tf2.py) trained with Imagenet data on Cloud TPUs with `TPUStrategy`.
4. [Tutorial](https://www.tensorflow.org/tutorials/distribute/multi_worker_with_keras) to train MNIST using `MultiWorkerMirroredStrategy`.
5. [NCF](https://github.com/tensorflow/models/blob/master/official/recommendation/ncf_keras_main.py) trained using `MirroredStrategy`.
6. [Transformer](https://github.com/tensorflow/models/blob/master/official/transformer/v2/transformer_main.py) trained using `MirroredStrategy`.

## Setting up TF\_CONFIG environment variable

For multi-worker training, as mentioned before, you need to set `TF_CONFIG` environment variable for each binary running in your cluster. The `TF_CONFIG` environment variable is a JSON string which specifies what tasks constitute a cluster, their addresses and each task's role in the cluster. We provide a Kubernetes template in the [tensorflow/ecosystem](https://github.com/tensorflow/ecosystem) repo which sets `TF_CONFIG` for your training tasks.

One example of `TF_CONFIG` is:

```javascript
os.environ["TF_CONFIG"] = json.dumps({
    "cluster": {
        "worker": ["host1:port", "host2:port", "host3:port"],
        "ps": ["host4:port", "host5:port"]
    },
   "task": {"type": "worker", "index": 1}
})
```

This `TF_CONFIG` specifies that there are three workers and two ps tasks in the cluster along with their hosts and ports. The "task" part specifies that the role of the current task in the cluster, worker 1 \(the second worker\). Valid roles in a cluster is "chief", "worker", "ps" and "evaluator". There should be no "ps" job except when using [`tf.distribute.experimental.ParameterServerStrategy`](https://www.tensorflow.org/api_docs/python/tf/distribute/experimental/ParameterServerStrategy).

