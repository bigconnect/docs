# Linear Methods - RDD-based API

* Mathematical formulation
  * Loss functions
  * Regularizers
  * Optimization
* Classification
  * Linear Support Vector Machines (SVMs)
  * Logistic regression
* Regression
  * Linear least squares, Lasso, and ridge regression
  * Streaming linear regression
* Implementation (developer)

### Mathematical formulation

Many standard _machine learning_ methods can be formulated as a convex optimization problem, i.e. the task of finding a minimizer of a convex function $$f$$ that depends on a variable vector $$w$$ (called `weights` in the code), which has $$d$$ entries. Formally, we can write this as the optimization problem $$minw∈Rdf(w)$$, where the objective function is of the form

$$f(w):=λR(w)+1n∑i=1nL(w;xi,yi) .(1)$$`.`

Here the vectors $$xi∈Rd$$ are the training data examples, for $$1≤i≤n$$, and $$yi∈R$$ are their corresponding labels, which we want to predict. We call the method _linear_ if $$L(w;x,y)$$ can be expressed as a function of $$wTx$$ and $$y$$. Several of `spark.mllib`’s classification and regression algorithms fall into this category, and are discussed here.

The objective function $$f$$ has two parts: the regularizer that controls the complexity of the model, and the loss that measures the error of the model on the training data. The loss function $$L(w;.)$$ is typically a convex function in $$w$$. The fixed regularization parameter $$λ≥0$$ (`regParam` in the code) defines the trade-off between the two goals of minimizing the loss (i.e., training error) and minimizing model complexity (i.e., to avoid overfitting).

#### Loss functions

The following table summarizes the loss functions and their gradients or sub-gradients for the methods `spark.mllib` supports:

|               | loss function $$L(w;x,y)$$      | gradient or sub-gradient       |
| ------------- | ------------------------------- | ------------------------------ |
| hinge loss    | $$max{0,1−ywTx},y∈{−1,+1}$$     | $${−y⋅x0if ywTx<1,otherwise.$$ |
| logistic loss | $$log(1+exp(−ywTx)),y∈{−1,+1}$$ | $$−y(1−11+exp(−ywTx))⋅x$$      |
| squared loss  | $$12(wTx−y)2,y∈R$$              | $$(wTx−y)⋅x$$                  |

Note that, in the mathematical formulation above, a binary label $$y$$ is denoted as either $$+1$$+1 (positive) or $$−1$$−1 (negative), which is convenient for the formulation. _However_, the negative label is represented by $$0$$0 in `spark.mllib` instead of $$−1$$−1, to be consistent with multiclass labeling.

#### Regularizers

The purpose of the [regularizer](http://en.wikipedia.org/wiki/Regularization\_\(mathematics\)) is to encourage simple models and avoid overfitting. We support the following regularizers in `spark.mllib`:

|                      | regularizer $$R(w)$$   | gradient or sub-gradient |
| -------------------- | ---------------------- | ------------------------ |
| zero (unregularized) | 0                      | $$0$$0                   |
| L2                   | $$12∥w∥22$$12          | $$w$$                    |
| L1                   | $$∥w∥1$$‖              | $$sign(w)$$              |
| elastic net          | $$α∥w∥1+(1−α)12∥w∥22$$ | $$αsign(w)+(1−α)w$$      |

Here $$sign(w)$$is the vector consisting of the signs ($$±1$$`±1`) of all the entries of $$w$$.

L2-regularized problems are generally easier to solve than L1-regularized due to smoothness. However, L1 regularization can help promote sparsity in weights leading to smaller and more interpretable models, the latter of which can be useful for feature selection. [Elastic net](http://en.wikipedia.org/wiki/Elastic\_net\_regularization) is a combination of L1 and L2 regularization. It is not recommended to train models without any regularization, especially when the number of training examples is small.

#### Optimization

Under the hood, linear methods use convex optimization methods to optimize the objective functions. `spark.mllib` uses two methods, SGD and L-BFGS, described in the optimization section. Currently, most algorithm APIs support Stochastic Gradient Descent (SGD), and a few support L-BFGS. Refer to this optimization section for guidelines on choosing between optimization methods.

### Classification

Classification aims to divide items into categories. The most common classification type is binary classification, where there are two categories, usually named positive and negative. If there are more than two categories, it is called multiclass classification. `spark.mllib` supports two linear methods for classification: linear Support Vector Machines (SVMs) and logistic regression. Linear SVMs supports only binary classification, while logistic regression supports both binary and multiclass classification problems. For both methods, `spark.mllib` supports L1 and L2 regularized variants. The training data set is represented by an RDD of LabeledPoint in MLlib, where labels are class indices starting from zero: $$0,1,2,…$$0,1,2,….

#### Linear Support Vector Machines (SVMs)

The [linear SVM](http://en.wikipedia.org/wiki/Support\_vector\_machine#Linear\_SVM) is a standard method for large-scale classification tasks. It is a linear method as described above in equation $$(1)$$`(1)`, with the loss function in the formulation given by the hinge loss:

$$L(w;x,y):=max{0,1−ywTx}.$$

By default, linear SVMs are trained with an L2 regularization. We also support alternative L1 regularization. In this case, the problem becomes a [linear program](http://en.wikipedia.org/wiki/Linear\_programming).

The linear SVMs algorithm outputs an SVM model. Given a new data point, denoted by $$x$$, the model makes predictions based on the value of $$wTx$$. By the default, if $$wTx≥0$$ then the outcome is positive, and negative otherwise.

**Examples**

* Python

The following example shows how to load a sample dataset, build SVM model, and make predictions with the resulting model to compute the training error.

Refer to the `SVMWithSGD` Python docs and `SVMModel` Python docs for more details on the API.

```
from pyspark.mllib.classification import SVMWithSGD, SVMModel
from pyspark.mllib.regression import LabeledPoint

# Load and parse the data
def parsePoint(line):
    values = [float(x) for x in line.split(' ')]
    return LabeledPoint(values[0], values[1:])

data = sc.textFile("data/mllib/sample_svm_data.txt")
parsedData = data.map(parsePoint)

# Build the model
model = SVMWithSGD.train(parsedData, iterations=100)

# Evaluating the model on training data
labelsAndPreds = parsedData.map(lambda p: (p.label, model.predict(p.features)))
trainErr = labelsAndPreds.filter(lambda lp: lp[0] != lp[1]).count() / float(parsedData.count())
print("Training Error = " + str(trainErr))

# Save and load model
model.save(sc, "target/tmp/pythonSVMWithSGDModel")
sameModel = SVMModel.load(sc, "target/tmp/pythonSVMWithSGDModel")
```

Find full example code at "examples/src/main/python/mllib/svm\_with\_sgd\_example.py" in the Spark repo.

* Scala

The following code snippet illustrates how to load a sample dataset, execute a training algorithm on this training data using a static method in the algorithm object, and make predictions with the resulting model to compute the training error.

Refer to the `SVMWithSGD` Scala docs and `SVMModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.classification.{SVMModel, SVMWithSGD}
import org.apache.spark.mllib.evaluation.BinaryClassificationMetrics
import org.apache.spark.mllib.util.MLUtils

// Load training data in LIBSVM format.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")

// Split data into training (60%) and test (40%).
val splits = data.randomSplit(Array(0.6, 0.4), seed = 11L)
val training = splits(0).cache()
val test = splits(1)

// Run training algorithm to build the model
val numIterations = 100
val model = SVMWithSGD.train(training, numIterations)

// Clear the default threshold.
model.clearThreshold()

// Compute raw scores on the test set.
val scoreAndLabels = test.map { point =>
  val score = model.predict(point.features)
  (score, point.label)
}

// Get evaluation metrics.
val metrics = new BinaryClassificationMetrics(scoreAndLabels)
val auROC = metrics.areaUnderROC()

println(s"Area under ROC = $auROC")

// Save and load model
model.save(sc, "target/tmp/scalaSVMWithSGDModel")
val sameModel = SVMModel.load(sc, "target/tmp/scalaSVMWithSGDModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/SVMWithSGDExample.scala" in the Spark repo.

The `SVMWithSGD.train()` method by default performs L2 regularization with the regularization parameter set to 1.0. If we want to configure this algorithm, we can customize `SVMWithSGD` further by creating a new object directly and calling setter methods. All other `spark.mllib` algorithms support customization in this way as well. For example, the following code produces an L1 regularized variant of SVMs with regularization parameter set to 0.1, and runs the training algorithm for 200 iterations.

```scala
import org.apache.spark.mllib.optimization.L1Updater

val svmAlg = new SVMWithSGD()
svmAlg.optimizer
  .setNumIterations(200)
  .setRegParam(0.1)
  .setUpdater(new L1Updater)
val modelL1 = svmAlg.run(training)
```

* Java

All of MLlib’s methods use Java-friendly types, so you can import and call them there the same way you do in Scala. The only caveat is that the methods take Scala RDD objects, while the Spark Java API uses a separate `JavaRDD` class. You can convert a Java RDD to a Scala one by calling `.rdd()` on your `JavaRDD` object. A self-contained application example that is equivalent to the provided example in Scala is given below:

Refer to the `SVMWithSGD` Java docs and `SVMModel` Java docs for details on the API.

```
import scala.Tuple2;

import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.classification.SVMModel;
import org.apache.spark.mllib.classification.SVMWithSGD;
import org.apache.spark.mllib.evaluation.BinaryClassificationMetrics;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.util.MLUtils;

String path = "data/mllib/sample_libsvm_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(sc, path).toJavaRDD();

// Split initial RDD into two... [60% training data, 40% testing data].
JavaRDD<LabeledPoint> training = data.sample(false, 0.6, 11L);
training.cache();
JavaRDD<LabeledPoint> test = data.subtract(training);

// Run training algorithm to build the model.
int numIterations = 100;
SVMModel model = SVMWithSGD.train(training.rdd(), numIterations);

// Clear the default threshold.
model.clearThreshold();

// Compute raw scores on the test set.
JavaRDD<Tuple2<Object, Object>> scoreAndLabels = test.map(p ->
  new Tuple2<>(model.predict(p.features()), p.label()));

// Get evaluation metrics.
BinaryClassificationMetrics metrics =
  new BinaryClassificationMetrics(JavaRDD.toRDD(scoreAndLabels));
double auROC = metrics.areaUnderROC();

System.out.println("Area under ROC = " + auROC);

// Save and load model
model.save(sc, "target/tmp/javaSVMWithSGDModel");
SVMModel sameModel = SVMModel.load(sc, "target/tmp/javaSVMWithSGDModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaSVMWithSGDExample.java" in the Spark repo.

The `SVMWithSGD.train()` method by default performs L2 regularization with the regularization parameter set to 1.0. If we want to configure this algorithm, we can customize `SVMWithSGD` further by creating a new object directly and calling setter methods. All other `spark.mllib` algorithms support customization in this way as well. For example, the following code produces an L1 regularized variant of SVMs with regularization parameter set to 0.1, and runs the training algorithm for 200 iterations.

```java
import org.apache.spark.mllib.optimization.L1Updater;

SVMWithSGD svmAlg = new SVMWithSGD();
svmAlg.optimizer()
  .setNumIterations(200)
  .setRegParam(0.1)
  .setUpdater(new L1Updater());
SVMModel modelL1 = svmAlg.run(training.rdd());
```

In order to run the above application, follow the instructions provided in the Self-Contained Applications section of the Spark quick-start guide. Be sure to also include _spark-mllib_ to your build file as a dependency.

#### Logistic regression

[Logistic regression](http://en.wikipedia.org/wiki/Logistic\_regression) is widely used to predict a binary response. It is a linear method as described above in equation $$(1)$$`(1)`, with the loss function in the formulation given by the logistic loss:

$$L(w;x,y):=log(1+exp(−ywTx)).$$

For binary classification problems, the algorithm outputs a binary logistic regression model. Given a new data point, denoted by $$x$$, the model makes predictions by applying the logistic function

$$f(z)=11+e−z$$ where $$z=wTx$$. By default, if $$f(wTx)>0.5$$, the outcome is positive, or negative otherwise, though unlike linear SVMs, the raw output of the logistic regression model, $$f(z)$$, has a probabilistic interpretation (i.e., the probability that $$x$$ is positive).

Binary logistic regression can be generalized into [multinomial logistic regression](http://en.wikipedia.org/wiki/Multinomial\_logistic\_regression) to train and predict multiclass classification problems. For example, for $$K$$ possible outcomes, one of the outcomes can be chosen as a “pivot”, and the other $$K−1$$ outcomes can be separately regressed against the pivot outcome. In `spark.mllib`, the first class $$0$$0 is chosen as the “pivot” class. See Section 4.4 of [The Elements of Statistical Learning](http://statweb.stanford.edu/\~tibs/ElemStatLearn/) for references. Here is a [detailed mathematical derivation](http://www.slideshare.net/dbtsai/2014-0620-mlor-36132297).

For multiclass classification problems, the algorithm will output a multinomial logistic regression model, which contains $$K−1$$ binary logistic regression models regressed against the first class. Given a new data points, $$K−1$$ models will be run, and the class with largest probability will be chosen as the predicted class.

We implemented two algorithms to solve logistic regression: mini-batch gradient descent and L-BFGS. We recommend L-BFGS over mini-batch gradient descent for faster convergence.

**Examples**

* Python

The following example shows how to load a sample dataset, build Logistic Regression model, and make predictions with the resulting model to compute the training error.

Note that the Python API does not yet support multiclass classification and model save/load but will in the future.

Refer to the `LogisticRegressionWithLBFGS` Python docs and `LogisticRegressionModel` Python docs for more details on the API.

```
from pyspark.mllib.classification import LogisticRegressionWithLBFGS, LogisticRegressionModel
from pyspark.mllib.regression import LabeledPoint

# Load and parse the data
def parsePoint(line):
    values = [float(x) for x in line.split(' ')]
    return LabeledPoint(values[0], values[1:])

data = sc.textFile("data/mllib/sample_svm_data.txt")
parsedData = data.map(parsePoint)

# Build the model
model = LogisticRegressionWithLBFGS.train(parsedData)

# Evaluating the model on training data
labelsAndPreds = parsedData.map(lambda p: (p.label, model.predict(p.features)))
trainErr = labelsAndPreds.filter(lambda lp: lp[0] != lp[1]).count() / float(parsedData.count())
print("Training Error = " + str(trainErr))

# Save and load model
model.save(sc, "target/tmp/pythonLogisticRegressionWithLBFGSModel")
sameModel = LogisticRegressionModel.load(sc,
                                         "target/tmp/pythonLogisticRegressionWithLBFGSModel")
```

Find full example code at "examples/src/main/python/mllib/logistic\_regression\_with\_lbfgs\_example.py" in the Spark repo.

* Scala

The following code illustrates how to load a sample multiclass dataset, split it into train and test, and use LogisticRegressionWithLBFGS to fit a logistic regression model. Then the model is evaluated against the test dataset and saved to disk.

Refer to the `LogisticRegressionWithLBFGS` Scala docs and `LogisticRegressionModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.classification.{LogisticRegressionModel, LogisticRegressionWithLBFGS}
import org.apache.spark.mllib.evaluation.MulticlassMetrics
import org.apache.spark.mllib.regression.LabeledPoint
import org.apache.spark.mllib.util.MLUtils

// Load training data in LIBSVM format.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")

// Split data into training (60%) and test (40%).
val splits = data.randomSplit(Array(0.6, 0.4), seed = 11L)
val training = splits(0).cache()
val test = splits(1)

// Run training algorithm to build the model
val model = new LogisticRegressionWithLBFGS()
  .setNumClasses(10)
  .run(training)

// Compute raw scores on the test set.
val predictionAndLabels = test.map { case LabeledPoint(label, features) =>
  val prediction = model.predict(features)
  (prediction, label)
}

// Get evaluation metrics.
val metrics = new MulticlassMetrics(predictionAndLabels)
val accuracy = metrics.accuracy
println(s"Accuracy = $accuracy")

// Save and load model
model.save(sc, "target/tmp/scalaLogisticRegressionWithLBFGSModel")
val sameModel = LogisticRegressionModel.load(sc,
  "target/tmp/scalaLogisticRegressionWithLBFGSModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/LogisticRegressionWithLBFGSExample.scala" in the Spark repo.

* Java

The following code illustrates how to load a sample multiclass dataset, split it into train and test, and use LogisticRegressionWithLBFGS to fit a logistic regression model. Then the model is evaluated against the test dataset and saved to disk.

Refer to the `LogisticRegressionWithLBFGS` Java docs and `LogisticRegressionModel` Java docs for details on the API.

```
import scala.Tuple2;

import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.classification.LogisticRegressionModel;
import org.apache.spark.mllib.classification.LogisticRegressionWithLBFGS;
import org.apache.spark.mllib.evaluation.MulticlassMetrics;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.util.MLUtils;

String path = "data/mllib/sample_libsvm_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(sc, path).toJavaRDD();

// Split initial RDD into two... [60% training data, 40% testing data].
JavaRDD<LabeledPoint>[] splits = data.randomSplit(new double[] {0.6, 0.4}, 11L);
JavaRDD<LabeledPoint> training = splits[0].cache();
JavaRDD<LabeledPoint> test = splits[1];

// Run training algorithm to build the model.
LogisticRegressionModel model = new LogisticRegressionWithLBFGS()
  .setNumClasses(10)
  .run(training.rdd());

// Compute raw scores on the test set.
JavaPairRDD<Object, Object> predictionAndLabels = test.mapToPair(p ->
  new Tuple2<>(model.predict(p.features()), p.label()));

// Get evaluation metrics.
MulticlassMetrics metrics = new MulticlassMetrics(predictionAndLabels.rdd());
double accuracy = metrics.accuracy();
System.out.println("Accuracy = " + accuracy);

// Save and load model
model.save(sc, "target/tmp/javaLogisticRegressionWithLBFGSModel");
LogisticRegressionModel sameModel = LogisticRegressionModel.load(sc,
  "target/tmp/javaLogisticRegressionWithLBFGSModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaLogisticRegressionWithLBFGSExample.java" in the Spark repo.

## Regression

#### Linear least squares, Lasso, and ridge regression

Linear least squares is the most common formulation for regression problems. It is a linear method as described above in equation $$(1)$$`(1)`, with the loss function in the formulation given by the squared loss:

$$L(w;x,y):=12(wTx−y)2.$$

Various related regression methods are derived by using different types of regularization: [_ordinary least squares_](http://en.wikipedia.org/wiki/Ordinary\_least\_squares) or [_linear least squares_](http://en.wikipedia.org/wiki/Linear\_least\_squares\_\(mathematics\)) uses no regularization; [_ridge regression_](http://en.wikipedia.org/wiki/Ridge\_regression) uses L2 regularization; and [_Lasso_](http://en.wikipedia.org/wiki/Lasso\_\(statistics\)) uses L1 regularization. For all of these models, the average loss or training error, $$1n∑ni=1(wTxi−yi)2$$, is known as the [mean squared error](http://en.wikipedia.org/wiki/Mean\_squared\_error).

#### Streaming linear regression

When data arrive in a streaming fashion, it is useful to fit regression models online, updating the parameters of the model as new data arrives. `spark.mllib` currently supports streaming linear regression using ordinary least squares. The fitting is similar to that performed offline, except fitting occurs on each batch of data, so that the model continually updates to reflect the data from the stream.

**Examples**

The following example demonstrates how to load training and testing data from two different input streams of text files, parse the streams as labeled points, fit a linear regression model online to the first stream, and make predictions on the second stream.

* Python

First, we import the necessary classes for parsing our input data and creating the model.

Then we make input streams for training and testing data. We assume a StreamingContext `ssc` has already been created, see Spark Streaming Programming Guide for more info. For this example, we use labeled points in training and testing streams, but in practice you will likely want to use unlabeled vectors for test data.

We create our model by initializing the weights to 0.

Now we register the streams for training and testing and start the job.

We can now save text files with data to the training or testing folders. Each line should be a data point formatted as `(y,[x1,x2,x3])` where `y` is the label and `x1,x2,x3` are the features. Anytime a text file is placed in `sys.argv[1]` the model will update. Anytime a text file is placed in `sys.argv[2]` you will see predictions. As you feed more data to the training directory, the predictions will get better!

Here a complete example:

```
import sys

from pyspark.mllib.linalg import Vectors
from pyspark.mllib.regression import LabeledPoint
from pyspark.mllib.regression import StreamingLinearRegressionWithSGD

def parse(lp):
    label = float(lp[lp.find('(') + 1: lp.find(',')])
    vec = Vectors.dense(lp[lp.find('[') + 1: lp.find(']')].split(','))
    return LabeledPoint(label, vec)

trainingData = ssc.textFileStream(sys.argv[1]).map(parse).cache()
testData = ssc.textFileStream(sys.argv[2]).map(parse)

numFeatures = 3
model = StreamingLinearRegressionWithSGD()
model.setInitialWeights([0.0, 0.0, 0.0])

model.trainOn(trainingData)
print(model.predictOnValues(testData.map(lambda lp: (lp.label, lp.features))))

ssc.start()
ssc.awaitTermination()
```

Find full example code at "examples/src/main/python/mllib/streaming\_linear\_regression\_example.py" in the Spark repo.

* Scala

First, we import the necessary classes for parsing our input data and creating the model.

Then we make input streams for training and testing data. We assume a StreamingContext `ssc` has already been created, see Spark Streaming Programming Guide for more info. For this example, we use labeled points in training and testing streams, but in practice you will likely want to use unlabeled vectors for test data.

We create our model by initializing the weights to zero and register the streams for training and testing then start the job. Printing predictions alongside true labels lets us easily see the result.

Finally, we can save text files with data to the training or testing folders. Each line should be a data point formatted as `(y,[x1,x2,x3])` where `y` is the label and `x1,x2,x3` are the features. Anytime a text file is placed in `args(0)` the model will update. Anytime a text file is placed in `args(1)` you will see predictions. As you feed more data to the training directory, the predictions will get better!

Here is a complete example:

```
import org.apache.spark.mllib.linalg.Vectors
import org.apache.spark.mllib.regression.LabeledPoint
import org.apache.spark.mllib.regression.StreamingLinearRegressionWithSGD

val trainingData = ssc.textFileStream(args(0)).map(LabeledPoint.parse).cache()
val testData = ssc.textFileStream(args(1)).map(LabeledPoint.parse)

val numFeatures = 3
val model = new StreamingLinearRegressionWithSGD()
  .setInitialWeights(Vectors.zeros(numFeatures))

model.trainOn(trainingData)
model.predictOnValues(testData.map(lp => (lp.label, lp.features))).print()

ssc.start()
ssc.awaitTermination()
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/StreamingLinearRegressionExample.scala" in the Spark repo.

## Implementation (developer)

Behind the scene, `spark.mllib` implements a simple distributed version of stochastic gradient descent (SGD), building on the underlying gradient descent primitive (as described in the [optimization](https://spark.apache.org/docs/latest/mllib-optimization.html) section). All provided algorithms take as input a regularization parameter (`regParam`) along with various parameters associated with stochastic gradient descent (`stepSize`, `numIterations`, `miniBatchFraction`). For each of them, we support all three possible regularizations (none, L1 or L2).

For Logistic Regression, [L-BFGS](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/mllib/optimization/LBFGS.html) version is implemented under [LogisticRegressionWithLBFGS](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/mllib/classification/LogisticRegressionWithLBFGS.html), and this version supports both binary and multinomial Logistic Regression while SGD version only supports binary Logistic Regression. However, L-BFGS version doesn’t support L1 regularization but SGD one supports L1 regularization. When L1 regularization is not required, L-BFGS version is strongly recommended since it converges faster and more accurately compared to SGD by approximating the inverse Hessian matrix using quasi-Newton method.

Algorithms are all implemented in Scala:

* SVMWithSGD
* LogisticRegressionWithLBFGS
* LogisticRegressionWithSGD
* LinearRegressionWithSGD
* RidgeRegressionWithSGD
* LassoWithSGD
