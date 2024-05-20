# Basic Statistics - RDD-based API

* Summary statistics
* Correlations
* Stratified sampling
* Hypothesis testing
  * Streaming Significance Testing
* Random data generation
* Kernel density estimation

### Summary statistics

We provide column summary statistics for `RDD[Vector]` through the function `colStats` available in `Statistics`.

* Python

`colStats()` returns an instance of `MultivariateStatisticalSummary`, which contains the column-wise max, min, mean, variance, and number of nonzeros, as well as the total count.

Refer to the `MultivariateStatisticalSummary` Python docs for more details on the API.

```
import numpy as np

from pyspark.mllib.stat import Statistics

mat = sc.parallelize(
    [np.array([1.0, 10.0, 100.0]), np.array([2.0, 20.0, 200.0]), np.array([3.0, 30.0, 300.0])]
)  # an RDD of Vectors

# Compute column summary statistics.
summary = Statistics.colStats(mat)
print(summary.mean())  # a dense vector containing the mean value for each column
print(summary.variance())  # column-wise variance
print(summary.numNonzeros())  # number of nonzeros in each column
```

Find full example code at "examples/src/main/python/mllib/summary\_statistics\_example.py" in the Spark repo.

* Scala

`colStats()` returns an instance of `MultivariateStatisticalSummary`, which contains the column-wise max, min, mean, variance, and number of nonzeros, as well as the total count.

Refer to the `MultivariateStatisticalSummary` Scala docs for details on the API.

```
import org.apache.spark.mllib.linalg.Vectors
import org.apache.spark.mllib.stat.{MultivariateStatisticalSummary, Statistics}

val observations = sc.parallelize(
  Seq(
    Vectors.dense(1.0, 10.0, 100.0),
    Vectors.dense(2.0, 20.0, 200.0),
    Vectors.dense(3.0, 30.0, 300.0)
  )
)

// Compute column summary statistics.
val summary: MultivariateStatisticalSummary = Statistics.colStats(observations)
println(summary.mean)  // a dense vector containing the mean value for each column
println(summary.variance)  // column-wise variance
println(summary.numNonzeros)  // number of nonzeros in each column
```

* Java

`colStats()` returns an instance of `MultivariateStatisticalSummary`, which contains the column-wise max, min, mean, variance, and number of nonzeros, as well as the total count.

Refer to the `MultivariateStatisticalSummary` Java docs for details on the API.

```
import java.util.Arrays;

import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;
import org.apache.spark.mllib.stat.MultivariateStatisticalSummary;
import org.apache.spark.mllib.stat.Statistics;

JavaRDD<Vector> mat = jsc.parallelize(
  Arrays.asList(
    Vectors.dense(1.0, 10.0, 100.0),
    Vectors.dense(2.0, 20.0, 200.0),
    Vectors.dense(3.0, 30.0, 300.0)
  )
); // an RDD of Vectors

// Compute column summary statistics.
MultivariateStatisticalSummary summary = Statistics.colStats(mat.rdd());
System.out.println(summary.mean());  // a dense vector containing the mean value for each column
System.out.println(summary.variance());  // column-wise variance
System.out.println(summary.numNonzeros());  // number of nonzeros in each column
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaSummaryStatisticsExample.java" in the Spark repo.

### Correlations

Calculating the correlation between two series of data is a common operation in Statistics. In `spark.mllib` we provide the flexibility to calculate pairwise correlations among many series. The supported correlation methods are currently Pearson’s and Spearman’s correlation.

* Python

`Statistics` provides methods to calculate correlations between series. Depending on the type of input, two `RDD[Double]`s or an `RDD[Vector]`, the output will be a `Double` or the correlation `Matrix` respectively.

Refer to the `Statistics` Python docs for more details on the API.

```
from pyspark.mllib.stat import Statistics

seriesX = sc.parallelize([1.0, 2.0, 3.0, 3.0, 5.0])  # a series
# seriesY must have the same number of partitions and cardinality as seriesX
seriesY = sc.parallelize([11.0, 22.0, 33.0, 33.0, 555.0])

# Compute the correlation using Pearson's method. Enter "spearman" for Spearman's method.
# If a method is not specified, Pearson's method will be used by default.
print("Correlation is: " + str(Statistics.corr(seriesX, seriesY, method="pearson")))

