# Command Line Interface

Ludwig provides six command line interface entry points

* train
* predict
* test
* experiment
* visualize
* collect\_weights
* collect\_activations
* serve

They are described in detail below.

## train

This command lets you train a model from your data. You can call it with:

`ludwig train [options]`

or with

`python -m ludwig.train [options]`

from within Ludwig's main directory.

These are the available arguments:

```text
usage: ludwig train [options]

This script trains a model.

optional arguments:
  -h, --help            show this help message and exit
  --output_directory OUTPUT_DIRECTORY
                        directory that contains the results
  --experiment_name EXPERIMENT_NAME
                        experiment name
  --model_name MODEL_NAME
                        name for the model
  --data_csv DATA_CSV   input data CSV file. If it has a split column, it will
                        be used for splitting (0: train, 1: validation, 2:
                        test), otherwise the dataset will be randomly split
  --data_train_csv DATA_TRAIN_CSV
                        input train data CSV file
  --data_validation_csv DATA_VALIDATION_CSV
                        input validation data CSV file
  --data_test_csv DATA_TEST_CSV
                        input test data CSV file
  --data_hdf5 DATA_HDF5
                        input data HDF5 file. It is an intermediate preprocess
                        version of the input CSV created the first time a CSV
                        file is used in the same directory with the same name
                        and a hdf5 extension
  --data_train_hdf5 DATA_TRAIN_HDF5
                        input train data HDF5 file. It is an intermediate
                        preprocess version of the input CSV created the first
                        time a CSV file is used in the same directory with the
                        same name and a hdf5 extension
  --data_validation_hdf5 DATA_VALIDATION_HDF5
                        input validation data HDF5 file. It is an intermediate
                        preprocess version of the input CSV created the first
                        time a CSV file is used in the same directory with the
                        same name and a hdf5 extension
  --data_test_hdf5 DATA_TEST_HDF5
                        input test data HDF5 file. It is an intermediate
                        preprocess version of the input CSV created the first
                        time a CSV file is used in the same directory with the
                        same name and a hdf5 extension
  --train_set_metadata_json TRAIN_SET_METADATA_JSON
                        input metadata JSON file. It is an intermediate
                        preprocess file containing the mappings of the input
                        CSV created the first time a CSV file is used in the
                        same directory with the same name and a json extension
  -sspi, --skip_save_processed_input
                        skips saving intermediate HDF5 and JSON files
  -md MODEL_DEFINITION, --model_definition MODEL_DEFINITION
                        model definition
  -mdf MODEL_DEFINITION_FILE, --model_definition_file MODEL_DEFINITION_FILE
                        YAML file describing the model. Ignores
                        --model_hyperparameters
  -mlp MODEL_LOAD_PATH, --model_load_path MODEL_LOAD_PATH
                        path of a pretrained model to load as initialization
  -mrp MODEL_RESUME_PATH, --model_resume_path MODEL_RESUME_PATH
                        path of a the model directory to resume training of
  -ssm, --skip_save_model
                        disables saving weights each time the model imrpoves. By
                        default Ludwig saves weights after each epoch the
                        validation measure improves, but if the model is
                        really big that can be time consuming if you do not
                        want to keep the weights and just find out what
                        performance can a model get with a set of
                        hyperparameters, use this parameter to skip it.
  -ssp, --skip_save_progress
                        disables saving weights after each epoch. By default
                        ludwig saves weights after each epoch for enabling
                        resuming of training, but if the model is really big
                        that can be time consuming and will save twice as much
                        space, use this parameter to skip it.
  -ssl, --skip_save_log
                        disables saving TensorBoard logs. By default Ludwig
                        saves logs for the TensorBoard, but if it is not
                        needed turning it off can slightly increase the
                        overall speed.
  -rs RANDOM_SEED, --random_seed RANDOM_SEED
                        a random seed that is going to be used anywhere there
                        is a call to a random number generator: data
                        splitting, parameter initialization and training set
                        shuffling
  -g GPUS [GPUS ...], --gpus GPUS [GPUS ...]
                        list of gpus to use
  -gf GPU_FRACTION, --gpu_fraction GPU_FRACTION
                        fraction of gpu memory to initialize the process with
  -uh, --use_horovod    uses horovod for distributed training
  -dbg, --debug         enables debugging mode
  -l {critical,error,warning,info,debug,notset}, --logging_level {critical,error,warning,info,debug,notset}
                        the level of logging to use
```

