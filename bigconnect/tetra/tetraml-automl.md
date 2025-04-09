---
description: Automatic Machine Learning
---

# AutoML

In recent years, the demand for machine learning experts has outpaced the supply, despite the surge of people entering the field. To address this gap, there have been big strides in the development of user-friendly machine learning software that can be used by non-experts. The first steps toward simplifying machine learning involved developing simple, unified interfaces to a variety of machine learning algorithms (eg. TetraML).

Although TetraML has made it easy for non-experts to experiment with machine learning, there is still a fair bit of knowledge and background in data science that is required to produce high-performing machine learning models. Deep Neural Networks in particular are notoriously difficult for a non-expert to tune properly. In order for machine learning software to truly be accessible to non-experts, we have designed an easy-to-use interface which automates the process of training a large selection of candidate models. TetraML’s AutoML can also be a helpful tool for the advanced user, by providing a simple wrapper function that performs a large number of modeling-related tasks that would typically require many lines of code, and by freeing up their time to focus on other aspects of the data science pipeline tasks such as data-preprocessing, feature engineering and model deployment.

TetraML’s AutoML can be used for automating the machine learning workflow, which includes automatic training and tuning of many models within a user-specified time-limit. Stacked Ensembles – one based on all previously trained models, another one on the best model of each family – will be automatically trained on collections of individual models to produce highly predictive ensemble models which, in most cases, will be the top performing models in the AutoML Leaderboard.

TetraML offers a number of model explainability methods that apply to AutoML objects (groups of models), as well as individual models (e.g. leader model). Explanations can be generated automatically with a single function call, providing a simple interface to exploring and explaining the AutoML models.

### AutoML Interface

The AutoML interface is designed to have as few parameters as possible so that all the user needs to do is point to their dataset, identify the response column and optionally specify a time constraint or limit on the number of total models trained.

In both the R and Python API, AutoML uses the same data-related arguments, `x`, `y`, `training_frame`, `validation_frame`, as the other algorithms. Most of the time, all you’ll need to do is specify the data arguments. You can then configure values for `max_runtime_secs` and/or `max_models` to set explicit time or number-of-model limits on your run.

#### Required Parameters

**Required Data Parameters**

* `y`: This argument is the name (or index) of the response column.
* `training_frame`: Specifies the training set.

**Required Stopping Parameters**

One of the following stopping strategies (time or number-of-model based) must be specified. When both options are set, then the AutoML run will stop as soon as it hits one of either of these limits.

* `max_runtime_secs`: This argument specifies the maximum time that the AutoML process will run for, prior to training the final Stacked Ensemble models. The default is 0 (no limit), but dynamically sets to 1 hour if none of `max_runtime_secs` and `max_models` are specified by the user.
* `max_models`: Specify the maximum number of models to build in an AutoML run, excluding the Stacked Ensemble models. Defaults to `NULL/None`.

#### Optional Parameters

**Optional Data Parameters**

* `x`: A list/vector of predictor column names or indexes. This argument only needs to be specified if the user wants to exclude columns from the set of predictors. If all columns (other than the response) should be used in prediction, then this does not need to be set.
* `validation_frame`: This argument is ignored unless `nfolds == 0`, in which a validation frame can be specified and used for early stopping of individual models and early stopping of the grid searches (unless `max_models` or `max_runtime_secs` overrides metric-based early stopping). By default and when `nfolds > 1`, cross-validation metrics will be used for early stopping and thus `validation_frame` will be ignored.
* `leaderboard_frame`: This argument allows the user to specify a particular data frame to use to score and rank models on the leaderboard. This frame will not be used for anything besides leaderboard scoring. If a leaderboard frame is not specified by the user, then the leaderboard will use cross-validation metrics instead, or if cross-validation is turned off by setting `nfolds = 0`, then a leaderboard frame will be generated automatically from the training frame.
* `blending_frame`: Specifies a frame to be used for computing the predictions that serve as the training frame for the Stacked Ensemble models metalearner. If provided, all Stacked Ensembles produced by AutoML will be trained using Blending (a.k.a. Holdout Stacking) instead of the default Stacking method based on cross-validation.
* `fold_column`: Specifies a column with cross-validation fold index assignment per observation. This is used to override the default, randomized, 5-fold cross-validation scheme for individual models in the AutoML run.
* `weights_column`: Specifies a column with observation weights. Giving some observation a weight of zero is equivalent to excluding it from the dataset; giving an observation a relative weight of 2 is equivalent to repeating that row twice. Negative weights are not allowed.