data = sc.parallelize(
    [np.array([1.0, 10.0, 100.0]), np.array([2.0, 20.0, 200.0]), np.array([5.0, 33.0, 366.0])]
)  # an RDD of Vectors

# calculate the correlation matrix using Pearson's method. Use "spearman" for Spearman's method.
# If a method is not specified, Pearson's method will be used by default.
print(Statistics.corr(data, method="pearson"))
```

Find full example code at "examples/src/main/python/mllib/correlations\_example.py" in the Spark repo.

* Scala

`Statistics` provides methods to calculate correlations between series. Depending on the type of input, two `RDD[Double]`s or an `RDD[Vector]`, the output will be a `Double` or the correlation `Matrix` respectively.

Refer to the `Statistics` Scala docs for details on the API.

```
import org.apache.spark.mllib.linalg._
import org.apache.spark.mllib.stat.Statistics
import org.apache.spark.rdd.RDD

val seriesX: RDD[Double] = sc.parallelize(Array(1, 2, 3, 3, 5))  // a series
// must have the same number of partitions and cardinality as seriesX
val seriesY: RDD[Double] = sc.parallelize(Array(11, 22, 33, 33, 555))

// compute the correlation using Pearson's method. Enter "spearman" for Spearman's method. If a
// method is not specified, Pearson's method will be used by default.
val correlation: Double = Statistics.corr(seriesX, seriesY, "pearson")
println(s"Correlation is: $correlation")

val data: RDD[Vector] = sc.parallelize(
  Seq(
    Vectors.dense(1.0, 10.0, 100.0),
    Vectors.dense(2.0, 20.0, 200.0),
    Vectors.dense(5.0, 33.0, 366.0))
)  // note that each Vector is a row and not a column

// calculate the correlation matrix using Pearson's method. Use "spearman" for Spearman's method
// If a method is not specified, Pearson's method will be used by default.
val correlMatrix: Matrix = Statistics.corr(data, "pearson")
println(correlMatrix.toString)
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/CorrelationsExample.scala" in the Spark repo.

* Java

`Statistics` provides methods to calculate correlations between series. Depending on the type of input, two `JavaDoubleRDD`s or a `JavaRDD<Vector>`, the output will be a `Double` or the correlation `Matrix` respectively.

Refer to the `Statistics` Java docs for details on the API.

```
import java.util.Arrays;

import org.apache.spark.api.java.JavaDoubleRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.linalg.Matrix;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;
import org.apache.spark.mllib.stat.Statistics;

JavaDoubleRDD seriesX = jsc.parallelizeDoubles(
  Arrays.asList(1.0, 2.0, 3.0, 3.0, 5.0));  // a series

// must have the same number of partitions and cardinality as seriesX
JavaDoubleRDD seriesY = jsc.parallelizeDoubles(
  Arrays.asList(11.0, 22.0, 33.0, 33.0, 555.0));

// compute the correlation using Pearson's method. Enter "spearman" for Spearman's method.
// If a method is not specified, Pearson's method will be used by default.
double correlation = Statistics.corr(seriesX.srdd(), seriesY.srdd(), "pearson");
System.out.println("Correlation is: " + correlation);

// note that each Vector is a row and not a column
JavaRDD<Vector> data = jsc.parallelize(
  Arrays.asList(
    Vectors.dense(1.0, 10.0, 100.0),
    Vectors.dense(2.0, 20.0, 200.0),
    Vectors.dense(5.0, 33.0, 366.0)
  )
);

// calculate the correlation matrix using Pearson's method.
// Use "spearman" for Spearman's method.
// If a method is not specified, Pearson's method will be used by default.
Matrix correlMatrix = Statistics.corr(data.rdd(), "pearson");
System.out.println(correlMatrix.toString());
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaCorrelationsExample.java" in the Spark repo.

### Stratified sampling

Unlike the other statistics functions, which reside in `spark.mllib`, stratified sampling methods, `sampleByKey` and `sampleByKeyExact`, can be performed on RDD’s of key-value pairs. For stratified sampling, the keys can be thought of as a label and the value as a specific attribute. For example the key can be man or woman, or document ids, and the respective values can be the list of ages of the people in the population or the list of words in the documents. The `sampleByKey` method will flip a coin to decide whether an observation will be sampled or not, therefore requires one pass over the data, and provides an _expected_ sample size. `sampleByKeyExact` requires significant more resources than the per-stratum simple random sampling used in `sampleByKey`, but will provide the exact sampling size with 99.99% confidence. `sampleByKeyExact` is currently not supported in python.

* Python

`sampleByKey()` allows users to sample approximately $$⌈fk⋅nk⌉∀k∈K$$ items, where $$fk$$ is the desired fraction for key $$k$$, $$nk$$ is the number of key-value pairs for key $$k$$, and $$K$$ is the set of keys.

_Note:_ `sampleByKeyExact()` is currently not supported in Python.

```
# an RDD of any key value pairs
data = sc.parallelize([(1, 'a'), (1, 'b'), (2, 'c'), (2, 'd'), (2, 'e'), (3, 'f')])

