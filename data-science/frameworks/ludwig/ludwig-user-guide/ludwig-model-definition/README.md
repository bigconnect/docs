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

## Feature types

See sub-sections for all supported feature types.

## Combiners

Combiners are the part of the model that take the outputs of the encoders of all input features and combine them before providing the combined representation to the different output decoders. If you don't specify a combiner, the concat combiner will be used.

### Concat Combiner

The concat combiner assumes all outputs from encoders are tensors of size `b x h` where `b` is the batch size and `h` is the hidden dimension, which can be different for each input. It concatenates along the `h` dimension, and then \(optionally\) passes the concatenated tensor through a stack of fully connected layers. It returns the final `b x h'` tensor where `h'` is the size of the last fully connected layer or the sum of the sizes of the `h` of all inputs in the case there are no fully connected layers. If there's only one input feature and no fully connected layers are specified, the output of the input feature is just passed through as output.

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

These are the available parameters of a concat combiner

* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation`, `dropout`, `initializer` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the decoder will be used instead.
* `num_fc_layers` \(default 0\): this is the number of stacked fully connected layers that the input to the feature passes through. Their output is projected in the feature's output space.
* `fc_size` \(default `256`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` this is the default `activation` that will be used for each layer. It indicates the activation function applied to the output.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` this is the default `norm` that will be used for each layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `dropout` \(default `false`\): determines if there should be a dropout layer after each layer.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the weights of the layers are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).

Example concat combiner in the model definition:

```yaml
type: concat
fc_layers: null
num_fc_layers: 0
fc_size: 256
activation: relu
norm: null
dropout: false
initializer: null
regularize: true
```

### Sequence Concat Combiner

The sequence concat combiner assumes at least one output from encoders is a tensors of size `b x s x h` where `b` is the batch size, `s` is the length of the sequence and `h` is the hidden dimension. The sequence / text / sequential input can be specified with the `main_sequence_feature` parameter that should have the name of the sequential feature as value. If no `main_sequence_feature` is specified, the combiner will look through all the features in the order they are defined in the model definition and will look for a feature with a rank 3 tensor output \(sequence, text or time series\). If it cannot find one it will raise an exception, otherwise the output of that feature will be used for concatenating the other features along the sequence `s` dimension.

If there are other input features with a rank 3 output tensor, the combiner will concatenate them alongside the `s` dimension, which means that all of them must have identical `s` dimension, otherwise an error will be thrown. Specifically, as the placeholders of the sequential features are of dimension `[None, None]` in order to make the `BucketedBatcher` trim longer sequences to their actual length, the check if the sequences are of the same length cannot be performed at model building time, and a dimension mismatch error will be returned during training when a datapoint with two sequential features of different lengths are provided.

Other features that have a `b x h` rank 2 tensor output will be replicated `s` times and concatenated to the `s` dimension. The final output is a `b x s x h'` tensor where `h'` is the size of the concatenation of the `h` dimensions of all input features.

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

These are the available parameters of a sequence concat combiner

* `main_sequence_feature` \(default `null`\): name fo the sequence / text/ time series feature to concatenate the outputs of the other features to. If no `main_sequence_feature` is specified, the combiner will look through all the features in the order they are defined in the model definition and will look for a feature with a rank 3 tensor output \(sequence, text or time series\). If it cannot find one it will raise an exception, otherwise the output of that feature will be used for concatenating the other features along the sequence `s` dimension. If there are other input features with a rank 3 output tensor, the combiner will concatenate them alongside the `s` dimension, which means that all of them must have identical `s` dimension, otherwise an error will be thrown.

Example sequence concat combiner in the model definition:

```yaml
type: sequence_concat
main_sequence_feature: null
```

### Sequence Combiner

The sequence combiner stacks a sequence concat combiner with a sequence encoder one on top of each other. All the considerations about inputs tensor ranks describer for the sequence concat combiner apply also in this case, but the main difference is that this combiner uses the `b x s x h'` output of the sequence concat combiner, where `b` is the batch size, `s` is the sequence length and `h'` is the sum of the hidden dimensions of all input features, as input fo any of the sequence encoders described in the sequence features encoders section. Refer to that section for more detailed information about the sequence encoders and their parameters. Also all the considerations on the shape of the outputs done for the sequence encoders apply in this case too.

```yaml
Sequence
Feature
Output

+---------+
|emb seq 1|
+---------+
|...      +--+
+---------+  |  +-----------------+
|emb seq n|  |  |emb seq 1|emb oth|   +--------+
+---------+  |  +-----------------+   |Sequence|
             +-->...      |...    +-->+Encoder +->