**Optional Miscellaneous Parameters**

* `nfolds`: Specify a value >= 2 for the number of folds for k-fold cross-validation of the models in the AutoML run. This value defaults to 5. Use 0 to disable cross-validation; this will also disable Stacked Ensembles (thus decreasing the overall best model performance).
* `balance_classes`: Specify whether to oversample the minority classes to balance the class distribution. This option is not enabled by default and can increase the data frame size. This option is only applicable for classification. If the oversampled size of the dataset exceeds the maximum size calculated using the `max_after_balance_size parameter`, then the majority classes will be undersampled to satisfy the size limit.
* `class_sampling_factors`: Specify the per-class (in lexicographical order) over/under-sampling ratios. By default, these ratios are automatically computed during training to obtain the class balance. Note that this requires `balance_classes=true`.
* `max_after_balance_size`: Specify the maximum relative size of the training data after balancing class counts (**balance\_classes** must be enabled). Defaults to 5.0. (The value can be less than 1.0).
* `max_runtime_secs_per_model`: Specify the max amount of time dedicated to the training of each individual model in the AutoML run. Defaults to 0 (disabled). Note that setting this parameter can affect AutoML reproducibility.
* `stopping_metric`: Specify the metric to use for early stopping. Defaults to `AUTO`. The available options are:
  * AUTO: This defaults to logloss for classification and deviance for regression.
  * deviance (mean residual deviance)
  * logloss
  * MSE
  * RMSE
  * MAE
  * RMSLE
  * AUC (area under the ROC curve)
  * AUCPR (area under the Precision-Recall curve)
  * lift\_top\_group
  * misclassification
  * mean\_per\_class\_error
* `stopping_tolerance`: This option specifies the relative tolerance for the metric-based stopping criterion to stop a grid search and the training of individual models within the AutoML run. This value defaults to 0.001 if the dataset is at least 1 million rows; otherwise it defaults to a bigger value determined by the size of the dataset and the non-NA-rate. In that case, the value is computed as 1/sqrt(nrows \* non-NA-rate).
* `stopping_rounds`: This argument is used to stop model training when the stopping metric (e.g. AUC) doesn’t improve for this specified number of training rounds, based on a simple moving average. In the context of AutoML, this controls early stopping both within the random grid searches as well as the individual models. Defaults to 3 and must be an non-negative integer. To disable early stopping altogether, set this to 0.
* `sort_metric`: Specifies the metric used to sort the Leaderboard by at the end of an AutoML run. Available options include:
  * AUTO: This defaults to AUC for binary classification, mean\_per\_class\_error for multinomial classification, and deviance for regression.
  * deviance (mean residual deviance)
  * logloss
  * MSE
  * RMSE
  * MAE
  * RMSLE
  * AUC (area under the ROC curve)
  * AUCPR (area under the Precision-Recall curve)
  * mean\_per\_class\_error
* seed: Integer. Set a seed for reproducibility. AutoML can only guarantee reproducibility under certain conditions. Deep Learning models are not reproducible by default for performance reasons, so if the user requires reproducibility, then `exclude_algos` must contain `"DeepLearning"`. In addition `max_models` must be used because `max_runtime_secs` is resource limited, meaning that if the available compute resources are not the same between runs, AutoML may be able to train more models on one run vs another. Defaults to `NULL/None`.
* `project_name`: Character string to identify an AutoML project. Defaults to `NULL/None`, which means a project name will be auto-generated based on the training frame ID. More models can be trained and added to an existing AutoML project by specifying the same project name in multiple calls to the AutoML function (as long as the same training frame is used in subsequent runs).
* exclude\_algos: A list/vector of character strings naming the algorithms to skip during the model-building phase. An example use is `exclude_algos = ["GLM", "DeepLearning", "DRF"]` in Python or `exclude_algos = c("GLM", "DeepLearning", "DRF")` in R. Defaults to `None/NULL`, which means that all appropriate algorithms will be used if the search stopping criteria allows and if the `include_algos` option is not specified. This option is mutually exclusive with `include_algos`. See `include_algos` below for the list of available options.
* `include_algos`: A list/vector of character strings naming the algorithms to include during the model-building phase. An example use is `include_algos = ["GLM", "DeepLearning", "DRF"]` in Python or `include_algos = c("GLM", "DeepLearning", "DRF")` in R. Defaults to `None/NULL`, which means that all appropriate algorithms will be used if the search stopping criteria allows and if no algorithms are specified in `exclude_algos`. This option is mutually exclusive with `exclude_algos`. The available algorithms are:
  * DRF (This includes both the Random Forest and Extremely Randomized Trees (XRT) models. Refer to the Extremely Randomized Trees section in the DRF chapter and the histogram\_type parameter description for more information.)
  * GLM
  * XGBoost (XGBoost GBM)
  * GBM
  * DeepLearning (Fully-connected multi-layer artificial neural network)
  * StackedEnsemble