When Ludwig trains a model it creates two intermediate files, one HDF5 and one JSON. The HDF5 file contains the data mapped to numpy ndarrays, while the JSON file contains the mappings from the values in the tensors to their original labels.

For instance, for a categorical feature with 3 possible values, the HDF5 file will contain integers from 0 to 3 \(with 0 being a `<UNK>` category\), while the JSON file will contain a `idx2str` list containing all tokens \(`[<UNK>, label_1, label_2, label_3]`\), a `str2idx` dictionary \(`{"<UNK>": 0, "label_1": 1, "label_2": 2, "label_3": 3}`\) and a `str2freq` dictionary \(`{"<UNK>": 0, "label_1": 93, "label_2": 55, "label_3": 24}`\).

The reason to have those intermediate files is two-fold: on one hand, if you are going to train your model again Ludwig will try to load them instead of recomputing all tensors, which saves a consistent amount of time, and on the other hand when you want to use your model to predict, data has to be mapped to tensors in exactly the same way it was mapped during training, so you'll be required to load the JSON metadata file in the `predict` command. The way this works is: the first time you provide a UTF-8 encoded CSV \(`--data_csv`\), the HDF5 and JSON files are created, from the second time on Ludwig will load them instead of the CSV even if you specify the CSV \(it looks in the same directory for files names in the same way but with a different extension\), finally you can directly specify the HDF5 and JSON files \(`--data_hdf5` and `--metadata_json`\).

As the mapping from raw data to tensors depends on the type of feature that you specify in your model definition, if you change type \(for instance from `sequential` to `text`\) you also have to redo the preprocessing, which is achieved by deleting the HDF5 and JSON files. Alternatively you can skip saving the HDF5 and JSON files specifying `--skip_save_processed_input`.

Splitting between train, validation and test set can be done in several ways. This allows for a few possible input data scenarios:

* one single UTF-8 encoded CSV file is provided \(`-data_csv`\). In this case if the CSV contains a `split` column with values `0` for training, `1` for validation and `2` for test, this split will be used. If you want to ignore the split column and perform a random split, use a `force_split` argument in the model definition. In the case when there is no split column, a random `70-20-10` split will be performed. You can set the percentages and specify if you want stratified sampling in the model definition preprocessing section.
* you can provide separate UTF-8 encoded train, validation and test CSVs \(`--data_train_csv`, `--data_validation_csv`, `--data_test_csv`\).
* the HDF5 and JSON file indications specified in the case of a single CSV file apply also in the multiple files case \(`--data_train_hdf5`, `--data_validation_hdf5`, `--data_test_hdf5`\), with the only difference that you need to specify only one JSON file \(`--metadata_json`\) instead of three. The validation set is optional, but if absent the training wil continue until the end of the training epochs, while when there's a validation set the default behavior is to perform early stopping after the validation measure does not improve for a a certain amount of epochs. The test set is optional too.

Other optional arguments are `--output_directory`, `--experiment_name` and `--model name`. By default the output directory is `./results`. That directory will contain a directory named `[experiment_name]_[model_name]_0` if model name and experiment name are specified. If the same combination of experiment and model name is used again, the integer at the end of the name wil be increased. If neither of them is specified the directory will be named `run_0`. The directory will contain

* `description.json` - a file containing a description of the training process with all the information to reproduce it.
* `training_statistics.json` which contains records of all measures and losses for each epoch.
* `model` - a directory containing model hyperparameters, weights, checkpoints and logs \(for TensorBoard\).