# specify the exact fraction desired from each key as a dictionary
fractions = {1: 0.1, 2: 0.6, 3: 0.3}

approxSample = data.sampleByKey(False, fractions)
```

Find full example code at "examples/src/main/python/mllib/stratified\_sampling\_example.py" in the Spark repo.

* Scala

`sampleByKeyExact()` allows users to sample exactly $$⌈fk⋅nk⌉∀k∈K$$ items, where $$fk$$ is the desired fraction for key $$k$$, $$nk$$ is the number of key-value pairs for key $$k$$, and $$K$$ is the set of keys. Sampling without replacement requires one additional pass over the RDD to guarantee sample size, whereas sampling with replacement requires two additional passes.

```
// an RDD[(K, V)] of any key value pairs
val data = sc.parallelize(
  Seq((1, 'a'), (1, 'b'), (2, 'c'), (2, 'd'), (2, 'e'), (3, 'f')))

// specify the exact fraction desired from each key
val fractions = Map(1 -> 0.1, 2 -> 0.6, 3 -> 0.3)

// Get an approximate sample from each stratum
val approxSample = data.sampleByKey(withReplacement = false, fractions = fractions)
// Get an exact sample from each stratum
val exactSample = data.sampleByKeyExact(withReplacement = false, fractions = fractions)
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/StratifiedSamplingExample.scala" in the Spark repo.

* Java

`sampleByKeyExact()` allows users to sample exactly $$⌈fk⋅nk⌉∀k∈K$$items, where $$fk$$ is the desired fraction for key $$k$$, $$nk$$ is the number of key-value pairs for key $$k$$, and $$K$$ is the set of keys. Sampling without replacement requires one additional pass over the RDD to guarantee sample size, whereas sampling with replacement requires two additional passes.

```
import java.util.*;

import scala.Tuple2;

import org.apache.spark.api.java.JavaPairRDD;

List<Tuple2<Integer, Character>> list = Arrays.asList(
    new Tuple2<>(1, 'a'),
    new Tuple2<>(1, 'b'),
    new Tuple2<>(2, 'c'),
    new Tuple2<>(2, 'd'),
    new Tuple2<>(2, 'e'),
    new Tuple2<>(3, 'f')
);

JavaPairRDD<Integer, Character> data = jsc.parallelizePairs(list);

// specify the exact fraction desired from each key Map<K, Double>
ImmutableMap<Integer, Double> fractions = ImmutableMap.of(1, 0.1, 2, 0.6, 3, 0.3);

// Get an approximate sample from each stratum
JavaPairRDD<Integer, Character> approxSample = data.sampleByKey(false, fractions);
// Get an exact sample from each stratum
JavaPairRDD<Integer, Character> exactSample = data.sampleByKeyExact(false, fractions);
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaStratifiedSamplingExample.java" in the Spark repo.

### Hypothesis testing

Hypothesis testing is a powerful tool in statistics to determine whether a result is statistically significant, whether this result occurred by chance or not. `spark.mllib` currently supports Pearson’s chi-squared ( $$χ2$$) tests for goodness of fit and independence. The input data types determine whether the goodness of fit or the independence test is conducted. The goodness of fit test requires an input type of `Vector`, whereas the independence test requires a `Matrix` as input.

`spark.mllib` also supports the input type `RDD[LabeledPoint]` to enable feature selection via chi-squared independence tests.

* Python

`Statistics` provides methods to run Pearson’s chi-squared tests. The following example demonstrates how to run and interpret hypothesis tests.

Refer to the `Statistics` Python docs for more details on the API.

```
from pyspark.mllib.linalg import Matrices, Vectors
from pyspark.mllib.regression import LabeledPoint
from pyspark.mllib.stat import Statistics

vec = Vectors.dense(0.1, 0.15, 0.2, 0.3, 0.25)  # a vector composed of the frequencies of events