* `modeling_plan`: The list of modeling steps to be used by the AutoML engine. (They may not all get executed, depending on other constraints.)
* `preprocessing`: The list of preprocessing steps to run. Only `["target_encoding"]` is currently supported. **Experimental**.
* `exploitation_ratio`: Specify the budget ratio (between 0 and 1) dedicated to the exploitation (vs exploration) phase. By default, the exploitation phase is disabled (exploitation\_ratio=0) as this is still experimental; to activate it, it is recommended to try a ratio around 0.1. Note that the current exploitation phase only tries to fine-tune the best XGBoost and the best GBM found during exploration. **Experimental**.
* `monotone_constraints`: A mapping that represents monotonic constraints. Use +1 to enforce an increasing constraint and -1 to specify a decreasing constraint.
* `keep_cross_validation_predictions`: Specify whether to keep the predictions of the cross-validation predictions. This needs to be set to TRUE if running the same AutoML object for repeated runs because CV predictions are required to build additional Stacked Ensemble models in AutoML. This option defaults to FALSE.
* `keep_cross_validation_models`: Specify whether to keep the cross-validated models. Keeping cross-validation models may consume significantly more memory in the TetraML cluster. This option defaults to FALSE.
* `keep_cross_validation_fold_assignment`: Enable this option to preserve the cross-validation fold assignment. Defaults to FALSE.
* `verbosity`: (Optional: Python and R only) The verbosity of the backend messages printed during training. Must be one of `"debug", "info", "warn"`. Defaults to `NULL/None` (client logging disabled).
* `export_checkpoints_dir`: Specify a directory to which generated models will automatically be exported.

### Leaderboard

The AutoML object includes a “leaderboard” of models that were trained in the process, including the 5-fold cross-validated model performance (by default). The number of folds used in the model evaluation process can be adjusted using the `nfolds` parameter. If you would like to score the models on a specific dataset, you can specify the `leaderboard_frame` argument in the AutoML run, and then the leaderboard will show scores on that dataset instead.

The models are ranked by a default metric based on the problem type (the second column of the leaderboard). In binary classification problems, that metric is AUC, and in multiclass classification problems, the metric is mean per-class error. In regression problems, the default sort metric is deviance. Some additional metrics are also provided, for convenience.

Here is an example of a basic leaderboard (no extra columns) for a binary classification task:

