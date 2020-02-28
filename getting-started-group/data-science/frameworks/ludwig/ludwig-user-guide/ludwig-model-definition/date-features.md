# Date Features

## Preprocessing

Ludwig will try to infer the date format automatically, but a specific fomrat can be provided. The format is the same one described in the [datetime package documentation](https://docs.python.org/2/library/time.html#time.strptime).

* `missing_value_strategy` \(default `fill_with_const`\): what strategy to follow when there's a missing value in a binary column. The value should be one of `fill_with_const` \(replaces the missing value with a specific value specified with the `fill_value` parameter\), `fill_with_mode` \(replaces the missing values with the most frequent value in the column\), `fill_with_mean` \(replaces the missing values with the mean of the values in the column\), `backfill` \(replaces the missing values with the next valid value\).
* `fill_value` \(default `""`\): the value to replace the missing values with in case the `missing_value_strategy` is `fill_value`. This can be a datetime string, if left empty the current datetime will be used.
* `datetime_format` \(default `null`\): this parameter can be either `null`, which implies the datetime format is inferred automaticall, or a datetime format string.

Example of a preprocessing specification:

```yaml
name: date_feature_name
type: date
preprocessing:
  missing_value_strategy: fill_with_const
  fill_value: ''
  datetime_format: "%d %b %Y"
```

## Input Features and Encoders

Input date features are transformed into a int valued tensors of size `N x 8` \(where `N` is the size of the dataset and the 8 dimensions contain year, month, day, weekday, yearday, hour, minute and second\) and added to HDF5 with a key that reflects the name of column in the CSV.

Currently there are two encoders supported for dates: Embed Encoder and Wave encoder which can be set by setting `encoder` parameter to `embed` or `wave` in the input feature dictionary in the model definition \(`embed` is the default one\).

### EMBED ENCODER

This encoder passes the year through a fully connected layer of one neuron and embeds all other elements for the date, concatenates them and passes the concatenated representation through fully connected layers. It takes the following optional parameters:

* `embedding_size` \(default `10`\): it is the maximum embedding size adopted..
* `embeddings_on_cpu` \(default `false`\): by default embeddings matrices are stored on GPU memory if a GPU is used, as it allows for faster access, but in some cases the embedding matrix may be really big and this parameter forces the placement of the embedding matrix in regular memory and the CPU is used to resolve them, slightly slowing down the process as a result of data transfer between CPU and GPU memory.
* `dropout` \(default `false`\): determines if there should be a dropout layer before returning the encoder output.
* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the encoder will be used instead. If both `fc_layers` and `num_fc_layers` are `null`, a default list will be assigned to `fc_layers` with the value `[{fc_size: 512}, {fc_size: 256}]`. \(only applies if `reduce_output` is not `null`\).
* `num_fc_layers` \(default `0`\): This is the number of stacked fully connected layers.
* `fc_size` \(default `10`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` or `conv_layers` this is the default `norm` that will be used for each layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` or `conv_layers` this is the default `activation` that will be used for each layer. It indicates the activation function applied to the output.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the embedding weights are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).

Example date feature entry in the input features list using an embed encoder:

```yaml
name: date_csv_column_name
type: date
encoder: embed
embedding_size: 10
embeddings_on_cpu: false
dropout: false
fc_layers: null
num_fc_layers: 0
fc_size: 10
norm: null
activation: relu
initializer: null
regularize: true
```

### WAVE ENCODER

This encoder passes the year through a fully connected layer of one neuron and represents all other elements for the date by taking the sine of their value with a different period \(12 for months, 31 for days, etc.\), concatenates them and passes the concatenated representation through fully connected layers. It takes the following optional parameters:

* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the encoder will be used instead. If both `fc_layers` and `num_fc_layers` are `null`, a default list will be assigned to `fc_layers` with the value `[{fc_size: 512}, {fc_size: 256}]`. \(only applies if `reduce_output` is not `null`\).
* `num_fc_layers` \(default `0`\): This is the number of stacked fully connected layers.
* `fc_size` \(default `10`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` or `conv_layers` this is the default `norm` that will be used for each layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` or `conv_layers` this is the default `activation` that will be used for each layer. It indicates the activation function applied to the output.
* `dropout` \(default `false`\): determines if there should be a dropout layer before returning the encoder output.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the embedding weights are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).

Example date feature entry in the input features list using a wave encoder:

```yaml
name: date_csv_column_name
type: date
encoder: wave
fc_layers: null
num_fc_layers: 0
fc_size: 10
norm: null
activation: relu
dropout: false
initializer: null
regularize: true
```

## Output Features and Decoders

There are no date decoders at the moment \(WIP\), so date cannot be used as output features.

## Measures

As no date decoders are available at the moment, there are also no date measures.