# compute the goodness of fit. If a second vector to test against
# is not supplied as a parameter, the test runs against a uniform distribution.
goodnessOfFitTestResult = Statistics.chiSqTest(vec)

# summary of the test including the p-value, degrees of freedom,
# test statistic, the method used, and the null hypothesis.
print("%s\n" % goodnessOfFitTestResult)

mat = Matrices.dense(3, 2, [1.0, 3.0, 5.0, 2.0, 4.0, 6.0])  # a contingency matrix

# conduct Pearson's independence test on the input contingency matrix
independenceTestResult = Statistics.chiSqTest(mat)

# summary of the test including the p-value, degrees of freedom,
# test statistic, the method used, and the null hypothesis.
print("%s\n" % independenceTestResult)

obs = sc.parallelize(
    [LabeledPoint(1.0, [1.0, 0.0, 3.0]),
     LabeledPoint(1.0, [1.0, 2.0, 0.0]),
     LabeledPoint(1.0, [-1.0, 0.0, -0.5])]
)  # LabeledPoint(label, feature)

# The contingency table is constructed from an RDD of LabeledPoint and used to conduct
# the independence test. Returns an array containing the ChiSquaredTestResult for every feature
# against the label.
featureTestResults = Statistics.chiSqTest(obs)

for i, result in enumerate(featureTestResults):
    print("Column %d:\n%s" % (i + 1, result))
```

Find full example code at "examples/src/main/python/mllib/hypothesis\_testing\_example.py" in the Spark repo.

* Scala

`Statistics` provides methods to run Pearson’s chi-squared tests. The following example demonstrates how to run and interpret hypothesis tests.

```
import org.apache.spark.mllib.linalg._
import org.apache.spark.mllib.regression.LabeledPoint
import org.apache.spark.mllib.stat.Statistics
import org.apache.spark.mllib.stat.test.ChiSqTestResult
import org.apache.spark.rdd.RDD

// a vector composed of the frequencies of events
val vec: Vector = Vectors.dense(0.1, 0.15, 0.2, 0.3, 0.25)

// compute the goodness of fit. If a second vector to test against is not supplied
// as a parameter, the test runs against a uniform distribution.
val goodnessOfFitTestResult = Statistics.chiSqTest(vec)
// summary of the test including the p-value, degrees of freedom, test statistic, the method
// used, and the null hypothesis.
println(s"$goodnessOfFitTestResult\n")

// a contingency matrix. Create a dense matrix ((1.0, 2.0), (3.0, 4.0), (5.0, 6.0))
val mat: Matrix = Matrices.dense(3, 2, Array(1.0, 3.0, 5.0, 2.0, 4.0, 6.0))

// conduct Pearson's independence test on the input contingency matrix
val independenceTestResult = Statistics.chiSqTest(mat)
// summary of the test including the p-value, degrees of freedom
println(s"$independenceTestResult\n")

val obs: RDD[LabeledPoint] =
  sc.parallelize(
    Seq(
      LabeledPoint(1.0, Vectors.dense(1.0, 0.0, 3.0)),
      LabeledPoint(1.0, Vectors.dense(1.0, 2.0, 0.0)),
      LabeledPoint(-1.0, Vectors.dense(-1.0, 0.0, -0.5)
      )
    )
  ) // (label, feature) pairs.

// The contingency table is constructed from the raw (label, feature) pairs and used to conduct
// the independence test. Returns an array containing the ChiSquaredTestResult for every feature
// against the label.
val featureTestResults: Array[ChiSqTestResult] = Statistics.chiSqTest(obs)
featureTestResults.zipWithIndex.foreach { case (k, v) =>
  println(s"Column ${(v + 1)} :")
  println(k)
}  // summary of the test
```

* Java

`Statistics` provides methods to run Pearson’s chi-squared tests. The following example demonstrates how to run and interpret hypothesis tests.

Refer to the `ChiSqTestResult` Java docs for details on the API.

```
import java.util.Arrays;

import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.linalg.Matrices;
import org.apache.spark.mllib.linalg.Matrix;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;
import org.apache.spark.mllib.regression.LabeledPoint;
import org.apache.spark.mllib.stat.Statistics;
import org.apache.spark.mllib.stat.test.ChiSqTestResult;

// a vector composed of the frequencies of events
Vector vec = Vectors.dense(0.1, 0.15, 0.2, 0.3, 0.25);