Other        |  +-----------------+   |        |
Feature      |  |emb seq n|emb oth|   +--------+
Output       |  +-----------------+
             |
+-------+    |
|emb oth+----+
+-------+
```

Example sequence concat combiner in the model definition:

```yaml
type: sequence
main_sequence_feature: null
encoder: parallel_cnn
... encoder parameters ...
```

## Visualizations

Several visualization can be obtained from the result files from both `train`, `predict` and `experiment` by using the `visualize` command. The command has several parameters, but not all the visualizations use all of them. Let's first present the parameters of the general script, and then, for each available visualization, we will discuss about the specific parameters needed and what visualization they produce.

```text
usage: ludwig visualize [options]

This script analyzes results and shows some nice plots.

optional arguments:
  -h, --help            show this help message and exit
  -d DATA_CSV, --data_csv DATA_CSV
                        raw data file
  -g GROUND_TRUTH, --ground_truth GROUND_TRUTH
                        ground truth file
  -gm GROUND_TRUTH_METADATA, --ground_truth_metadata GROUND_TRUTH_METADATA
                        input metadata JSON file
  -v {learning_curves,compare_performance,compare_classifiers_performance_from_prob,compare_classifiers_performance_from_pred,compare_classifiers_performance_subset,compare_classifiers_performance_changing_k,compare_classifiers_multiclass_multimetric,compare_classifiers_predictions,compare_classifiers_predictions_distribution,confidence_thresholding,confidence_thresholding_data_vs_acc,confidence_thresholding_data_vs_acc_subset,confidence_thresholding_data_vs_acc_subset_per_class,confidence_thresholding_2thresholds_2d,confidence_thresholding_2thresholds_3d,binary_threshold_vs_metric,roc_curves,roc_curves_from_test_statistics,calibration_1_vs_all,calibration_multiclass,confusion_matrix,frequency_vs_f1}, --visualization {learning_curves,compare_performance,compare_classifiers_performance_from_prob,compare_classifiers_performance_from_pred,compare_classifiers_performance_subset,compare_classifiers_performance_changing_k,compare_classifiers_multiclass_multimetric,compare_classifiers_predictions,compare_classifiers_predictions_distribution,confidence_thresholding,confidence_thresholding_data_vs_acc,confidence_thresholding_data_vs_acc_subset,confidence_thresholding_data_vs_acc_subset_per_class,confidence_thresholding_2thresholds_2d,confidence_thresholding_2thresholds_3d,binary_threshold_vs_metric,roc_curves,roc_curves_from_test_statistics,calibration_1_vs_all,calibration_multiclass,confusion_matrix,frequency_vs_f1}
                        type of visualization
  -f FIELD, --field FIELD
                        field containing ground truth
  -tf THRESHOLD_FIELDS [THRESHOLD_FIELDS ...], --threshold_fields THRESHOLD_FIELDS [THRESHOLD_FIELDS ...]
                        fields for 2d threshold
  -pred PREDICTIONS [PREDICTIONS ...], --predictions PREDICTIONS [PREDICTIONS ...]
                        predictions files
  -prob PROBABILITIES [PROBABILITIES ...], --probabilities PROBABILITIES [PROBABILITIES ...]
                        probabilities files
  -trs TRAINING_STATS [TRAINING_STATS ...], --training_statistics TRAINING_STATS [TRAINING_STATS ...]
                        training stats files
  -tes TEST_STATS [TEST_STATS ...], --test_statistics TEST_STATS [TEST_STATS ...]
                        test stats files
  -mn MODEL_NAMES [MODEL_NAMES ...], --model_names MODEL_NAMES [MODEL_NAMES ...]
                        names of the models to use as labels
  -tn TOP_N_CLASSES [TOP_N_CLASSES ...], --top_n_classes TOP_N_CLASSES [TOP_N_CLASSES ...]
                        number of classes to plot
  -k TOP_K, --top_k TOP_K
                        number of elements in the ranklist to consider
  -ll LABELS_LIMIT, --labels_limit LABELS_LIMIT
                        maximum numbers of labels. If labels in dataset are
                        higher than this number, "rare" label
  -ss {ground_truth,predictions}, --subset {ground_truth,predictions}
                        type of subset filtering
  -n, --normalize       normalize rows in confusion matrix
  -m METRICS [METRICS ...], --metrics METRICS [METRICS ...]
                        metrics to dispay in threshold_vs_metric
  -pl POSITIVE_LABEL, --positive_label POSITIVE_LABEL
                        label of the positive class for the roc curve
  -l {critical,error,warning,info,debug,notset}, --logging_level {critical,error,warning,info,debug,notset}
                        the level of logging to use