The model definition can be provided either as a string \(`--model_definition`\) or as YAML file \(`--model_definition_file`\). Details on how to write your model definition are provided in the **Model Definition** section.

During training Ludwig saves two sets of weights for the model, one that is the weights at the end of the epoch where the best performance on the validation measure was achieved and one that is the weights at the end of the latest epoch. The reason for keeping the second set is to be able to resume training in case the training process gets interrupted somehow.

To resume training using the latest weights and the whole history of progress so far you have to specify the `--model_resume_path` argument. You can avoid saving the latest weights and the overall progress so far by using the argument `--skip_save_progress`, but you will not be able to resume it afterwards. Another available option is to load a previously trained model as an initialization for a new training process. In this case Ludwig will start a new training process, without knowing any progress of the previous model, no training statistics, nor the number of epochs the model has been trained on so far. It's not resuming training, just initializing training with a previously trained model with the same model definition, and it is accomplished through the `--model_load_path` argument.

You can specify a random seed to be used by the python environment, python random package, numpy and TensorFlow with the `--random_seed` argument. This is useful for reproducibility. Be aware that due to asynchronicity in the TensorFlow GPU execution, when training on GPU results may not be reproducible.

You can manage which GPUs on your machine are used with the `--gpus` argument, which accepts a string identical to the format of `CUDA_VISIBLE_DEVICES` environment variable, namely a list of integers separated by comma. You can also specify the fraction of the GPU memory that will be initially assigned to TensorFlow with `--gpu_fraction`. By default it is 1.0, but you can set it, for instance, to 0.2 to use only 1/5 of the available memory. If TensorFlow will need more GPU memory it will try to increase this amount.

Finally the `--logging_level` argument lets you set the amount of logging that you want to see during training and the `--debug` argument turns on TensorFlow's `tfdbg`. Be careful when doing so, as it will help in catching errors, in particular infs and NaNs but it will consume much more memory.

Example:

`ludwig train --data_csv reuters-allcats.csv --model_definition "{input_features: [{name: text, type: text, encoder: parallel_cnn, level: word}], output_features: [{name: class, type: category}]}"`

## predict

This command lets you use a previously trained model to predict on new data. You can call it with:

`ludwig predict [options]`

or with

`python -m ludwig.predict [options]`

from within Ludwig's main directory.

These are the available arguments:

```text
usage: ludwig predict [options]

This script loads a pretrained model and uses it to predict.

optional arguments:
  -h, --help            show this help message and exit
  --data_csv DATA_CSV   input data CSV file. If it has a split column, it will
                        be used for splitting (0: train, 1: validation, 2:
                        test), otherwise the dataset will be randomly split
  --data_hdf5 DATA_HDF5
                        input data HDF5 file. It is an intermediate preprocess
                        version of the input CSV created the first time a CSV
                        file is used in the same directory with the same name
                        and a hdf5 extension
  --train_set_metadata_json TRAIN_SET_METADATA_JSON
                        input metadata JSON file. It is an intermediate
                        preprocess file containing the mappings of the input
                        CSV created the first time a CSV file is used in the
                        same directory with the same name and a json extension
  -s {training,validation,test,full}, --split {training,validation,test,full}
                        the split to test the model on
  -m MODEL_PATH, --model_path MODEL_PATH
                        model to load
  -od OUTPUT_DIRECTORY, --output_directory OUTPUT_DIRECTORY
                        directory that contains the results
  -ssuo, --skip_save_unprocessed_output
                        skips saving intermediate NPY output files
  -bs BATCH_SIZE, --batch_size BATCH_SIZE
                        size of batches
  -g GPUS, --gpus GPUS  list of gpu to use
  -gf GPU_FRACTION, --gpu_fraction GPU_FRACTION
                        fraction of gpu memory to initialize the process with
  -uh, --use_horovod    uses horovod for distributed training
  -dbg, --debug         enables debugging mode
  -l {critical,error,warning,info,debug,notset}, --logging_level {critical,error,warning,info,debug,notset}
                        the level of logging to use
```

