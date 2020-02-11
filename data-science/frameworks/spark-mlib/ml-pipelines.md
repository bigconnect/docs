# Spark ML Pipelines

In this section, we introduce the concept of _**ML Pipelines**_. ML Pipelines provide a uniform set of high-level APIs built on top of [DataFrames](https://spark.apache.org/docs/2.4.4/sql-programming-guide.html) that help users create and tune practical machine learning pipelines.

## Main concepts in Pipelines

MLlib standardizes APIs for machine learning algorithms to make it easier to combine multiple algorithms into a single pipeline, or workflow. This section covers the key concepts introduced by the Pipelines API, where the pipeline concept is mostly inspired by the [scikit-learn](http://scikit-learn.org/) project.

* [**`DataFrame`**](https://spark.apache.org/docs/2.4.4/ml-pipeline.html#dataframe): This ML API uses `DataFrame` from Spark SQL as an ML dataset, which can hold a variety of data types. E.g., a `DataFrame` could have different columns storing text, feature vectors, true labels, and predictions.
* [**`Transformer`**](https://spark.apache.org/docs/2.4.4/ml-pipeline.html#transformers): A `Transformer` is an algorithm which can transform one `DataFrame` into another `DataFrame`. E.g., an ML model is a `Transformer` which transforms a `DataFrame` with features into a `DataFrame` with predictions.
* [**`Estimator`**](https://spark.apache.org/docs/2.4.4/ml-pipeline.html#estimators): An `Estimator` is an algorithm which can be fit on a `DataFrame` to produce a `Transformer`. E.g., a learning algorithm is an `Estimator` which trains on a `DataFrame` and produces a model.
* [**`Pipeline`**](https://spark.apache.org/docs/2.4.4/ml-pipeline.html#pipeline): A `Pipeline` chains multiple `Transformer`s and `Estimator`s together to specify an ML workflow.
* [**`Parameter`**](https://spark.apache.org/docs/2.4.4/ml-pipeline.html#parameters): All `Transformer`s and `Estimator`s now share a common API for specifying parameters.

### DataFrame <a id="dataframe"></a>

Machine learning can be applied to a wide variety of data types, such as vectors, text, images, and structured data. This API adopts the `DataFrame` from Spark SQL in order to support a variety of data types.

`DataFrame` supports many basic and structured types; see the [Spark SQL datatype reference](https://spark.apache.org/docs/2.4.4/sql-reference.html#data-types) for a list of supported types. In addition to the types listed in the Spark SQL guide, `DataFrame` can use ML [`Vector`](https://spark.apache.org/docs/2.4.4/mllib-data-types.html#local-vector) types.

A `DataFrame` can be created either implicitly or explicitly from a regular `RDD`. See the code examples below and the [Spark SQL programming guide](https://spark.apache.org/docs/2.4.4/sql-programming-guide.html) for examples.

Columns in a `DataFrame` are named. The code examples below use names such as “text,” “features,” and “label.”

### Pipeline components <a id="pipeline-components"></a>

#### Transformers

A `Transformer` is an abstraction that includes feature transformers and learned models. Technically, a `Transformer` implements a method `transform()`, which converts one `DataFrame` into another, generally by appending one or more columns. For example:

* A feature transformer might take a `DataFrame`, read a column \(e.g., text\), map it into a new column \(e.g., feature vectors\), and output a new `DataFrame` with the mapped column appended.
* A learning model might take a `DataFrame`, read the column containing feature vectors, predict the label for each feature vector, and output a new `DataFrame` with predicted labels appended as a column.

#### Estimators

An `Estimator` abstracts the concept of a learning algorithm or any algorithm that fits or trains on data. Technically, an `Estimator` implements a method `fit()`, which accepts a `DataFrame` and produces a `Model`, which is a `Transformer`. For example, a learning algorithm such as `LogisticRegression` is an `Estimator`, and calling `fit()` trains a `LogisticRegressionModel`, which is a `Model` and hence a `Transformer`.

#### Properties of pipeline components

`Transformer.transform()`s and `Estimator.fit()`s are both stateless. In the future, stateful algorithms may be supported via alternative concepts.

Each instance of a `Transformer` or `Estimator` has a unique ID, which is useful in specifying parameters \(discussed below\).

### Pipeline

In machine learning, it is common to run a sequence of algorithms to process and learn from data. E.g., a simple text document processing workflow might include several stages:

* Split each document’s text into words.
* Convert each document’s words into a numerical feature vector.
* Learn a prediction model using the feature vectors and labels.

MLlib represents such a workflow as a `Pipeline`, which consists of a sequence of `PipelineStage`s \(`Transformer`s and `Estimator`s\) to be run in a specific order. We will use this simple workflow as a running example in this section.

#### How it works

A `Pipeline` is specified as a sequence of stages, and each stage is either a `Transformer` or an `Estimator`. These stages are run in order, and the input `DataFrame` is transformed as it passes through each stage. For `Transformer` stages, the `transform()` method is called on the `DataFrame`. For `Estimator` stages, the `fit()` method is called to produce a `Transformer` \(which becomes part of the `PipelineModel`, or fitted `Pipeline`\), and that `Transformer`’s `transform()` method is called on the `DataFrame`.

We illustrate this for the simple text document workflow. The figure below is for the _training time_ usage of a `Pipeline`.

![](../../../.gitbook/assets/image%20%2810%29.png)

Above, the top row represents a `Pipeline` with three stages. The first two \(`Tokenizer` and `HashingTF`\) are `Transformer`s \(blue\), and the third \(`LogisticRegression`\) is an `Estimator` \(red\). The bottom row represents data flowing through the pipeline, where cylinders indicate `DataFrame`s. The `Pipeline.fit()` method is called on the original `DataFrame`, which has raw text documents and labels. The `Tokenizer.transform()` method splits the raw text documents into words, adding a new column with words to the `DataFrame`. The `HashingTF.transform()` method converts the words column into feature vectors, adding a new column with those vectors to the `DataFrame`. Now, since `LogisticRegression` is an `Estimator`, the `Pipeline` first calls `LogisticRegression.fit()` to produce a `LogisticRegressionModel`. If the `Pipeline` had more `Estimator`s, it would call the `LogisticRegressionModel`’s `transform()` method on the `DataFrame` before passing the `DataFrame` to the next stage.

A `Pipeline` is an `Estimator`. Thus, after a `Pipeline`’s `fit()` method runs, it produces a `PipelineModel`, which is a `Transformer`. This `PipelineModel` is used at _test time_; the figure below illustrates this usage.

![](../../../.gitbook/assets/image%20%28316%29.png)

In the figure above, the `PipelineModel` has the same number of stages as the original `Pipeline`, but all `Estimator`s in the original `Pipeline` have become `Transformer`s. When the `PipelineModel`’s `transform()` method is called on a test dataset, the data are passed through the fitted pipeline in order. Each stage’s `transform()` method updates the dataset and passes it to the next stage.

`Pipeline` and `PipelineModel` help to ensure that training and test data go through identical feature processing steps.

#### Details

_DAG `Pipeline`s_: A `Pipeline`’s stages are specified as an ordered array. The examples given here are all for linear `Pipeline`s, i.e., `Pipeline`s in which each stage uses data produced by the previous stage. It is possible to create non-linear `Pipeline`s as long as the data flow graph forms a Directed Acyclic Graph \(DAG\). This graph is currently specified implicitly based on the input and output column names of each stage \(generally specified as parameters\). If the `Pipeline` forms a DAG, then the stages must be specified in topological order.

_Runtime checking_: Since `Pipeline`s can operate on `DataFrame`s with varied types, they cannot use compile-time type checking. `Pipeline`s and `PipelineModel`s instead do runtime checking before actually running the `Pipeline`. This type checking is done using the `DataFrame` _schema_, a description of the data types of columns in the `DataFrame`.

_Unique Pipeline stages_: A `Pipeline`’s stages should be unique instances. E.g., the same instance `myHashingTF` should not be inserted into the `Pipeline` twice since `Pipeline` stages must have unique IDs. However, different instances `myHashingTF1` and `myHashingTF2` \(both of type `HashingTF`\) can be put into the same `Pipeline` since different instances will be created with different IDs.

### Parameters

MLlib `Estimator`s and `Transformer`s use a uniform API for specifying parameters.

A `Param` is a named parameter with self-contained documentation. A `ParamMap` is a set of \(parameter, value\) pairs.

There are two main ways to pass parameters to an algorithm:

1. Set parameters for an instance. E.g., if `lr` is an instance of `LogisticRegression`, one could call `lr.setMaxIter(10)` to make `lr.fit()` use at most 10 iterations. This API resembles the API used in `spark.mllib` package.
2. Pass a `ParamMap` to `fit()` or `transform()`. Any parameters in the `ParamMap` will override parameters previously specified via setter methods.

Parameters belong to specific instances of `Estimator`s and `Transformer`s. For example, if we have two `LogisticRegression` instances `lr1` and `lr2`, then we can build a `ParamMap` with both `maxIter` parameters specified: `ParamMap(lr1.maxIter -> 10, lr2.maxIter -> 20)`. This is useful if there are two algorithms with the `maxIter` parameter in a `Pipeline`.

### ML persistence: Saving and Loading Pipelines

Often times it is worth it to save a model or a pipeline to disk for later use. In Spark 1.6, a model import/export functionality was added to the Pipeline API. As of Spark 2.3, the DataFrame-based API in `spark.ml` and `pyspark.ml` has complete coverage.

ML persistence works across Scala, Java and Python. However, R currently uses a modified format, so models saved in R can only be loaded back in R; this should be fixed in the future and is tracked in [SPARK-15572](https://issues.apache.org/jira/browse/SPARK-15572).

### Backwards compatibility for ML persistence

In general, MLlib maintains backwards compatibility for ML persistence. I.e., if you save an ML model or Pipeline in one version of Spark, then you should be able to load it back and use it in a future version of Spark. However, there are rare exceptions, described below.

Model persistence: Is a model or Pipeline saved using Apache Spark ML persistence in Spark version X loadable by Spark version Y?

* Major versions: No guarantees, but best-effort.
* Minor and patch versions: Yes; these are backwards compatible.
* Note about the format: There are no guarantees for a stable persistence format, but model loading itself is designed to be backwards compatible.

Model behavior: Does a model or Pipeline in Spark version X behave identically in Spark version Y?

* Major versions: No guarantees, but best-effort.
* Minor and patch versions: Identical behavior, except for bug fixes.

For both model persistence and model behavior, any breaking changes across a minor version or patch version are reported in the Spark version release notes. If a breakage is not reported in release notes, then it should be treated as a bug to be fixed.

## Code examples

This section gives code examples illustrating the functionality discussed above. For more info, please refer to the API documentation \([Scala](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.package), [Java](https://spark.apache.org/docs/2.4.4/api/java/org/apache/spark/ml/package-summary.html), and [Python](https://spark.apache.org/docs/2.4.4/api/python/pyspark.ml.html)\).

### Example: Estimator, Transformer, and Param

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.classification.LogisticRegression
import org.apache.spark.ml.linalg.{Vector, Vectors}
import org.apache.spark.ml.param.ParamMap
import org.apache.spark.sql.Row

// Prepare training data from a list of (label, features) tuples.
val training = spark.createDataFrame(Seq(
  (1.0, Vectors.dense(0.0, 1.1, 0.1)),
  (0.0, Vectors.dense(2.0, 1.0, -1.0)),
  (0.0, Vectors.dense(2.0, 1.3, 1.0)),
  (1.0, Vectors.dense(0.0, 1.2, -0.5))
)).toDF("label", "features")

// Create a LogisticRegression instance. This instance is an Estimator.
val lr = new LogisticRegression()
// Print out the parameters, documentation, and any default values.
println(s"LogisticRegression parameters:\n ${lr.explainParams()}\n")

// We may set parameters using setter methods.
lr.setMaxIter(10)
  .setRegParam(0.01)

// Learn a LogisticRegression model. This uses the parameters stored in lr.
val model1 = lr.fit(training)
// Since model1 is a Model (i.e., a Transformer produced by an Estimator),
// we can view the parameters it used during fit().
// This prints the parameter (name: value) pairs, where names are unique IDs for this
// LogisticRegression instance.
println(s"Model 1 was fit using parameters: ${model1.parent.extractParamMap}")

// We may alternatively specify parameters using a ParamMap,
// which supports several methods for specifying parameters.
val paramMap = ParamMap(lr.maxIter -> 20)
  .put(lr.maxIter, 30)  // Specify 1 Param. This overwrites the original maxIter.
  .put(lr.regParam -> 0.1, lr.threshold -> 0.55)  // Specify multiple Params.

// One can also combine ParamMaps.
val paramMap2 = ParamMap(lr.probabilityCol -> "myProbability")  // Change output column name.
val paramMapCombined = paramMap ++ paramMap2

// Now learn a new model using the paramMapCombined parameters.
// paramMapCombined overrides all parameters set earlier via lr.set* methods.
val model2 = lr.fit(training, paramMapCombined)
println(s"Model 2 was fit using parameters: ${model2.parent.extractParamMap}")

// Prepare test data.
val test = spark.createDataFrame(Seq(
  (1.0, Vectors.dense(-1.0, 1.5, 1.3)),
  (0.0, Vectors.dense(3.0, 2.0, -0.1)),
  (1.0, Vectors.dense(0.0, 2.2, -1.5))
)).toDF("label", "features")

// Make predictions on test data using the Transformer.transform() method.
// LogisticRegression.transform will only use the 'features' column.
// Note that model2.transform() outputs a 'myProbability' column instead of the usual
// 'probability' column since we renamed the lr.probabilityCol parameter previously.
model2.transform(test)
  .select("features", "label", "myProbability", "prediction")
  .collect()
  .foreach { case Row(features: Vector, label: Double, prob: Vector, prediction: Double) =>
    println(s"($features, $label) -> prob=$prob, prediction=$prediction")
  }
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.classification.LogisticRegression;
import org.apache.spark.ml.classification.LogisticRegressionModel;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.ml.param.ParamMap;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

// Prepare training data.
List<Row> dataTraining = Arrays.asList(
    RowFactory.create(1.0, Vectors.dense(0.0, 1.1, 0.1)),
    RowFactory.create(0.0, Vectors.dense(2.0, 1.0, -1.0)),
    RowFactory.create(0.0, Vectors.dense(2.0, 1.3, 1.0)),
    RowFactory.create(1.0, Vectors.dense(0.0, 1.2, -0.5))
);
StructType schema = new StructType(new StructField[]{
    new StructField("label", DataTypes.DoubleType, false, Metadata.empty()),
    new StructField("features", new VectorUDT(), false, Metadata.empty())
});
Dataset<Row> training = spark.createDataFrame(dataTraining, schema);

// Create a LogisticRegression instance. This instance is an Estimator.
LogisticRegression lr = new LogisticRegression();
// Print out the parameters, documentation, and any default values.
System.out.println("LogisticRegression parameters:\n" + lr.explainParams() + "\n");

// We may set parameters using setter methods.
lr.setMaxIter(10).setRegParam(0.01);

// Learn a LogisticRegression model. This uses the parameters stored in lr.
LogisticRegressionModel model1 = lr.fit(training);
// Since model1 is a Model (i.e., a Transformer produced by an Estimator),
// we can view the parameters it used during fit().
// This prints the parameter (name: value) pairs, where names are unique IDs for this
// LogisticRegression instance.
System.out.println("Model 1 was fit using parameters: " + model1.parent().extractParamMap());

// We may alternatively specify parameters using a ParamMap.
ParamMap paramMap = new ParamMap()
  .put(lr.maxIter().w(20))  // Specify 1 Param.
  .put(lr.maxIter(), 30)  // This overwrites the original maxIter.
  .put(lr.regParam().w(0.1), lr.threshold().w(0.55));  // Specify multiple Params.

// One can also combine ParamMaps.
ParamMap paramMap2 = new ParamMap()
  .put(lr.probabilityCol().w("myProbability"));  // Change output column name
ParamMap paramMapCombined = paramMap.$plus$plus(paramMap2);

// Now learn a new model using the paramMapCombined parameters.
// paramMapCombined overrides all parameters set earlier via lr.set* methods.
LogisticRegressionModel model2 = lr.fit(training, paramMapCombined);
System.out.println("Model 2 was fit using parameters: " + model2.parent().extractParamMap());

// Prepare test documents.
List<Row> dataTest = Arrays.asList(
    RowFactory.create(1.0, Vectors.dense(-1.0, 1.5, 1.3)),
    RowFactory.create(0.0, Vectors.dense(3.0, 2.0, -0.1)),
    RowFactory.create(1.0, Vectors.dense(0.0, 2.2, -1.5))
);
Dataset<Row> test = spark.createDataFrame(dataTest, schema);

// Make predictions on test documents using the Transformer.transform() method.
// LogisticRegression.transform will only use the 'features' column.
// Note that model2.transform() outputs a 'myProbability' column instead of the usual
// 'probability' column since we renamed the lr.probabilityCol parameter previously.
Dataset<Row> results = model2.transform(test);
Dataset<Row> rows = results.select("features", "label", "myProbability", "prediction");
for (Row r: rows.collectAsList()) {
  System.out.println("(" + r.get(0) + ", " + r.get(1) + ") -> prob=" + r.get(2)
    + ", prediction=" + r.get(3));
}
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.linalg import Vectors
from pyspark.ml.classification import LogisticRegression

# Prepare training data from a list of (label, features) tuples.
training = spark.createDataFrame([
    (1.0, Vectors.dense([0.0, 1.1, 0.1])),
    (0.0, Vectors.dense([2.0, 1.0, -1.0])),
    (0.0, Vectors.dense([2.0, 1.3, 1.0])),
    (1.0, Vectors.dense([0.0, 1.2, -0.5]))], ["label", "features"])

# Create a LogisticRegression instance. This instance is an Estimator.
lr = LogisticRegression(maxIter=10, regParam=0.01)
# Print out the parameters, documentation, and any default values.
print("LogisticRegression parameters:\n" + lr.explainParams() + "\n")

# Learn a LogisticRegression model. This uses the parameters stored in lr.
model1 = lr.fit(training)

# Since model1 is a Model (i.e., a transformer produced by an Estimator),
# we can view the parameters it used during fit().
# This prints the parameter (name: value) pairs, where names are unique IDs for this
# LogisticRegression instance.
print("Model 1 was fit using parameters: ")
print(model1.extractParamMap())

# We may alternatively specify parameters using a Python dictionary as a paramMap
paramMap = {lr.maxIter: 20}
paramMap[lr.maxIter] = 30  # Specify 1 Param, overwriting the original maxIter.
paramMap.update({lr.regParam: 0.1, lr.threshold: 0.55})  # Specify multiple Params.

# You can combine paramMaps, which are python dictionaries.
paramMap2 = {lr.probabilityCol: "myProbability"}  # Change output column name
paramMapCombined = paramMap.copy()
paramMapCombined.update(paramMap2)

# Now learn a new model using the paramMapCombined parameters.
# paramMapCombined overrides all parameters set earlier via lr.set* methods.
model2 = lr.fit(training, paramMapCombined)
print("Model 2 was fit using parameters: ")
print(model2.extractParamMap())

# Prepare test data
test = spark.createDataFrame([
    (1.0, Vectors.dense([-1.0, 1.5, 1.3])),
    (0.0, Vectors.dense([3.0, 2.0, -0.1])),
    (1.0, Vectors.dense([0.0, 2.2, -1.5]))], ["label", "features"])

# Make predictions on test data using the Transformer.transform() method.
# LogisticRegression.transform will only use the 'features' column.
# Note that model2.transform() outputs a "myProbability" column instead of the usual
# 'probability' column since we renamed the lr.probabilityCol parameter previously.
prediction = model2.transform(test)
result = prediction.select("features", "label", "myProbability", "prediction") \
    .collect()

for row in result:
    print("features=%s, label=%s -> prob=%s, prediction=%s"
          % (row.features, row.label, row.myProbability, row.prediction))yt
```
{% endtab %}
{% endtabs %}

### Example: Pipeline

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.{Pipeline, PipelineModel}
import org.apache.spark.ml.classification.LogisticRegression
import org.apache.spark.ml.feature.{HashingTF, Tokenizer}
import org.apache.spark.ml.linalg.Vector
import org.apache.spark.sql.Row

// Prepare training documents from a list of (id, text, label) tuples.
val training = spark.createDataFrame(Seq(
  (0L, "a b c d e spark", 1.0),
  (1L, "b d", 0.0),
  (2L, "spark f g h", 1.0),
  (3L, "hadoop mapreduce", 0.0)
)).toDF("id", "text", "label")

// Configure an ML pipeline, which consists of three stages: tokenizer, hashingTF, and lr.
val tokenizer = new Tokenizer()
  .setInputCol("text")
  .setOutputCol("words")
val hashingTF = new HashingTF()
  .setNumFeatures(1000)
  .setInputCol(tokenizer.getOutputCol)
  .setOutputCol("features")
val lr = new LogisticRegression()
  .setMaxIter(10)
  .setRegParam(0.001)
val pipeline = new Pipeline()
  .setStages(Array(tokenizer, hashingTF, lr))

// Fit the pipeline to training documents.
val model = pipeline.fit(training)

// Now we can optionally save the fitted pipeline to disk
model.write.overwrite().save("/tmp/spark-logistic-regression-model")

// We can also save this unfit pipeline to disk
pipeline.write.overwrite().save("/tmp/unfit-lr-model")

// And load it back in during production
val sameModel = PipelineModel.load("/tmp/spark-logistic-regression-model")

// Prepare test documents, which are unlabeled (id, text) tuples.
val test = spark.createDataFrame(Seq(
  (4L, "spark i j k"),
  (5L, "l m n"),
  (6L, "spark hadoop spark"),
  (7L, "apache hadoop")
)).toDF("id", "text")

// Make predictions on test documents.
model.transform(test)
  .select("id", "text", "probability", "prediction")
  .collect()
  .foreach { case Row(id: Long, text: String, prob: Vector, prediction: Double) =>
    println(s"($id, $text) --> prob=$prob, prediction=$prediction")
  }
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;

import org.apache.spark.ml.Pipeline;
import org.apache.spark.ml.PipelineModel;
import org.apache.spark.ml.PipelineStage;
import org.apache.spark.ml.classification.LogisticRegression;
import org.apache.spark.ml.feature.HashingTF;
import org.apache.spark.ml.feature.Tokenizer;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;

// Prepare training documents, which are labeled.
Dataset<Row> training = spark.createDataFrame(Arrays.asList(
  new JavaLabeledDocument(0L, "a b c d e spark", 1.0),
  new JavaLabeledDocument(1L, "b d", 0.0),
  new JavaLabeledDocument(2L, "spark f g h", 1.0),
  new JavaLabeledDocument(3L, "hadoop mapreduce", 0.0)
), JavaLabeledDocument.class);

// Configure an ML pipeline, which consists of three stages: tokenizer, hashingTF, and lr.
Tokenizer tokenizer = new Tokenizer()
  .setInputCol("text")
  .setOutputCol("words");
HashingTF hashingTF = new HashingTF()
  .setNumFeatures(1000)
  .setInputCol(tokenizer.getOutputCol())
  .setOutputCol("features");
LogisticRegression lr = new LogisticRegression()
  .setMaxIter(10)
  .setRegParam(0.001);
Pipeline pipeline = new Pipeline()
  .setStages(new PipelineStage[] {tokenizer, hashingTF, lr});

// Fit the pipeline to training documents.
PipelineModel model = pipeline.fit(training);

// Prepare test documents, which are unlabeled.
Dataset<Row> test = spark.createDataFrame(Arrays.asList(
  new JavaDocument(4L, "spark i j k"),
  new JavaDocument(5L, "l m n"),
  new JavaDocument(6L, "spark hadoop spark"),
  new JavaDocument(7L, "apache hadoop")
), JavaDocument.class);

// Make predictions on test documents.
Dataset<Row> predictions = model.transform(test);
for (Row r : predictions.select("id", "text", "probability", "prediction").collectAsList()) {
  System.out.println("(" + r.get(0) + ", " + r.get(1) + ") --> prob=" + r.get(2)
    + ", prediction=" + r.get(3));
}
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml import Pipeline
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.feature import HashingTF, Tokenizer

# Prepare training documents from a list of (id, text, label) tuples.
training = spark.createDataFrame([
    (0, "a b c d e spark", 1.0),
    (1, "b d", 0.0),
    (2, "spark f g h", 1.0),
    (3, "hadoop mapreduce", 0.0)
], ["id", "text", "label"])

# Configure an ML pipeline, which consists of three stages: tokenizer, hashingTF, and lr.
tokenizer = Tokenizer(inputCol="text", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.001)
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

# Fit the pipeline to training documents.
model = pipeline.fit(training)

# Prepare test documents, which are unlabeled (id, text) tuples.
test = spark.createDataFrame([
    (4, "spark i j k"),
    (5, "l m n"),
    (6, "spark hadoop spark"),
    (7, "apache hadoop")
], ["id", "text"])

# Make predictions on test documents and print columns of interest.
prediction = model.transform(test)
selected = prediction.select("id", "text", "probability", "prediction")
for row in selected.collect():
    rid, text, prob, prediction = row
    print("(%d, %s) --> prob=%s, prediction=%f" % (rid, text, str(prob), prediction))
```
{% endtab %}
{% endtabs %}

### Model selection \(hyperparameter tuning\)

A big benefit of using ML Pipelines is hyperparameter optimization. See the [ML Tuning Guide](model-selection-and-tuning.md) for more information on automatic model selection.