// compute the goodness of fit. If a second vector to test against is not supplied
// as a parameter, the test runs against a uniform distribution.
ChiSqTestResult goodnessOfFitTestResult = Statistics.chiSqTest(vec);
// summary of the test including the p-value, degrees of freedom, test statistic,
// the method used, and the null hypothesis.
System.out.println(goodnessOfFitTestResult + "\n");

// Create a contingency matrix ((1.0, 2.0), (3.0, 4.0), (5.0, 6.0))
Matrix mat = Matrices.dense(3, 2, new double[]{1.0, 3.0, 5.0, 2.0, 4.0, 6.0});

// conduct Pearson's independence test on the input contingency matrix
ChiSqTestResult independenceTestResult = Statistics.chiSqTest(mat);
// summary of the test including the p-value, degrees of freedom...
System.out.println(independenceTestResult + "\n");

// an RDD of labeled points
JavaRDD<LabeledPoint> obs = jsc.parallelize(
  Arrays.asList(
    new LabeledPoint(1.0, Vectors.dense(1.0, 0.0, 3.0)),
    new LabeledPoint(1.0, Vectors.dense(1.0, 2.0, 0.0)),
    new LabeledPoint(-1.0, Vectors.dense(-1.0, 0.0, -0.5))
  )
);

// The contingency table is constructed from the raw (label, feature) pairs and used to conduct
// the independence test. Returns an array containing the ChiSquaredTestResult for every feature
// against the label.
ChiSqTestResult[] featureTestResults = Statistics.chiSqTest(obs.rdd());
int i = 1;
for (ChiSqTestResult result : featureTestResults) {
  System.out.println("Column " + i + ":");
  System.out.println(result + "\n");  // summary of the test
  i++;
}
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaHypothesisTestingExample.java" in the Spark repo.

Additionally, `spark.mllib` provides a 1-sample, 2-sided implementation of the Kolmogorov-Smirnov (KS) test for equality of probability distributions. By providing the name of a theoretical distribution (currently solely supported for the normal distribution) and its parameters, or a function to calculate the cumulative distribution according to a given theoretical distribution, the user can test the null hypothesis that their sample is drawn from that distribution. In the case that the user tests against the normal distribution (`distName="norm"`), but does not provide distribution parameters, the test initializes to the standard normal distribution and logs an appropriate message.

* Python

[`Statistics`](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.mllib.stat.Statistics.html) provides methods to run a 1-sample, 2-sided Kolmogorov-Smirnov test. The following example demonstrates how to run and interpret the hypothesis tests.

Refer to the [`Statistics` Python docs](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.mllib.stat.Statistics.html) for more details on the API.

```
from pyspark.mllib.stat import Statistics

parallelData = sc.parallelize([0.1, 0.15, 0.2, 0.3, 0.25])

# run a KS test for the sample versus a standard normal distribution
testResult = Statistics.kolmogorovSmirnovTest(parallelData, "norm", 0, 1)
# summary of the test including the p-value, test statistic, and null hypothesis
# if our p-value indicates significance, we can reject the null hypothesis
# Note that the Scala functionality of calling Statistics.kolmogorovSmirnovTest with
# a lambda to calculate the CDF is not made available in the Python API
print(testResult)
```

Find full example code at "examples/src/main/python/mllib/hypothesis\_testing\_kolmogorov\_smirnov\_test\_example.py" in the Spark repo.

* Scala

`Statistics` provides methods to run a 1-sample, 2-sided Kolmogorov-Smirnov test. The following example demonstrates how to run and interpret the hypothesis tests.

Refer to the `Statistics` Scala docs for details on the API.

```
import org.apache.spark.mllib.stat.Statistics
import org.apache.spark.rdd.RDD

val data: RDD[Double] = sc.parallelize(Seq(0.1, 0.15, 0.2, 0.3, 0.25))  // an RDD of sample data

// run a KS test for the sample versus a standard normal distribution
val testResult = Statistics.kolmogorovSmirnovTest(data, "norm", 0, 1)
// summary of the test including the p-value, test statistic, and null hypothesis if our p-value
// indicates significance, we can reject the null hypothesis.
println(testResult)
println()

// perform a KS test using a cumulative distribution function of our making
val myCDF = Map(0.1 -> 0.2, 0.15 -> 0.6, 0.2 -> 0.05, 0.3 -> 0.05, 0.25 -> 0.1)
val testResult2 = Statistics.kolmogorovSmirnovTest(data, myCDF)
println(testResult2)
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/HypothesisTestingKolmogorovSmirnovTestExample.scala" in the Spark repo.

* Java

`Statistics` provides methods to run a 1-sample, 2-sided Kolmogorov-Smirnov test. The following example demonstrates how to run and interpret the hypothesis tests.

Refer to the `Statistics` Java docs for details on the API.

```
import java.util.Arrays;

