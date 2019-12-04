# Data input pipelines

The [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) API enables you to build complex input pipelines from simple, reusable pieces. For example, the pipeline for an image model might aggregate data from files in a distributed file system, apply random perturbations to each image, and merge randomly selected images into a batch for training. The pipeline for a text model might involve extracting symbols from raw text data, converting them to embedding identifiers with a lookup table, and batching together sequences of different lengths. The [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) API makes it possible to handle large amounts of data, read from different data formats, and perform complex transformations.

The [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) API introduces a [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) abstraction that represents a sequence of elements, in which each element consists of one or more components. For example, in an image pipeline, an element might be a single training example, with a pair of tensor components representing the image and its label.

There are two distinct ways to create a dataset:

* A data **source** constructs a `Dataset` from data stored in memory or in one or more files.
* A data **transformation** constructs a dataset from one or more [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) objects.

```python
from __future__ import absolute_import, division, print_function, unicode_literals
import tensorflow as tf
import pathlib
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

np.set_printoptions(precision=4)
```

## Basic mechanics

To create an input pipeline, you must start with a data _source_. For example, to construct a `Dataset` from data in memory, you can use [`tf.data.Dataset.from_tensors()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#from_tensors) or [`tf.data.Dataset.from_tensor_slices()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#from_tensor_slices). Alternatively, if your input data is stored in a file in the recommended TFRecord format, you can use [`tf.data.TFRecordDataset()`](https://www.tensorflow.org/api_docs/python/tf/data/TFRecordDataset).

Once you have a `Dataset` object, you can _transform_ it into a new `Dataset` by chaining method calls on the [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) object. For example, you can apply per-element transformations such as [`Dataset.map()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#map), and multi-element transformations such as [`Dataset.batch()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#batch). See the documentation for [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) for a complete list of transformations.

The `Dataset` object is a Python iterable. This makes it possible to consume its elements using a for loop:

```python
dataset = tf.data.Dataset.from_tensor_slices([8, 3, 0, 8, 2, 1])
for elem in dataset:
  print(elem.numpy())
```

Or by explicitly creating a Python iterator using `iter` and consuming its elements using `next`:

```python
it = iter(dataset)

print(next(it).numpy())
```

Alternatively, dataset elements can be consumed using the `reduce` transformation, which reduces all elements to produce a single result. The following example illustrates how to use the `reduce` transformation to compute the sum of a dataset of integers.

```python
print(dataset.reduce(0, lambda state, value: state + value).numpy())
```

### Dataset structure

A dataset contains elements that each have the same \(nested\) structure and the individual components of the structure can be of any type representable by [`tf.TypeSpec`](https://www.tensorflow.org/api_docs/python/tf/TypeSpec), including `Tensor`, `SparseTensor`, `RaggedTensor`, `TensorArray`, or `Dataset`.

The [`Dataset.element_spec`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#element_spec) property allows you to inspect the type of each element component. The property returns a _nested structure_ of [`tf.TypeSpec`](https://www.tensorflow.org/api_docs/python/tf/TypeSpec) objects, matching the structure of the element, which may be a single component, a tuple of components, or a nested tuple of components. For example:

```python
dataset1 = tf.data.Dataset.from_tensor_slices(tf.random.uniform([4, 10]))

dataset2 = tf.data.Dataset.from_tensor_slices(
   (tf.random.uniform([4]),
    tf.random.uniform([4, 100], maxval=100, dtype=tf.int32)))

dataset3 = tf.data.Dataset.zip((dataset1, dataset2))

# Dataset containing a sparse tensor.
dataset4 = tf.data.Dataset.from_tensors(tf.SparseTensor(indices=[[0, 0], [1, 2]], values=[1, 2], dense_shape=[3, 4]))

# Use value_type to see the type of value represented by the element spec
dataset4.element_spec.value_type
```

The `Dataset` transformations support datasets of any structure. When using the [`Dataset.map()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#map), and [`Dataset.filter()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#filter) transformations, which apply a function to each element, the element structure determines the arguments of the function:

```python
dataset1 = tf.data.Dataset.from_tensor_slices(
    tf.random.uniform([4, 10], minval=1, maxval=10, dtype=tf.int32))

for z in dataset1:
  print(z.numpy())
```

```python
dataset2 = tf.data.Dataset.from_tensor_slices(
   (tf.random.uniform([4]),
    tf.random.uniform([4, 100], maxval=100, dtype=tf.int32)))

dataset3 = tf.data.Dataset.zip((dataset1, dataset2))
for a, (b,c) in dataset3:
  print('shapes: {a.shape}, {b.shape}, {c.shape}'.format(a=a, b=b, c=c))
```

## Reading input data

### Consuming NumPy arrays

See [Loading NumPy arrays](https://www.tensorflow.org/tutorials/load_data/numpy) for more examples.

If all of your input data fits in memory, the simplest way to create a `Dataset` from them is to convert them to [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor) objects and use [`Dataset.from_tensor_slices()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#from_tensor_slices).

```python
train, test = tf.keras.datasets.fashion_mnist.load_data()

images, labels = train
images = images/255

dataset = tf.data.Dataset.from_tensor_slices((images, labels))
```

{% hint style="info" %}
The above code snippet will embed the `features` and `labels` arrays in your TensorFlow graph as [`tf.constant()`](https://www.tensorflow.org/api_docs/python/tf/constant) operations. This works well for a small dataset, but wastes memory---because the contents of the array will be copied multiple times---and can run into the 2GB limit for the `tf.GraphDef` protocol buffer.
{% endhint %}

### Consuming Python generators

Another common data source that can easily be ingested as a [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) is the python generator.

{% hint style="warning" %}
While this is a convienient approach it has limited portability and scalibility. It must run in the same python process that created the generator, and is still subject to the Python [GIL](https://en.wikipedia.org/wiki/Global_interpreter_lock).
{% endhint %}

```python
def count(stop):
  i = 0
  while i<stop:
    yield i
    i += 1
    
for n in count(5):
  print(n)
```

The [`Dataset.from_generator`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#from_generator) constructor converts the python generator to a fully functional [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset).

The constructor takes a callable as input, not an iterator. This allows it to restart the generator when it reaches the end. It takes an optional `args` argument, which is passed as the callable's arguments.

The `output_types` argument is required because [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) builds a [`tf.Graph`](https://www.tensorflow.org/api_docs/python/tf/Graph) internally, and graph edges require a `tf.dtype`.

```python
ds_counter = tf.data.Dataset.from_generator(count, args=[25], output_types=tf.int32, output_shapes = (), )
for count_batch in ds_counter.repeat().batch(10).take(10):
  print(count_batch.numpy())
```

The `output_shapes` argument is not _required_ but is highly recomended as many tensorflow operations do not support tensors with unknown rank. If the length of a particular axis is unknown or variable, set it as `None` in the `output_shapes`.

It's also important to note that the `output_shapes` and `output_types` follow the same nesting rules as other dataset methods.

Here is an example generator that demonstrates both aspects, it returns tuples of arrays, where the second array is a vector with unknown length.

```python
def gen_series():
  i = 0
  while True:
    size = np.random.randint(0, 10)
    yield i, np.random.normal(size=(size,))
    i += 1

for i, series in gen_series():
  print(i, ":", str(series))
  if i > 5:
    break
```

The first output is an `int32` the second is a `float32`.

The first item is a scalar, shape `()`, and the second is a vector of unknown length, shape `(None,)`

```python
ds_series = tf.data.Dataset.from_generator(
    gen_series, 
    output_types=(tf.int32, tf.float32), 
    output_shapes=((), (None,)))
```

Now it can be used like a regular [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset). Note that when batching a dataset with a variable shape, you need to use [`Dataset.padded_batch`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#padded_batch).

```python
ds_series_batch = ds_series.shuffle(20).padded_batch(10, padded_shapes=([], [None]))

ids, sequence_batch = next(iter(ds_series_batch))
print(ids.numpy())
print()
print(sequence_batch.numpy())
```

For a more realistic example, try wrapping [`preprocessing.image.ImageDataGenerator`](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator) as a [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset).

First download the data:

```python
flowers = tf.keras.utils.get_file(
    'flower_photos',
    'https://storage.googleapis.com/download.tensorflow.org/example_images/flower_photos.tgz',
    untar=True)
```

Create the [`image.ImageDataGenerator`](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator)

```python
img_gen = tf.keras.preprocessing.image.ImageDataGenerator(rescale=1./255, rotation_range=20)
images, labels = next(img_gen.flow_from_directory(flowers))
print(images.dtype, images.shape)
print(labels.dtype, labels.shape)

ds = tf.data.Dataset.from_generator(
    img_gen.flow_from_directory, args=[flowers], 
    output_types=(tf.float32, tf.float32), 
    output_shapes=([32,256,256,3], [32,5])
)
ds
```

### Consuming TFRecord data

See [Loading TFRecords](https://www.tensorflow.org/tutorials/load_data/tf_records) for an end-to-end example.

The [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) API supports a variety of file formats so that you can process large datasets that do not fit in memory. For example, the TFRecord file format is a simple record-oriented binary format that many TensorFlow applications use for training data. The [`tf.data.TFRecordDataset`](https://www.tensorflow.org/api_docs/python/tf/data/TFRecordDataset) class enables you to stream over the contents of one or more TFRecord files as part of an input pipeline.

Here is an example using the test file from the French Street Name Signs \(FSNS\).

```python
# Creates a dataset that reads all of the examples from two files.
fsns_test_file = tf.keras.utils.get_file("fsns.tfrec", "https://storage.googleapis.com/download.tensorflow.org/data/fsns-20160927/testdata/fsns-00000-of-00001")

```

The `filenames` argument to the `TFRecordDataset` initializer can either be a string, a list of strings, or a [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor) of strings. Therefore if you have two sets of files for training and validation purposes, you can create a factory method that produces the dataset, taking filenames as an input argument:

```python
dataset = tf.data.TFRecordDataset(filenames = [fsns_test_file])
```

Many TensorFlow projects use serialized [`tf.train.Example`](https://www.tensorflow.org/api_docs/python/tf/train/Example) records in their TFRecord files. These need to be decoded before they can be inspected:

```python
raw_example = next(iter(dataset))
parsed = tf.train.Example.FromString(raw_example.numpy())

parsed.features.feature['image/text']
```

### Consuming text data

See [Loading Text](https://www.tensorflow.org/tutorials/load_data/text) for an end to end example.

Many datasets are distributed as one or more text files. The [`tf.data.TextLineDataset`](https://www.tensorflow.org/api_docs/python/tf/data/TextLineDataset) provides an easy way to extract lines from one or more text files. Given one or more filenames, a `TextLineDataset` will produce one string-valued element per line of those files.

```python
directory_url = 'https://storage.googleapis.com/download.tensorflow.org/data/illiad/'
file_names = ['cowper.txt', 'derby.txt', 'butler.txt']

file_paths = [
    tf.keras.utils.get_file(file_name, directory_url + file_name)
    for file_name in file_names
]

dataset = tf.data.TextLineDataset(file_paths)

for line in dataset.take(5):
  print(line.numpy())
```

To alternate lines between files use [`Dataset.interleave`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#interleave). This makes it easier to shuffle files together. Here are the first, second and third lines from each translation:

```python
files_ds = tf.data.Dataset.from_tensor_slices(file_paths)
lines_ds = files_ds.interleave(tf.data.TextLineDataset, cycle_length=3)

for i, line in enumerate(lines_ds.take(9)):
  if i % 3 == 0:
    print()
  print(line.numpy())
```

By default, a `TextLineDataset` yields _every_ line of each file, which may not be desirable, for example, if the file starts with a header line, or contains comments. These lines can be removed using the [`Dataset.skip()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#skip) or [`Dataset.filter()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#filter) transformations. Here we skip the first line, then filter to find only survivors.

```python
titanic_file = tf.keras.utils.get_file("train.csv", "https://storage.googleapis.com/tf-datasets/titanic/train.csv")
titanic_lines = tf.data.TextLineDataset(titanic_file)
for line in titanic_lines.take(10):
  print(line.numpy())

def survived(line):
  return tf.not_equal(tf.strings.substr(line, 0, 1), "0")

survivors = titanic_lines.skip(1).filter(survived)

for line in survivors.take(10):
  print(line.numpy())
```

### Consuming CSV data

See [Loading CSV Files](https://www.tensorflow.org/tutorials/load_data/csv), and [Loading Pandas DataFrames](https://www.tensorflow.org/tutorials/load_data/pandas) for more examples.

The CSV file format is a popular format for storing tabular data in plain text.

For example:

```python
titanic_file = tf.keras.utils.get_file("train.csv", "https://storage.googleapis.com/tf-datasets/titanic/train.csv")
df = pd.read_csv(titanic_file, index_col=None)
df.head()
```

If your data fits in memory the same [`Dataset.from_tensor_slices`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#from_tensor_slices) method works on dictionaries, allowing this data to be easily imported:

```python
titanic_slices = tf.data.Dataset.from_tensor_slices(dict(df))

for feature_batch in titanic_slices.take(1):
  for key, value in feature_batch.items():
    print("  {!r:20s}: {}".format(key, value))
```

A more scalable approach is to load from disk as necessary.

The [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) module provides methods to extract records from one or more CSV files that comply with [RFC 4180](https://tools.ietf.org/html/rfc4180).

The [`experimental.make_csv_dataset`](https://www.tensorflow.org/api_docs/python/tf/data/experimental/make_csv_dataset) function is the high level interface for reading sets of csv files. It supports column type inference and many other features, like batching and shuffling, to make usage simple.

```python
titanic_batches = tf.data.experimental.make_csv_dataset(
    titanic_file, batch_size=4,
    label_name="survived")

for feature_batch, label_batch in titanic_batches.take(1):
  print("'survived': {}".format(label_batch))
  print("features:")
  for key, value in feature_batch.items():
    print("  {!r:20s}: {}".format(key, value))
```

You can use the `select_columns` argument if you only need a subset of columns.

```python
titanic_batches = tf.data.experimental.make_csv_dataset(
    titanic_file, batch_size=4,
    label_name="survived", select_columns=['class', 'fare', 'survived'])

for feature_batch, label_batch in titanic_batches.take(1):
  print("'survived': {}".format(label_batch))
  for key, value in feature_batch.items():
    print("  {!r:20s}: {}".format(key, value))
```

There is also a lower-level [`experimental.CsvDataset`](https://www.tensorflow.org/api_docs/python/tf/data/experimental/CsvDataset) class which provides finer grained control. It does not support column type inference. Instead you must specify the type of each column.

```python
titanic_types  = [tf.int32, tf.string, tf.float32, tf.int32, tf.int32, tf.float32, tf.string, tf.string, tf.string, tf.string] 
dataset = tf.data.experimental.CsvDataset(titanic_file, titanic_types , header=True)

for line in dataset.take(10):
  print([item.numpy() for item in line])
```

If some columns are empty, this low-level interface allows you to provide default values instead of column types.

```text
%% writefile missing.csv
1,2,3,4
,2,3,4
1,,3,4
1,2,,4
1,2,3,
,,,
```

```python
# Creates a dataset that reads all of the records from two CSV files, each with
# four float columns which may have missing values.

record_defaults = [999,999,999,999]
dataset = tf.data.experimental.CsvDataset("missing.csv", record_defaults)
dataset = dataset.map(lambda *items: tf.stack(items))
for line in dataset:
  print(line.numpy())
```

By default, a `CsvDataset` yields _every_ column of _every_ line of the file, which may not be desirable, for example if the file starts with a header line that should be ignored, or if some columns are not required in the input. These lines and fields can be removed with the `header` and `select_cols` arguments respectively.

```python
# Creates a dataset that reads all of the records from two CSV files with
# headers, extracting float data from columns 2 and 4.
record_defaults = [999, 999] # Only provide defaults for the selected columns
dataset = tf.data.experimental.CsvDataset("missing.csv", record_defaults, select_cols=[1, 3])
dataset = dataset.map(lambda *items: tf.stack(items))
for line in dataset:
  print(line.numpy())
```

### Consuming sets of files

There are many datasets distributed as a set of files, where each file is an example.

```python
flowers_root = tf.keras.utils.get_file(
    'flower_photos',
    'https://storage.googleapis.com/download.tensorflow.org/example_images/flower_photos.tgz',
    untar=True)
flowers_root = pathlib.Path(flowers_root)
```

The root directory contains a directory for each class:

```python
for item in flowers_root.glob("*"):
  print(item.name)
```

The files in each class directory are examples:

```python
list_ds = tf.data.Dataset.list_files(str(flowers_root/'*/*'))

for f in list_ds.take(5):
  print(f.numpy())
```

We can read the data using the [`tf.io.read_file`](https://www.tensorflow.org/api_docs/python/tf/io/read_file) function and extract the label from the path, returning `(image, label)` pairs:

```python
def process_path(file_path):
  label = tf.strings.split(file_path, '/')[-2]
  return tf.io.read_file(file_path), label

labeled_ds = list_ds.map(process_path)

for image_raw, label_text in labeled_ds.take(1):
  print(repr(image_raw.numpy()[:100]))
  print()
  print(label_text.numpy())
```

## Batching dataset elements

### Simple batching

The simplest form of batching stacks `n` consecutive elements of a dataset into a single element. The [`Dataset.batch()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#batch) transformation does exactly this, with the same constraints as the [`tf.stack()`](https://www.tensorflow.org/api_docs/python/tf/stack) operator, applied to each component of the elements: i.e. for each component _i_, all elements must have a tensor of the exact same shape.

```python
inc_dataset = tf.data.Dataset.range(100)
dec_dataset = tf.data.Dataset.range(0, -100, -1)
dataset = tf.data.Dataset.zip((inc_dataset, dec_dataset))
batched_dataset = dataset.batch(4)

for batch in batched_dataset.take(4):
  print([arr.numpy() for arr in batch])
```

While [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) tries to propagate shape information, the default settings of [`Dataset.batch`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#batch) result in an unknown batch size because the last batch may not be full. Note the `None`s in the shape:

```python
batched_dataset
```

Use the `drop_remainder` argument to ignore that last batch, and get full shape propagation:

```python
batched_dataset = dataset.batch(7, drop_remainder=True)
batched_dataset
```

### Batching tensors with padding

The above recipe works for tensors that all have the same size. However, many models \(e.g. sequence models\) work with input data that can have varying size \(e.g. sequences of different lengths\). To handle this case, the [`Dataset.padded_batch()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#padded_batch) transformation enables you to batch tensors of different shape by specifying one or more dimensions in which they may be padded.

```python
dataset = tf.data.Dataset.range(100)
dataset = dataset.map(lambda x: tf.fill([tf.cast(x, tf.int32)], x))
dataset = dataset.padded_batch(4, padded_shapes=(None,))

for batch in dataset.take(2):
  print(batch.numpy())
  print()
```

The [`Dataset.padded_batch()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#padded_batch) transformation allows you to set different padding for each dimension of each component, and it may be variable-length \(signified by `None` in the example above\) or constant-length. It is also possible to override the padding value, which defaults to 0.

## Training workflows

### Processing multiple epochs

The [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data) API offers two main ways to process multiple epochs of the same data.

The simplest way to iterate over a dataset in multiple epochs is to use the [`Dataset.repeat()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#repeat) transformation. First we create a dataset of titanic data:

```python
titanic_file = tf.keras.utils.get_file("train.csv", "https://storage.googleapis.com/tf-datasets/titanic/train.csv")
titanic_lines = tf.data.TextLineDataset(titanic_file)

def plot_batch_sizes(ds):
  batch_sizes = [batch.shape[0] for batch in ds]
  plt.bar(range(len(batch_sizes)), batch_sizes)
  plt.xlabel('Batch number')
  plt.ylabel('Batch size')
```

Applying the [`Dataset.repeat()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#repeat) transformation with no arguments will repeat the input indefinitely.

The [`Dataset.repeat`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#repeat) transformation concatenates its arguments without signaling the end of one epoch and the beginning of the next epoch. Because of this a [`Dataset.batch`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#batch) applied after [`Dataset.repeat`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#repeat) will yield batches that stradle epoch boundaries:

```python
titanic_batches = titanic_lines.repeat(3).batch(128)
plot_batch_sizes(titanic_batches)
```

If you need clear epoch separation, put [`Dataset.batch`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#batch) before the repeat:

```python
titanic_batches = titanic_lines.batch(128).repeat(3)

plot_batch_sizes(titanic_batches)
```

If you would like to perform a custom computation \(e.g. to collect statistics\) at the end of each epoch then it's simplest to restart the dataset iteration on each epoch:

```python
epochs = 3
dataset = titanic_lines.batch(128)

for epoch in range(epochs):
  for batch in dataset:
    print(batch.shape)
  print("End of epoch: ", epoch)
```

### Randomly shuffling input data

The [`Dataset.shuffle()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#shuffle) transformation maintains a fixed-size buffer and chooses the next element uniformly at random from that buffer.

{% hint style="info" %}
While large buffer\_sizes shuffle more thoroughly, they can take a lot of memory, and significant time to fill. Consider using [`Dataset.interleave`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#interleave) across files if this becomes a problem.
{% endhint %}

Add an index to the dataset so you can see the effect:

```python
lines = tf.data.TextLineDataset(titanic_file)
counter = tf.data.experimental.Counter()

dataset = tf.data.Dataset.zip((counter, lines))
dataset = dataset.shuffle(buffer_size=100)
dataset = dataset.batch(20)
```

Since the `buffer_size` is 100, and the batch size is 20, the first batch contains no elements with an index over 120.

```python
n,line_batch = next(iter(dataset))
print(n.numpy())
```

As with [`Dataset.batch`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#batch) the order relative to [`Dataset.repeat`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#repeat) matters.

[`Dataset.shuffle`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#shuffle) doesn't signal the end of an epoch until the shuffle buffer is empty. So a shuffle placed before a repeat will show every element of one epoch before moving to the next:

```python
dataset = tf.data.Dataset.zip((counter, lines))
shuffled = dataset.shuffle(buffer_size=100).batch(10).repeat(2)

print("Here are the item ID's near the epoch boundary:\n")
for n, line_batch in shuffled.skip(60).take(5):
  print(n.numpy())
  
shuffle_repeat = [n.numpy().mean() for n, line_batch in shuffled]
plt.plot(shuffle_repeat, label="shuffle().repeat()")
plt.ylabel("Mean item ID")
plt.legend()
```

But a repeat before a shuffle mixes the epoch boundaries together:

```python
dataset = tf.data.Dataset.zip((counter, lines))
shuffled = dataset.repeat(2).shuffle(buffer_size=100).batch(10)

print("Here are the item ID's near the epoch boundary:\n")
for n, line_batch in shuffled.skip(55).take(15):
  print(n.numpy())
  
repeat_shuffle = [n.numpy().mean() for n, line_batch in shuffled]

plt.plot(shuffle_repeat, label="shuffle().repeat()")
plt.plot(repeat_shuffle, label="repeat().shuffle()")
plt.ylabel("Mean item ID")
plt.legend()
```

## Preprocessing data

The [`Dataset.map(f)`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#map) transformation produces a new dataset by applying a given function `f` to each element of the input dataset. It is based on the [`map()`](https://en.wikipedia.org/wiki/Map_%28higher-order_function%29) function that is commonly applied to lists \(and other structures\) in functional programming languages. The function `f` takes the [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor) objects that represent a single element in the input, and returns the [`tf.Tensor`](https://www.tensorflow.org/api_docs/python/tf/Tensor) objects that will represent a single element in the new dataset. Its implementation uses standard TensorFlow operations to transform one element into another.

This section covers common examples of how to use [`Dataset.map()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#map).

### Decoding image data and resizing it

When training a neural network on real-world image data, it is often necessary to convert images of different sizes to a common size, so that they may be batched into a fixed size.

Rebuild the flower filenames dataset:

```python
list_ds = tf.data.Dataset.list_files(str(flowers_root/'*/*'))
```

Write a function that manipulates the dataset elements.

```python
# Reads an image from a file, decodes it into a dense tensor, and resizes it
# to a fixed shape.
def parse_image(filename):
  parts = tf.strings.split(file_path, '/')
  label = parts[-2]

  image = tf.io.read_file(filename)
  image = tf.image.decode_jpeg(image)
  image = tf.image.convert_image_dtype(image, tf.float32)
  image = tf.image.resize(image, [128, 128])
  return image, label
```

Test that it works.

```python
file_path = next(iter(list_ds))
image, label = parse_image(file_path)

def show(image, label):
  plt.figure()
  plt.imshow(image)
  plt.title(label.numpy().decode('utf-8'))
  plt.axis('off')

show(image, label)
```

Map it over the dataset.

```python
images_ds = list_ds.map(parse_image)

for image, label in images_ds.take(2):
  show(image, label)
```

### Applying arbitrary Python logic

For performance reasons, we encourage you to use TensorFlow operations for preprocessing your data whenever possible. However, it is sometimes useful to call external Python libraries when parsing your input data. You can use the [`tf.py_function()`](https://www.tensorflow.org/api_docs/python/tf/py_function) operation in a [`Dataset.map()`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#map) transformation.

For example, if you want to apply a random rotation, the [`tf.image`](https://www.tensorflow.org/api_docs/python/tf/image) module only has [`tf.image.rot90`](https://www.tensorflow.org/api_docs/python/tf/image/rot90), which is not very useful for image augmentation.

{% hint style="info" %}
`tensorflow_addons` has a TensorFlow compatible `rotate` in `tensorflow_addons.image.rotate`.
{% endhint %}

To demonstrate [`tf.py_function`](https://www.tensorflow.org/api_docs/python/tf/py_function), try using the `scipy.ndimage.rotate` function instead:

```python
import scipy.ndimage as ndimage

def random_rotate_image(image):
  image = ndimage.rotate(image, np.random.uniform(-30, 30), reshape=False)
  return image

image, label = next(iter(images_ds))
image = random_rotate_image(image)
show(image, label)
```

To use this function with [`Dataset.map`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#map) the same caveats apply as with [`Dataset.from_generator`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#from_generator), you need to describe the return shapes and types when you apply the function:

```python
def tf_random_rotate_image(image, label):
  im_shape = image.shape
  [image,] = tf.py_function(random_rotate_image, [image], [tf.float32])
  image.set_shape(im_shape)
  return image, label

rot_ds = images_ds.map(tf_random_rotate_image)

for image, label in rot_ds.take(2):
  show(image, label)
```

### Parsing tf.Example protocol buffer messages

Many input pipelines extract [`tf.train.Example`](https://www.tensorflow.org/api_docs/python/tf/train/Example) protocol buffer messages from a TFRecord format. Each [`tf.train.Example`](https://www.tensorflow.org/api_docs/python/tf/train/Example) record contains one or more "features", and the input pipeline typically converts these features into tensors.

```python
fsns_test_file = tf.keras.utils.get_file("fsns.tfrec", "https://storage.googleapis.com/download.tensorflow.org/data/fsns-20160927/testdata/fsns-00000-of-00001")
dataset = tf.data.TFRecordDataset(filenames = [fsns_test_file])
```

You can work with [`tf.train.Example`](https://www.tensorflow.org/api_docs/python/tf/train/Example) protos outside of a [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) to understand the data:

```python
raw_example = next(iter(dataset))
parsed = tf.train.Example.FromString(raw_example.numpy())

feature = parsed.features.feature
raw_img = feature['image/encoded'].bytes_list.value[0]
img = tf.image.decode_png(raw_img)
plt.imshow(img)
plt.axis('off')
_ = plt.title(feature["image/text"].bytes_list.value[0])
raw_example = next(iter(dataset))

def tf_parse(eg):
  example = tf.io.parse_example(
      eg[tf.newaxis], {
          'image/encoded': tf.io.FixedLenFeature(shape=(), dtype=tf.string),
          'image/text': tf.io.FixedLenFeature(shape=(), dtype=tf.string)
      })
  return example['image/encoded'][0], example['image/text'][0]

img, txt = tf_parse(raw_example)
print(txt.numpy())
print(repr(img.numpy()[:20]), "...")

decoded = dataset.map(tf_parse)
image_batch, text_batch = next(iter(decoded.batch(10)))
```



