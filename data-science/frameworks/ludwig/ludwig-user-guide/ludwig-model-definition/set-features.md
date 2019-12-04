# Set Features

### Preprocessing

Set features are transformed into a binary \(int8 actually\) valued matrix of size `n x l` \(where `n` is the size of the dataset and `l` is the minimum of the size of the biggest set and a `max_size` parameter\) and added to HDF5 with a key that reflects the name of column in the CSV. The way sets are mapped into integers consists in first using a formatter to map from strings to sequences of set items \(by default this is done by splitting on spaces\). Then a a dictionary of all the different set item strings present in the column of the CSV is collected, then they are ranked by frequency and an increasing integer ID is assigned to them from the most frequent to the most rare \(with 0 being assigned to `<PAD>` used for padding and 1 assigned to `<UNK>` item\). The column name is added to the JSON file, with an associated dictionary containing 1. the mapping from integer to string \(`idx2str`\) 2. the mapping from string to id \(`str2idx`\) 3. the mapping from string to frequency \(`str2freq`\) 4. the maximum size of all sets \(`max_set_size`\) 5. additional preprocessing information \(by default how to fill missing values and what token to use to fill missing values\)

The parameters available for preprocessing arehe parameters available for preprocessing are

* `missing_value_strategy` \(default `fill_with_const`\): what strategy to follow when there's a missing value in a binary column. The value should be one of `fill_with_const` \(replaces the missing value with a specific value specified with the `fill_value` parameter\), `fill_with_mode` \(replaces the missing values with the most frequent value in the column\), `fill_with_mean` \(replaces the missing values with the mean of the values in the column\), `backfill` \(replaces the missing values with the next valid value\).
* `fill_value` \(default `0`\): the value to replace the missing values with in case the `missing_value_strategy` is `fill-value`.
* `format` \(default `space`\): defines how to map from the raw string content of the CSV column to a set of elements. The default value `space` splits the string on spaces. Other options are: `underscore` \(splits on underscore\), `comma`\(splits on comma\), `json` \(decodes the string into a set or a list through a JSON parser\).
* `lowercase` \(default `false`\): if the string has to be lowercased before being handled by the formatter.
* `most_common` \(default `10000`\): the maximum number of most common tokens to be considered. if the data contains more than this amount, the most infrequent tokens will be treated as unknown.

### **Input Features and Encoders**

Set features have one encoder, the raw binary values coming from the input placeholders are first transformed in sparse integer lists, then they are mapped to either dense or sparse embeddings \(one-hot encodings\), finally they are aggregated and returned as outputs. Inputs are of size `b` while outputs are fo size `b x h` where `b` is the batch size and `h` is the dimensionally of the embeddings.

```yaml
+-+
|0|          +-----+
|0|   +-+    |emb 2|   +-----------+
|1|   |2|    +-----+   |Aggregation|
|0+--->4+---->emb 4+--->Reduce     +->
|1|   |5|    +-----+   |Operation  |
|1|   +-+    |emb 5|   +-----------+
|0|          +-----+
+-+
```

The available encoder parameters are