import org.apache.spark.api.java.JavaDoubleRDD;
import org.apache.spark.mllib.stat.Statistics;
import org.apache.spark.mllib.stat.test.KolmogorovSmirnovTestResult;

JavaDoubleRDD data = jsc.parallelizeDoubles(Arrays.asList(0.1, 0.15, 0.2, 0.3, 0.25));
KolmogorovSmirnovTestResult testResult =
  Statistics.kolmogorovSmirnovTest(data, "norm", 0.0, 1.0);
// summary of the test including the p-value, test statistic, and null hypothesis
// if our p-value indicates significance, we can reject the null hypothesis
System.out.println(testResult);
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaHypothesisTestingKolmogorovSmirnovTestExample.java" in the Spark repo.

#### Streaming Significance Testing

`spark.mllib` provides online implementations of some tests to support use cases like A/B testing. These tests may be performed on a Spark Streaming `DStream[(Boolean, Double)]` where the first element of each tuple indicates control group (`false`) or treatment group (`true`) and the second element is the value of an observation.

Streaming significance testing supports the following parameters:

* `peacePeriod` - The number of initial data points from the stream to ignore, used to mitigate novelty effects.
* `windowSize` - The number of past batches to perform hypothesis testing over. Setting to `0` will perform cumulative processing using all prior batches.
* Scala
* Java

`StreamingTest` provides streaming hypothesis testing.

```
val data = ssc.textFileStream(dataDir).map(line => line.split(",") match {
  case Array(label, value) => BinarySample(label.toBoolean, value.toDouble)
})

val streamingTest = new StreamingTest()
  .setPeacePeriod(0)
  .setWindowSize(0)
  .setTestMethod("welch")

val out = streamingTest.registerStream(data)
out.print()
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/StreamingTestExample.scala" in the Spark repo.

* Java

`StreamingTest` provides streaming hypothesis testing.

```
import org.apache.spark.mllib.stat.test.BinarySample;
import org.apache.spark.mllib.stat.test.StreamingTest;
import org.apache.spark.mllib.stat.test.StreamingTestResult;

JavaDStream<BinarySample> data = ssc.textFileStream(dataDir).map(line -> {
  String[] ts = line.split(",");
  boolean label = Boolean.parseBoolean(ts[0]);
  double value = Double.parseDouble(ts[1]);
  return new BinarySample(label, value);
});

StreamingTest streamingTest = new StreamingTest()
  .setPeacePeriod(0)
  .setWindowSize(0)
  .setTestMethod("welch");

JavaDStream<StreamingTestResult> out = streamingTest.registerStream(data);
out.print();
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaStreamingTestExample.java" in the Spark repo.

### Random data generation

Random data generation is useful for randomized algorithms, prototyping, and performance testing. `spark.mllib` supports generating random RDDs with i.i.d. values drawn from a given distribution: uniform, standard normal, or Poisson.

* Python

`RandomRDDs` provides factory methods to generate random double RDDs or vector RDDs. The following example generates a random double RDD, whose values follows the standard normal distribution `N(0, 1)`, and then map it to `N(1, 4)`.

Refer to the `RandomRDDs` Python docs for more details on the API.

```python
from pyspark.mllib.random import RandomRDDs

sc = ... # SparkContext

# Generate a random double RDD that contains 1 million i.i.d. values drawn from the
# standard normal distribution `N(0, 1)`, evenly distributed in 10 partitions.
u = RandomRDDs.normalRDD(sc, 1000000L, 10)
# Apply a transform to get a random double RDD following `N(1, 4)`.
v = u.map(lambda x: 1.0 + 2.0 * x)
```

* Scala

`RandomRDDs` provides factory methods to generate random double RDDs or vector RDDs. The following example generates a random double RDD, whose values follows the standard normal distribution `N(0, 1)`, and then map it to `N(1, 4)`.

Refer to the `RandomRDDs` Scala docs for details on the API.

