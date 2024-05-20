# Ensembles - RDD-based API

* Gradient-Boosted Trees vs. Random Forests
* Random Forests
  * Basic algorithm
    * Training
    * Prediction
  * Usage tips
  * Examples
    * Classification
    * Regression
* Gradient-Boosted Trees (GBTs)
  * Basic algorithm
    * Losses
  * Usage tips
    * Validation while training
  * Examples
    * Classification
    * Regression

An ensemble method is a learning algorithm which creates a model composed of a set of other base models. `spark.mllib` supports two major ensemble algorithms: `GradientBoostedTrees` and `RandomForest`. Both use decision trees as their base models.

### Gradient-Boosted Trees vs. Random Forests

Both Gradient-Boosted Trees (GBTs) and Random Forests are algorithms for learning ensembles of trees, but the training processes are different. There are several practical trade-offs:

* GBTs train one tree at a time, so they can take longer to train than random forests. Random Forests can train multiple trees in parallel.
  * On the other hand, it is often reasonable to use smaller (shallower) trees with GBTs than with Random Forests, and training smaller trees takes less time.
* Random Forests can be less prone to overfitting. Training more trees in a Random Forest reduces the likelihood of overfitting, but training more trees with GBTs increases the likelihood of overfitting. (In statistical language, Random Forests reduce variance by using more trees, whereas GBTs reduce bias by using more trees.)
* Random Forests can be easier to tune since performance improves monotonically with the number of trees (whereas performance can start to decrease for GBTs if the number of trees grows too large).

In short, both algorithms can be effective, and the choice should be based on the particular dataset.

### Random Forests

