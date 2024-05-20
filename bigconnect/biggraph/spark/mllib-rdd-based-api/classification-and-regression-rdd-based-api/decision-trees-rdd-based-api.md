# Decision Trees - RDD-based API

* Basic algorithm
  * Node impurity and information gain
  * Split candidates
  * Stopping rule
* Usage tips
  * Problem specification parameters
  * Stopping criteria
  * Tunable parameters
  * Caching and checkpointing
* Scaling
* Examples
  * Classification
  * Regression

Decision trees and their ensembles are popular methods for the machine learning tasks of classification and regression. Decision trees are widely used since they are easy to interpret, handle categorical features, extend to the multiclass classification setting, do not require feature scaling, and are able to capture non-linearities and feature interactions. Tree ensemble algorithms such as random forests and boosting are among the top performers for classification and regression tasks.

`spark.mllib` supports decision trees for binary and multiclass classification and for regression, using both continuous and categorical features. The implementation partitions data by rows, allowing distributed training with millions of instances.

Ensembles of trees (Random Forests and Gradient-Boosted Trees) are described in the Ensembles guide.

### Basic algorithm

The decision tree is a greedy algorithm that performs a recursive binary partitioning of the feature space. The tree predicts the same label for each bottommost (leaf) partition. Each partition is chosen greedily by selecting the _best split_ from a set of possible splits, in order to maximize the information gain at a tree node. In other words, the split chosen at each tree node is chosen from the set $$argmaxsIG(D,s)$$ where $$IG(D,s)$$ is the information gain when a split $$s$$ is applied to a dataset $$D$$.

#### Node impurity and information gain

The _node impurity_ is a measure of the homogeneity of the labels at the node. The current implementation provides two impurity measures for classification (Gini impurity and entropy) and one impurity measure for regression (variance).

| Impurity      | Task           | Formula             | Description                                                                                                    |
| ------------- | -------------- | ------------------- | -------------------------------------------------------------------------------------------------------------- |
| Gini impurity | Classification | $$∑Ci=1fi(1−fi)$$   | $$fi$$ is the frequency of label $$i$$ at a node and $$C$$ is the number of unique labels.                     |
| Entropy       | Classification | $$∑Ci=1−filog(fi)$$ | $$fi$$ is the frequency of label $$i$$ at a node and $$C$$ is the number of unique labels.                     |
| Variance      | Regression     | $$1N∑Ni=1(yi−μ)2$$  | $$yi$$ is label for an instance, $$N$$ is the number of instances and $$μ$$ is the mean given by $$1N∑Ni=1yi$$ |

The _information gain_ is the difference between the parent node impurity and the weighted sum of the two child node impurities. Assuming that a split $$s$$ partitions the dataset $$D$$ of size $$N$$ into two datasets $$Dleft$$ and $$Dright$$ of sizes $$Nleft$$ and $$Nright$$, respectively, the information gain is:

$$IG(D,s)=Impurity(D)−NleftNImpurity(Dleft)−NrightNImpurity(Dright)$$

#### Split candidates

**Continuous features**

For small datasets in single-machine implementations, the split candidates for each continuous feature are typically the unique values for the feature. Some implementations sort the feature values and then use the ordered unique values as split candidates for faster tree calculations.

Sorting feature values is expensive for large distributed datasets. This implementation computes an approximate set of split candidates by performing a quantile calculation over a sampled fraction of the data. The ordered splits create “bins” and the maximum number of such bins can be specified using the `maxBins` parameter.

Note that the number of bins cannot be greater than the number of instances $$N$$ (a rare scenario since the default `maxBins` value is 32). The tree algorithm automatically reduces the number of bins if the condition is not satisfied.

**Categorical features**

