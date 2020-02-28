# Tree Ensembles

The DataFrame API supports two major tree ensemble algorithms: [Random Forests](http://en.wikipedia.org/wiki/Random_forest) and [Gradient-Boosted Trees \(GBTs\)](http://en.wikipedia.org/wiki/Gradient_boosting). Both use `spark.ml` [decision trees](decision-trees.md) as their base models.

Users can find more information about ensemble algorithms in the [MLlib Ensemble guide](https://spark.apache.org/docs/2.4.4/mllib-ensembles.html).  
In this section, we demonstrate the DataFrame API for ensembles.

The main differences between this API and the [original MLlib ensembles API](https://spark.apache.org/docs/2.4.4/mllib-ensembles.html) are:

* support for DataFrames and ML Pipelines
* separation of classification vs. regression
* use of DataFrame metadata to distinguish continuous and categorical features
* more functionality for random forests: estimates of feature importance, as well as the predicted probability of each class \(a.k.a. class conditional probabilities\) for classification.

## Random Forests

[Random forests](http://en.wikipedia.org/wiki/Random_forest) are ensembles of [decision trees](https://spark.apache.org/docs/2.4.4/ml-classification-regression.html#decision-trees). Random forests combine many decision trees in order to reduce the risk of overfitting. The `spark.ml` implementation supports random forests for binary and multiclass classification and for regression, using both continuous and categorical features.

For more information on the algorithm itself, please see the [`spark.mllib` documentation on random forests](https://spark.apache.org/docs/2.4.4/mllib-ensembles.html#random-forests).

### Inputs and Outputs

We list the input and output \(prediction\) column types here. All output columns are optional; to exclude an output column, set its corresponding Param to an empty string.

**Input Columns**

| Param name | Type\(s\) | Default | Description |
| :--- | :--- | :--- | :--- |
| labelCol | Double | "label" | Label to predict |
| featuresCol | Vector | "features" | Feature vector |

**Output Columns \(Predictions\)**

| Param name | Type\(s\) | Default | Description | Notes |
| :--- | :--- | :--- | :--- | :--- |
| predictionCol | Double | "prediction" | Predicted label |  |
| rawPredictionCol | Vector | "rawPrediction" | Vector of length \# classes, with the counts of training instance labels at the tree node which makes the prediction | Classification only |
| probabilityCol | Vector | "probability" | Vector of length \# classes equal to rawPrediction normalized to a multinomial distribution | Classification only |

## Gradient-Boosted Trees \(GBTs\)

[Gradient-Boosted Trees \(GBTs\)](http://en.wikipedia.org/wiki/Gradient_boosting) are ensembles of [decision trees](https://spark.apache.org/docs/2.4.4/ml-classification-regression.html#decision-trees). GBTs iteratively train decision trees in order to minimize a loss function. The `spark.ml` implementation supports GBTs for binary classification and for regression, using both continuous and categorical features.

For more information on the algorithm itself, please see the [`spark.mllib` documentation on GBTs](https://spark.apache.org/docs/2.4.4/mllib-ensembles.html#gradient-boosted-trees-gbts).

### Inputs and Outputs

We list the input and output \(prediction\) column types here. All output columns are optional; to exclude an output column, set its corresponding Param to an empty string.

**Input Columns**

| Param name | Type\(s\) | Default | Description |
| :--- | :--- | :--- | :--- |
| labelCol | Double | "label" | Label to predict |
| featuresCol | Vector | "features" | Feature vector |

Note that `GBTClassifier` currently only supports binary labels.

**Output Columns \(Predictions\)**

| Param name | Type\(s\) | Default | Description |
| :--- | :--- | :--- | :--- |
| predictionCol | Double | "prediction" | Predicted label |

In the future, `GBTClassifier` will also output columns for `rawPrediction` and `probability`, just as `RandomForestClassifier` does.