```

Some additional information on the parameters:

* The list parameters are considered to be aligned, meaning `predictions`, `probabilities`, `training_statistics`, `test_statistics` and `model_names` are indexed altogether, for instance the name of the model producing the second predictions in the list will be the second in the model names.
* `data_csv` is intended to be the data the model\(s\) were trained on.
* `ground_truth` and `ground_truth_metadata` are respectively the `HDF5` and `JSON` file obtained during training preprocessing. If you plan to use the visualizations then be sure not to use the `skip_save_preprocessing` when training. Those files are needed because they contain the split performed at preprocessing time, so it is easy to extract the test set from them.
* `field` is the output feature to use for creating the visualization.

Other parameters will be detailed for each visualization as different ones use them differently.

Example commands to generate the visualizations are based on running two experiments and comparing them. The experiments themselves are run with the following:

```text
ludwig experiment --experiment_name titanic --model_name Model1 --data_csv train.csv -mdf titanic_model1.yaml
ludwig experiment --experiment_name titanic --model_name Model2 --data_csv train.csv -mdf titanic_model2.yaml
```

For this, you need to download the Kaggle Titanic dataset to get `train.csv`. Note that the images associated with each visualization below are not from the Titanic dataset. The two models are defined with `titanic_model1.yaml`

```yaml
input_features:
    -
        name: Pclass
        type: category
    -
        name: Sex
        type: category
    -
        name: Age
        type: numerical
        preprocessing:
          missing_value_strategy: fill_with_mean
    -
        name: SibSp
        type: numerical
    -
        name: Parch
        type: numerical
    -
        name: Fare
        type: numerical
        preprocessing:
          missing_value_strategy: fill_with_mean
    -
        name: Embarked
        type: category

output_features:
    -
        name: Survived
        type: binary
```

and with `titanic_model2.yaml`:

```yaml
input_features:
    -
        name: Pclass
        type: category
    -
        name: Sex
        type: category
    -
        name: SibSp
        type: numerical
    -
        name: Parch
        type: numerical
    -
        name: Embarked
        type: category

output_features:
    -
        name: Survived
        type: binary