[Random forests](http://en.wikipedia.org/wiki/Random\_forest) are ensembles of [decision trees](https://spark.apache.org/docs/latest/mllib-decision-tree.html). Random forests are one of the most successful machine learning models for classification and regression. They combine many decision trees in order to reduce the risk of overfitting. Like decision trees, random forests handle categorical features, extend to the multiclass classification setting, do not require feature scaling, and are able to capture non-linearities and feature interactions.

`spark.mllib` supports random forests for binary and multiclass classification and for regression, using both continuous and categorical features. `spark.mllib` implements random forests using the existing [decision tree](https://spark.apache.org/docs/latest/mllib-decision-tree.html) implementation. Please see the decision tree guide for more information on trees.

#### Basic algorithm

Random forests train a set of decision trees separately, so the training can be done in parallel. The algorithm injects randomness into the training process so that each decision tree is a bit different. Combining the predictions from each tree reduces the variance of the predictions, improving the performance on test data.

**Training**

The randomness injected into the training process includes:

* Subsampling the original dataset on each iteration to get a different training set (a.k.a. bootstrapping).
* Considering different random subsets of features to split on at each tree node.

Apart from these randomizations, decision tree training is done in the same way as for individual decision trees.

**Prediction**

To make a prediction on a new instance, a random forest must aggregate the predictions from its set of decision trees. This aggregation is done differently for classification and regression.

_Classification_: Majority vote. Each tree’s prediction is counted as a vote for one class. The label is predicted to be the class which receives the most votes.

_Regression_: Averaging. Each tree predicts a real value. The label is predicted to be the average of the tree predictions.

#### Usage tips

We include a few guidelines for using random forests by discussing the various parameters. We omit some decision tree parameters since those are covered in the [decision tree guide](https://spark.apache.org/docs/latest/mllib-decision-tree.html).

The first two parameters we mention are the most important, and tuning them can often improve performance:

* **`numTrees`**: Number of trees in the forest.
  * Increasing the number of trees will decrease the variance in predictions, improving the model’s test-time accuracy.
  * Training time increases roughly linearly in the number of trees.
* **`maxDepth`**: Maximum depth of each tree in the forest.
  * Increasing the depth makes the model more expressive and powerful. However, deep trees take longer to train and are also more prone to overfitting.
  * In general, it is acceptable to train deeper trees when using random forests than when using a single decision tree. One tree is more likely to overfit than a random forest (because of the variance reduction from averaging multiple trees in the forest).

The next two parameters generally do not require tuning. However, they can be tuned to speed up training.

* **`subsamplingRate`**: This parameter specifies the size of the dataset used for training each tree in the forest, as a fraction of the size of the original dataset. The default (1.0) is recommended, but decreasing this fraction can speed up training.
* **`featureSubsetStrategy`**: Number of features to use as candidates for splitting at each tree node. The number is specified as a fraction or function of the total number of features. Decreasing this number will speed up training, but can sometimes impact performance if too low.

#### Examples

**Classification**

The example below demonstrates how to load a [LIBSVM data file](http://www.csie.ntu.edu.tw/\~cjlin/libsvmtools/datasets/), parse it as an RDD of `LabeledPoint` and then perform classification using a Random Forest. The test error is calculated to measure the algorithm accuracy.

* Python

Refer to the `RandomForest` Python docs and `RandomForest` Python docs for more details on the API.

```
from pyspark.mllib.tree import RandomForest, RandomForestModel
from pyspark.mllib.util import MLUtils

# Load and parse the data file into an RDD of LabeledPoint.
data = MLUtils.loadLibSVMFile(sc, 'data/mllib/sample_libsvm_data.txt')
# Split the data into training and test sets (30% held out for testing)
(trainingData, testData) = data.randomSplit([0.7, 0.3])

# Train a RandomForest model.
#  Empty categoricalFeaturesInfo indicates all features are continuous.
#  Note: Use larger numTrees in practice.
#  Setting featureSubsetStrategy="auto" lets the algorithm choose.
model = RandomForest.trainClassifier(trainingData, numClasses=2, categoricalFeaturesInfo={},
                                     numTrees=3, featureSubsetStrategy="auto",
                                     impurity='gini', maxDepth=4, maxBins=32)

# Evaluate model on test instances and compute test error
predictions = model.predict(testData.map(lambda x: x.features))
labelsAndPredictions = testData.map(lambda lp: lp.label).zip(predictions)
testErr = labelsAndPredictions.filter(
    lambda lp: lp[0] != lp[1]).count() / float(testData.count())
print('Test Error = ' + str(testErr))
print('Learned classification forest model:')
print(model.toDebugString())

# Save and load model
model.save(sc, "target/tmp/myRandomForestClassificationModel")
sameModel = RandomForestModel.load(sc, "target/tmp/myRandomForestClassificationModel")
```

Find full example code at "examples/src/main/python/mllib/random\_forest\_classification\_example.py" in the Spark repo.

* Scala

Refer to the `RandomForest` Scala docs and `RandomForestModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.tree.RandomForest
import org.apache.spark.mllib.tree.model.RandomForestModel
import org.apache.spark.mllib.util.MLUtils

// Load and parse the data file.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
// Split the data into training and test sets (30% held out for testing)
val splits = data.randomSplit(Array(0.7, 0.3))
val (trainingData, testData) = (splits(0), splits(1))

// Train a RandomForest model.
// Empty categoricalFeaturesInfo indicates all features are continuous.
val numClasses = 2
val categoricalFeaturesInfo = Map[Int, Int]()
val numTrees = 3 // Use more in practice.
val featureSubsetStrategy = "auto" // Let the algorithm choose.
val impurity = "gini"
val maxDepth = 4
val maxBins = 32

val model = RandomForest.trainClassifier(trainingData, numClasses, categoricalFeaturesInfo,
  numTrees, featureSubsetStrategy, impurity, maxDepth, maxBins)

// Evaluate model on test instances and compute test error
val labelAndPreds = testData.map { point =>
  val prediction = model.predict(point.features)
  (point.label, prediction)
}
val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / testData.count()
println(s"Test Error = $testErr")
println(s"Learned classification forest model:\n ${model.toDebugString}")

// Save and load model
model.save(sc, "target/tmp/myRandomForestClassificationModel")
val sameModel = RandomForestModel.load(sc, "target/tmp/myRandomForestClassificationModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/RandomForestClassificationExample.scala" in the Spark repo.

* Java

Refer to the `RandomForest` Java docs and `RandomForestModel` Java docs for details on the API.

```
import java.util.HashMap;
import java.util.Map;

import scala.Tuple2;

import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.tree.RandomForest;
import org.apache.spark.mllib.tree.model.RandomForestModel;
import org.apache.spark.mllib.util.MLUtils;

SparkConf sparkConf = new SparkConf().setAppName("JavaRandomForestClassificationExample");
JavaSparkContext jsc = new JavaSparkContext(sparkConf);
// Load and parse the data file.
String datapath = "data/mllib/sample_libsvm_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(jsc.sc(), datapath).toJavaRDD();
// Split the data into training and test sets (30% held out for testing)
JavaRDD<LabeledPoint>[] splits = data.randomSplit(new double[]{0.7, 0.3});
JavaRDD<LabeledPoint> trainingData = splits[0];
JavaRDD<LabeledPoint> testData = splits[1];

// Train a RandomForest model.
// Empty categoricalFeaturesInfo indicates all features are continuous.
int numClasses = 2;
Map<Integer, Integer> categoricalFeaturesInfo = new HashMap<>();
int numTrees = 3; // Use more in practice.
String featureSubsetStrategy = "auto"; // Let the algorithm choose.
String impurity = "gini";
int maxDepth = 5;
int maxBins = 32;
int seed = 12345;

RandomForestModel model = RandomForest.trainClassifier(trainingData, numClasses,
  categoricalFeaturesInfo, numTrees, featureSubsetStrategy, impurity, maxDepth, maxBins,
  seed);

// Evaluate model on test instances and compute test error
JavaPairRDD<Double, Double> predictionAndLabel =
  testData.mapToPair(p -> new Tuple2<>(model.predict(p.features()), p.label()));
double testErr =
  predictionAndLabel.filter(pl -> !pl._1().equals(pl._2())).count() / (double) testData.count();
System.out.println("Test Error: " + testErr);
System.out.println("Learned classification forest model:\n" + model.toDebugString());

// Save and load model
model.save(jsc.sc(), "target/tmp/myRandomForestClassificationModel");
RandomForestModel sameModel = RandomForestModel.load(jsc.sc(),
  "target/tmp/myRandomForestClassificationModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaRandomForestClassificationExample.java" in the Spark repo.

**Regression**

The example below demonstrates how to load a [LIBSVM data file](http://www.csie.ntu.edu.tw/\~cjlin/libsvmtools/datasets/), parse it as an RDD of `LabeledPoint` and then perform regression using a Random Forest. The Mean Squared Error (MSE) is computed at the end to evaluate [goodness of fit](http://en.wikipedia.org/wiki/Goodness\_of\_fit).

* Python

Refer to the `RandomForest` Python docs and `RandomForest` Python docs for more details on the API.

```
from pyspark.mllib.tree import RandomForest, RandomForestModel
from pyspark.mllib.util import MLUtils

# Load and parse the data file into an RDD of LabeledPoint.
data = MLUtils.loadLibSVMFile(sc, 'data/mllib/sample_libsvm_data.txt')
# Split the data into training and test sets (30% held out for testing)
(trainingData, testData) = data.randomSplit([0.7, 0.3])

# Train a RandomForest model.
#  Empty categoricalFeaturesInfo indicates all features are continuous.
#  Note: Use larger numTrees in practice.
#  Setting featureSubsetStrategy="auto" lets the algorithm choose.
model = RandomForest.trainRegressor(trainingData, categoricalFeaturesInfo={},
                                    numTrees=3, featureSubsetStrategy="auto",
                                    impurity='variance', maxDepth=4, maxBins=32)

# Evaluate model on test instances and compute test error
predictions = model.predict(testData.map(lambda x: x.features))
labelsAndPredictions = testData.map(lambda lp: lp.label).zip(predictions)
testMSE = labelsAndPredictions.map(lambda lp: (lp[0] - lp[1]) * (lp[0] - lp[1])).sum() /\
    float(testData.count())
print('Test Mean Squared Error = ' + str(testMSE))
print('Learned regression forest model:')
print(model.toDebugString())

# Save and load model
model.save(sc, "target/tmp/myRandomForestRegressionModel")
sameModel = RandomForestModel.load(sc, "target/tmp/myRandomForestRegressionModel")
```

Find full example code at "examples/src/main/python/mllib/random\_forest\_regression\_example.py" in the Spark repo.

* Scala

Refer to the `RandomForest` Scala docs and `RandomForestModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.tree.RandomForest
import org.apache.spark.mllib.tree.model.RandomForestModel
import org.apache.spark.mllib.util.MLUtils

// Load and parse the data file.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
// Split the data into training and test sets (30% held out for testing)
val splits = data.randomSplit(Array(0.7, 0.3))
val (trainingData, testData) = (splits(0), splits(1))

// Train a RandomForest model.
// Empty categoricalFeaturesInfo indicates all features are continuous.
val numClasses = 2
val categoricalFeaturesInfo = Map[Int, Int]()
val numTrees = 3 // Use more in practice.
val featureSubsetStrategy = "auto" // Let the algorithm choose.
val impurity = "variance"
val maxDepth = 4
val maxBins = 32

val model = RandomForest.trainRegressor(trainingData, categoricalFeaturesInfo,
  numTrees, featureSubsetStrategy, impurity, maxDepth, maxBins)

// Evaluate model on test instances and compute test error
val labelsAndPredictions = testData.map { point =>
  val prediction = model.predict(point.features)
  (point.label, prediction)
}
val testMSE = labelsAndPredictions.map{ case(v, p) => math.pow((v - p), 2)}.mean()
println(s"Test Mean Squared Error = $testMSE")
println(s"Learned regression forest model:\n ${model.toDebugString}")

// Save and load model
model.save(sc, "target/tmp/myRandomForestRegressionModel")
val sameModel = RandomForestModel.load(sc, "target/tmp/myRandomForestRegressionModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/RandomForestRegressionExample.scala" in the Spark repo.

* Java

Refer to the `RandomForest` Java docs and `RandomForestModel` Java docs for details on the API.

```
import java.util.HashMap;
import java.util.Map;

import scala.Tuple2;

import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.tree.RandomForest;
import org.apache.spark.mllib.tree.model.RandomForestModel;
import org.apache.spark.mllib.util.MLUtils;
import org.apache.spark.SparkConf;

SparkConf sparkConf = new SparkConf().setAppName("JavaRandomForestRegressionExample");
JavaSparkContext jsc = new JavaSparkContext(sparkConf);
// Load and parse the data file.
String datapath = "data/mllib/sample_libsvm_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(jsc.sc(), datapath).toJavaRDD();
// Split the data into training and test sets (30% held out for testing)
JavaRDD<LabeledPoint>[] splits = data.randomSplit(new double[]{0.7, 0.3});
JavaRDD<LabeledPoint> trainingData = splits[0];
JavaRDD<LabeledPoint> testData = splits[1];

// Set parameters.
// Empty categoricalFeaturesInfo indicates all features are continuous.
Map<Integer, Integer> categoricalFeaturesInfo = new HashMap<>();
int numTrees = 3; // Use more in practice.
String featureSubsetStrategy = "auto"; // Let the algorithm choose.
String impurity = "variance";
int maxDepth = 4;
int maxBins = 32;
int seed = 12345;
// Train a RandomForest model.
RandomForestModel model = RandomForest.trainRegressor(trainingData,
  categoricalFeaturesInfo, numTrees, featureSubsetStrategy, impurity, maxDepth, maxBins, seed);

// Evaluate model on test instances and compute test error
JavaPairRDD<Double, Double> predictionAndLabel =
  testData.mapToPair(p -> new Tuple2<>(model.predict(p.features()), p.label()));
double testMSE = predictionAndLabel.mapToDouble(pl -> {
  double diff = pl._1() - pl._2();
  return diff * diff;
}).mean();
System.out.println("Test Mean Squared Error: " + testMSE);
System.out.println("Learned regression forest model:\n" + model.toDebugString());

// Save and load model
model.save(jsc.sc(), "target/tmp/myRandomForestRegressionModel");
RandomForestModel sameModel = RandomForestModel.load(jsc.sc(),
  "target/tmp/myRandomForestRegressionModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaRandomForestRegressionExample.java" in the Spark repo.

### Gradient-Boosted Trees (GBTs)

[Gradient-Boosted Trees (GBTs)](http://en.wikipedia.org/wiki/Gradient\_boosting) are ensembles of decision trees. GBTs iteratively train decision trees in order to minimize a loss function. Like decision trees, GBTs handle categorical features, extend to the multiclass classification setting, do not require feature scaling, and are able to capture non-linearities and feature interactions.

`spark.mllib` supports GBTs for binary classification and for regression, using both continuous and categorical features. `spark.mllib` implements GBTs using the existing decision tree implementation. Please see the decision tree guide for more information on trees.

_Note_: GBTs do not yet support multiclass classification. For multiclass problems, please use decision trees or Random Forests.

#### Basic algorithm

Gradient boosting iteratively trains a sequence of decision trees. On each iteration, the algorithm uses the current ensemble to predict the label of each training instance and then compares the prediction with the true label. The dataset is re-labeled to put more emphasis on training instances with poor predictions. Thus, in the next iteration, the decision tree will help correct for previous mistakes.

The specific mechanism for re-labeling instances is defined by a loss function (discussed below). With each iteration, GBTs further reduce this loss function on the training data.

**Losses**

The table below lists the losses currently supported by GBTs in `spark.mllib`. Note that each loss is applicable to one of classification or regression, not both.

Notation: $$N$$ = number of instances. $$yi$$ = label of instance $$i$$. $$xi$$ = features of instance $$i$$. $$F(xi)$$ = model’s predicted label for instance $$i$$.

| Loss           | Task           | Formula                         | Description                                                             |
| -------------- | -------------- | ------------------------------- | ----------------------------------------------------------------------- |
| Log Loss       | Classification | $$2∑Ni=1log(1+exp(−2yiF(xi)))$$ | Twice binomial negative log likelihood.                                 |
| Squared Error  | Regression     | $$∑Ni=1(yi−F(xi))2$$            | Also called L2 loss. Default loss for regression tasks.                 |
| Absolute Error | Regression     | $$∑Ni=1|yi−F(xi)|$$             | Also called L1 loss. Can be more robust to outliers than Squared Error. |

#### Usage tips

We include a few guidelines for using GBTs by discussing the various parameters. We omit some decision tree parameters since those are covered in the [decision tree guide](https://spark.apache.org/docs/latest/mllib-decision-tree.html).

* **`loss`**: See the section above for information on losses and their applicability to tasks (classification vs. regression). Different losses can give significantly different results, depending on the dataset.
* **`numIterations`**: This sets the number of trees in the ensemble. Each iteration produces one tree. Increasing this number makes the model more expressive, improving training data accuracy. However, test-time accuracy may suffer if this is too large.
* **`learningRate`**: This parameter should not need to be tuned. If the algorithm behavior seems unstable, decreasing this value may improve stability.
* **`algo`**: The algorithm or task (classification vs. regression) is set using the tree \[Strategy] parameter.

**Validation while training**

Gradient boosting can overfit when trained with more trees. In order to prevent overfitting, it is useful to validate while training. The method runWithValidation has been provided to make use of this option. It takes a pair of RDD’s as arguments, the first one being the training dataset and the second being the validation dataset.

The training is stopped when the improvement in the validation error is not more than a certain tolerance (supplied by the `validationTol` argument in `BoostingStrategy`). In practice, the validation error decreases initially and later increases. There might be cases in which the validation error does not change monotonically, and the user is advised to set a large enough negative tolerance and examine the validation curve using `evaluateEachIteration` (which gives the error or loss per iteration) to tune the number of iterations.

#### Examples

**Classification**

The example below demonstrates how to load a [LIBSVM data file](http://www.csie.ntu.edu.tw/\~cjlin/libsvmtools/datasets/), parse it as an RDD of `LabeledPoint` and then perform classification using Gradient-Boosted Trees with log loss. The test error is calculated to measure the algorithm accuracy.

* Python

Refer to the `GradientBoostedTrees` Python docs and `GradientBoostedTreesModel` Python docs for more details on the API.

```
from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
from pyspark.mllib.util import MLUtils

# Load and parse the data file.
data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
# Split the data into training and test sets (30% held out for testing)
(trainingData, testData) = data.randomSplit([0.7, 0.3])

# Train a GradientBoostedTrees model.
#  Notes: (a) Empty categoricalFeaturesInfo indicates all features are continuous.
#         (b) Use more iterations in practice.
model = GradientBoostedTrees.trainClassifier(trainingData,
                                             categoricalFeaturesInfo={}, numIterations=3)

# Evaluate model on test instances and compute test error
predictions = model.predict(testData.map(lambda x: x.features))
labelsAndPredictions = testData.map(lambda lp: lp.label).zip(predictions)
testErr = labelsAndPredictions.filter(
    lambda lp: lp[0] != lp[1]).count() / float(testData.count())
print('Test Error = ' + str(testErr))
print('Learned classification GBT model:')
print(model.toDebugString())

# Save and load model
model.save(sc, "target/tmp/myGradientBoostingClassificationModel")
sameModel = GradientBoostedTreesModel.load(sc,
                                           "target/tmp/myGradientBoostingClassificationModel")
```

Find full example code at "examples/src/main/python/mllib/gradient\_boosting\_classification\_example.py" in the Spark repo.

* Scala

Refer to the `GradientBoostedTrees` Scala docs and `GradientBoostedTreesModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.tree.GradientBoostedTrees
import org.apache.spark.mllib.tree.configuration.BoostingStrategy
import org.apache.spark.mllib.tree.model.GradientBoostedTreesModel
import org.apache.spark.mllib.util.MLUtils

// Load and parse the data file.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
// Split the data into training and test sets (30% held out for testing)
val splits = data.randomSplit(Array(0.7, 0.3))
val (trainingData, testData) = (splits(0), splits(1))

// Train a GradientBoostedTrees model.
// The defaultParams for Classification use LogLoss by default.
val boostingStrategy = BoostingStrategy.defaultParams("Classification")
boostingStrategy.numIterations = 3 // Note: Use more iterations in practice.
boostingStrategy.treeStrategy.numClasses = 2
boostingStrategy.treeStrategy.maxDepth = 5
// Empty categoricalFeaturesInfo indicates all features are continuous.
boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]()

val model = GradientBoostedTrees.train(trainingData, boostingStrategy)

// Evaluate model on test instances and compute test error
val labelAndPreds = testData.map { point =>
  val prediction = model.predict(point.features)
  (point.label, prediction)
}
val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / testData.count()
println(s"Test Error = $testErr")
println(s"Learned classification GBT model:\n ${model.toDebugString}")

// Save and load model
model.save(sc, "target/tmp/myGradientBoostingClassificationModel")
val sameModel = GradientBoostedTreesModel.load(sc,
  "target/tmp/myGradientBoostingClassificationModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/GradientBoostingClassificationExample.scala" in the Spark repo.

* Java

Refer to the [`GradientBoostedTrees` Java docs](ensembles-rdd-based-api.md#gradient-boosted-trees-gbts) and [`GradientBoostedTreesModel` Java docs](https://spark.apache.org/docs/latest/api/java/org/apache/spark/mllib/tree/model/GradientBoostedTreesModel.html) for details on the API.

```
import java.util.HashMap;
import java.util.Map;

import scala.Tuple2;

import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.tree.GradientBoostedTrees;
import org.apache.spark.mllib.tree.configuration.BoostingStrategy;
import org.apache.spark.mllib.tree.model.GradientBoostedTreesModel;
import org.apache.spark.mllib.util.MLUtils;

SparkConf sparkConf = new SparkConf()
  .setAppName("JavaGradientBoostedTreesClassificationExample");
JavaSparkContext jsc = new JavaSparkContext(sparkConf);

// Load and parse the data file.
String datapath = "data/mllib/sample_libsvm_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(jsc.sc(), datapath).toJavaRDD();
// Split the data into training and test sets (30% held out for testing)
JavaRDD<LabeledPoint>[] splits = data.randomSplit(new double[]{0.7, 0.3});
JavaRDD<LabeledPoint> trainingData = splits[0];
JavaRDD<LabeledPoint> testData = splits[1];

// Train a GradientBoostedTrees model.
// The defaultParams for Classification use LogLoss by default.
BoostingStrategy boostingStrategy = BoostingStrategy.defaultParams("Classification");
boostingStrategy.setNumIterations(3); // Note: Use more iterations in practice.
boostingStrategy.getTreeStrategy().setNumClasses(2);
boostingStrategy.getTreeStrategy().setMaxDepth(5);
// Empty categoricalFeaturesInfo indicates all features are continuous.
Map<Integer, Integer> categoricalFeaturesInfo = new HashMap<>();
boostingStrategy.treeStrategy().setCategoricalFeaturesInfo(categoricalFeaturesInfo);

GradientBoostedTreesModel model = GradientBoostedTrees.train(trainingData, boostingStrategy);

// Evaluate model on test instances and compute test error
JavaPairRDD<Double, Double> predictionAndLabel =
  testData.mapToPair(p -> new Tuple2<>(model.predict(p.features()), p.label()));
double testErr =
  predictionAndLabel.filter(pl -> !pl._1().equals(pl._2())).count() / (double) testData.count();
System.out.println("Test Error: " + testErr);
System.out.println("Learned classification GBT model:\n" + model.toDebugString());

// Save and load model
model.save(jsc.sc(), "target/tmp/myGradientBoostingClassificationModel");
GradientBoostedTreesModel sameModel = GradientBoostedTreesModel.load(jsc.sc(),
  "target/tmp/myGradientBoostingClassificationModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaGradientBoostingClassificationExample.java" in the Spark repo.

**Regression**

The example below demonstrates how to load a [LIBSVM data file](http://www.csie.ntu.edu.tw/\~cjlin/libsvmtools/datasets/), parse it as an RDD of `LabeledPoint` and then perform regression using Gradient-Boosted Trees with Squared Error as the loss. The Mean Squared Error (MSE) is computed at the end to evaluate [goodness of fit](http://en.wikipedia.org/wiki/Goodness\_of\_fit).

* Python

Refer to the `GradientBoostedTrees` Python docs and `GradientBoostedTreesModel` Python docs for more details on the API.

```
from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
from pyspark.mllib.util import MLUtils

# Load and parse the data file.
data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
# Split the data into training and test sets (30% held out for testing)
(trainingData, testData) = data.randomSplit([0.7, 0.3])

# Train a GradientBoostedTrees model.
#  Notes: (a) Empty categoricalFeaturesInfo indicates all features are continuous.
#         (b) Use more iterations in practice.
model = GradientBoostedTrees.trainRegressor(trainingData,
                                            categoricalFeaturesInfo={}, numIterations=3)

# Evaluate model on test instances and compute test error
predictions = model.predict(testData.map(lambda x: x.features))
labelsAndPredictions = testData.map(lambda lp: lp.label).zip(predictions)
testMSE = labelsAndPredictions.map(lambda lp: (lp[0] - lp[1]) * (lp[0] - lp[1])).sum() /\
    float(testData.count())
print('Test Mean Squared Error = ' + str(testMSE))
print('Learned regression GBT model:')
print(model.toDebugString())

# Save and load model
model.save(sc, "target/tmp/myGradientBoostingRegressionModel")
sameModel = GradientBoostedTreesModel.load(sc, "target/tmp/myGradientBoostingRegressionModel")
```

Find full example code at "examples/src/main/python/mllib/gradient\_boosting\_regression\_example.py" in the Spark repo.

* Scala

Refer to the `GradientBoostedTrees` Scala docs and `GradientBoostedTreesModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.tree.GradientBoostedTrees
import org.apache.spark.mllib.tree.configuration.BoostingStrategy
import org.apache.spark.mllib.tree.model.GradientBoostedTreesModel
import org.apache.spark.mllib.util.MLUtils

// Load and parse the data file.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
// Split the data into training and test sets (30% held out for testing)
val splits = data.randomSplit(Array(0.7, 0.3))
val (trainingData, testData) = (splits(0), splits(1))

// Train a GradientBoostedTrees model.
// The defaultParams for Regression use SquaredError by default.
val boostingStrategy = BoostingStrategy.defaultParams("Regression")
boostingStrategy.numIterations = 3 // Note: Use more iterations in practice.
boostingStrategy.treeStrategy.maxDepth = 5
// Empty categoricalFeaturesInfo indicates all features are continuous.
boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]()

val model = GradientBoostedTrees.train(trainingData, boostingStrategy)

// Evaluate model on test instances and compute test error
val labelsAndPredictions = testData.map { point =>
  val prediction = model.predict(point.features)
  (point.label, prediction)
}
val testMSE = labelsAndPredictions.map{ case(v, p) => math.pow((v - p), 2)}.mean()
println(s"Test Mean Squared Error = $testMSE")
println(s"Learned regression GBT model:\n ${model.toDebugString}")

// Save and load model
model.save(sc, "target/tmp/myGradientBoostingRegressionModel")
val sameModel = GradientBoostedTreesModel.load(sc,
  "target/tmp/myGradientBoostingRegressionModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/GradientBoostingRegressionExample.scala" in the Spark repo.

* Java

Refer to the `GradientBoostedTrees` Java docs and `GradientBoostedTreesModel` Java docs for details on the API.

```
import java.util.HashMap;
import java.util.Map;

import scala.Tuple2;

import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.tree.GradientBoostedTrees;
import org.apache.spark.mllib.tree.configuration.BoostingStrategy;
import org.apache.spark.mllib.tree.model.GradientBoostedTreesModel;
import org.apache.spark.mllib.util.MLUtils;

SparkConf sparkConf = new SparkConf()
  .setAppName("JavaGradientBoostedTreesRegressionExample");
JavaSparkContext jsc = new JavaSparkContext(sparkConf);
// Load and parse the data file.
String datapath = "data/mllib/sample_libsvm_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(jsc.sc(), datapath).toJavaRDD();
// Split the data into training and test sets (30% held out for testing)
JavaRDD<LabeledPoint>[] splits = data.randomSplit(new double[]{0.7, 0.3});
JavaRDD<LabeledPoint> trainingData = splits[0];
JavaRDD<LabeledPoint> testData = splits[1];

// Train a GradientBoostedTrees model.
// The defaultParams for Regression use SquaredError by default.
BoostingStrategy boostingStrategy = BoostingStrategy.defaultParams("Regression");
boostingStrategy.setNumIterations(3); // Note: Use more iterations in practice.
boostingStrategy.getTreeStrategy().setMaxDepth(5);
// Empty categoricalFeaturesInfo indicates all features are continuous.
Map<Integer, Integer> categoricalFeaturesInfo = new HashMap<>();
boostingStrategy.treeStrategy().setCategoricalFeaturesInfo(categoricalFeaturesInfo);

GradientBoostedTreesModel model = GradientBoostedTrees.train(trainingData, boostingStrategy);

// Evaluate model on test instances and compute test error
JavaPairRDD<Double, Double> predictionAndLabel =
  testData.mapToPair(p -> new Tuple2<>(model.predict(p.features()), p.label()));
double testMSE = predictionAndLabel.mapToDouble(pl -> {
  double diff = pl._1() - pl._2();
  return diff * diff;
}).mean();
System.out.println("Test Mean Squared Error: " + testMSE);
System.out.println("Learned regression GBT model:\n" + model.toDebugString());

// Save and load model
model.save(jsc.sc(), "target/tmp/myGradientBoostingRegressionModel");
GradientBoostedTreesModel sameModel = GradientBoostedTreesModel.load(jsc.sc(),
  "target/tmp/myGradientBoostingRegressionModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaGradientBoostingRegressionExample.java" in the Spark repo.