The same distinction between UTF-8 encoded CSV files and HDF5 / JSON files explained in the **train** section also applies here. In either case, the JSON metadata file obtained during training is needed in order to map the new data into tensors. If the new data contains a split column, you can specify which split to use to calculate the predictions with the `--split` argument. By default it's `full` which means all the splits will be used.

A model to load is needed, and you can specify its path with the `--model_path` argument. If you trained a model previously and got the results in, for instance, `./results/experiment_run_0`, you have to specify `./results/experiment_run_0/model` for using it to predict.

You can specify an output directory with the argument `--output-directory`, by default it will be `./result_0`, with increasing numbers if a directory with the same name is present.

The directory will contain a prediction CSV file and a probability CSV file for each output feature, together with raw NPY files containing raw tensors. You can specify not to save the raw NPY output files with the argument `skip_save_unprocessed_output`. If the argument `--evaluate_performance` if provided, a `predict_statistics.json` file containing all prediction statistics will also be outputted. If this parameter is specified, the data must contain columns for each output feature with ground truth output values in order to compute the performance statistics. If you receive an error regarding a missing output feature column in your data, it means that the data does not contain the columns for each output feature to use as ground truth.

A specific batch size for speeding up the prediction can be specified using the argument `--batch_size`.

Finally the `--logging_level`, `--debug` and `--gpus` related arguments behave exactly like described in the train command section.

Example:

`ludwig predict --data_csv reuters-allcats.csv --model_path results/experiment_run_0/model/`

## test

This command lets you use a previously trained model to predict on new data and evaluate the performance of the prediction compared to ground truth. You can call it with:

`ludwig test [options]`

or with

`python -m ludwig.test_performance [options]`

from within Ludwig's main directory.

These are the available arguments:

```text
usage: ludwig test [options]

This script loads a pretrained model and uses it to predict.

optional arguments:
  -h, --help            show this help message and exit
  --data_csv DATA_CSV   input data CSV file. If it has a split column, it will
                        be used for splitting (0: train, 1: validation, 2:
                        test), otherwise the dataset will be randomly split
  --data_hdf5 DATA_HDF5
                        input data HDF5 file. It is an intermediate preprocess
                        version of the input CSV created the first time a CSV
                        file is used in the same directory with the same name
                        and a hdf5 extension
  --train_set_metadata_json TRAIN_SET_METADATA_JSON
                        input metadata JSON file. It is an intermediate
                        preprocess file containing the mappings of the input
                        CSV created the first time a CSV file is used in the
                        same directory with the same name and a json extension
  -s {training,validation,test,full}, --split {training,validation,test,full}
                        the split to test the model on
  -m MODEL_PATH, --model_path MODEL_PATH
                        model to load
  -od OUTPUT_DIRECTORY, --output_directory OUTPUT_DIRECTORY
                        directory that contains the results
  -ssuo, --skip_save_unprocessed_output
                        skips saving intermediate NPY output files
  -bs BATCH_SIZE, --batch_size BATCH_SIZE
                        size of batches
  -g GPUS, --gpus GPUS  list of gpu to use
  -gf GPU_FRACTION, --gpu_fraction GPU_FRACTION
                        fraction of gpu memory to initialize the process with
  -uh, --use_horovod    uses horovod for distributed training
  -dbg, --debug         enables debugging mode
  -l {critical,error,warning,info,debug,notset}, --logging_level {critical,error,warning,info,debug,notset}
                        the level of logging to use
```

All parameters are the same of **predict** and the behavior is the same. The only difference isthat `test` requires the dataset to contain also columns with the same name of output features. This is needed because `test` compares the predictions produced by the model with the ground truth and will save all those statistics in a `test_statistics.json` file in the result directory.

Example:

`ludwig test --data_csv reuters-allcats.csv --model_path results/experiment_run_0/model/`

## experiment

This command combines training and test into a single handy command. You can call it with:

`ludwig experiment [options]`

or with

