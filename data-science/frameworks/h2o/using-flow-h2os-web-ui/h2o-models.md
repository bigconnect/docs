# Build Models

## Building Models

There are several ways to build a model, you can:

* Click the **Assist Me!** button in the row of buttons below the menus and select **buildModel**
* Click the **Assist Me!** button, select **getFrames**, then click the **Build Model…** button below the parsed .hex data set
* Click the **View** button after parsing data, then click the **Build Model** button
* Click the drop-down **Model** menu and select the model type from the list

The **Build Model…** button can be accessed from any page containing the .hex key for the parsed data \(for example, `getJobs` &gt; `getFrame`\). The following image depicts the K-Means model type. Available options vary depending on model type.

![](../../../../.gitbook/assets/image%20%28288%29.png)



In the **Build a Model** cell, select an algorithm from the drop-down menu. Available algorithms include:

* **Aggregator**: Create an Aggregator model.
* **CoxPH**: Create a Cox Proportional Hazards model.
* **Deep Learning**: Create a Deep Learning model.
* **Distributed Random Forest**: Create a distributed Random Forest model.
* **Gradient Boosting Machine**: Create a Gradient Boosted model.
* **MOJO Model**: Create a MOJO model.
* **Generalized Linear Model**: Create a Generalized Linear model.
* **Generalized Low Rank Model**: Create a Generalized Low Rank model.
* **Isolation Forest**: Create an Isolation Forest model.
* **K-means**: Create a K-Means model.
* **Naïve Bayes**: Create a Naïve Bayes model.
* **Principal Component Analysis**: Create a Principal Components Analysis model for modeling without regularization or performing dimensionality reduction.
* **Stacked Ensemble**: Create a Stacked Ensemble model.
* **Word2Vec**: Create a Word2Vec model.
* **XGBoost**: Create an XGBoost model.

You can also specify to run AutoML, which automatically trains and tunes models while requiring as few parameters as possible. All the user needs to do is point to a dataset, identify the response column, and optionally specify a time-constraint.

![](../../../../.gitbook/assets/image%20%2813%29.png)

The available options vary depending on the selected model. If an option is only available for a specific model type, the model type is listed. If no model type is specified, the option is applicable to all model types.

* **model\_id**: \(Optional\) Enter a custom name for the model to use as a reference. By default, H2O automatically generates an ID containing the model type \(for example, `gbm-6f6bdc8b-ccbc-474a-b590-4579eea44596`\).
* **training\_frame**: \(Required\) Select the dataset used to build the model.
* **validation\_frame**: \(Optional\) Select the dataset used to evaluate the accuracy of the model.
* **nfolds**: \(GLM, GBM, DL, DRF\) Specify the number of folds for cross-validation.
* **response\_column**: \(Required for GBM, DRF, Deep Learning, GLM, Naïve-Bayes, Stacked Ensembles, AutoML, XGBoost, CoxPH\) Select the column to use as the dependent variable.
* **ignored\_columns**: \(Optional\) Click the checkbox next to a column name to add it to the list of columns excluded from the model. To add all columns, click the **All** button. To remove a column from the list of ignored columns, click the X next to the column name. To remove all columns from the list of ignored columns, click the **None** button. To search for a specific column, type the column name in the **Search** field above the column list. To only show columns with a specific percentage of missing values, specify the percentage in the **Only show columns with more than 0% missing values** field. To change the selections for the hidden columns, use the **Select Visible** or **Deselect Visible** buttons.
* **ignore\_const\_cols**: \(Optional\) Check this checkbox to ignore constant training columns, since no information can be gained from them. This option is selected by default.
* **transform**: \(PCA\) Select the transformation method for the training data: None, Standardize, Normalize, Demean, or Descale.
* **pca\_method**: \(PCA\) Select the algorithm to use for computing the principal components:
  * _GramSVD_: Uses a distributed computation of the Gram matrix, followed by a local SVD using the JAMA package
  * _Power_: Computes the SVD using the power iteration method
  * _Randomized_: Uses randomized subspace iteration method
  * _GLRM_: Fits a generalized low-rank model with L2 loss function and no regularization and solves for the SVD using local matrix algebra
