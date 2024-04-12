# Machine learning functions

## Machine learning functions[#](https://trino.io/docs/current/functions/ml.html#functions-ml--page-root) <a href="#functions-ml--page-root" id="functions-ml--page-root"></a>

The machine learning plugin provides machine learning functionality as an aggregation function. It enables you to train Support Vector Machine (SVM) based classifiers and regressors for the supervised learning problems.

Note

The machine learning functions are not optimized for distributed processing. The capability to train large data sets is limited by this execution of the final training on a single instance.

### Feature vector[#](https://trino.io/docs/current/functions/ml.html#feature-vector) <a href="#feature-vector" id="feature-vector"></a>

To solve a problem with the machine learning technique, especially as a supervised learning problem, it is necessary to represent the data set with the sequence of pairs of labels and feature vector. A label is a target value you want to predict from the unseen feature and a feature is a A N-dimensional vector whose elements are numerical values. In Trino, a feature vector is represented as a map-type value, whose key is an index of each feature, so that it can express a sparse vector. Since classifiers and regressors can recognize the map-type feature vector, there is a function to construct the feature from the existing numerical values, [`features()`](https://trino.io/docs/current/functions/ml.html#features):

```
SELECT features(1.0, 2.0, 3.0) AS features;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
       features
-----------------------
 {0=1.0, 1=2.0, 2=3.0}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The output from [`features()`](https://trino.io/docs/current/functions/ml.html#features) can be directly passed to ML functions.

### Classification[#](https://trino.io/docs/current/functions/ml.html#classification) <a href="#classification" id="classification"></a>

Classification is a type of supervised learning problem to predict the distinct label from the given feature vector. The interface looks similar to the construction of the SVM model from the sequence of pairs of labels and features implemented in Teradata Aster or [BigQuery ML](https://cloud.google.com/bigquery-ml/docs/bigqueryml-intro). The function to train a classification model looks like as follows:

```
SELECT
  learn_classifier(
    species,
    features(sepal_length, sepal_width, petal_length, petal_width)
  ) AS model
FROM
  iris
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

It returns the trained model in a serialized format.

```
                      model
-------------------------------------------------
 3c 43 6c 61 73 73 69 66 69 65 72 28 76 61 72 63
 68 61 72 29 3e
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

[`classify()`](https://trino.io/docs/current/functions/ml.html#classify) returns the predicted label by using the trained model. The trained model can not be saved natively, and needs to be passed in the format of a nested query:

```
SELECT
  classify(features(5.9, 3, 5.1, 1.8), model) AS predicted_label
FROM (
  SELECT
    learn_classifier(species, features(sepal_length, sepal_width, petal_length, petal_width)) AS model
  FROM
    iris
) t
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 predicted_label
-----------------
 Iris-virginica
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

As a result you need to run the training process at the same time when predicting values. Internally, the model is trained by [libsvm](https://www.csie.ntu.edu.tw/\~cjlin/libsvm/). You can use [`learn_libsvm_classifier()`](https://trino.io/docs/current/functions/ml.html#learn\_libsvm\_classifier) to control the internal parameters of the model.

### Regression[#](https://trino.io/docs/current/functions/ml.html#regression) <a href="#regression" id="regression"></a>

Regression is another type of supervised learning problem, predicting continuous value, unlike the classification problem. The target must be numerical values that can be described as `double`.

The following code shows the creation of the model predicting `sepal_length` from the other 3 features:

```
SELECT
  learn_regressor(sepal_length, features(sepal_width, petal_length, petal_width)) AS model
FROM
  iris
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The way to use the model is similar to the classification case:

```
SELECT
  regress(features(3, 5.1, 1.8), model) AS predicted_target
FROM (
  SELECT
    learn_regressor(sepal_length, features(sepal_width, petal_length, petal_width)) AS model
  FROM iris
) t;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 predicted_target
-------------------
 6.407376822560477
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Internally, the model is trained by [libsvm](https://www.csie.ntu.edu.tw/\~cjlin/libsvm/). [`learn_libsvm_regressor()`](https://trino.io/docs/current/functions/ml.html#learn\_libsvm\_regressor) provides you a way to control the training process.

### Machine learning functions[#](https://trino.io/docs/current/functions/ml.html#id1) <a href="#id1" id="id1"></a>

features(_double_, _...) -> map(bigint_, _double_)[#](https://trino.io/docs/current/functions/ml.html#features)

Returns the map representing the feature vector.

learn\_classifier(_label_, _features_) → Classifier[#](https://trino.io/docs/current/functions/ml.html#learn\_classifier)

Returns an SVM-based classifier model, trained with the given label and feature data sets.

learn\_libsvm\_classifier(_label_, _features_, _params_) → Classifier[#](https://trino.io/docs/current/functions/ml.html#learn\_libsvm\_classifier)

Returns an SVM-based classifier model, trained with the given label and feature data sets. You can control the training process by libsvm parameters.

classify(_features_, _model_) → label[#](https://trino.io/docs/current/functions/ml.html#classify)

Returns a label predicted by the given classifier SVM model.

learn\_regressor(_target_, _features_) → Regressor[#](https://trino.io/docs/current/functions/ml.html#learn\_regressor)

Returns an SVM-based regressor model, trained with the given target and feature data sets.

learn\_libsvm\_regressor(_target_, _features_, _params_) → Regressor[#](https://trino.io/docs/current/functions/ml.html#learn\_libsvm\_regressor)

Returns an SVM-based regressor model, trained with the given target and feature data sets. You can control the training process by libsvm parameters.

regress(_features_, _model_) → target[#](https://trino.io/docs/current/functions/ml.html#regress)

Returns a predicted target value by the given regressor SVM model.
