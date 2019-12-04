# Model Definition

The model definition is the core of Ludwig. It is a dictionary that contains all the information needed to build and train a Ludwig model. It mixes ease of use, by means of reasonable defaults, with flexibility, by means of detailed control over the parameters of your model. It is provided to both `experiment` and `train` commands either as a string \(`--model_definition`\) or as a file \(`--model_definition_file`\). The string or the content of the file will be parsed by PyYAML into a dictionary in memory, so any style of YAML accepted by the parser is considered to be valid, so both multiline and oneline formats are accepted. For instance a list of dictionaries can be written both as

```javascript
mylist: [{name: item1, score: 2}, {name: item2, score: 1}, {name: item3, score: 4}]
```

or as:

```yaml
mylist:
    -
        name: item1
        score: 2
    -
        name: item2
        score: 1
    -
        name: item3
        score: 4
```

The structure of the model definition file is a dictionary with five keys:

```yaml
input_features: []
combiner: {}
output_features: []
training: {}
preprocessing: {}
```

Only `input_features` and `output_features` are required, the other three fields have default values, but you are free to modify them.

## Input features

The `input_features` list contains a list of dictionaries, each of them containing two required fields `name` and `type`. `name` is the name of the feature and is the same name of the column of the CSV input file, `type` is one of the supported datatypes. Input features may have different ways to be encoded and the parameter to decide it is `encoder`.

All the other parameters you specify in an input feature will be passed as parameters to the function that build the encoder, and each encoder can have different parameters.

For instance a `sequence` feature can be encoded by a `stacked_cnn` or by and `rnn`, but only the `stacked_cnn` will accept the parameter `num_filters` while only the `rnn` will accept the parameter `bidirectional`.

A list of all the encoders available for all the datatypes alongside with the description of all parameters will be provided in the datatype-specific sections. Some datatypes have only one type of encoder, so you are not required to specify it.

The role of the encoders is to map inputs into tensors, usually vectors in the case of datatype without a temporal / sequential aspect, matrices in case there is a temporal / sequential aspect or higher rank tensors in case there is a spatial or a spatio-temporal aspect to the input data.

Different configurations of the same encoder may return a tensor with different rank, for instance a sequential encoder may return a vector of size `h` that is either the final vector of a sequence or the result of pooling over the sequence length, or it can return a matrix of size `l x h` where `l` is the length of the sequence and `h` is the hidden dimension if you specify the pooling reduce operation \(`reduce_output`\) to be `null`. For the sake of simplicity you can imagine the output to be a vector in most of the cases, but there is a `reduce_output` parameter one can specify to change the default behavior.

An additional feature that ludwig provides is the option to have tied weights between different encoders. For instance if my model takes two sentences as input and return the probability of their entailment, I may want to encode both sentences with the same encoder. The way to do it is by specifying the `tied-weights` parameter of the second feature you define to be the name of the first feature you defined.

```yaml
input_features:
    -
        name: sentence1
        type: text
    -
        name: sentence2
        type: text
        tied_weights: sentence1
```

If you specify a name of an input feature that has not been defined yet, it will result in an error. Also, in order to be able to have tied weights, all encoder parameters have to be identical between the two input features.

## Combiner

Combiners are part of the model that take all the outputs of the different input features and combine them in a single representation that is passed to the outputs. You can specify which one to use in the `combiner` section of the model definition. Different combiners implement different combination logic, but the default one `concat` just concatenates all outputs of input feature encoders and optionally passes the concatenation through fully connected layers, with the output of the last layer being forwarded to the outputs decoders.

```yaml
+-----------+
|Input      |
|Feature 1  +-+
+-----------+ |            +---------+
+-----------+ | +------+   |Fully    |
|...        +--->Concat+--->Connected+->
+-----------+ | +------+   |Layers   |
+-----------+ |            +---------+
|Input      +-+
|Feature N  |
+-----------+
```

For the sake of simplicity you can imagine the both inputs and outputs are vectors in most of the cases, but there are `reduce_input` and `reduce_output` parameters to specify to change the default behavior.

## Output Features

The `output_features` list has the same structure of the `input_features` list: it is a list of dictionaries containing a `name` and a `type`. They represent outputs / targets that you want your model to predict. In most machine learning tasks you want to predict only one target variable, but in Ludwig you are allowed to specify as many outputs as you want and they are going to be optimized in a multi-task fashion, using a weighted sum of their losses as a combined loss to optimize.