* **family**: \(GLM\) Select the model type \(Gaussian, Binomial, Multinomial, Poisson, Gamma, Tweedie, Negativebinomial, or Ordinal\).
* **solver**: \(GLM\) Select the solver to use \(AUTO, IRLSM, L\_BFGS, COORDINATE\_DESCENT\_NAIVE, or COORDINATE\_DESCENT\). IRLSM is fast on on problems with a small number of predictors and for lambda-search with L1 penalty, while [L\_BFGS](http://cran.r-project.org/web/packages/lbfgs/vignettes/Vignette.pdf) scales better for datasets with many columns. COORDINATE\_DESCENT is IRLSM with the covariance updates version of cyclical coordinate descent in the innermost loop. COORDINATE\_DESCENT\_NAIVE is IRLSM with the naive updates version of cyclical coordinate descent in the innermost loop. COORDINATE\_DESCENT\_NAIVE and COORDINATE\_DESCENT are currently experimental.
* **link**: \(GLM\) Select a link function \(Identity, Family\_Default, Logit, Log, Inverse, Tweedie, or Ologit\).
* **alpha**: \(GLM\) Specify the regularization distribution between L2 and L2. A value of 1 for alpha represents Lasso regression, a value of 0 produces Ridge regression and anything in between specifies the amount of mixing between the two. Default value of alpha is 0 when SOLVER = ‘L-BFGS’, 0.5 otherwise.
* **lambda**: \(GLM\) Specify the regularization strength.
* **lambda\_search**: \(GLM\) Check this checkbox to enable lambda search, starting with lambda max. The given lambda is then interpreted as lambda min.
* **non-negative**: \(GLM\) To force coefficients to be non-negative, check this checkbox.
* **standardize**: \(K-Means, GLM\) To standardize the numeric columns to have mean of zero and unit variance, check this checkbox. Standardization is highly recommended; if you do not use standardization, the results can include components that are dominated by variables that appear to have larger variances relative to other attributes as a matter of scale, rather than true contribution. This option is selected by default.
* **beta\_constraints**: \(GLM\) To use beta constraints, select a dataset from the drop-down menu. The selected frame is used to constraint the coefficient vector to provide upper and lower bounds.
* **ntrees**: \(GBM, DRF, XGBoost, IF\) Specify the number of trees.
* **max\_depth**: \(GBM, DRF, XGBoost, IF\) Specify the maximum tree depth.
* **min\_rows**: \(GBM, DRF, XGBoost, IF\) Specify the minimum number of observations for a leaf \(“nodesize” in R\).
* **nbins**: \(GBM, DRF\) \(Numerical \[real/int\] only\) Specify the minimum number of bins for the histogram to build, then split at the best point.
* **nbins\_cats**: \(GBM, DRF\) \(Categorical \[factors/enums\] only\) Specify the maximum number of bins for the histogram to build, then split at the best point. Higher values can lead to more overfitting. The levels are ordered alphabetically; if there are more levels than bins, adjacent levels share bins. This value has a more significant impact on model fitness than **nbins**. Larger values may increase runtime, especially for deep trees and large clusters, so tuning may be required to find the optimal value for your configuration.
* **learn\_rate**: \(GBM, XGBoost\) Specify the learning rate. The range is 0.0 to 1.0.
* **distribution**: \(GBM, DL\) Select the distribution type from the drop-down list. The options are auto, bernoulli, multinomial, gaussian, poisson, gamma, or tweedie.
* **sample\_rate**: \(GBM, DRF, XGBoost, IF\) Specify the row sampling rate \(x-axis\). The range is 0.0 to 1.0. Higher values may improve training accuracy. Test accuracy improves when either columns or rows are sampled. For details, refer to “Stochastic Gradient Boosting” \([Friedman, 1999](https://statweb.stanford.edu/~jhf/ftp/stobst.pdf)\).
* **sample\_size**: \(IF\) The number of randomly sampled observations used to train each Isolation Forest tree. Only one of `sample_size` or `sample_rate` should be defined. If `sample_rate` is defined, `sample_size` will be ignored. This value defaults to 256.
* **col\_sample\_rate**: \(GBM, DRF, XGBoost\) Specify the column sampling rate \(y-axis\). The range is 0.0 to 1.0. Higher values may improve training accuracy. Test accuracy improves when either columns or rows are sampled. For details, refer to “Stochastic Gradient Boosting” \([Friedman, 1999](https://statweb.stanford.edu/~jhf/ftp/stobst.pdf)\).
* **mtries**: \(DRF, IF\) Specify the columns to randomly select at each level. If the default value of `-1` is used, the number of variables is the square root of the number of columns for classification and p/3 for regression \(where p is the number of predictors\).
* **binomial\_double\_trees**: \(DRF\) \(Binary classification only\) Build twice as many trees \(one per class\). Enabling this option can lead to higher accuracy, while disabling can result in faster model building. This option is disabled by default.
* **score\_each\_iteration**: \(K-Means, DL, DRF, Naïve Bayes, PCA, GBM, GLM, XGBoost, IF\) To score during each iteration of the model training, check this checkbox.
* **k**\*: \(K-Means, PCA\) For K-Means, specify the number of clusters. For PCA, specify the rank of matrix approximation.
* **estimate\_k**: \(K-Means\) Specify whether to estimate the number of clusters \(&lt;=k\) iteratively \(independent of the seed\) and deterministically \(beginning with `k=1,2,3...`\). If enabled, for each **k** that, the estimate will go up to **max\_iteration**. This option is disabled by default.
* **user\_points**: \(K-Means\) For K-Means, specify the number of initial cluster centers.
* **max\_iterations**: \(K-Means, PCA, GLM, CoxPH\) Specify the number of training iterations.
* **init**: \(K-Means\) Select the initialization mode. The options are Furthest, PlusPlus, Random, or User.
* **tweedie\_variance\_power**: \(GLM\) \(Only applicable if _Tweedie_ is selected for **Family**\) Specify the Tweedie variance power.
* **tweedie\_link\_power**: \(GLM\) \(Only applicable if _Tweedie_ is selected for **Family**\) Specify the Tweedie link power.
* **theta** \(GLM\) \(Only applicable if _Negativebinomial_ is selected for **Family**\) Specify the theta value for negative binomial regression
* **activation**: \(DL\) Select the activation function \(Tanh, TanhWithDropout, Rectifier, RectifierWithDropout, Maxout, MaxoutWithDropout\). The default option is Rectifier.
* **hidden**: \(DL\) Specify the hidden layer sizes \(e.g., 100,100\). For Grid Search, use comma-separated values: \(10,10\),\(20,20,20\). The default value is \[200,200\]. The specified value\(s\) must be positive.
* **epochs**: \(DL\) Specify the number of times to iterate \(stream\) the dataset. The value can be a fraction.
* **variable\_importances**: \(DL\) Check this checkbox to compute variable importance. This option is not selected by default.
* **laplace**: \(Naïve Bayes\) Specify the Laplace smoothing parameter.
* **min\_sdev**: \(Naïve Bayes\) Specify the minimum standard deviation to use for observations without enough data.
* **eps\_sdev**: \(Naïve Bayes\) Specify the threshold for standard deviation. If this threshold is not met, the **min\_sdev** value is used.
* **min\_prob**: \(Naïve Bayes\) Specify the minimum probability to use for observations without enough data.
* **eps\_prob**: \(Naïve Bayes\) Specify the threshold for standard deviation. If this threshold is not met, the **min\_sdev** value is used.
* **compute\_metrics**: \(Naïve Bayes, PCA\) To compute metrics on training data, check this checkbox. The Naïve Bayes classifier assumes independence between predictor variables conditional on the response, and a Gaussian distribution of numeric predictors with mean and standard deviation computed from the training dataset. When building a Naïve Bayes classifier, every row in the training dataset that contains at least one NA will be skipped completely. If the test dataset has missing values, then those predictors are omitted in the probability calculation during prediction.
* **max\_models**: \(AutoML\) This option allows the user to specify the maximum number of models to build in an AutoML run.
* **max\_runtime\_secs**: \(XGBoost, AutoML, IF\) This option controls how long the AutoML run will execute. This value defaults to 3600 seconds.
* **base\_model**: \(Stacked Ensembles\) Specify a list of models \(or model IDs\) that can be stacked together. Models must have been cross-validated \(i.e. `nfolds`&gt;1 or `fold_column` was specified\), they all must use the same cross-validation folds, and `keep_cross_validation_predictions` must have been set to True. One way to guarantee identical folds across base models is to set `fold_assignment = "Modulo"` in all the base models. It is also possible to get identical folds by setting `fold_assignment = "Random"` when the same seed is used in all base models.
* **metalearner\_algorithm**: \(Stacked Ensembles\) Type of algorithm to use as the metalearner. Options include ‘AUTO’ \(GLM with non negative weights; if validation\_frame is present, a lambda search is performed\), ‘glm’ \(GLM with default parameters\), ‘gbm’ \(GBM with default parameters\), ‘drf’ \(Random Forest with default parameters\), or ‘deeplearning’ \(Deep Learning with default parameters\).
* **metalearner\_nfolds**: \(Stacked Ensembles\) Number of folds for K-fold cross-validation of the metalearner algorithm \(0 to disable or &gt;= 2\).
* **metalearner\_params**: \(Stacked Ensembles\) A dictionary/list of parameters to be passed in along with the metalearner\_algorithm. If this is not specified, then default values for the specified algorithm will be used.
* **start\_column**: \(CoxPH\) \(Optional\) The name of an integer column in the **source** data set representing the start time. If supplied, the value of the **start\_column** must be strictly less than the **stop\_column** in each row.
* **stop\_column**: \(CoxPH\) The name of an integer column in the **source** data set representing the stop time.

## Viewing Models

Click the **Assist Me!** button, then click the **getModels** link, or enter `getModels` in the cell in CS mode and press **Ctrl+Enter**. A list of available models displays.

![](../../../../.gitbook/assets/image%20%28181%29.png)

To view all current models, you can also click the **Model** menu and click **List All Models**.

Click on a model name to view details about the model. The information that displays varies based on the algorithm that was used to build the model, but it can include the parameters used when building the model, scoring history, training metrics, coefficient tables, and a POJO preview.

![](../../../../.gitbook/assets/image%20%2883%29.png)

The following additional functions are available when viewing a model:

* **Refresh**: Refreshes the model.
* **Predict**: Use this model to make predictions.
* **Download POJO**: Generates a Plain Old Java Object \(POJO\) that can use the model outside of H2O. Note that a POJO can be run in standalone mode or it can be integrated into a platform, such as [Hadoop’s Storm](https://github.com/h2oai/h2o-tutorials/tree/master/tutorials/streaming/storm). To make the POJO work in your Java application, you will also need the `h2o-genmodel.jar` file \(available via the **Download Generated Model** button, from the **Admin** menu, or in `h2o-3/h2o-genmodel/build/libs/h2o-genmodel.jar`\). Note that POJOs are are not supported for XGBoost models.
* **Download Model Deployment Package \(MOJO\)**: Downloads a zip file containing the Model ObJect, Optimized \(MOJO\). This file includes the outputting model information in JSON format. Note that MOJOs are available for AutoML, Deep Learning, DRF, GBM, GLM, GLRM, K-Means, Stacked Ensembles, SVM, Word2vec, and XGBoost models.
* **Export**: Exports a built model.
* **Inspect**: Inspect the model. Clicking this button displays a data table of the model parameters and output information.
* **Delete**: Deletes the model.
* **Download Gen Model**: Downloads the Generated Model \(h2o-genmodel.jar\) file for this model. Note that this is also available from the **Admin** dropdown menu.

## Exporting and Importing Models

**To export a built model:**

1. Click the **Model** menu at the top of the screen.
2. Select _Export Model…_
3. In the `exportModel` cell that appears, select the model from the drop-down _Model:_ list.
4. Enter a location for the exported model in the _Path:_ entry field. **Note**: If you specify a location that doesn’t exist, it will be created. For example, if you only enter `test` in the _Path:_ entry field, the model will be exported to `h2o-3/test`.
5. To overwrite any files with the same name, check the _Overwrite:_ checkbox.
6. Click the **Export** button. A confirmation message displays when the model has been successfully exported.

![](../../../../.gitbook/assets/image%20%28271%29.png)

**To import a built model:**

1. Click the **Model** menu at the top of the screen.
2. Select _Import Model…_
3. Enter the location of the model in the _Path:_ entry field. **Note**: The file path must be complete \(e.g., `Users/h2o-user/h2o-3/exported_models`\). Do not rename models while importing.
4. To overwrite any files with the same name, check the _Overwrite:_ checkbox.
5. Click the **Import** button. A confirmation message displays when the model has been successfully imported. To view the imported model, click the **View Model** button.

![](../../../../.gitbook/assets/image%20%28618%29.png)

## Run AutoML

AutoML automatically trains and tunes models while requiring as few parameters as possible. A user is only required to point to a dataset, identify the response column and optionally specify a time constraint, a maximum number of models constraint, and early stopping parameters. AutoML will then begin training models and will stop as specified in the configuration \(i.e., when the maximum number of models has been reached, when the maximum run time has been reached, or when the stopping criteria are met\).

Stacked Ensembles will also be automatically trained on the collection of individual models to produce a highly predictive ensemble model which, in most cases, will be the top performing model in the AutoML leaderboard. Note that Stacked Ensembles are not yet available for multiclass classification problems, so in that case, only singleton models will be trained.

The outputted models will display on a leaderboard, showing the best results first. The Leaderboard Frame can be specified when configuring the AutoML run. The frame will not be used for anything besides creating the leaderboard. If a Leaderboard Frame is not specified, then one will be created from the Training Frame.

To begin an AutoML run, select **Models &gt; Run AutoML** from the top menu.

![](../../../../.gitbook/assets/image%20%28391%29.png)

At a minimum, specify the training frame and the response column. \(Note that by default, the AutoML run will end after 3600 seconds.\) Click **Build Model** to start the run.

![](../../../../.gitbook/assets/image%20%2816%29.png)

Click the **View** button to view the Leaderboard and/or monitor the current AutoML run.

![](../../../../.gitbook/assets/image%20%2886%29.png)

After clicking **View**, the Leaderboard displays the list of models that were built in the order of `mean_residual_deviance` \(best model first\). While AutoML is running, click the **Monitor Live** button and scroll down to view a live feed of the User Feedback progress of AutoML. This section provides details about each step taken by AutoML, including the parameters being configured, the dataset’s features, and model training information.

![](../../../../.gitbook/assets/image%20%28123%29.png)

## Using Grid Search

To include a parameter in a grid search in Flow, check the checkbox in the _GRID?_ column to the right of the parameter name \(highlighted in red in the image below\).

![](../../../../.gitbook/assets/image%20%28561%29.png)

* If the parameter selected for grid search is Boolean \(T/F or Y/N\), both values are included when the _Grid?_ checkbox is selected.
* If the parameter selected for grid search is a list of values, the values display as checkboxes when the _Grid?_ checkbox is selected. More than one option can be selected.
* If the parameter selected for grid search is a numerical value, use a semicolon \(;\) to separate each additional value.
* To view a list of all grid searches, select the **Model** menu, then click **List All Grid Search Results**, or click the **Assist Me** button and select **getGrids**.

## Interpreting Model Results

**Scoring history**: \(GBM, DL\) Represents the error rate of the model as it is built. Typically, the error rate will be higher at the beginning \(the left side of the graph\) then decrease as the model building completes and accuracy improves. Can include mean squared error \(MSE\) and deviance.

![](../../../../.gitbook/assets/image%20%2824%29.png)

**Variable importances**: \(GBM, DRF, DL\) Represents the statistical significance of each variable in the data in terms of its affect on the model. Variables are listed in order of most to least importance. The percentage values represent the percentage of importance across all variables, scaled to 100%. The method of computing each variable’s importance depends on the algorithm. To view the scaled importance value of a variable, use your mouse to hover over the bar representing the variable.

![](../../../../.gitbook/assets/image%20%28503%29.png)

**Confusion Matrix**: \(RF, GBM\) Table depicting performance of algorithm in terms of false positives, false negatives, true positives, and true negatives. The actual results display in the columns and the predictions display in the rows; correct predictions are highlighted in yellow. In the example below, `0` was predicted correctly 902 times, while `8` was predicted correctly 822 times and `0` was predicted as `4` once.

![](../../../../.gitbook/assets/image%20%28551%29.png)

**ROC Curve**: \(DRF\) A [ROC Curve](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) is a graph that represents the ratio of true positives to false positives. \(For more information, refer to the Linear Digressions [podcast](http://lineardigressions.com/episodes/2017/1/29/rock-the-roc-curve) describing ROC Curves.\) To view a specific threshold, select a value from the drop-down **Threshold** list. To view any of the following details, select it from the drop-down **Criterion** list:

* Max f1
* Max f2
* Max f0point5
* Max accuracy
* Max precision
* Max absolute MCC \(the threshold that maximizes the absolute Matthew’s Correlation Coefficient\)
* Max min per class accuracy

The lower-left side of the graph represents less tolerance for false positives while the upper-right represents more tolerance for false positives. Ideally, a highly accurate ROC resembles the following example.

![](../../../../.gitbook/assets/image%20%28595%29.png)

**Hit Ratio**: \(GBM, DRF, NaiveBayes, DL, GLM\) \(Multinomial Classification only\) Table representing the number of times that the prediction was correct out of the total number of predictions.

![](../../../../.gitbook/assets/image%20%28537%29.png)

**Standardized Coefficient Magnitudes** \(GLM\) Bar chart representing the relationship of a specific feature to the response variable. Coefficients can be positive \(orange\) or negative \(blue\). A positive coefficient indicates a positive relationship between the feature and the response, where an increase in the feature corresponds with an increase in the response, while a negative coefficient represents a negative relationship between the feature and the response where an increase in the feature corresponds with a decrease in the response \(or vice versa\).

![](../../../../.gitbook/assets/image%20%2837%29.png)



