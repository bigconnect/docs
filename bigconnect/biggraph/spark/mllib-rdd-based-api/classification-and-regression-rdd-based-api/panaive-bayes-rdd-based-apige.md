# PaNaive Bayes - RDD-based APIge

Naive Bayes is a simple multiclass classification algorithm with the assumption of independence between every pair of features. Naive Bayes can be trained very efficiently. Within a single pass to the training data, it computes the conditional probability distribution of each feature given label, and then it applies Bayes’ theorem to compute the conditional probability distribution of label given an observation and use it for prediction.

`spark.mllib` supports multinomial naive Bayes and Bernoulli naive Bayes. These models are typically used for document classification. Within that context, each observation is a document and each feature represents a term whose value is the frequency of the term (in multinomial naive Bayes) or a zero or one indicating whether the term was found in the document (in Bernoulli naive Bayes). Feature values must be nonnegative. The model type is selected with an optional parameter “multinomial” or “bernoulli” with “multinomial” as the default. Additive smoothing can be used by setting the parameter $$λ$$ (default to $$1.0$$1.0). For document classification, the input feature vectors are usually sparse, and sparse vectors should be supplied as input to take advantage of sparsity. Since the training data is only used once, it is not necessary to cache it.

### Examples

* Python

NaiveBayes implements multinomial naive Bayes. It takes an RDD of LabeledPoint and an optionally smoothing parameter `lambda` as input, and output a NaiveBayesModel, which can be used for evaluation and prediction.

Note that the Python API does not yet support model save/load but will in the future.

Refer to the `NaiveBayes` Python docs and `NaiveBayesModel` Python docs for more details on the API.

```
from pyspark.mllib.classification import NaiveBayes, NaiveBayesModel
from pyspark.mllib.util import MLUtils



# Load and parse the data file.
data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")

# Split data approximately into training (60%) and test (40%)
training, test = data.randomSplit([0.6, 0.4])

# Train a naive Bayes model.
model = NaiveBayes.train(training, 1.0)

# Make prediction and test accuracy.
predictionAndLabel = test.map(lambda p: (model.predict(p.features), p.label))
accuracy = 1.0 * predictionAndLabel.filter(lambda pl: pl[0] == pl[1]).count() / test.count()
print('model accuracy {}'.format(accuracy))

# Save and load model
output_dir = 'target/tmp/myNaiveBayesModel'
shutil.rmtree(output_dir, ignore_errors=True)
model.save(sc, output_dir)
sameModel = NaiveBayesModel.load(sc, output_dir)
predictionAndLabel = test.map(lambda p: (sameModel.predict(p.features), p.label))
accuracy = 1.0 * predictionAndLabel.filter(lambda pl: pl[0] == pl[1]).count() / test.count()
print('sameModel accuracy {}'.format(accuracy))
```

Find full example code at "examples/src/main/python/mllib/naive\_bayes\_example.py" in the Spark repo.

* Scala

NaiveBayes implements multinomial naive Bayes. It takes an RDD of LabeledPoint and an optional smoothing parameter `lambda` as input, an optional model type parameter (default is “multinomial”), and outputs a NaiveBayesModel, which can be used for evaluation and prediction.

Refer to the `NaiveBayes` Scala docs and `NaiveBayesModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.classification.{NaiveBayes, NaiveBayesModel}
import org.apache.spark.mllib.util.MLUtils

// Load and parse the data file.
val data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")

// Split data into training (60%) and test (40%).
val Array(training, test) = data.randomSplit(Array(0.6, 0.4))

val model = NaiveBayes.train(training, lambda = 1.0, modelType = "multinomial")

val predictionAndLabel = test.map(p => (model.predict(p.features), p.label))
val accuracy = 1.0 * predictionAndLabel.filter(x => x._1 == x._2).count() / test.count()

// Save and load model
model.save(sc, "target/tmp/myNaiveBayesModel")
val sameModel = NaiveBayesModel.load(sc, "target/tmp/myNaiveBayesModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/NaiveBayesExample.scala" in the Spark repo.

* Java

NaiveBayes implements multinomial naive Bayes. It takes a Scala RDD of LabeledPoint and an optionally smoothing parameter `lambda` as input, and output a NaiveBayesModel, which can be used for evaluation and prediction.

Refer to the `NaiveBayes` Java docs and `NaiveBayesModel` Java docs for details on the API.

```
import scala.Tuple2;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.mllib.classification.NaiveBayes;
import org.apache.spark.mllib.classification.NaiveBayesModel;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.util.MLUtils;

String path = "data/mllib/sample_libsvm_data.txt";
JavaRDD<LabeledPoint> inputData = MLUtils.loadLibSVMFile(jsc.sc(), path).toJavaRDD();
JavaRDD<LabeledPoint>[] tmp = inputData.randomSplit(new double[]{0.6, 0.4});
JavaRDD<LabeledPoint> training = tmp[0]; // training set
JavaRDD<LabeledPoint> test = tmp[1]; // test set
NaiveBayesModel model = NaiveBayes.train(training.rdd(), 1.0);
JavaPairRDD<Double, Double> predictionAndLabel =
  test.mapToPair(p -> new Tuple2<>(model.predict(p.features()), p.label()));
double accuracy =
  predictionAndLabel.filter(pl -> pl._1().equals(pl._2())).count() / (double) test.count();

// Save and load model
model.save(jsc.sc(), "target/tmp/myNaiveBayesModel");
NaiveBayesModel sameModel = NaiveBayesModel.load(jsc.sc(), "target/tmp/myNaiveBayesModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaNaiveBayesExample.java" in the Spark repo.