```scala
import org.apache.spark.SparkContext
import org.apache.spark.mllib.random.RandomRDDs._

val sc: SparkContext = ...

// Generate a random double RDD that contains 1 million i.i.d. values drawn from the
// standard normal distribution `N(0, 1)`, evenly distributed in 10 partitions.
val u = normalRDD(sc, 1000000L, 10)
// Apply a transform to get a random double RDD following `N(1, 4)`.
val v = u.map(x => 1.0 + 2.0 * x)
```

* Java

`RandomRDDs` provides factory methods to generate random double RDDs or vector RDDs. The following example generates a random double RDD, whose values follows the standard normal distribution `N(0, 1)`, and then map it to `N(1, 4)`.

Refer to the `RandomRDDs` Java docs for details on the API.

```java
import org.apache.spark.SparkContext;
import org.apache.spark.api.JavaDoubleRDD;
import static org.apache.spark.mllib.random.RandomRDDs.*;

JavaSparkContext jsc = ...

// Generate a random double RDD that contains 1 million i.i.d. values drawn from the
// standard normal distribution `N(0, 1)`, evenly distributed in 10 partitions.
JavaDoubleRDD u = normalJavaRDD(jsc, 1000000L, 10);
// Apply a transform to get a random double RDD following `N(1, 4)`.
JavaDoubleRDD v = u.mapToDouble(x -> 1.0 + 2.0 * x);
```

### Kernel density estimation

[Kernel density estimation](https://en.wikipedia.org/wiki/Kernel\_density\_estimation) is a technique useful for visualizing empirical probability distributions without requiring assumptions about the particular distribution that the observed samples are drawn from. It computes an estimate of the probability density function of a random variables, evaluated at a given set of points. It achieves this estimate by expressing the PDF of the empirical distribution at a particular point as the mean of PDFs of normal distributions centered around each of the samples.

* Python

`KernelDensity` provides methods to compute kernel density estimates from an RDD of samples. The following example demonstrates how to do so.

Refer to the `KernelDensity` Python docs for more details on the API.

```
from pyspark.mllib.stat import KernelDensity

# an RDD of sample data
data = sc.parallelize([1.0, 1.0, 1.0, 2.0, 3.0, 4.0, 5.0, 5.0, 6.0, 7.0, 8.0, 9.0, 9.0])

# Construct the density estimator with the sample data and a standard deviation for the Gaussian
# kernels
kd = KernelDensity()
kd.setSample(data)
kd.setBandwidth(3.0)

# Find density estimates for the given values
densities = kd.estimate([-1.0, 2.0, 5.0])
```

Find full example code at "examples/src/main/python/mllib/kernel\_density\_estimation\_example.py" in the Spark repo.

* Scala

`KernelDensity` provides methods to compute kernel density estimates from an RDD of samples. The following example demonstrates how to do so.

Refer to the `KernelDensity` Scala docs for details on the API.

```
import org.apache.spark.mllib.stat.KernelDensity
import org.apache.spark.rdd.RDD

// an RDD of sample data
val data: RDD[Double] = sc.parallelize(Seq(1, 1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9, 9))

// Construct the density estimator with the sample data and a standard deviation
// for the Gaussian kernels
val kd = new KernelDensity()
  .setSample(data)
  .setBandwidth(3.0)

// Find density estimates for the given values
val densities = kd.estimate(Array(-1.0, 2.0, 5.0))
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/KernelDensityEstimationExample.scala" in the Spark repo.

* Java

`KernelDensity` provides methods to compute kernel density estimates from an RDD of samples. The following example demonstrates how to do so.

Refer to the `KernelDensity` Java docs for details on the API.

```
import java.util.Arrays;

import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.stat.KernelDensity;

// an RDD of sample data
JavaRDD<Double> data = jsc.parallelize(
  Arrays.asList(1.0, 1.0, 1.0, 2.0, 3.0, 4.0, 5.0, 5.0, 6.0, 7.0, 8.0, 9.0, 9.0));

// Construct the density estimator with the sample data
// and a standard deviation for the Gaussian kernels
KernelDensity kd = new KernelDensity().setSample(data).setBandwidth(3.0);

// Find density estimates for the given values
double[] densities = kd.estimate(new double[]{-1.0, 2.0, 5.0});

System.out.println(Arrays.toString(densities));
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaKernelDensityEstimationExample.java" in the Spark repo.
