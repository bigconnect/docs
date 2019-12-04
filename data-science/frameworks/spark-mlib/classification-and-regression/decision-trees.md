# Decision trees

[Decision trees](http://en.wikipedia.org/wiki/Decision_tree_learning) and their ensembles are popular methods for the machine learning tasks of classification and regression. Decision trees are widely used since they are easy to interpret, handle categorical features, extend to the multiclass classification setting, do not require feature scaling, and are able to capture non-linearities and feature interactions. Tree ensemble algorithms such as random forests and boosting are among the top performers for classification and regression tasks.

The `spark.ml` implementation supports decision trees for binary and multiclass classification and for regression, using both continuous and categorical features. The implementation partitions data by rows, allowing distributed training with millions or even billions of instances.

Users can find more information about the decision tree algorithm in the [MLlib Decision Tree guide](https://spark.apache.org/docs/2.4.4/mllib-decision-tree.html). The main differences between this API and the [original MLlib Decision Tree API](https://spark.apache.org/docs/2.4.4/mllib-decision-tree.html) are:

* support for ML Pipelines
* separation of Decision Trees for classification vs. regression
* use of DataFrame metadata to distinguish continuous and categorical features

The Pipelines API for Decision Trees offers a bit more functionality than the original API.  
In particular, for classification, users can get the predicted probability of each class \(a.k.a. class conditional probabilities\); for regression, users can get the biased sample variance of prediction.

Ensembles of trees \(Random Forests and Gradient-Boosted Trees\) are described in the [Tree ensembles](tree-ensembles.md) section.

## Inputs and Outputs

We list the input and output \(prediction\) column types here. All output columns are optional; to exclude an output column, set its corresponding Param to an empty string.

#### Input Columns

| Param name | Type\(s\) | Default | Description |
| :--- | :--- | :--- | :--- |
| labelCol | Double | "label" | Label to predict |
| featuresCol | Vector | "features" | Feature vector |

#### Output Columns

| Param name | Type\(s\) | Default | Description | Notes |
| :--- | :--- | :--- | :--- | :--- |
| predictionCol | Double | "prediction" | Predicted label |  |
| rawPredictionCol | Vector | "rawPrediction" | Vector of length \# classes, with the counts of training instance labels at the tree node which makes the prediction | Classification only |
| probabilityCol | Vector | "probability" | Vector of length \# classes equal to rawPrediction normalized to a multinomial distribution | Classification only |
| varianceCol | Double |  | The biased sample variance of prediction | Regression only |