`python -m ludwig.experiment [options]`

from within Ludwig's main directory.

These are the available arguments:

```text
usage: ludwig experiment [options]

This script trains and tests a model.

optional arguments:
  -h, --help            show this help message and exit
  --output_directory OUTPUT_DIRECTORY
                        directory that contains the results
  --experiment_name EXPERIMENT_NAME
                        experiment name
  --model_name MODEL_NAME
                        name for the model
  --data_csv DATA_CSV   input data CSV file. If it has a split column, it will
                        be used for splitting (0: train, 1: validation, 2:
                        test), otherwise the dataset will be randomly split
  --data_train_csv DATA_TRAIN_CSV
                        input train data CSV file
  --data_validation_csv DATA_VALIDATION_CSV
                        input validation data CSV file
  --data_test_csv DATA_TEST_CSV
                        input test data CSV file
  --data_hdf5 DATA_HDF5
                        input data HDF5 file. It is an intermediate preprocess
                        version of the input CSV created the first time a CSV
                        file is used in the same directory with the same name
                        and a hdf5 extension
  --data_train_hdf5 DATA_TRAIN_HDF5
                        input train data HDF5 file. It is an intermediate
                        preprocess version of the input CSV created the first
                        time a CSV file is used in the same directory with the
                        same name and a hdf5 extension
  --data_validation_hdf5 DATA_VALIDATION_HDF5
                        input validation data HDF5 file. It is an intermediate
                        preprocess version of the input CSV created the first
                        time a CSV file is used in the same directory with the
                        same name and a hdf5 extension
  --data_test_hdf5 DATA_TEST_HDF5
                        input test data HDF5 file. It is an intermediate
                        preprocess version of the input CSV created the first
                        time a CSV file is used in the same directory with the
                        same name and a hdf5 extension
  --train_set_metadata_json TRAIN_SET_METADATA_JSON
                        input train set metadata JSON file. It is an intermediate
                        preprocess file containing the mappings of the input
                        CSV created the first time a CSV file is used in the
                        same directory with the same name and a json extension
  -sspi, --skip_save_processed_input
                        skips saving intermediate HDF5 and JSON files
  -ssuo, --skip_save_unprocessed_output
                        skips saving intermediate NPY output files
  -md MODEL_DEFINITION, --model_definition MODEL_DEFINITION
                        model definition
  -mdf MODEL_DEFINITION_FILE, --model_definition_file MODEL_DEFINITION_FILE
                        YAML file describing the model. Ignores
                        --model_hyperparameters
  -mlp MODEL_LOAD_PATH, --model_load_path MODEL_LOAD_PATH
                        path of a pretrained model to load as initialization
  -mrp MODEL_RESUME_PATH, --model_resume_path MODEL_RESUME_PATH
                        path of a the model directory to resume training of
  -ssp SKIP_SAVE_PROGRESS_WEIGHTS, --skip_save_progress SKIP_SAVE_PROGRESS_WEIGHTS
                        disables saving weights after each epoch. By default
                        Ludwig saves weights after each epoch for enabling
                        resuming of training, but if the model is really big
                        that can be time consuming and will use twice as much
                        storage space, use this parameter to skip it.
  -rs RANDOM_SEED, --random_seed RANDOM_SEED
                        a random seed that is going to be used anywhere there
                        is a call to a random number generator: data
                        splitting, parameter initialization and training set
                        shuffling
  -g GPUS [GPUS ...], --gpus GPUS [GPUS ...]
                        list of gpus to use
  -gf GPU_FRACTION, --gpu_fraction GPU_FRACTION
                        fraction of gpu memory to initialize the process with
  -dbg, --debug         enables debugging mode
  -l {critical,error,warning,info,debug,notset}, --logging_level {critical,error,warning,info,debug,notset}
                        the level of logging to use
```

The parameters combine parameters from both **train** and **test** so refer to those sections for an in depth explanation. The output directory will contain the outputs both commands produce.

Example:

`ludwig experiment --data_csv reuters-allcats.csv --model_definition "{input_features: [{name: text, type: text, encoder: parallel_cnn, level: word}], output_features: [{name: class, type: category}]}"`

## visualize

This command lets you visualize training and prediction statistics, alongside with comparing different models performances and predictions. You can call it with:

`ludwig visualize [options]`

or with

`python -m ludwig.visualize [options]`

from within Ludwig's main directory.

These are the available arguments:

```text
usage: ludwig visualize [options]

This script analyzes results and shows some nice plots.

optional arguments:
  -h, --help            show this help message and exit
  -d DATA_CSV, --data_csv DATA_CSV
                        raw data file
  -g GROUND_TRUTH, --ground_truth GROUND_TRUTH
                        ground truth file
  -gts GROUND_TRUTH_SPLIT, --ground_truth_split GROUND_TRUTH_SPLIT
                       ground truth split - 0:train, 1:validation, 2:test split
  -gm GROUND_TRUTH_METADATA, --ground_truth_metadata GROUND_TRUTH_METADATA
                        input metadata JSON file
  -v {compare_performance,compare_classifiers_performance_from_prob,compare_classifiers_performance_from_pred,compare_classifiers_performance_changing_k,compare_classifiers_performance_subset,compare_classifiers_predictions,compare_classifiers_predictions_distribution,confidence_thresholding,confidence_thresholding_2thresholds_3d,confidence_thresholding_data_vs_acc,confidence_thresholding_2thresholds_2d,confidence_thresholding_data_vs_acc_subset,confidence_thresholding_data_vs_acc_subset_per_class,binary_threshold_vs_metric,roc_curves,roc_curves_from_test_statistics,data_vs_acc_subset,data_vs_acc_subset_per_class,calibration_1_vs_all,calibration_multiclass,confusion_matrix,compare_classifiers_multiclass_multimetric,frequency_vs_f1,learning_curves}, --visualization {compare_performance,compare_classifiers_performance_from_prob,compare_classifiers_performance_from_pred,compare_classifiers_performance_changing_k,compare_classifiers_performance_subset,compare_classifiers_predictions,compare_classifiers_predictions_distribution,confidence_thresholding,confidence_thresholding_2thresholds_3d,confidence_thresholding_data_vs_acc,confidence_thresholding_2thresholds_2d,confidence_thresholding_data_vs_acc_subset,confidence_thresholding_data_vs_acc_subset_per_class,binary_threshold_vs_metric,roc_curves,roc_curves_from_test_statistics,data_vs_acc_subset,data_vs_acc_subset_per_class,calibration_1_vs_all,calibration_multiclass,confusion_matrix,compare_classifiers_multiclass_multimetric,frequency_vs_f1,learning_curves}
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
  -alg ALGORITHMS [ALGORITHMS ...], --algorithms ALGORITHMS [ALGORITHMS ...]
                        names of the algorithms (for better graphs)
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

As the `--visualization` parameters suggests, there is a vast number of visualizations readily available. Each of them requires a different subset of this command's arguments, so they will be described one by one in the **Visualizations** page.

## collect\_weights

This command lets you load a pre-trained model and collect the tensors with a specific name in order to save them in a NPY format. This may be useful in order to visualize the learned weights \(for instance collecting embedding matrices\) and for some post-hoc analyses. You can call it with:

`ludwig collect_weights [options]`

or with

`python -m ludwig.collect weights [options]`

from within Ludwig's main directory.

These are the available arguments:

```text
usage: ludwig collect_weights [options]

This script loads a pretrained model and uses it collect weights.

optional arguments:
  -h, --help            show this help message and exit
  -m MODEL_PATH, --model_path MODEL_PATH
                        model to load
  -t TENSORS [TENSORS ...], --tensors TENSORS [TENSORS ...]
                        tensors to collect
  -od OUTPUT_DIRECTORY, --output_directory OUTPUT_DIRECTORY
                        directory that contains the results
  -dbg, --debug         enables debugging mode
  -l {critical,error,warning,info,debug,notset}, --logging_level {critical,error,warning,info,debug,notset}
                        the level of logging to use
