# Evaluation Metrics - RDD-based API

* Classification model evaluation
  * Binary classification
    * Threshold tuning
  * Multiclass classification
    * Label based metrics
  * Multilabel classification
  * Ranking systems
* Regression model evaluation

`spark.mllib` comes with a number of machine learning algorithms that can be used to learn from and make predictions on data. When these algorithms are applied to build machine learning models, there is a need to evaluate the performance of the model on some criteria, which depends on the application and its requirements. `spark.mllib` also provides a suite of metrics for the purpose of evaluating the performance of machine learning models.

Specific machine learning algorithms fall under broader types of machine learning applications like classification, regression, clustering, etc. Each of these types have well-established metrics for performance evaluation and those metrics that are currently available in `spark.mllib` are detailed in this section.

### Classification model evaluation

While there are many different types of classification algorithms, the evaluation of classification models all share similar principles. In a supervised classification problem, there exists a true output and a model-generated predicted output for each data point. For this reason, the results for each data point can be assigned to one of four categories:

* True Positive (TP) - label is positive and prediction is also positive
* True Negative (TN) - label is negative and prediction is also negative
* False Positive (FP) - label is negative but prediction is positive
* False Negative (FN) - label is positive but prediction is negative

These four numbers are the building blocks for most classifier evaluation metrics. A fundamental point when considering classifier evaluation is that pure accuracy (i.e. was the prediction correct or incorrect) is not generally a good metric. The reason for this is because a dataset may be highly unbalanced. For example, if a model is designed to predict fraud from a dataset where 95% of the data points are _not fraud_ and 5% of the data points are _fraud_, then a naive classifier that predicts _not fraud_, regardless of input, will be 95% accurate. For this reason, metrics like precision and recall are typically used because they take into account the _type_ of error. In most applications there is some desired balance between precision and recall, which can be captured by combining the two into a single metric, called the F-measure.

#### Binary classification

Binary classifiers are used to separate the elements of a given dataset into one of two possible groups (e.g. fraud or not fraud) and is a special case of multiclass classification. Most binary classification metrics can be generalized to multiclass classification metrics.

**Threshold tuning**

It is import to understand that many classification models actually output a “score” (often times a probability) for each class, where a higher score indicates higher likelihood. In the binary case, the model may output a probability for each class: $$P(Y=1|X)$$ and $$P(Y=0|X)$$. Instead of simply taking the higher probability, there may be some cases where the model might need to be tuned so that it only predicts a class when the probability is very high (e.g. only block a credit card transaction if the model predicts fraud with >90% probability). Therefore, there is a prediction _threshold_ which determines what the predicted class will be based on the probabilities that the model outputs.

Tuning the prediction threshold will change the precision and recall of the model and is an important part of model optimization. In order to visualize how precision, recall, and other metrics change as a function of the threshold it is common practice to plot competing metrics against one another, parameterized by threshold. A P-R curve plots (precision, recall) points for different threshold values, while a receiver operating characteristic, or ROC, curve plots (recall, false positive rate) points.

**Available metrics**

| Metric                                  | Definition                             |
| --------------------------------------- | -------------------------------------- |
| Precision (Positive Predictive Value)   | $$PPV=TPTP+FP$$                        |
| Recall (True Positive Rate)             | $$TPR=TPP=TPTP+FN$$                    |
| F-measure                               | $$F(β)=(1+β2)⋅(PPV⋅TPRβ2⋅PPV+TPR)$$    |
| Receiver Operating Characteristic (ROC) | $$FPR(T)=∫∞TP0(T)dTTPR(T)=∫∞TP1(T)dT$$ |
| Area Under ROC Curve                    | $$AUROC=∫10TPPd(FPN)$$                 |
| Area Under Precision-Recall Curve       | $$AUPRC=∫10TPTP+FPd(TPP)$$             |

**Examples**

* Python

The following code snippets illustrate how to load a sample dataset, train a binary classification algorithm on the data, and evaluate the performance of the algorithm by several binary evaluation metrics.

Refer to the `BinaryClassificationMetrics` Python docs and `LogisticRegressionWithLBFGS` Python docs for more details on the API.

```
from pyspark.mllib.classification import LogisticRegressionWithLBFGS
from pyspark.mllib.evaluation import BinaryClassificationMetrics
from pyspark.mllib.util import MLUtils

# Several of the methods available in scala are currently missing from pyspark
# Load training data in LIBSVM format
data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_binary_classification_data.txt")

# Split data into training (60%) and test (40%)
training, test = data.randomSplit([0.6, 0.4], seed=11)
training.cache()

# Run training algorithm to build the model
model = LogisticRegressionWithLBFGS.train(training)

# Compute raw scores on the test set
predictionAndLabels = test.map(lambda lp: (float(model.predict(lp.features)), lp.label))

# Instantiate metrics object
metrics = BinaryClassificationMetrics(predictionAndLabels)

# Area under precision-recall curve
print("Area under PR = %s" % metrics.areaUnderPR)

# Area under ROC curve
print("Area under ROC = %s" % metrics.areaUnderROC)
```

Find full example code at "examples/src/main/python/mllib/binary\_classification\_metrics\_example.py" in the Spark repo.

* Scala

The following code snippets illustrate how to load a sample dataset, train a binary classification algorithm on the data, and evaluate the performance of the algorithm by several binary evaluation metrics.

Refer to the `LogisticRegressionWithLBFGS` Scala docs and `BinaryClassificationMetrics` Scala docs for details on the API.