* `representation'` \(default `dense`\): the possible values are `dense` and `sparse`. `dense` means the embeddings are initialized randomly, `sparse` means they are initialized to be one-hot encodings.
* `embedding_size` \(default `50`\): it is the maximum embedding size, the actual size will be `min(vocabulary_size, embedding_size)` for `dense` representations and exactly `vocabulary_size` for the `sparse` encoding, where `vocabulary_size` is the number of different strings appearing in the training set in the column the feature is named after \(plus 1 for `<UNK>`\).
* `embeddings_on_cpu` \(default `false`\): by default embeddings matrices are stored on GPU memory if a GPU is used, as it allows for faster access, but in some cases the embedding matrix may be really big and this parameter forces the placement of the embedding matrix in regular memory and the CPU is used to resolve them, slightly slowing down the process as a result of data transfer between CPU and GPU memory.
* `pretrained_embeddings` \(default `null`\): by default `dense` embeddings are initialized randomly, but this parameter allow to specify a path to a file containing embeddings in the [GloVe format](https://nlp.stanford.edu/projects/glove/). When the file containing the embeddings is loaded, only the embeddings with labels present in the vocabulary are kept, the others are discarded. If the vocabulary contains strings that have no match in the embeddings file, their embeddings are initialized with the average of all other embedding plus some random noise to make them different from each other. This parameter has effect only if `representation` is `dense`.
* `embeddings_trainable` \(default `true`\): If `true` embeddings are trained during the training process, if `false` embeddings are fixed. It may be useful when loading pretrained embeddings for avoiding finetuning them. This parameter has effect only for `representation` is `dense` as `sparse` one-hot encodings are not trainable.
* `dropout` \(default `false`\): determines if there should be a dropout layer before returning the encoder output.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the embedding weights are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).
* `reduce_output` \(default `sum`\): describes the strategy to use to aggregate the embeddings of the items of the set. Possible values are `sum`, `mean` and `sqrt` \(the weighted sum divided by the square root of the sum of the squares of the weights\).
* `tied_weights` \(default `null`\): name of the input feature to tie the weights the encoder with. It needs to be the name of a feature of the same type and with the same encoder parameters.

Example set feature entry in the output features list:

```yaml
name: set_csv_column_name
type: set
representation: dense
embedding_size: 50
embeddings_on_cpu: false
pretrained_embeddings: null
embeddings_trainable: true
dropout: false
initializer: null
regularize: true
reduce_output: sum
tied_weights: null
```

### Output Features and Decoders

Set features can be used when multi-label classification needs to be performed. There is only one decoder available for set features and it is a \(potentially empty\) stack of fully connected layers, followed by a projection into a vector of size of the number of available classes, followed by a sigmoid.

```yaml
+--------------+   +---------+   +-----------+
|Combiner      |   |Fully    |   |Projection |   +-------+
|Output        +--->Connected+--->into Output+--->Sigmoid|
|Representation|   |Layers   |   |Space      |   +-------+
+--------------+   +---------+   +-----------+
```

These are the available parameters of the set output feature

* `reduce_inputs` \(default `sum`\): defines how to reduce an input that is not a vector, but a matrix or a higher order tensor, on the first dimension 9second if you count the batch dimension\). Available values are: `sum`, `mean` or `avg`, `max`, `concat` \(concatenates along the first dimension\), `last` \(returns the last vector of the first dimension\).
* `dependencies` \(default `[]`\): the output features this one is dependent on. For a detailed explanation refer to [Output Features Dependencies](./#output-features-dependencies).
* `reduce_dependencies` \(default `sum`\): defines how to reduce the output of a dependent feature that is not a vector, but a matrix or a higher order tensor, on the first dimension 9second if you count the batch dimension\). Available values are: `sum`, `mean` or `avg`, `max`, `concat` \(concatenates along the first dimension\), `last` \(returns the last vector of the first dimension\).
* `loss` \(default `{type: sigmoid_cross_entropy}`\): is a dictionary containing a loss `type`. The available loss `type` is `sigmoid_cross_entropy`.

These are the available parameters of a set output feature decoder

* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation`, `dropout`, `initializer` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the decoder will be used instead.
* `num_fc_layers` \(default 0\): this is the number of stacked fully connected layers that the input to the feature passes through. Their output is projected in the feature's output space.
* `fc_size` \(default `256`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` this is the default `activation` that will be used for each layer. It indicates the activation function applied to the output.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` this is the default `norm` that will be used for each layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `dropout` \(default `false`\): determines if there should be a dropout layer after each layer.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the wights of the layers are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).
* `threshold` \(default `0.5`\): The threshold above \(greater or equal\) which the predicted output of the sigmoid will be mapped to 1.

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

### Measures

The measures that are calculated every epoch and are available for category features are `jaccard_index` and the `loss` itself. You can set either of them as `validation_measure` in the `training` section of the model definition if you set the `validation_field` to be the name of a set feature.

