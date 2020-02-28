# Vector Features

Vector features allow to provide an ordered set of numerical values all at once. This is useful for providing pre-trained representations or activations obtained from other models or for providing multivariate inputs and outputs. An interesting use of vector features is the possibility to provide a probability distribution as output for a multiclass classification problem instead of just the correct class like it is possible to do with category features. This is useful for distillation and noise-aware losses.

## Preprocessing

* The data is expected as whitespace separated numerical values. Example: "1.0 0.0 1.04 10.49".
* All vectors are expected to be of the same size.
* A `vector_size` parameter can be provided in the `preprocessing` dictionary.
* If the `vector_size` is not provided in the `preprocessing` section, it will be inferred from the data.
* Currently no missing values handling is supported, we expect the user to make sure there are no missing values.

## Feature Encoders

### DENSE ENCODER

For vector features, you can use a dense encoder \(stack of fully connected layers\). It takes the following parameters:

* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the encoder will be used instead. If both `fc_layers` and `num_fc_layers` are `null`, a default list will be assigned to `fc_layers` with the value `[{fc_size: 512}, {fc_size: 256}]`. \(only applies if `reduce_output` is not `null`\).
* `num_fc_layers` \(default `0`\): This is the number of stacked fully connected layers.
* `fc_size` \(default `10`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` or `conv_layers` this is the default `norm` that will be used for each layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` or `conv_layers` this is the default `activation` that will be used for each layer. It indicates the activation function applied to the output.
* `dropout` \(default `false`\): determines if there should be a dropout layer before returning the encoder output.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the embedding weights are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).

Example vector feature entry in the input features list using an dense encoder:

```yaml
name: vector_csv_column_name
type: vector
encoder: dense
fc_layers: null
num_fc_layers: 0
fc_size: 10
norm: null
activation: relu
dropout: false
initializer: null
regularize: true
```

## Feature Decoders

Vector features can be used when multi-class classification needs to be performed with a noise-aware loss or when the task is multivariate regression. There is only one decoder available for set features and it is a \(potentially empty\) stack of fully connected layers, followed by a projection into a vector of size \(optionally followed by a softmax in the case of multi-class classification\).

```yaml
+--------------+   +---------+   +-----------+
|Combiner      |   |Fully    |   |Projection |   +------------------+
|Output        +--->Connected+--->into Output+--->Softmax (optional)|
|Representation|   |Layers   |   |Space      |   +------------------+
+--------------+   +---------+   +-----------+
```

These are the available parameters of the set output feature

* `reduce_inputs` \(default `sum`\): defines how to reduce an input that is not a vector, but a matrix or a higher order tensor, on the first dimension 9second if you count the batch dimension\). Available values are: `sum`, `mean` or `avg`, `max`, `concat` \(concatenates along the first dimension\), `last` \(returns the last vector of the first dimension\).
* `dependencies` \(default `[]`\): the output features this one is dependent on. For a detailed explanation refer to [Output Features Dependencies](./#output-features-dependencies).
* `reduce_dependencies` \(default `sum`\): defines how to reduce the output of a dependent feature that is not a vector, but a matrix or a higher order tensor, on the first dimension 9second if you count the batch dimension\). Available values are: `sum`, `mean` or `avg`, `max`, `concat` \(concatenates along the first dimension\), `last` \(returns the last vector of the first dimension\).
* `softmax` \(default `False`\): determines if to apply a softmax at the end of the decoder. It is useful for predicting a vector of values that sum up to 1 and can be interpreted as probabilities.
* `loss` \(default `{type: mean_squared_error}`\): is a dictionary containing a loss `type`. The available loss `type` are `mean_squared_error`, `mean_absolute_error` and `softmax_cross_entropy` \(use it only if `softmax` is `True`\).

These are the available parameters of a set output feature decoder

* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation`, `dropout`, `initializer` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the decoder will be used instead.
* `num_fc_layers` \(default 0\): this is the number of stacked fully connected layers that the input to the feature passes through. Their output is projected in the feature's output space.
* `fc_size` \(default `256`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` this is the default `activation` that will be used for each layer. It indicates the activation function applied to the output.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` this is the default `norm` that will be used for each layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `dropout` \(default `false`\): determines if there should be a dropout layer after each layer.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the wights of the layers are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).

Example set feature entry \(with default parameters\) in the output features list:

```yaml
name: set_csv_column_name
type: set
reduce_inputs: sum
dependencies: []
reduce_dependencies: sum
loss:
    type: sigmoid_cross_entropy
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

## Measures

The measures that are calculated every epoch and are available for numerical features are `mean_squared_error`, `mean_absolute_error`, `r2` and the `loss` itself. You can set either of them as `validation_measure` in the `training` section of the model definition if you set the `validation_field` to be the name of a numerical feature.