Instead of having `encoders`, output features have `decoders`, but most of them have only one decoder so you don't have to specify it.

Decoders take the output of the combiner as input, process it further, for instance passing it through fully connected layers, and finally predict values and compute a loss and some measures \(depending on the datatype different losses and measures apply\).

Decoders have additional parameters, in particular `loss` that allows you to specify a different loss to optimize for this specific decoder, for instance numerical features support both `mean_squared_error` and `mean_absolute_error` as losses. Details about the available decoders and losses alongside with the description of all parameters will be provided in the datatype-specific sections.

For the sake of simplicity you can imagine the input coming from the combiner to be a vector in most of the cases, but there is a `reduce_input` parameter one can specify to change the default behavior.

## Output Features Dependencies

An additional feature that Ludwig provides is the concept of dependency between `output_features`. You can specify a list of output features as dependencies when you write the dictionary of a specific feature. At model building time Ludwig checks that no cyclic dependency exists. If you do so Ludwig will concatenate all the final representations before the prediction of those output features to the original input of the decoder. The reason is that if different output features have a causal dependency, knowing which prediction has been made for one can help making the prediction of the other.

For instance if two output features are one coarse grained category and one fine-grained category that are in a hierarchical structure with each other, knowing the prediction made for coarse grained restricts the possible categories to predict for the fine-grained. In this case the following model definition structure can be used:

```yaml
output_features:
    -
        name: coarse_class
        type: category
        num_fc_layers: 2
        fc_size: 64
    -
        name: fine_class
        type: category
        dependencies:
            - coarse_class
        num_fc_layers: 1
        fc_size: 64
```

Assuming the input coming from the combiner has hidden dimension `h` 128, there are two fully connected layers that return a vector with hidden size 64 at the end of the `coarse_class` decoder \(that vector will be used for the final layer before projecting in the output `coarse_class` space\) In the decoder of `fine_class`, the 64 dimensional vector of `coarse_class` will be concatenated to the combiner output vector, making a vector of hidden size 192 that will be passed through a fully connected layer and the 64 dimensional output will be used for the final layer before projecting in the output class space of the `fine_class`.

## Training



The `training` section of the model definition lets you specify some parameters of the training process, like for instance the number of epochs or the learning rate.

These are the available training parameters:

* `batch_size` \(default `128`\): size of the batch used for training the model.
* `eval_batch_size` \(default `0`\): size of the batch used for evaluating the model. If it is `0`, the same value of `batch_size` is used. This is usefult to speedup evaluation with a much bigger batch size than training, if enough memory is available, or to decrease the batch size when `sampled_softmax_cross_entropy` is used as loss for sequential and categorical features with big vocabulary sizes \(evaluation needs to be performed on the full vocabulary, so a much smaller batch size may be needed to fit the activation tensors in memory\).
* `epochs` \(default `100`\): number of epochs the training process will run for.
* `early_stop` \(default `5`\): if there's a validation set, number of epochs of patience without an improvement on the validation measure before the training is stopped.
* `optimizer` \(default `{type: adam, beta1: 0.9, beta2: 0.999, epsilon: 1e-08}`\): which optimizer to use with the relative parameters. The available optimizers are: `sgd` \(or `stochastic_gradient_descent`, `gd`, `gradient_descent`, they are all the same\), `adam`, `adadelta`, `adagrad`, `adagradda`, `momentum`, `ftrl`, `proximalgd`, `proximaladagrad`, `rmsprop`. To know their parameters check [TensorFlow's optimizer documentation](https://www.tensorflow.org/api_docs/python/tf/train).
* `learning_rate` \(default `0.001`\): the learning rate to use.
* `decay` \(default `false`\): if to use exponential decay of the learning rate or not.
* `decay_rate` \(default `0.96`\): the rate of the exponential learning rate decay.
* `decay_steps` \(default `10000`\): the number of steps of the exponential learning rate decay.
* `staircase` \(default `false`\): decays the learning rate at discrete intervals.
* `regularization_lambda` \(default `0`\): the lambda parameter used for adding a l2 regularization loss to the overall loss.
* `dropout_rate` \(default `0.0`\): the probability to drop neurons in dropout. The `dropout_rate` is used throughout the whole model, but to decide which parts of the model will use it, use the `dropout` boolean parameter available in each encoder, combiner and decoder.
* `reduce_learning_rate_on_plateau` \(default `0`\): if there's a validation set, how many times to reduce the learning rate when a plateau of validation measure is reached.
* `reduce_learning_rate_on_plateau_patience` \(default `5`\): if there's a validation set, number of epochs of patience without an improvement on the validation measure before reducing the learning rate.
* `reduce_learning_rate_on_plateau_rate` \(default `0.5`\): if there's a validation set, the reduction rate of the learning rate.
* `increase_batch_size_on_plateau` \(default `0`\): if there's a validation set, how many times to increase the batch size when a plateau of validation measure is reached.
* `increase_batch_size_on_plateau_patience` \(default `5`\): if there's a validation set, number of epochs of patience without an improvement on the validation measure before increasing the learning rate.
* `increase_batch_size_on_plateau_rate` \(default `2`\): if there's a validation set, the increase rate of the batch size.
* `increase_batch_size_on_plateau_max` \(default `512`\): if there's a validation set, the maximum value of batch size.
* `validation_field` \(default `combined`\): when there is more than one output feature, which one to use for computing if there was an improvement on validation. The measure to use to determine if there was an improvement can be set with the `validation_measure` parameter. Different datatypes have different available measures, refer to the datatype-specific section for more details. `combined` indicates the use the combination of all features. For instance the combination of `combined` and `loss` as measure uses a decrease in the combined loss of all output features to check for improvement on validation, while `combined` and `accuracy` considers on how many datapoints the predictions for all output features were correct \(but consider that for some features, for instance `numeric` there is no accuracy measure, so you should use `accuracy` only if all your output features have an accuracy measure\).
* `validation_measure:` \(default `loss`\): the measure to use to determine if there was an improvement. The measure is considered for the output feature specified in `validation_field`. Different datatypes have different available measures, refer to the datatype-specific section for more details.
* `bucketing_field` \(default `null`\): when not `null`, when creating batches, instead of shuffling randomly, the length along the last dimension of the matrix of the specified input feature is used for bucketing datapoints and then randomly shuffled datapoints from the same bin are sampled. Padding is trimmed to the longest datapoint in the batch. The specified feature should be either a `sequence` or `text` feature and the encoder encoding it has to be `rnn`. When used, bucketing improves speed of `rnn` encoding up to 1.5x, depending on the length distribution of the inputs.
* `learning_rate_warmup_epochs` \(default `1`\): It's the number or training epochs where learning rate warmup will be used. It is calculated as ```described in [Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour](https://arxiv.org/abs/1706.02677). In the paper the authors suggest``6\` epochs of warmup, that parameter is suggested for large datasets and big batches.

## Preprocessing

The `preprocessing` section of the model definition makes it possible to specify datatype specific parameters to perform data preprocessing. The preprocessing dictionary contains one key of each datatype, but you have to specify only the ones that apply to your case, the other ones will be kept as defaults. Moreover, the preprocessing dictionary contains parameters related to how to split the data that are not feature specific.

* `force_split` \(default `false`\): if `true` the `split` column in the CSV data file is ignored and the dataset is randomly split. If `false` the `split` column is used if available.
* `split_probabilities` \(default `[0.7, 0.1, 0.2]`\): the proportion of the CSV data to end up in training, validation and test. The three values have to sum up to one.
* `stratify` \(default `null`\): if `null` the split is random, otherwise you can specify the name of a `category` feature and the split will be stratified on that feature.

Example preprocessing dictionary \(showing default values\):

```yaml
preprocessing:
    force_split: false
    split_probabilities: [0.7, 0.1, 0.2]
    stratify: null
    category: {...}
    sequence: {...}
    text: {...}
    ...
```

The details about the preprocessing parameters that each datatype accepts will be provided in the datatype-specific sections.

It is important to point out that different features within the same datatype may require different preprocessing. For instance a document classification model may have two text input features, one for the title of the document and one for the body.

As the length of the title is much shorter than the length of the body, the parameter `word_length_limit` should be set to 10 for the title and 2000 for the body, but both of them share the same parameter `most_common_words` with value 10000.

The way to do this is adding a `preprocessing` key inside the title `input_feature` dictionary and one in the `body` input feature dictionary containing the desired parameter and value. The model definition will look like:

```yaml
preprocessing:
    text:
        most_common_word: 10000
input_features:
    -
        name: title
        type: text
        preprocessing:
            word_length_limit: 20
    -
        name: body
        type: text
        preprocessing:
            word_length_limit: 2000
```



### 

