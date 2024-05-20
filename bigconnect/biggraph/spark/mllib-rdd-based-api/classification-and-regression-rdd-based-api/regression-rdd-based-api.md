# Regression - RDD-based API

### Isotonic regression

[Isotonic regression](http://en.wikipedia.org/wiki/Isotonic\_regression) belongs to the family of regression algorithms. Formally isotonic regression is a problem where given a finite set of real numbers $$Y=y1,y2,...,yn$$ representing observed responses and $$X=x1,x2,...,xn$$ the unknown response values to be fitted finding a function that minimizes

$$f(x)=∑i=1nwi(yi−xi)2(1)$$

with respect to complete order subject to $$x1≤x2≤...≤xn$$ where $$wi$$ are positive weights. The resulting function is called isotonic regression and it is unique. It can be viewed as least squares problem under order restriction. Essentially isotonic regression is a [monotonic function](http://en.wikipedia.org/wiki/Monotonic\_function) best fitting the original data points.

`spark.mllib` supports a [pool adjacent violators algorithm](https://doi.org/10.1198/TECH.2010.10111) which uses an approach to [parallelizing isotonic regression](https://doi.org/10.1007/978-3-642-99789-1\_10). The training input is an RDD of tuples of three double values that represent label, feature and weight in this order. In case there are multiple tuples with the same feature then these tuples are aggregated into a single tuple as follows:

* Aggregated label is the weighted average of all labels.
* Aggregated feature is the unique feature value.
* Aggregated weight is the sum of all weights.

Additionally, IsotonicRegression algorithm has one optional parameter called $$isotonic$$ defaulting to true. This argument specifies if the isotonic regression is isotonic (monotonically increasing) or antitonic (monotonically decreasing).

Training returns an IsotonicRegressionModel that can be used to predict labels for both known and unknown features. The result of isotonic regression is treated as piecewise linear function. The rules for prediction therefore are:

* If the prediction input exactly matches a training feature then associated prediction is returned.
* If the prediction input is lower or higher than all training features then prediction with lowest or highest feature is returned respectively.
* If the prediction input falls between two training features then prediction is treated as piecewise linear function and interpolated value is calculated from the predictions of the two closest features.

#### Examples

* Python

Data are read from a file where each line has a format label,feature i.e. 4710.28,500.00. The data are split to training and testing set. Model is created using the training set and a mean squared error is calculated from the predicted labels and real labels in the test set.

Refer to the `IsotonicRegression` Python docs and `IsotonicRegressionModel` Python docs for more details on the API.

```
import math
from pyspark.mllib.regression import IsotonicRegression, IsotonicRegressionModel
from pyspark.mllib.util import MLUtils

# Load and parse the data
def parsePoint(labeledData):
    return (labeledData.label, labeledData.features[0], 1.0)

data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_isotonic_regression_libsvm_data.txt")

# Create label, feature, weight tuples from input data with weight set to default value 1.0.
parsedData = data.map(parsePoint)

# Split data into training (60%) and test (40%) sets.
training, test = parsedData.randomSplit([0.6, 0.4], 11)

# Create isotonic regression model from training data.
# Isotonic parameter defaults to true so it is only shown for demonstration
model = IsotonicRegression.train(training)

# Create tuples of predicted and real labels.
predictionAndLabel = test.map(lambda p: (model.predict(p[1]), p[0]))

# Calculate mean squared error between predicted and real labels.
meanSquaredError = predictionAndLabel.map(lambda pl: math.pow((pl[0] - pl[1]), 2)).mean()
print("Mean Squared Error = " + str(meanSquaredError))

# Save and load model
model.save(sc, "target/tmp/myIsotonicRegressionModel")
sameModel = IsotonicRegressionModel.load(sc, "target/tmp/myIsotonicRegressionModel")
```

Find full example code at "examples/src/main/python/mllib/isotonic\_regression\_example.py" in the Spark repo.

* Scala

Data are read from a file where each line has a format label,feature i.e. 4710.28,500.00. The data are split to training and testing set. Model is created using the training set and a mean squared error is calculated from the predicted labels and real labels in the test set.

Refer to the `IsotonicRegression` Scala docs and `IsotonicRegressionModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.regression.{IsotonicRegression, IsotonicRegressionModel}
import org.apache.spark.mllib.util.MLUtils

val data = MLUtils.loadLibSVMFile(sc,
  "data/mllib/sample_isotonic_regression_libsvm_data.txt").cache()

// Create label, feature, weight tuples from input data with weight set to default value 1.0.
val parsedData = data.map { labeledPoint =>
  (labeledPoint.label, labeledPoint.features(0), 1.0)
}

// Split data into training (60%) and test (40%) sets.
val splits = parsedData.randomSplit(Array(0.6, 0.4), seed = 11L)
val training = splits(0)
val test = splits(1)

// Create isotonic regression model from training data.
// Isotonic parameter defaults to true so it is only shown for demonstration
val model = new IsotonicRegression().setIsotonic(true).run(training)

// Create tuples of predicted and real labels.
val predictionAndLabel = test.map { point =>
  val predictedLabel = model.predict(point._2)
  (predictedLabel, point._1)
}

// Calculate mean squared error between predicted and real labels.
val meanSquaredError = predictionAndLabel.map { case (p, l) => math.pow((p - l), 2) }.mean()
println(s"Mean Squared Error = $meanSquaredError")

// Save and load model
model.save(sc, "target/tmp/myIsotonicRegressionModel")
val sameModel = IsotonicRegressionModel.load(sc, "target/tmp/myIsotonicRegressionModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/IsotonicRegressionExample.scala" in the Spark repo.

* Java

Data are read from a file where each line has a format label,feature i.e. 4710.28,500.00. The data are split to training and testing set. Model is created using the training set and a mean squared error is calculated from the predicted labels and real labels in the test set.

Refer to the `IsotonicRegression` Java docs and `IsotonicRegressionModel` Java docs for details on the API.

```
import scala.Tuple2;
import scala.Tuple3;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.regression.IsotonicRegression;
import org.apache.spark.mllib.regression.IsotonicRegressionModel;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.util.MLUtils;

JavaRDD<LabeledPoint> data = MLUtils.loadLibSVMFile(
  jsc.sc(), "data/mllib/sample_isotonic_regression_libsvm_data.txt").toJavaRDD();

// Create label, feature, weight tuples from input data with weight set to default value 1.0.
JavaRDD<Tuple3<Double, Double, Double>> parsedData = data.map(point ->
  new Tuple3<>(point.label(), point.features().apply(0), 1.0));

// Split data into training (60%) and test (40%) sets.
JavaRDD<Tuple3<Double, Double, Double>>[] splits =
  parsedData.randomSplit(new double[]{0.6, 0.4}, 11L);
JavaRDD<Tuple3<Double, Double, Double>> training = splits[0];
JavaRDD<Tuple3<Double, Double, Double>> test = splits[1];

// Create isotonic regression model from training data.
// Isotonic parameter defaults to true so it is only shown for demonstration
IsotonicRegressionModel model = new IsotonicRegression().setIsotonic(true).run(training);

// Create tuples of predicted and real labels.
JavaPairRDD<Double, Double> predictionAndLabel = test.mapToPair(point ->
  new Tuple2<>(model.predict(point._2()), point._1()));

// Calculate mean squared error between predicted and real labels.
double meanSquaredError = predictionAndLabel.mapToDouble(pl -> {
  double diff = pl._1() - pl._2();
  return diff * diff;
}).mean();
System.out.println("Mean Squared Error = " + meanSquaredError);

// Save and load model
model.save(jsc.sc(), "target/tmp/myIsotonicRegressionModel");
IsotonicRegressionModel sameModel =
  IsotonicRegressionModel.load(jsc.sc(), "target/tmp/myIsotonicRegressionModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaIsotonicRegressionExample.java" in the Spark repo.