| model\_id                                                   | auc      | logloss  | aucpr    | mean\_per\_class\_error | rmse     | mse      | training\_time\_ms | predict\_time\_per\_row\_ms |
| ----------------------------------------------------------- | -------- | -------- | -------- | ----------------------- | -------- | -------- | ------------------ | --------------------------- |
| StackedEnsemble\_AllModels\_AutoML\_20191213\_174603        | 0.789844 | 0.551067 | 0.804672 | 0.314665                | 0.432045 | 0.186663 | 924                | 0.05695                     |
| StackedEnsemble\_BestOfFamily\_AutoML\_20191213\_174603     | 0.789768 | 0.550906 | 0.805696 | 0.313059                | 0.431977 | 0.186604 | 639                | 0.024567                    |
| XGBoost\_grid\_\_1\_AutoML\_20191213\_174603\_model\_4      | 0.784698 | 0.55681  | 0.80312  | 0.323143                | 0.434743 | 0.189002 | 3092               | 0.002083                    |
| XGBoost\_3\_AutoML\_20191213\_174603                        | 0.784232 | 0.557749 | 0.802341 | 0.317933                | 0.434976 | 0.189204 | 2878               | 0.002173                    |
| XGBoost\_2\_AutoML\_20191213\_174603                        | 0.783533 | 0.555997 | 0.803189 | 0.32475                 | 0.434678 | 0.188945 | 4635               | 0.003292                    |
| XGBoost\_grid\_\_1\_AutoML\_20191213\_174603\_model\_3      | 0.782582 | 0.560218 | 0.800749 | 0.34334                 | 0.435944 | 0.190047 | 2695               | 0.002269                    |
| GBM\_5\_AutoML\_20191213\_174603                            | 0.78219  | 0.558353 | 0.800234 | 0.319658                | 0.435512 | 0.18967  | 768                | 0.004318                    |
| XGBoost\_1\_AutoML\_20191213\_174603                        | 0.781901 | 0.557944 | 0.801237 | 0.325446                | 0.435519 | 0.189676 | 4428               | 0.003039                    |
| XGBoost\_grid\_\_1\_AutoML\_20191213\_174603\_model\_1      | 0.781648 | 0.561112 | 0.799203 | 0.312015                | 0.436434 | 0.190474 | 5430               | 0.002557                    |
| GBM\_2\_AutoML\_20191213\_174603                            | 0.777673 | 0.562514 | 0.796181 | 0.334056                | 0.437583 | 0.191479 | 655                | 0.003772                    |
| GBM\_1\_AutoML\_20191213\_174603                            | 0.777294 | 0.562744 | 0.798244 | 0.356261                | 0.437727 | 0.191605 | 700                | 0.003571                    |
| GBM\_3\_AutoML\_20191213\_174603                            | 0.775488 | 0.564794 | 0.793585 | 0.327971                | 0.438722 | 0.192477 | 635                | 0.003748                    |
| XGBoost\_grid\_\_1\_AutoML\_20191213\_174603\_model\_2      | 0.773621 | 0.578141 | 0.791949 | 0.341118                | 0.443963 | 0.197104 | 9722               | 0.003896                    |
| GBM\_grid\_\_1\_AutoML\_20191213\_174603\_model\_1          | 0.772656 | 0.568314 | 0.79164  | 0.332175                | 0.440049 | 0.193643 | 647                | 0.004546                    |
| GBM\_4\_AutoML\_20191213\_174603                            | 0.77248  | 0.569483 | 0.791078 | 0.336913                | 0.440873 | 0.194369 | 800                | 0.004142                    |
| DRF\_1\_AutoML\_20191213\_174603                            | 0.764975 | 0.5801   | 0.781588 | 0.336001                | 0.445222 | 0.198222 | 1399               | 0.007475                    |
| XRT\_1\_AutoML\_20191213\_174603                            | 0.759957 | 0.585158 | 0.776857 | 0.338976                | 0.447598 | 0.200344 | 1426               | 0.00495                     |
| GBM\_grid\_\_1\_AutoML\_20191213\_174603\_model\_2          | 0.748007 | 0.632981 | 0.758833 | 0.375308                | 0.462237 | 0.213663 | 588                | 0.003119                    |
| DeepLearning\_grid\_\_2\_AutoML\_20191213\_174603\_model\_1 | 0.739884 | 0.600688 | 0.747948 | 0.359883                | 0.455205 | 0.207212 | 40408              | 0.010939                    |
| DeepLearning\_1\_AutoML\_20191213\_174603                   | 0.700406 | 0.63169  | 0.70199  | 0.395333                | 0.469084 | 0.22004  | 445                | 0.002288                    |
| DeepLearning\_grid\_\_1\_AutoML\_20191213\_174603\_model\_1 | 0.692235 | 0.671512 | 0.691883 | 0.409836                | 0.478393 | 0.22886  | 32546              | 0.003623                    |
| GLM\_1\_AutoML\_20191213\_174603                            | 0.682648 | 0.63852  | 0.680344 | 0.397234                | 0.472683 | 0.223429 | 195                | 0.001312                    |