```

The three most important arguments are `--model_path` where you have to specify the path of the model to load, `--tensors` that lets you specify a list of tensor names in the TensorFlow graph that contain the weights you want to collect, and finally `--output_directory` that lets you specify where the NPY files \(one for each tensor name specified\) will be saved.

In order to figure out the names fo the tensors containing the weights you want to collect, the best way is to inspect the graph of the model with TensorBoard.

`tensorboard --logdir /path/to/model/log`

## collect\_activations

This command lets you load a pre-trained model and input data and collects the values of activations contained in tensors with a specific name in order to save them in a NPY format. This may be useful in order to visualize the activations \(for instance collecting last layer's activations as embeddings representations of the input datapoint\) and for some post-hoc analyses. 

You can call it with:

`ludwig collect_activations [options]`

or with

`python -m ludwig.collect activations [options]`

from within Ludwig's main directory.

These are the available arguments:

```text
usage: ludwig collect_activations [options]

This script loads a pretrained model and uses it collect tensors for each
datapoint in the dataset.

optional arguments:
  -h, --help            show this help message and exit
  --data_csv DATA_CSV   input data CSV file
  --data_hdf5 DATA_HDF5
                        input data HDF5 file
  -s {training,validation,test,full}, --split {training,validation,test,full}
                        the split to test the model on
  -m MODEL_PATH, --model_path MODEL_PATH
                        model to load
  -t TENSORS [TENSORS ..], --tensors TENSORS [TENSORS ..]
                        tensors to collect
  -od OUTPUT_DIRECTORY, --output_directory OUTPUT_DIRECTORY
                        directory that contains the results
  -bs BATCH_SIZE, --batch_size BATCH_SIZE
                        size of batches
  -g GPUS, --gpus GPUS  list of gpu to use
  -gf GPU_FRACTION, --gpu_fraction GPU_FRACTION
                        fraction of gpu memory to initialize the process with
  -dbg, --debug         enables debugging mode
  -l {critical,error,warning,info,debug,notset}, --logging_level {critical,error,warning,info,debug,notset}
                        the level of logging to use
```

The data related and runtime related arguments \(GPUs, batch size, etc.\) are the same used in **predict**, you can refer to that section for an explanation. The collect specific arguments `--model_path`, `--tensors` and `--output_directory` are the same used in **collect\_weights**, you can refer to that section for an explanation.

In order to figure out the names fo the tensors containing the activations you want to collect, the best way is to inspect the graph of the model with TensorBoard.

`tensorboard --logdir /path/to/model/log`

## serve

This command lets you load a pre-trained model and serve it on an http server.

You can call it with:

`ludwig serve [options]`

or with

`python -m ludwig.serve [options]`

from within Ludwig's main directory.

These are the available arguments:

```text
usage: ludwig serve [options]

This script serves a pretrained model

optional arguments:
  -h, --help            show this help message and exit
  -m MODEL_PATH, --model_path MODEL_PATH
                        model to load
  -l {critical,error,warning,info,debug,notset}, --logging_level {critical,error,warning,info,debug,notset}
                        the level of logging to use
  -p PORT, --port PORT  port for server (default: 8000)
  -H HOST, --host HOST  host for server (default: 0.0.0.0)
```

The most important argument is `--model_path` where you have to specify the path of the model to load.

Once running, you can make a POST request on the `/predict` endpoint to run inference on the form data submitted.

**EXAMPLE CURL**

**File**

`curl http://0.0.0.0:8000/predict -X POST -F 'image_path=@path_to_image/example.png'`

**Text**

`curl http://0.0.0.0:8000/predict -X POST -F 'english_text=words to be translated'`

**Both Text and File**

`curl`[`http://0.0.0.0:8000/predict`](http://0.0.0.0:8000/predict) `-X POST -F 'text=mixed together with' -F 'image=@path_to_image/example.png'`



