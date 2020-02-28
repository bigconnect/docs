# Binary Features

### Preprocessing

Binary features are directly transformed into a binary valued vector of length `n` \(where `n` is the size of the dataset\) and added to HDF5 with a key that reflects the name of column in the CSV. No additional information about them is available in the JSON metadata file.

The parameters available for preprocessing are

* `missing_value_strategy` \(default `fill_with_const`\): what strategy to follow when there's a missing value in a binary column. The value should be one of `fill_with_const` \(replaces the missing value with a specific value specified with the `fill_value` parameter\), `fill_with_mode` \(replaces the missing values with the most frequent value in the column\), `fill_with_mean` \(replaces the missing values with the mean of the values in the column\), `backfill` \(replaces the missing values with the next valid value\).
* `fill_value` \(default `0`\): the value to replace the missing values with in case the `missing_value_strategy` is `fill_with_const`.

### Input Features and Encoders

Binary features have no encoder, the raw binary values coming from the input placeholders are just returned as outputs. By consequence there are no encoding parameters. Inputs are of size `b` while outputs are of size `b x 1` where `b` is the batch size.

Example binary feature entry in the output features list:

```yaml
name: binary_csv_column_name
type: binary
```

### Output Features and Decoders

Binary features can be used when a binary classification needs to be performed or when the output is a single probability. There is only one decoder available for binary features and it is a \(potentially empty\) stack of fully connected layers, followed by a projection into a single number followed by a sigmoid function.

These are the available parameters of a binary output feature

* `reduce_inputs` \(default `sum`\): defines how to reduce an input that is not a vector, but a matrix or a higher order tensor, on the first dimension 9second if you count the batch dimension\). Available values are: `sum`, `mean` or `avg`, `max`, `concat` \(concatenates along the first dimension\), `last` \(returns the last vector of the first dimension\).
* `dependencies` \(default `[]`\): the output features this one is dependent on. For a detailed explanation refer to [**Output Features Dependencies**](./#output-features-dependencies).
* `reduce_dependencies` \(default `sum`\): defines how to reduce the output of a dependent feature that is not a vector, but a matrix or a higher order tensor, on the first dimension 9second if you count the batch dimension\). Available values are: `sum`, `mean` or `avg`, `max`, `concat` \(concatenates along the first dimension\), `last` \(returns the last vector of the first dimension\).
* `loss` \(default `{type: cross_entropy, confidence_penalty: 0, robust_lambda: 0}`\): is a dictionary containing a loss `type` and its hyperparameters. The only available loss `type` is `cross_entropy` \(cross entropy\), and the two optional parameters are `confidence_penalty` \(an additional term that penalizes too confident predictions by adding a `a * (max_entropy - entropy) / max_entropy` term to the loss, where a is the value of this parameter\) and `robust_lambda` \(replaces the loss with `(1 - robust_lambda) * loss + robust_lambda / 2` which is useful in case of noisy labels\).

These are the available parameters of a binary output feature decoder

* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation`, `dropout`, `initializer` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the decoder will be used instead.
* `num_fc_layers` \(default 0\): this is the number of stacked fully connected layers that the input to the feature passes through. Their output is projected in the feature's output space.
* `fc_size` \(default `256`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` this is the default `activation` that will be used for each layer. It indicates the activation function applied to the output.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` this is the default `norm` that will be used for each layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `dropout` \(default `false`\): determines if there should be a dropout layer after each layer.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the wights of the layers are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).
* `threshold` \(default `0.5`\): The threshold above \(greater or equal\) which the predicted output of the sigmoid will be mapped to 1.

Example binary feature entry \(with default parameters\) in the output features list:

```yaml
name: binary_csv_column_name
type: binary
reduce_inputs: sum
dependencies: []
reduce_dependencies: sum
loss:
    type: cross_entropy
    confidence_penalty: 0
    robust_lambda: 0
fc_layers: null
num_fc_layers: 0
fc_size: 256
activation: relu
norm: null
dropout: false
initializer: null
regularize: true
threshold: 0.5
```

### Measures

The only measures that are calculated every epoch and are available for binary features are the `accuracy` and the `loss` itself. You can set either of them as `validation_measure` in the `training` section of the model definition if you set the `validation_field` to be the name of a binary feature.