For a categorical feature with $$M$$ possible values (categories), one could come up with $$2M−1−1$$ split candidates. For binary (0/1) classification and regression, we can reduce the number of split candidates to $$M−1$$ by ordering the categorical feature values by the average label. (See Section 9.2.4 in [Elements of Statistical Machine Learning](https://web.stanford.edu/\~hastie/ElemStatLearn/) for details.) For example, for a binary classification problem with one categorical feature with three categories A, B and C whose corresponding proportions of label 1 are 0.2, 0.6 and 0.4, the categorical features are ordered as A, C, B. The two split candidates are A | C, B and A , C | B where | denotes the split.

In multiclass classification, all $$2M−1−1$$ possible splits are used whenever possible. When $$2M−1−1$$ is greater than the `maxBins` parameter, we use a (heuristic) method similar to the method used for binary classification and regression. The $$M$$ categorical feature values are ordered by impurity, and the resulting $$M−1$$ split candidates are considered.

#### Stopping rule

The recursive tree construction is stopped at a node when one of the following conditions is met:

1. The node depth is equal to the `maxDepth` training parameter.
2. No split candidate leads to an information gain greater than `minInfoGain`.
3. No split candidate produces child nodes which each have at least `minInstancesPerNode` training instances.

### Usage tips

We include a few guidelines for using decision trees by discussing the various parameters. The parameters are listed below roughly in order of descending importance. New users should mainly consider the “Problem specification parameters” section and the `maxDepth` parameter.

#### Problem specification parameters

These parameters describe the problem you want to solve and your dataset. They should be specified and do not require tuning.

* **`algo`**: Type of decision tree, either `Classification` or `Regression`.
* **`numClasses`**: Number of classes (for `Classification` only).
* **`categoricalFeaturesInfo`**: Specifies which features are categorical and how many categorical values each of those features can take. This is given as a map from feature indices to feature arity (number of categories). Any features not in this map are treated as continuous.
  * For example, `Map(0 -> 2, 4 -> 10)` specifies that feature `0` is binary (taking values `0` or `1`) and that feature `4` has 10 categories (values `{0, 1, ..., 9}`). Note that feature indices are 0-based: features `0` and `4` are the 1st and 5th elements of an instance’s feature vector.
  * Note that you do not have to specify `categoricalFeaturesInfo`. The algorithm will still run and may get reasonable results. However, performance should be better if categorical features are properly designated.

#### Stopping criteria

These parameters determine when the tree stops building (adding new nodes). When tuning these parameters, be careful to validate on held-out test data to avoid overfitting.

* **`maxDepth`**: Maximum depth of a tree. Deeper trees are more expressive (potentially allowing higher accuracy), but they are also more costly to train and are more likely to overfit.
* **`minInstancesPerNode`**: For a node to be split further, each of its children must receive at least this number of training instances. This is commonly used with [RandomForest](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/mllib/tree/RandomForest$.html) since those are often trained deeper than individual trees.
* **`minInfoGain`**: For a node to be split further, the split must improve at least this much (in terms of information gain).

#### Tunable parameters

These parameters may be tuned. Be careful to validate on held-out test data when tuning in order to avoid overfitting.

* **`maxBins`**: Number of bins used when discretizing continuous features.
  * Increasing `maxBins` allows the algorithm to consider more split candidates and make fine-grained split decisions. However, it also increases computation and communication.
  * Note that the `maxBins` parameter must be at least the maximum number of categories $$M$$ for any categorical feature.
* **`maxMemoryInMB`**: Amount of memory to be used for collecting sufficient statistics.
  * The default value is conservatively chosen to be 256 MiB to allow the decision algorithm to work in most scenarios. Increasing `maxMemoryInMB` can lead to faster training (if the memory is available) by allowing fewer passes over the data. However, there may be decreasing returns as `maxMemoryInMB` grows since the amount of communication on each iteration can be proportional to `maxMemoryInMB`.
  * _Implementation details_: For faster processing, the decision tree algorithm collects statistics about groups of nodes to split (rather than 1 node at a time). The number of nodes which can be handled in one group is determined by the memory requirements (which vary per features). The `maxMemoryInMB` parameter specifies the memory limit in terms of megabytes which each worker can use for these statistics.
* **`subsamplingRate`**: Fraction of the training data used for learning the decision tree. This parameter is most relevant for training ensembles of trees (using `RandomForest` and `GradientBoostedTrees`), where it can be useful to subsample the original data. For training a single decision tree, this parameter is less useful since the number of training instances is generally not the main constraint.
* **`impurity`**: Impurity measure (discussed above) used to choose between candidate splits. This measure must match the `algo` parameter.

#### Caching and checkpointing

MLlib 1.2 adds several features for scaling up to larger (deeper) trees and tree ensembles. When `maxDepth` is set to be large, it can be useful to turn on node ID caching and checkpointing. These parameters are also useful for RandomForest when `numTrees` is set to be large.

* **`useNodeIdCache`**: If this is set to true, the algorithm will avoid passing the current model (tree or trees) to executors on each iteration.
  * This can be useful with deep trees (speeding up computation on workers) and for large Random Forests (reducing communication on each iteration).
  * _Implementation details_: By default, the algorithm communicates the current model to executors so that executors can match training instances with tree nodes. When this setting is turned on, then the algorithm will instead cache this information.

Node ID caching generates a sequence of RDDs (1 per iteration). This long lineage can cause performance problems, but checkpointing intermediate RDDs can alleviate those problems. Note that checkpointing is only applicable when `useNodeIdCache` is set to true.

* **`checkpointDir`**: Directory for checkpointing node ID cache RDDs.
* **`checkpointInterval`**: Frequency for checkpointing node ID cache RDDs. Setting this too low will cause extra overhead from writing to HDFS; setting this too high can cause problems if executors fail and the RDD needs to be recomputed.

### Scaling

Computation scales approximately linearly in the number of training instances, in the number of features, and in the `maxBins` parameter. Communication scales approximately linearly in the number of features and in `maxBins`.

The implemented algorithm reads both sparse and dense data. However, it is not optimized for sparse input.

### Examples

#### Classification

The example below demonstrates how to load a [LIBSVM data file](http://www.csie.ntu.edu.tw/\~cjlin/libsvmtools/datasets/), parse it as an RDD of `LabeledPoint` and then perform classification using a decision tree with Gini impurity as an impurity measure and a maximum tree depth of 5. The test error is calculated to measure the algorithm accuracy.

* Python

Refer to the `DecisionTree` Python docs and `DecisionTreeModel` Python docs for more details on the API.

```
from pyspark.mllib.tree import DecisionTree, DecisionTreeModel
from pyspark.mllib.util import MLUtils

# Load and parse the data file into an RDD of LabeledPoint.
data = MLUtils.loadLibSVMFile(sc, 'data/mllib/sample_libsvm_data.txt')
# Split the data into training and test sets (30% held out for testing)
(trainingData, testData) = data.randomSplit([0.7, 0.3])

# Train a DecisionTree model.
#  Empty categoricalFeaturesInfo indicates all features are continuous.
model = DecisionTree.trainClassifier(trainingData, numClasses=2, categoricalFeaturesInfo={},
                                     impurity='gini', maxDepth=5, maxBins=32)

# Evaluate model on test instances and compute test error
predictions = model.predict(testData.map(lambda x: x.features))
labelsAndPredictions = testData.map(lambda lp: lp.label).zip(predictions)
testErr = labelsAndPredictions.filter(
    lambda lp: lp[0] != lp[1]).count() / float(testData.count())
print('Test Error = ' + str(testErr))
print('Learned classification tree model:')
print(model.toDebugString())

# Save and load model
model.save(sc, "target/tmp/myDecisionTreeClassificationModel")
sameModel = DecisionTreeModel.load(sc, "target/tmp/myDecisionTreeClassificationModel")
```

Find full example code at "examples/src/main/python/mllib/decision\_tree\_classification\_example.py" in the Spark repo.

* Scala

Refer to the `DecisionTree` Scala docs and `DecisionTreeModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.tree.DecisionTree
import org.apache.spark.mllib.tree.model.DecisionTreeModel
import org.apache.spark.mllib.util.MLUtils

// Load and parse the data file.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
// Split the data into training and test sets (30% held out for testing)
val splits = data.randomSplit(Array(0.7, 0.3))
val (trainingData, testData) = (splits(0), splits(1))

// Train a DecisionTree model.
//  Empty categoricalFeaturesInfo indicates all features are continuous.
val numClasses = 2
val categoricalFeaturesInfo = Map[Int, Int]()
val impurity = "gini"
val maxDepth = 5
val maxBins = 32

val model = DecisionTree.trainClassifier(trainingData, numClasses, categoricalFeaturesInfo,
  impurity, maxDepth, maxBins)

// Evaluate model on test instances and compute test error
val labelAndPreds = testData.map { point =>
  val prediction = model.predict(point.features)
  (point.label, prediction)
}
val testErr = labelAndPreds.filter(r => r._1 != r._2).count().toDouble / testData.count()
println(s"Test Error = $testErr")
println(s"Learned classification tree model:\n ${model.toDebugString}")

// Save and load model
model.save(sc, "target/tmp/myDecisionTreeClassificationModel")
val sameModel = DecisionTreeModel.load(sc, "target/tmp/myDecisionTreeClassificationModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/DecisionTreeClassificationExample.scala" in the Spark repo.

* Java

Refer to the `DecisionTree` Java docs and `DecisionTreeModel` Java docs for details on the API.

```
import java.util.HashMap;
import java.util.Map;

import scala.Tuple2;

import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.tree.DecisionTree;
import org.apache.spark.mllib.tree.model.DecisionTreeModel;
import org.apache.spark.mllib.util.MLUtils;

SparkConf sparkConf = new SparkConf().setAppName("JavaDecisionTreeClassificationExample");
JavaSparkContext jsc = new JavaSparkContext(sparkConf);

// Load and parse the data file.
String datapath = "data/mllib/sample_libsvm_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(jsc.sc(), datapath).toJavaRDD();
// Split the data into training and test sets (30% held out for testing)
JavaRDD<LabeledPoint>[] splits = data.randomSplit(new double[]{0.7, 0.3});
JavaRDD<LabeledPoint> trainingData = splits[0];
JavaRDD<LabeledPoint> testData = splits[1];

// Set parameters.
//  Empty categoricalFeaturesInfo indicates all features are continuous.
int numClasses = 2;
Map<Integer, Integer> categoricalFeaturesInfo = new HashMap<>();
String impurity = "gini";
int maxDepth = 5;
int maxBins = 32;

// Train a DecisionTree model for classification.
DecisionTreeModel model = DecisionTree.trainClassifier(trainingData, numClasses,
  categoricalFeaturesInfo, impurity, maxDepth, maxBins);

// Evaluate model on test instances and compute test error
JavaPairRDD<Double, Double> predictionAndLabel =
  testData.mapToPair(p -> new Tuple2<>(model.predict(p.features()), p.label()));
double testErr =
  predictionAndLabel.filter(pl -> !pl._1().equals(pl._2())).count() / (double) testData.count();

System.out.println("Test Error: " + testErr);
System.out.println("Learned classification tree model:\n" + model.toDebugString());

// Save and load model
model.save(jsc.sc(), "target/tmp/myDecisionTreeClassificationModel");
DecisionTreeModel sameModel = DecisionTreeModel
  .load(jsc.sc(), "target/tmp/myDecisionTreeClassificationModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaDecisionTreeClassificationExample.java" in the Spark repo.

#### Regression

The example below demonstrates how to load a LIBSVM data file, parse it as an RDD of `LabeledPoint` and then perform regression using a decision tree with variance as an impurity measure and a maximum tree depth of 5. The Mean Squared Error (MSE) is computed at the end to evaluate goodness of fit.

* Python

Refer to the `DecisionTree` Python docs and `DecisionTreeModel` Python docs for more details on the API.

```
from pyspark.mllib.tree import DecisionTree, DecisionTreeModel
from pyspark.mllib.util import MLUtils

# Load and parse the data file into an RDD of LabeledPoint.
data = MLUtils.loadLibSVMFile(sc, 'data/mllib/sample_libsvm_data.txt')
# Split the data into training and test sets (30% held out for testing)
(trainingData, testData) = data.randomSplit([0.7, 0.3])

# Train a DecisionTree model.
#  Empty categoricalFeaturesInfo indicates all features are continuous.
model = DecisionTree.trainRegressor(trainingData, categoricalFeaturesInfo={},
                                    impurity='variance', maxDepth=5, maxBins=32)

# Evaluate model on test instances and compute test error
predictions = model.predict(testData.map(lambda x: x.features))
labelsAndPredictions = testData.map(lambda lp: lp.label).zip(predictions)
testMSE = labelsAndPredictions.map(lambda lp: (lp[0] - lp[1]) * (lp[0] - lp[1])).sum() /\
    float(testData.count())
print('Test Mean Squared Error = ' + str(testMSE))
print('Learned regression tree model:')
print(model.toDebugString())

# Save and load model
model.save(sc, "target/tmp/myDecisionTreeRegressionModel")
sameModel = DecisionTreeModel.load(sc, "target/tmp/myDecisionTreeRegressionModel")
```

Find full example code at "examples/src/main/python/mllib/decision\_tree\_regression\_example.py" in the Spark repo.

* Scala

Refer to the `DecisionTree` Scala docs and `DecisionTreeModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.tree.DecisionTree
import org.apache.spark.mllib.tree.model.DecisionTreeModel
import org.apache.spark.mllib.util.MLUtils

// Load and parse the data file.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
// Split the data into training and test sets (30% held out for testing)
val splits = data.randomSplit(Array(0.7, 0.3))
val (trainingData, testData) = (splits(0), splits(1))

// Train a DecisionTree model.
//  Empty categoricalFeaturesInfo indicates all features are continuous.
val categoricalFeaturesInfo = Map[Int, Int]()
val impurity = "variance"
val maxDepth = 5
val maxBins = 32

val model = DecisionTree.trainRegressor(trainingData, categoricalFeaturesInfo, impurity,
  maxDepth, maxBins)

// Evaluate model on test instances and compute test error
val labelsAndPredictions = testData.map { point =>
  val prediction = model.predict(point.features)
  (point.label, prediction)
}
val testMSE = labelsAndPredictions.map{ case (v, p) => math.pow(v - p, 2) }.mean()
println(s"Test Mean Squared Error = $testMSE")
println(s"Learned regression tree model:\n ${model.toDebugString}")

// Save and load model
model.save(sc, "target/tmp/myDecisionTreeRegressionModel")
val sameModel = DecisionTreeModel.load(sc, "target/tmp/myDecisionTreeRegressionModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/DecisionTreeRegressionExample.scala" in the Spark repo.

* Java

Refer to the `DecisionTree` Java docs and `DecisionTreeModel` Java docs for details on the API.

```
import java.util.HashMap;
import java.util.Map;

import scala.Tuple2;

import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.tree.DecisionTree;
import org.apache.spark.mllib.tree.model.DecisionTreeModel;
import org.apache.spark.mllib.util.MLUtils;

SparkConf sparkConf = new SparkConf().setAppName("JavaDecisionTreeRegressionExample");
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
String impurity = "variance";
int maxDepth = 5;
int maxBins = 32;

// Train a DecisionTree model.
DecisionTreeModel model = DecisionTree.trainRegressor(trainingData,
  categoricalFeaturesInfo, impurity, maxDepth, maxBins);

// Evaluate model on test instances and compute test error
JavaPairRDD<Double, Double> predictionAndLabel =
  testData.mapToPair(p -> new Tuple2<>(model.predict(p.features()), p.label()));
double testMSE = predictionAndLabel.mapToDouble(pl -> {
  double diff = pl._1() - pl._2();
  return diff * diff;
}).mean();
System.out.println("Test Mean Squared Error: " + testMSE);
System.out.println("Learned regression tree model:\n" + model.toDebugString());

// Save and load model
model.save(jsc.sc(), "target/tmp/myDecisionTreeRegressionModel");
DecisionTreeModel sameModel = DecisionTreeModel
  .load(jsc.sc(), "target/tmp/myDecisionTreeRegressionModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaDecisionTreeRegressionExample.java" in the Spark repo.