```
import org.apache.spark.mllib.classification.LogisticRegressionWithLBFGS
import org.apache.spark.mllib.evaluation.BinaryClassificationMetrics
import org.apache.spark.mllib.regression.LabeledPoint
import org.apache.spark.mllib.util.MLUtils

// Load training data in LIBSVM format
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_binary_classification_data.txt")

// Split data into training (60%) and test (40%)
val Array(training, test) = data.randomSplit(Array(0.6, 0.4), seed = 11L)
training.cache()

// Run training algorithm to build the model
val model = new LogisticRegressionWithLBFGS()
  .setNumClasses(2)
  .run(training)

// Clear the prediction threshold so the model will return probabilities
model.clearThreshold

// Compute raw scores on the test set
val predictionAndLabels = test.map { case LabeledPoint(label, features) =>
  val prediction = model.predict(features)
  (prediction, label)
}

// Instantiate metrics object
val metrics = new BinaryClassificationMetrics(predictionAndLabels)

// Precision by threshold
val precision = metrics.precisionByThreshold
precision.collect.foreach { case (t, p) =>
  println(s"Threshold: $t, Precision: $p")
}

// Recall by threshold
val recall = metrics.recallByThreshold
recall.collect.foreach { case (t, r) =>
  println(s"Threshold: $t, Recall: $r")
}

// Precision-Recall Curve
val PRC = metrics.pr

// F-measure
val f1Score = metrics.fMeasureByThreshold
f1Score.collect.foreach { case (t, f) =>
  println(s"Threshold: $t, F-score: $f, Beta = 1")
}

val beta = 0.5
val fScore = metrics.fMeasureByThreshold(beta)
fScore.collect.foreach { case (t, f) =>
  println(s"Threshold: $t, F-score: $f, Beta = 0.5")
}

// AUPRC
val auPRC = metrics.areaUnderPR
println(s"Area under precision-recall curve = $auPRC")

// Compute thresholds used in ROC and PR curves
val thresholds = precision.map(_._1)

// ROC Curve
val roc = metrics.roc

// AUROC
val auROC = metrics.areaUnderROC
println(s"Area under ROC = $auROC")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/BinaryClassificationMetricsExample.scala" in the Spark repo.

* Java

The following code snippets illustrate how to load a sample dataset, train a binary classification algorithm on the data, and evaluate the performance of the algorithm by several binary evaluation metrics.

Refer to the `LogisticRegressionModel` Java docs and `LogisticRegressionWithLBFGS` Java docs for details on the API.

```
import scala.Tuple2;

import org.apache.spark.api.java.*;
import org.apache.spark.mllib.classification.LogisticRegressionModel;
import org.apache.spark.mllib.classification.LogisticRegressionWithLBFGS;
import org.apache.spark.mllib.evaluation.BinaryClassificationMetrics;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.util.MLUtils;

String path = "data/mllib/sample_binary_classification_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(sc, path).toJavaRDD();

// Split initial RDD into two... [60% training data, 40% testing data].
JavaRDD<LabeledPoint>[] splits =
  data.randomSplit(new double[]{0.6, 0.4}, 11L);
JavaRDD<LabeledPoint> training = splits[0].cache();
JavaRDD<LabeledPoint> test = splits[1];

// Run training algorithm to build the model.
LogisticRegressionModel model = new LogisticRegressionWithLBFGS()
  .setNumClasses(2)
  .run(training.rdd());

// Clear the prediction threshold so the model will return probabilities
model.clearThreshold();

// Compute raw scores on the test set.
JavaPairRDD<Object, Object> predictionAndLabels = test.mapToPair(p ->
  new Tuple2<>(model.predict(p.features()), p.label()));

// Get evaluation metrics.
BinaryClassificationMetrics metrics =
  new BinaryClassificationMetrics(predictionAndLabels.rdd());

// Precision by threshold
JavaRDD<Tuple2<Object, Object>> precision = metrics.precisionByThreshold().toJavaRDD();
System.out.println("Precision by threshold: " + precision.collect());

// Recall by threshold
JavaRDD<?> recall = metrics.recallByThreshold().toJavaRDD();
System.out.println("Recall by threshold: " + recall.collect());

// F Score by threshold
JavaRDD<?> f1Score = metrics.fMeasureByThreshold().toJavaRDD();
System.out.println("F1 Score by threshold: " + f1Score.collect());

JavaRDD<?> f2Score = metrics.fMeasureByThreshold(2.0).toJavaRDD();
System.out.println("F2 Score by threshold: " + f2Score.collect());

// Precision-recall curve
JavaRDD<?> prc = metrics.pr().toJavaRDD();
System.out.println("Precision-recall curve: " + prc.collect());

// Thresholds
JavaRDD<Double> thresholds = precision.map(t -> Double.parseDouble(t._1().toString()));

// ROC Curve
JavaRDD<?> roc = metrics.roc().toJavaRDD();
System.out.println("ROC curve: " + roc.collect());

// AUPRC
System.out.println("Area under precision-recall curve = " + metrics.areaUnderPR());

// AUROC
System.out.println("Area under ROC = " + metrics.areaUnderROC());