```

### Learning Curves

This visualization uses the `training_statistics` and `model_names` parameters. For each model \(in the aligned lists of `training_statistics` and `model_names`\) and for each output feature and measure of the model, it produces a line plot showing how that measure changed over the course of the epochs of training on the training and validation sets.

Example command:

`ludwig visualize --visualization learning_curves --model_names Model1 Model2 --training_statistics results\titanic_Model1_0\training_statistics.json results\titanic_Model2_0\training_statistics.json`

![](../../../../../.gitbook/assets/image%20%28146%29.png)

### Confusion Matrix

This visualization uses the `top_n_classes`, `normalize`, `ground_truth_metadata`, `test_statistics` and `model_names` parameters. For each model \(in the aligned lists of `test_statistics` and `model_names`\) it produces a heatmap of the confusion matrix in the predictions for each field that has a confusion matrix in `test_statistics`. The value of `top_n_classes` limits the heatmap to the `n` most frequent classes.

Example command:

`ludwig visualize --visualization confusion_matrix --top_n_classes 2 --test_statistics results\titanic_Model1_0\test_statistics.json --ground_truth_metadata results\titanic_Model1_0\model\train_set_metadata.json`

![](../../../../../.gitbook/assets/image%20%28116%29.png)

The second plot produced, is a barplot showing the entropy of each class, ranked from most entropic to least entropic.

![](../../../../../.gitbook/assets/image%20%28334%29.png)

### Compare Performance

This visualization uses the `field`, `test_statistics` and `model_names` parameters. For each model \(in the aligned lists of `test_statistics` and `model_names`\) it produces bars in a bar plot, one for each overall metric available in the `test_statistics` file for the specified `field`.

Example command:

`ludwig visualize --visualization compare_performance --model_names Model1 Model2 --test_statistics results\titanic_Model1_0\test_statistics.json results\titanic_Model2_0\test_statistics.json --field Survived`

![](../../../../../.gitbook/assets/image%20%28147%29.png)

### Compare Classifier Predictions

This visualization uses the `ground_truth`, `field`, `predictions` and `model_names` parameters. `field` needs to be a category and there must be two and only two models \(in the aligned lists of `predictions` and `model_names`\). This visualization produces a pie chart comparing the predictions of the two models for the specified `field`.

Example command:

`ludwig visualize --visualization compare_classifiers_predictions --model_names Model1 Model2 --ground_truth train.hdf5 --field Survived --predictions results\titanic_Model1_0\Survived_predictions.csv results\titanic_Model2_0\Survived_predictions.csv`

![](../../../../../.gitbook/assets/image%20%28276%29.png)

### Confidence\_Thresholding

This visualization uses the `ground_truth`, `field`, `probabilities` and `model_names` parameters. `field` needs to be a category. For each model \(in the aligned lists of `probabilities` and `model_names`\) it produces a pair of lines indicating the accuracy of the model and the data coverage while increasing a threshold \(x axis\) on the probabilities of predictions for the specified `field`.

![](../../../../../.gitbook/assets/image%20%28214%29.png)

### Binary Threshold vs. Metric

This visualization uses the `positive_label`, `metrics`, `ground_truth`, `field`, `probabilities` and `model_names` parameters. `field` can be a category or binary feature. For each metric specified in `metrics` \(options are `f1`, `precision`, `recall`, `accuracy`\), this visualization produces a line chart plotting a threshold on the confidence of the model against the metric for the specified `field`. If `field` is a category feature, `positive_label` indicates which is the class to be considered positive class and all the others will be considered negative. It needs to be an integer, to figure out the association between classes and integers check the `ground_truth_metadata` JSON file.

![](../../../../../.gitbook/assets/image%20%28302%29.png)

### ROC Curves

**roc\_curves**

This visualization uses the `positive_label`, `ground_truth`, `field`, `probabilities` and `model_names` parameters. `field` can be a category or binary feature. This visualization produces a line chart plotting the roc curves for the specified `field`. If `field` is a category feature, `positive_label` indicates which is the class to be considered positive class and all the others will be considered negative. It needs to be an integer, to figure out the association between classes and integers check the `ground_truth_metadata` JSON file.

![](../../../../../.gitbook/assets/image%20%28159%29.png)

**roc\_curves\_from\_test\_statistics**

This visualization uses the `field`, `test_statistics` and `model_names` parameters. `field` needs to be binary feature. This visualization produces a line chart plotting the roc curves for the specified `field`.

![](../../../../../.gitbook/assets/image%20%28259%29.png)

### **Calibration Plot**

**calibration\_1\_vs\_all**

This visualization uses the `top_k`, `ground_truth`, `field`, `probabilities` and `model_names` parameters. `field` needs to be a category or binary. For each class or each of the `k` most frequent classes if `top_k` is specified, it produces two plots computed on the fly from the probabilities of predictions for the specified `field`.

The first plot is a calibration curve that shows the calibration of the predictions considering the current class to be the true one and all others to be a false one, drawing one line for each model \(in the aligned lists of `probabilities` and `model_names`\).

![](../../../../../.gitbook/assets/image%20%28247%29.png)

The second plot shows the distributions of the predictions considering the current class to be the true one and all others to be a false one, drawing the distribution for each model \(in the aligned lists of `probabilities` and `model_names`\).

![](../../../../../.gitbook/assets/image%20%28119%29.png)

**calibration\_multiclass**

This visualization uses the `ground_truth`, `field`, `probabilities` and `model_names` parameters. `field` needs to be a category. For each class, produces two plots computed on the fly from the probabilities of predictions for the specified `field`.

The first plot is a calibration curve that shows the calibration of the predictions considering al classes, drawing one line for each model \(in the aligned lists of `probabilities` and `model_names`\).

![](../../../../../.gitbook/assets/image%20%28203%29.png)

The second plot shows a bar plot of the brier score \(that calculates how calibrated are the probabilities of the predictions of a model\), drawing one bar for each model \(in the aligned lists of `probabilities` and `model_names`\).

![](../../../../../.gitbook/assets/image%20%2868%29.png)

### **Class Frequency vs. F1 score**

**frequency\_vs\_f1**

This visualization uses the `ground_truth_metadata`, `field`, `test_statistics` and `model_names` parameters. `field` needs to be a category. For each model \(in the aligned lists of `test_statistics` and `model_names`\), produces two plots statistics of predictions for the specified `field`.

The first plot is a line plot with one x axis representing the different classes and two vertical axes colored in orange and blue respectively. The orange one is the frequency of the class and an orange line is plotted to show the trend. The blue one is the F1 score for that class and a blue line is plotted to show the trend. The classes on the x axis are sorted by f1 score.

![](../../../../../.gitbook/assets/image%20%28248%29.png)

The second plot has the same structure of the first one, but the axes are flipped and the classes on the x axis are sorted by frequency.

![](../../../../../.gitbook/assets/image%20%2817%29.png)