// Save and load model
model.save(sc, "target/tmp/LogisticRegressionModel");
LogisticRegressionModel.load(sc, "target/tmp/LogisticRegressionModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaBinaryClassificationMetricsExample.java" in the Spark repo.

#### Multiclass classification

A [multiclass classification](https://en.wikipedia.org/wiki/Multiclass\_classification) describes a classification problem where there are $$M>2$$ possible labels for each data point (the case where $$M=2$$ is the binary classification problem). For example, classifying handwriting samples to the digits 0 to 9, having 10 possible classes.

For multiclass metrics, the notion of positives and negatives is slightly different. Predictions and labels can still be positive or negative, but they must be considered under the context of a particular class. Each label and prediction take on the value of one of the multiple classes and so they are said to be positive for their particular class and negative for all other classes. So, a true positive occurs whenever the prediction and the label match, while a true negative occurs when neither the prediction nor the label take on the value of a given class. By this convention, there can be multiple true negatives for a given data sample. The extension of false negatives and false positives from the former definitions of positive and negative labels is straightforward.

**Label based metrics**

Opposed to binary classification where there are only two possible labels, multiclass classification problems have many possible labels and so the concept of label-based metrics is introduced. Accuracy measures precision across all labels - the number of times any class was predicted correctly (true positives) normalized by the number of data points. Precision by label considers only one class, and measures the number of time a specific label was predicted correctly normalized by the number of times that label appears in the output.

**Available metrics**

Define the class, or label, set as

$$L={ℓ0,ℓ1,…,ℓM−1}$$

The true output vector $$y$$ consists of $$N$$ elements

$$y0,y1,…,yN−1∈L$$

A multiclass prediction algorithm generates a prediction vector $$y^$$ of $$N$$ elements

$$y^0,y^1,…,y^N−1∈L$$

For this section, a modified delta function $$δ (x)$$will prove useful

$$δ^(x)={10if x=0,otherwise.$$δ (x) ={1 if x=0, 0 otherwise.

| Metric             | Definition                                                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Confusion Matrix   | $$Cij=∑N−1k=0δ^(yk−ℓi)⋅δ^(y^k−ℓj)   ∑N−1k=0δ^(yk−ℓ1)⋅δ^(y^k−ℓ1)⋮∑N−1k=0δ^(yk−ℓN)⋅δ^(y^k−ℓ1)…⋱…∑N−1k=0δ^(yk−ℓ1)⋅δ^(y^k−ℓN)⋮∑N−1k=0δ^(yk−ℓN)⋅δ^(y^k−ℓN)$$ |
| Accuracy           | $$ACC=TPTP+FP=1N∑N−1i=0δ^(y^i−yi)$$                                                                                                                     |
| Precision by label | $$PPV(ℓ)=TPTP+FP=∑N−1i=0δ^(y^i−ℓ)⋅δ^(yi−ℓ)∑N−1i=0δ^(y^i−ℓ)$$                                                                                            |
| Recall by label    | $$TPR(ℓ)=TPP=∑N−1i=0δ^(y^i−ℓ)⋅δ^(yi−ℓ)∑N−1i=0δ^(yi−ℓ)$$                                                                                                 |
| F-measure by label | $$F(β,ℓ)=(1+β2)⋅(PPV(ℓ)⋅TPR(ℓ)β2⋅PPV(ℓ)+TPR(ℓ))$$                                                                                                       |
| Weighted precision | $$PPVw=1N∑ℓ∈LPPV(ℓ)⋅∑N−1i=0δ^(yi−ℓ)$$                                                                                                                   |
| Weighted recall    | $$TPRw=1N∑ℓ∈LTPR(ℓ)⋅∑N−1i=0δ^(yi−ℓ)$$                                                                                                                   |
| Weighted F-measure | $$Fw(β)=1N∑ℓ∈LF(β,ℓ)⋅∑N−1i=0δ^(yi−ℓ)$$                                                                                                                  |

**Examples**

* Python

The following code snippets illustrate how to load a sample dataset, train a multiclass classification algorithm on the data, and evaluate the performance of the algorithm by several multiclass classification evaluation metrics.

Refer to the `MulticlassMetrics` Python docs for more details on the API.

```
from pyspark.mllib.classification import LogisticRegressionWithLBFGS
from pyspark.mllib.util import MLUtils
from pyspark.mllib.evaluation import MulticlassMetrics

# Load training data in LIBSVM format
data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_multiclass_classification_data.txt")

# Split data into training (60%) and test (40%)
training, test = data.randomSplit([0.6, 0.4], seed=11)
training.cache()

# Run training algorithm to build the model
model = LogisticRegressionWithLBFGS.train(training, numClasses=3)

# Compute raw scores on the test set
predictionAndLabels = test.map(lambda lp: (float(model.predict(lp.features)), lp.label))

# Instantiate metrics object
metrics = MulticlassMetrics(predictionAndLabels)

# Overall statistics
precision = metrics.precision(1.0)
recall = metrics.recall(1.0)
f1Score = metrics.fMeasure(1.0)
print("Summary Stats")
print("Precision = %s" % precision)
print("Recall = %s" % recall)
print("F1 Score = %s" % f1Score)

# Statistics by class
labels = data.map(lambda lp: lp.label).distinct().collect()
for label in sorted(labels):
    print("Class %s precision = %s" % (label, metrics.precision(label)))
    print("Class %s recall = %s" % (label, metrics.recall(label)))
    print("Class %s F1 Measure = %s" % (label, metrics.fMeasure(label, beta=1.0)))

# Weighted stats
print("Weighted recall = %s" % metrics.weightedRecall)
print("Weighted precision = %s" % metrics.weightedPrecision)
print("Weighted F(1) Score = %s" % metrics.weightedFMeasure())
print("Weighted F(0.5) Score = %s" % metrics.weightedFMeasure(beta=0.5))
print("Weighted false positive rate = %s" % metrics.weightedFalsePositiveRate)
```

Find full example code at "examples/src/main/python/mllib/multi\_class\_metrics\_example.py" in the Spark repo.

* Scala

The following code snippets illustrate how to load a sample dataset, train a multiclass classification algorithm on the data, and evaluate the performance of the algorithm by several multiclass classification evaluation metrics.

Refer to the `MulticlassMetrics` Scala docs for details on the API.

```
import org.apache.spark.mllib.classification.LogisticRegressionWithLBFGS
import org.apache.spark.mllib.evaluation.MulticlassMetrics
import org.apache.spark.mllib.regression.LabeledPoint
import org.apache.spark.mllib.util.MLUtils

// Load training data in LIBSVM format
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_multiclass_classification_data.txt")

// Split data into training (60%) and test (40%)
val Array(training, test) = data.randomSplit(Array(0.6, 0.4), seed = 11L)
training.cache()

// Run training algorithm to build the model
val model = new LogisticRegressionWithLBFGS()
  .setNumClasses(3)
  .run(training)

// Compute raw scores on the test set
val predictionAndLabels = test.map { case LabeledPoint(label, features) =>
  val prediction = model.predict(features)
  (prediction, label)
}

// Instantiate metrics object
val metrics = new MulticlassMetrics(predictionAndLabels)

// Confusion matrix
println("Confusion matrix:")
println(metrics.confusionMatrix)

// Overall Statistics
val accuracy = metrics.accuracy
println("Summary Statistics")
println(s"Accuracy = $accuracy")

// Precision by label
val labels = metrics.labels
labels.foreach { l =>
  println(s"Precision($l) = " + metrics.precision(l))
}

// Recall by label
labels.foreach { l =>
  println(s"Recall($l) = " + metrics.recall(l))
}

// False positive rate by label
labels.foreach { l =>
  println(s"FPR($l) = " + metrics.falsePositiveRate(l))
}

// F-measure by label
labels.foreach { l =>
  println(s"F1-Score($l) = " + metrics.fMeasure(l))
}

// Weighted stats
println(s"Weighted precision: ${metrics.weightedPrecision}")
println(s"Weighted recall: ${metrics.weightedRecall}")
println(s"Weighted F1 score: ${metrics.weightedFMeasure}")
println(s"Weighted false positive rate: ${metrics.weightedFalsePositiveRate}")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/MulticlassMetricsExample.scala" in the Spark repo.

* Java

The following code snippets illustrate how to load a sample dataset, train a multiclass classification algorithm on the data, and evaluate the performance of the algorithm by several multiclass classification evaluation metrics.

Refer to the `MulticlassMetrics` Java docs for details on the API.

```
import scala.Tuple2;

import org.apache.spark.api.java.*;
import org.apache.spark.mllib.classification.LogisticRegressionModel;
import org.apache.spark.mllib.classification.LogisticRegressionWithLBFGS;
import org.apache.spark.mllib.evaluation.MulticlassMetrics;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.util.MLUtils;
import org.apache.spark.mllib.linalg.Matrix;

String path = "data/mllib/sample_multiclass_classification_data.txt";
JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(sc, path).toJavaRDD();

// Split initial RDD into two... [60% training data, 40% testing data].
JavaRDD<LabeledPoint>[] splits = data.randomSplit(new double[]{0.6, 0.4}, 11L);
JavaRDD<LabeledPoint> training = splits[0].cache();
JavaRDD<LabeledPoint> test = splits[1];

// Run training algorithm to build the model.
LogisticRegressionModel model = new LogisticRegressionWithLBFGS()
  .setNumClasses(3)
  .run(training.rdd());

// Compute raw scores on the test set.
JavaPairRDD<Object, Object> predictionAndLabels = test.mapToPair(p ->
  new Tuple2<>(model.predict(p.features()), p.label()));

// Get evaluation metrics.
MulticlassMetrics metrics = new MulticlassMetrics(predictionAndLabels.rdd());

// Confusion matrix
Matrix confusion = metrics.confusionMatrix();
System.out.println("Confusion matrix: \n" + confusion);

// Overall statistics
System.out.println("Accuracy = " + metrics.accuracy());

// Stats by labels
for (int i = 0; i < metrics.labels().length; i++) {
  System.out.format("Class %f precision = %f\n", metrics.labels()[i],metrics.precision(
    metrics.labels()[i]));
  System.out.format("Class %f recall = %f\n", metrics.labels()[i], metrics.recall(
    metrics.labels()[i]));
  System.out.format("Class %f F1 score = %f\n", metrics.labels()[i], metrics.fMeasure(
    metrics.labels()[i]));
}

//Weighted stats
System.out.format("Weighted precision = %f\n", metrics.weightedPrecision());
System.out.format("Weighted recall = %f\n", metrics.weightedRecall());
System.out.format("Weighted F1 score = %f\n", metrics.weightedFMeasure());
System.out.format("Weighted false positive rate = %f\n", metrics.weightedFalsePositiveRate());

// Save and load model
model.save(sc, "target/tmp/LogisticRegressionModel");
LogisticRegressionModel sameModel = LogisticRegressionModel.load(sc,
  "target/tmp/LogisticRegressionModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaMulticlassClassificationMetricsExample.java" in the Spark repo.

#### Multilabel classification

A [multilabel classification](https://en.wikipedia.org/wiki/Multi-label\_classification) problem involves mapping each sample in a dataset to a set of class labels. In this type of classification problem, the labels are not mutually exclusive. For example, when classifying a set of news articles into topics, a single article might be both science and politics.

Because the labels are not mutually exclusive, the predictions and true labels are now vectors of label _sets_, rather than vectors of labels. Multilabel metrics, therefore, extend the fundamental ideas of precision, recall, etc. to operations on sets. For example, a true positive for a given class now occurs when that class exists in the predicted set and it exists in the true label set, for a specific data point.

**Available metrics**

Here we define a set $$D$$ of $$N$$ documents

$$D={d0,d1,...,dN−1}$$

Define $$L0,L1,…,LN−1$$ to be a family of label sets and $$P0,P1,…,PN−1$$ to be a family of prediction sets where $$Li$$ and $$Pi$$ are the label set and prediction set, respectively, that correspond to document $$di$$.

The set of all unique labels is given by

$$L=⋃k=0N−1Lk$$

The following definition of indicator function $$IA(x)$$on a set $$A$$ will be necessary

$$IA(x)={10if x∈A,otherwise.$$

| Metric              | Definition                                                                        |
| ------------------- | --------------------------------------------------------------------------------- |
| Precision           | $$1N∑N−1i=0|Pi∩Li||Pi|$$                                                          |
| Recall              | $$1N∑N−1i=0|Li∩Pi||Li|$$                                                          |
| Accuracy            | $$1N∑N−1i=0|Li∩Pi||Li|+|Pi|−|Li∩Pi|$$                                             |
| Precision by label  | $$PPV(ℓ)=TPTP+FP=∑N−1i=0IPi(ℓ)⋅ILi(ℓ)∑N−1i=0IPi(ℓ)$$                              |
| Recall by label     | $$TPR(ℓ)=TPP=∑N−1i=0IPi(ℓ)⋅ILi(ℓ)∑N−1i=0ILi(ℓ)$$                                  |
| F1-measure by label | $$F1(ℓ)=2⋅(PPV(ℓ)⋅TPR(ℓ)PPV(ℓ)+TPR(ℓ))$$                                          |
| Hamming Loss        | $$1N⋅|L|∑N−1i=0|Li|+|Pi|−2|Li∩Pi|$$                                               |
| Subset Accuracy     | $$1N∑N−1i=0I{Li}(Pi)$$                                                            |
| F1 Measure          | $$1N∑N−1i=02|Pi∩Li||Pi|⋅|Li|$$                                                    |
| Micro precision     | $$TPTP+FP=∑N−1i=0|Pi∩Li|∑N−1i=0|Pi∩Li|+∑N−1i=0|Pi−Li|$$                           |
| Micro recall        | $$TPTP+FN=∑N−1i=0|Pi∩Li|∑N−1i=0|Pi∩Li|+∑N−1i=0|Li−Pi|$$                           |
| Micro F1 Measure    | $$2⋅TP2⋅TP+FP+FN=2⋅∑N−1i=0|Pi∩Li|2⋅∑N−1i=0|Pi∩Li|+∑N−1i=0|Li−Pi|+∑N−1i=0|Pi−Li|$$ |

**Examples**

The following code snippets illustrate how to evaluate the performance of a multilabel classifier. The examples use the fake prediction and label data for multilabel classification that is shown below.

Document predictions:

* doc 0 - predict 0, 1 - class 0, 2
* doc 1 - predict 0, 2 - class 0, 1
* doc 2 - predict none - class 0
* doc 3 - predict 2 - class 2
* doc 4 - predict 2, 0 - class 2, 0
* doc 5 - predict 0, 1, 2 - class 0, 1
* doc 6 - predict 1 - class 1, 2

Predicted classes:

* class 0 - doc 0, 1, 4, 5 (total 4)
* class 1 - doc 0, 5, 6 (total 3)
* class 2 - doc 1, 3, 4, 5 (total 4)

True classes:

* class 0 - doc 0, 1, 2, 4, 5 (total 5)
* class 1 - doc 1, 5, 6 (total 3)
* class 2 - doc 0, 3, 4, 6 (total 4)



* Python

Refer to the `MultilabelMetrics` Python docs for more details on the API.

```
from pyspark.mllib.evaluation import MultilabelMetrics

scoreAndLabels = sc.parallelize([
    ([0.0, 1.0], [0.0, 2.0]),
    ([0.0, 2.0], [0.0, 1.0]),
    ([], [0.0]),
    ([2.0], [2.0]),
    ([2.0, 0.0], [2.0, 0.0]),
    ([0.0, 1.0, 2.0], [0.0, 1.0]),
    ([1.0], [1.0, 2.0])])

# Instantiate metrics object
metrics = MultilabelMetrics(scoreAndLabels)

# Summary stats
print("Recall = %s" % metrics.recall())
print("Precision = %s" % metrics.precision())
print("F1 measure = %s" % metrics.f1Measure())
print("Accuracy = %s" % metrics.accuracy)

# Individual label stats
labels = scoreAndLabels.flatMap(lambda x: x[1]).distinct().collect()
for label in labels:
    print("Class %s precision = %s" % (label, metrics.precision(label)))
    print("Class %s recall = %s" % (label, metrics.recall(label)))
    print("Class %s F1 Measure = %s" % (label, metrics.f1Measure(label)))

# Micro stats
print("Micro precision = %s" % metrics.microPrecision)
print("Micro recall = %s" % metrics.microRecall)
print("Micro F1 measure = %s" % metrics.microF1Measure)

# Hamming loss
print("Hamming loss = %s" % metrics.hammingLoss)

# Subset accuracy
print("Subset accuracy = %s" % metrics.subsetAccuracy)
```

Find full example code at "examples/src/main/python/mllib/multi\_label\_metrics\_example.py" in the Spark repo.

* Scala

Refer to the `MultilabelMetrics` Scala docs for details on the API.

```
import org.apache.spark.mllib.evaluation.MultilabelMetrics
import org.apache.spark.rdd.RDD

val scoreAndLabels: RDD[(Array[Double], Array[Double])] = sc.parallelize(
  Seq((Array(0.0, 1.0), Array(0.0, 2.0)),
    (Array(0.0, 2.0), Array(0.0, 1.0)),
    (Array.empty[Double], Array(0.0)),
    (Array(2.0), Array(2.0)),
    (Array(2.0, 0.0), Array(2.0, 0.0)),
    (Array(0.0, 1.0, 2.0), Array(0.0, 1.0)),
    (Array(1.0), Array(1.0, 2.0))), 2)

// Instantiate metrics object
val metrics = new MultilabelMetrics(scoreAndLabels)

// Summary stats
println(s"Recall = ${metrics.recall}")
println(s"Precision = ${metrics.precision}")
println(s"F1 measure = ${metrics.f1Measure}")
println(s"Accuracy = ${metrics.accuracy}")

// Individual label stats
metrics.labels.foreach(label =>
  println(s"Class $label precision = ${metrics.precision(label)}"))
metrics.labels.foreach(label => println(s"Class $label recall = ${metrics.recall(label)}"))
metrics.labels.foreach(label => println(s"Class $label F1-score = ${metrics.f1Measure(label)}"))

// Micro stats
println(s"Micro recall = ${metrics.microRecall}")
println(s"Micro precision = ${metrics.microPrecision}")
println(s"Micro F1 measure = ${metrics.microF1Measure}")

// Hamming loss
println(s"Hamming loss = ${metrics.hammingLoss}")

// Subset accuracy
println(s"Subset accuracy = ${metrics.subsetAccuracy}")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/MultiLabelMetricsExample.scala" in the Spark repo.

* Java

Refer to the `MultilabelMetrics` Java docs for details on the API.

```
import java.util.Arrays;
import java.util.List;

import scala.Tuple2;

import org.apache.spark.api.java.*;
import org.apache.spark.mllib.evaluation.MultilabelMetrics;
import org.apache.spark.SparkConf;

List<Tuple2<double[], double[]>> data = Arrays.asList(
  new Tuple2<>(new double[]{0.0, 1.0}, new double[]{0.0, 2.0}),
  new Tuple2<>(new double[]{0.0, 2.0}, new double[]{0.0, 1.0}),
  new Tuple2<>(new double[]{}, new double[]{0.0}),
  new Tuple2<>(new double[]{2.0}, new double[]{2.0}),
  new Tuple2<>(new double[]{2.0, 0.0}, new double[]{2.0, 0.0}),
  new Tuple2<>(new double[]{0.0, 1.0, 2.0}, new double[]{0.0, 1.0}),
  new Tuple2<>(new double[]{1.0}, new double[]{1.0, 2.0})
);
JavaRDD<Tuple2<double[], double[]>> scoreAndLabels = sc.parallelize(data);

// Instantiate metrics object
MultilabelMetrics metrics = new MultilabelMetrics(scoreAndLabels.rdd());

// Summary stats
System.out.format("Recall = %f\n", metrics.recall());
System.out.format("Precision = %f\n", metrics.precision());
System.out.format("F1 measure = %f\n", metrics.f1Measure());
System.out.format("Accuracy = %f\n", metrics.accuracy());

// Stats by labels
for (int i = 0; i < metrics.labels().length - 1; i++) {
  System.out.format("Class %1.1f precision = %f\n", metrics.labels()[i], metrics.precision(
    metrics.labels()[i]));
  System.out.format("Class %1.1f recall = %f\n", metrics.labels()[i], metrics.recall(
    metrics.labels()[i]));
  System.out.format("Class %1.1f F1 score = %f\n", metrics.labels()[i], metrics.f1Measure(
    metrics.labels()[i]));
}

// Micro stats
System.out.format("Micro recall = %f\n", metrics.microRecall());
System.out.format("Micro precision = %f\n", metrics.microPrecision());
System.out.format("Micro F1 measure = %f\n", metrics.microF1Measure());

// Hamming loss
System.out.format("Hamming loss = %f\n", metrics.hammingLoss());

// Subset accuracy
System.out.format("Subset accuracy = %f\n", metrics.subsetAccuracy());
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaMultiLabelClassificationMetricsExample.java" in the Spark repo.

#### Ranking systems

The role of a ranking algorithm (often thought of as a [recommender system](https://en.wikipedia.org/wiki/Recommender\_system)) is to return to the user a set of relevant items or documents based on some training data. The definition of relevance may vary and is usually application specific. Ranking system metrics aim to quantify the effectiveness of these rankings or recommendations in various contexts. Some metrics compare a set of recommended documents to a ground truth set of relevant documents, while other metrics may incorporate numerical ratings explicitly.

**Available metrics**

A ranking system usually deals with a set of $$M$$� users

$$U={u0,u1,...,uM−1}$$

Each user ($$ui$$) having a set of $$Ni$$ ground truth relevant documents

$$Di={d0,d1,...,dNi−1}$$

And a list of $$Qi$$ recommended documents, in order of decreasing relevance

$$Ri=[r0,r1,...,rQi−1]$$

The goal of the ranking system is to produce the most relevant set of documents for each user. The relevance of the sets and the effectiveness of the algorithms can be measured using the metrics listed below.

It is necessary to define a function which, provided a recommended document and a set of ground truth relevant documents, returns a relevance score for the recommended document.

$$relD(r)={10if r∈D,otherwise.$$

| Metric                                | Definition                                                                                                             | Notes                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Precision at k                        | $$p(k)=1M∑M−1i=01k∑min(Qi,k)−1j=0relDi(Ri(j))$$                                                                        | [Precision at k](https://en.wikipedia.org/wiki/Evaluation\_measures\_\(information\_retrieval\)#Precision\_at\_K) is a measure of how many of the first k recommended documents are in the set of true relevant documents averaged across all users. In this metric, the order of the recommendations is not taken into account.                                                          |
| Mean Average Precision                | $$MAP=1M∑M−1i=01Ni∑Qi−1j=0relDi(Ri(j))j+1$$                                                                            | [MAP](https://en.wikipedia.org/wiki/Evaluation\_measures\_\(information\_retrieval\)#Mean\_average\_precision) is a measure of how many of the recommended documents are in the set of true relevant documents, where the order of the recommendations is taken into account (i.e. penalty for highly relevant documents is higher).                                                      |
| Normalized Discounted Cumulative Gain | $$NDCG(k)=1M∑M−1i=01IDCG(Di,k)∑n−1j=0relDi(Ri(j))log(j+2)Wheren=min(max(Qi,Ni),k)IDCG(D,k)=∑min(|D|,k)−1j=01log(j+2)$$ | [NDCG at k](https://en.wikipedia.org/wiki/Discounted\_cumulative\_gain#Normalized\_DCG) is a measure of how many of the first k recommended documents are in the set of true relevant documents averaged across all users. In contrast to precision at k, this metric takes into account the order of the recommendations (documents are assumed to be in order of decreasing relevance). |

**Examples**

The following code snippets illustrate how to load a sample dataset, train an alternating least squares recommendation model on the data, and evaluate the performance of the recommender by several ranking metrics. A brief summary of the methodology is provided below.

MovieLens ratings are on a scale of 1-5:

* 5: Must see
* 4: Will enjoy
* 3: It’s okay
* 2: Fairly bad
* 1: Awful

So we should not recommend a movie if the predicted rating is less than 3. To map ratings to confidence scores, we use:

* 5 -> 2.5
* 4 -> 1.5
* 3 -> 0.5
* 2 -> -0.5
* 1 -> -1.5.

This mappings means unobserved entries are generally between It’s okay and Fairly bad. The semantics of 0 in this expanded world of non-positive weights are “the same as never having interacted at all.”

* Python

Refer to the `RegressionMetrics` Python docs and `RankingMetrics` Python docs for more details on the API.

```
from pyspark.mllib.recommendation import ALS, Rating
from pyspark.mllib.evaluation import RegressionMetrics

# Read in the ratings data
lines = sc.textFile("data/mllib/sample_movielens_data.txt")

def parseLine(line):
    fields = line.split("::")
    return Rating(int(fields[0]), int(fields[1]), float(fields[2]) - 2.5)
ratings = lines.map(lambda r: parseLine(r))

# Train a model on to predict user-product ratings
model = ALS.train(ratings, 10, 10, 0.01)

# Get predicted ratings on all existing user-product pairs
testData = ratings.map(lambda p: (p.user, p.product))
predictions = model.predictAll(testData).map(lambda r: ((r.user, r.product), r.rating))

ratingsTuple = ratings.map(lambda r: ((r.user, r.product), r.rating))
scoreAndLabels = predictions.join(ratingsTuple).map(lambda tup: tup[1])

# Instantiate regression metrics to compare predicted and actual ratings
metrics = RegressionMetrics(scoreAndLabels)

# Root mean squared error
print("RMSE = %s" % metrics.rootMeanSquaredError)

# R-squared
print("R-squared = %s" % metrics.r2)
```

Find full example code at "examples/src/main/python/mllib/ranking\_metrics\_example.py" in the Spark repo.

* Scala

Refer to the `RegressionMetrics` Scala docs and `RankingMetrics` Scala docs for details on the API.

```
import org.apache.spark.mllib.evaluation.{RankingMetrics, RegressionMetrics}
import org.apache.spark.mllib.recommendation.{ALS, Rating}

// Read in the ratings data
val ratings = spark.read.textFile("data/mllib/sample_movielens_data.txt").rdd.map { line =>
  val fields = line.split("::")
  Rating(fields(0).toInt, fields(1).toInt, fields(2).toDouble - 2.5)
}.cache()

// Map ratings to 1 or 0, 1 indicating a movie that should be recommended
val binarizedRatings = ratings.map(r => Rating(r.user, r.product,
  if (r.rating > 0) 1.0 else 0.0)).cache()

// Summarize ratings
val numRatings = ratings.count()
val numUsers = ratings.map(_.user).distinct().count()
val numMovies = ratings.map(_.product).distinct().count()
println(s"Got $numRatings ratings from $numUsers users on $numMovies movies.")

// Build the model
val numIterations = 10
val rank = 10
val lambda = 0.01
val model = ALS.train(ratings, rank, numIterations, lambda)

// Define a function to scale ratings from 0 to 1
def scaledRating(r: Rating): Rating = {
  val scaledRating = math.max(math.min(r.rating, 1.0), 0.0)
  Rating(r.user, r.product, scaledRating)
}

// Get sorted top ten predictions for each user and then scale from [0, 1]
val userRecommended = model.recommendProductsForUsers(10).map { case (user, recs) =>
  (user, recs.map(scaledRating))
}

// Assume that any movie a user rated 3 or higher (which maps to a 1) is a relevant document
// Compare with top ten most relevant documents
val userMovies = binarizedRatings.groupBy(_.user)
val relevantDocuments = userMovies.join(userRecommended).map { case (user, (actual,
predictions)) =>
  (predictions.map(_.product), actual.filter(_.rating > 0.0).map(_.product).toArray)
}

// Instantiate metrics object
val metrics = new RankingMetrics(relevantDocuments)

// Precision at K
Array(1, 3, 5).foreach { k =>
  println(s"Precision at $k = ${metrics.precisionAt(k)}")
}

// Mean average precision
println(s"Mean average precision = ${metrics.meanAveragePrecision}")

// Mean average precision at k
println(s"Mean average precision at 2 = ${metrics.meanAveragePrecisionAt(2)}")

// Normalized discounted cumulative gain
Array(1, 3, 5).foreach { k =>
  println(s"NDCG at $k = ${metrics.ndcgAt(k)}")
}

// Recall at K
Array(1, 3, 5).foreach { k =>
  println(s"Recall at $k = ${metrics.recallAt(k)}")
}

// Get predictions for each data point
val allPredictions = model.predict(ratings.map(r => (r.user, r.product))).map(r => ((r.user,
  r.product), r.rating))
val allRatings = ratings.map(r => ((r.user, r.product), r.rating))
val predictionsAndLabels = allPredictions.join(allRatings).map { case ((user, product),
(predicted, actual)) =>
  (predicted, actual)
}

// Get the RMSE using regression metrics
val regressionMetrics = new RegressionMetrics(predictionsAndLabels)
println(s"RMSE = ${regressionMetrics.rootMeanSquaredError}")

// R-squared
println(s"R-squared = ${regressionMetrics.r2}")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/RankingMetricsExample.scala" in the Spark repo.

* Java

Refer to the `RegressionMetrics` Java docs and `RankingMetrics` Java docs for details on the API.

```
import java.util.*;

import scala.Tuple2;

import org.apache.spark.api.java.*;
import org.apache.spark.mllib.evaluation.RegressionMetrics;
import org.apache.spark.mllib.evaluation.RankingMetrics;
import org.apache.spark.mllib.recommendation.ALS;
import org.apache.spark.mllib.recommendation.MatrixFactorizationModel;
import org.apache.spark.mllib.recommendation.Rating;

String path = "data/mllib/sample_movielens_data.txt";
JavaRDD<String> data = sc.textFile(path);
JavaRDD<Rating> ratings = data.map(line -> {
    String[] parts = line.split("::");
    return new Rating(Integer.parseInt(parts[0]), Integer.parseInt(parts[1]), Double
        .parseDouble(parts[2]) - 2.5);
  });
ratings.cache();

// Train an ALS model
MatrixFactorizationModel model = ALS.train(JavaRDD.toRDD(ratings), 10, 10, 0.01);

// Get top 10 recommendations for every user and scale ratings from 0 to 1
JavaRDD<Tuple2<Object, Rating[]>> userRecs = model.recommendProductsForUsers(10).toJavaRDD();
JavaRDD<Tuple2<Object, Rating[]>> userRecsScaled = userRecs.map(t -> {
    Rating[] scaledRatings = new Rating[t._2().length];
    for (int i = 0; i < scaledRatings.length; i++) {
      double newRating = Math.max(Math.min(t._2()[i].rating(), 1.0), 0.0);
      scaledRatings[i] = new Rating(t._2()[i].user(), t._2()[i].product(), newRating);
    }
    return new Tuple2<>(t._1(), scaledRatings);
  });
JavaPairRDD<Object, Rating[]> userRecommended = JavaPairRDD.fromJavaRDD(userRecsScaled);

// Map ratings to 1 or 0, 1 indicating a movie that should be recommended
JavaRDD<Rating> binarizedRatings = ratings.map(r -> {
    double binaryRating;
    if (r.rating() > 0.0) {
      binaryRating = 1.0;
    } else {
      binaryRating = 0.0;
    }
    return new Rating(r.user(), r.product(), binaryRating);
  });

// Group ratings by common user
JavaPairRDD<Object, Iterable<Rating>> userMovies = binarizedRatings.groupBy(Rating::user);

// Get true relevant documents from all user ratings
JavaPairRDD<Object, List<Integer>> userMoviesList = userMovies.mapValues(docs -> {
    List<Integer> products = new ArrayList<>();
    for (Rating r : docs) {
      if (r.rating() > 0.0) {
        products.add(r.product());
      }
    }
    return products;
  });

// Extract the product id from each recommendation
JavaPairRDD<Object, List<Integer>> userRecommendedList = userRecommended.mapValues(docs -> {
    List<Integer> products = new ArrayList<>();
    for (Rating r : docs) {
      products.add(r.product());
    }
    return products;
  });
JavaRDD<Tuple2<List<Integer>, List<Integer>>> relevantDocs = userMoviesList.join(
  userRecommendedList).values();

// Instantiate the metrics object
RankingMetrics<Integer> metrics = RankingMetrics.of(relevantDocs);

// Precision, NDCG and Recall at k
Integer[] kVector = {1, 3, 5};
for (Integer k : kVector) {
  System.out.format("Precision at %d = %f\n", k, metrics.precisionAt(k));
  System.out.format("NDCG at %d = %f\n", k, metrics.ndcgAt(k));
  System.out.format("Recall at %d = %f\n", k, metrics.recallAt(k));
}

// Mean average precision
System.out.format("Mean average precision = %f\n", metrics.meanAveragePrecision());

//Mean average precision at k
System.out.format("Mean average precision at 2 = %f\n", metrics.meanAveragePrecisionAt(2));

// Evaluate the model using numerical ratings and regression metrics
JavaRDD<Tuple2<Object, Object>> userProducts =
    ratings.map(r -> new Tuple2<>(r.user(), r.product()));

JavaPairRDD<Tuple2<Integer, Integer>, Object> predictions = JavaPairRDD.fromJavaRDD(
  model.predict(JavaRDD.toRDD(userProducts)).toJavaRDD().map(r ->
    new Tuple2<>(new Tuple2<>(r.user(), r.product()), r.rating())));
JavaRDD<Tuple2<Object, Object>> ratesAndPreds =
  JavaPairRDD.fromJavaRDD(ratings.map(r ->
    new Tuple2<Tuple2<Integer, Integer>, Object>(
      new Tuple2<>(r.user(), r.product()),
      r.rating())
  )).join(predictions).values();

// Create regression metrics object
RegressionMetrics regressionMetrics = new RegressionMetrics(ratesAndPreds.rdd());

// Root mean squared error
System.out.format("RMSE = %f\n", regressionMetrics.rootMeanSquaredError());

// R-squared
System.out.format("R-squared = %f\n", regressionMetrics.r2());
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaRankingMetricsExample.java" in the Spark repo.

### Regression model evaluation

[Regression analysis](https://en.wikipedia.org/wiki/Regression\_analysis) is used when predicting a continuous output variable from a number of independent variables.

**Available metrics**

| Metric                                    | Definition                                                 |
| ----------------------------------------- | ---------------------------------------------------------- |
| Mean Squared Error (MSE)                  | $$MSE=∑N−1i=0(yi−y^i)2N$$                                  |
| Root Mean Squared Error (RMSE)            | $$RMSE=√∑N−1i=0(yi−y^i)2N$$                                |
| Mean Absolute Error (MAE)                 | $$MAE=1N∑N−1i=0|yi−y^i|$$                                  |
| Coefficient of Determination $$(R2)$$(�2) | $$R2=1−MSEVAR(y)⋅(N−1)=1−∑N−1i=0(yi−y^i)2∑N−1i=0(yi−y¯)2$$ |
| Explained Variance                        | $$1−VAR(y−y^)VAR(y)$$                                      |
