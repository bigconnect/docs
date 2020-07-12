# Basic Statistics

### Correlation <a id="correlation"></a>

Calculating the correlation between two series of data is a common operation in Statistics. In `spark.ml` we provide the flexibility to calculate pairwise correlations among many series. The supported correlation methods are currently Pearson’s and Spearman’s correlation.

[`Correlation`](https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.ml.stat.Correlation$) computes the correlation matrix for the input Dataset of Vectors using the specified method. The output will be a DataFrame that contains the correlation matrix of the column of vectors.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.linalg.{Matrix, Vectors}
import org.apache.spark.ml.stat.Correlation
import org.apache.spark.sql.Row

val data = Seq(
  Vectors.sparse(4, Seq((0, 1.0), (3, -2.0))),
  Vectors.dense(4.0, 5.0, 0.0, 3.0),
  Vectors.dense(6.0, 7.0, 0.0, 8.0),
  Vectors.sparse(4, Seq((0, 9.0), (3, 1.0)))
)

val df = data.map(Tuple1.apply).toDF("features")
val Row(coeff1: Matrix) = Correlation.corr(df, "features").head
println(s"Pearson correlation matrix:\n $coeff1")

val Row(coeff2: Matrix) = Correlation.corr(df, "features", "spearman").head
println(s"Spearman correlation matrix:\n $coeff2")
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.stat.Correlation;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.*;

List<Row> data = Arrays.asList(
  RowFactory.create(Vectors.sparse(4, new int[]{0, 3}, new double[]{1.0, -2.0})),
  RowFactory.create(Vectors.dense(4.0, 5.0, 0.0, 3.0)),
  RowFactory.create(Vectors.dense(6.0, 7.0, 0.0, 8.0)),
  RowFactory.create(Vectors.sparse(4, new int[]{0, 3}, new double[]{9.0, 1.0}))
);

StructType schema = new StructType(new StructField[]{
  new StructField("features", new VectorUDT(), false, Metadata.empty()),
});

Dataset<Row> df = spark.createDataFrame(data, schema);
Row r1 = Correlation.corr(df, "features").head();
System.out.println("Pearson correlation matrix:\n" + r1.get(0).toString());

Row r2 = Correlation.corr(df, "features", "spearman").head();
System.out.println("Spearman correlation matrix:\n" + r2.get(0).toString());
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.linalg import Vectors
from pyspark.ml.stat import Correlation

data = [(Vectors.sparse(4, [(0, 1.0), (3, -2.0)]),),
        (Vectors.dense([4.0, 5.0, 0.0, 3.0]),),
        (Vectors.dense([6.0, 7.0, 0.0, 8.0]),),
        (Vectors.sparse(4, [(0, 9.0), (3, 1.0)]),)]
df = spark.createDataFrame(data, ["features"])

r1 = Correlation.corr(df, "features").head()
print("Pearson correlation matrix:\n" + str(r1[0]))

r2 = Correlation.corr(df, "features", "spearman").head()
print("Spearman correlation matrix:\n" + str(r2[0]))
```
{% endtab %}
{% endtabs %}

### Hypothesis testing <a id="hypothesis-testing"></a>

Hypothesis testing is a powerful tool in statistics to determine whether a result is statistically significant, whether this result occurred by chance or not. `spark.ml` currently supports Pearson’s Chi-squared \( χ2\) tests for independence.

`ChiSquareTest` conducts Pearson’s independence test for every feature against the label. For each feature, the \(feature, label\) pairs are converted into a contingency matrix for which the Chi-squared statistic is computed. All label and feature values must be categorical.

Refer to the [`ChiSquareTest`](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.stat.ChiSquareTest$) Scala docs for details on the API.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.linalg.{Vector, Vectors}
import org.apache.spark.ml.stat.ChiSquareTest

val data = Seq(
  (0.0, Vectors.dense(0.5, 10.0)),
  (0.0, Vectors.dense(1.5, 20.0)),
  (1.0, Vectors.dense(1.5, 30.0)),
  (0.0, Vectors.dense(3.5, 30.0)),
  (0.0, Vectors.dense(3.5, 40.0)),
  (1.0, Vectors.dense(3.5, 40.0))
)

val df = data.toDF("label", "features")
val chi = ChiSquareTest.test(df, "features", "label").head
println(s"pValues = ${chi.getAs[Vector](0)}")
println(s"degreesOfFreedom ${chi.getSeq[Int](1).mkString("[", ",", "]")}")
println(s"statistics ${chi.getAs[Vector](2)}")
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.stat.ChiSquareTest;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.*;

List<Row> data = Arrays.asList(
  RowFactory.create(0.0, Vectors.dense(0.5, 10.0)),
  RowFactory.create(0.0, Vectors.dense(1.5, 20.0)),
  RowFactory.create(1.0, Vectors.dense(1.5, 30.0)),
  RowFactory.create(0.0, Vectors.dense(3.5, 30.0)),
  RowFactory.create(0.0, Vectors.dense(3.5, 40.0)),
  RowFactory.create(1.0, Vectors.dense(3.5, 40.0))
);

StructType schema = new StructType(new StructField[]{
  new StructField("label", DataTypes.DoubleType, false, Metadata.empty()),
  new StructField("features", new VectorUDT(), false, Metadata.empty()),
});

Dataset<Row> df = spark.createDataFrame(data, schema);
Row r = ChiSquareTest.test(df, "features", "label").head();
System.out.println("pValues: " + r.get(0).toString());
System.out.println("degreesOfFreedom: " + r.getList(1).toString());
System.out.println("statistics: " + r.get(2).toString());
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.linalg import Vectors
from pyspark.ml.stat import ChiSquareTest

data = [(0.0, Vectors.dense(0.5, 10.0)),
        (0.0, Vectors.dense(1.5, 20.0)),
        (1.0, Vectors.dense(1.5, 30.0)),
        (0.0, Vectors.dense(3.5, 30.0)),
        (0.0, Vectors.dense(3.5, 40.0)),
        (1.0, Vectors.dense(3.5, 40.0))]
df = spark.createDataFrame(data, ["label", "features"])

r = ChiSquareTest.test(df, "features", "label").head()
print("pValues: " + str(r.pValues))
print("degreesOfFreedom: " + str(r.degreesOfFreedom))
print("statistics: " + str(r.statistics))
```
{% endtab %}
{% endtabs %}

### Summarizer <a id="summarizer"></a>

We provide vector column summary statistics for `Dataframe` through `Summarizer`. Available metrics are the column-wise max, min, mean, variance, and number of nonzeros, as well as the total count.

The following example demonstrates using [`Summarizer`](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.stat.Summarizer$) to compute the mean and variance for a vector column of the input dataframe, with and without a weight column.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.linalg.{Vector, Vectors}
import org.apache.spark.ml.stat.Summarizer

val data = Seq(
  (Vectors.dense(2.0, 3.0, 5.0), 1.0),
  (Vectors.dense(4.0, 6.0, 7.0), 2.0)
)

val df = data.toDF("features", "weight")

val (meanVal, varianceVal) = df.select(metrics("mean", "variance")
  .summary($"features", $"weight").as("summary"))
  .select("summary.mean", "summary.variance")
  .as[(Vector, Vector)].first()

println(s"with weight: mean = ${meanVal}, variance = ${varianceVal}")

val (meanVal2, varianceVal2) = df.select(mean($"features"), variance($"features"))
  .as[(Vector, Vector)].first()

println(s"without weight: mean = ${meanVal2}, sum = ${varianceVal2}")
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.linalg.Vector;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.stat.Summarizer;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(Vectors.dense(2.0, 3.0, 5.0), 1.0),
  RowFactory.create(Vectors.dense(4.0, 6.0, 7.0), 2.0)
);

StructType schema = new StructType(new StructField[]{
  new StructField("features", new VectorUDT(), false, Metadata.empty()),
  new StructField("weight", DataTypes.DoubleType, false, Metadata.empty())
});

Dataset<Row> df = spark.createDataFrame(data, schema);

Row result1 = df.select(Summarizer.metrics("mean", "variance")
  .summary(new Column("features"), new Column("weight")).as("summary"))
  .select("summary.mean", "summary.variance").first();
System.out.println("with weight: mean = " + result1.<Vector>getAs(0).toString() +
  ", variance = " + result1.<Vector>getAs(1).toString());

Row result2 = df.select(
  Summarizer.mean(new Column("features")),
  Summarizer.variance(new Column("features"))
).first();
System.out.println("without weight: mean = " + result2.<Vector>getAs(0).toString() +
  ", variance = " + result2.<Vector>getAs(1).toString());
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.stat import Summarizer
from pyspark.sql import Row
from pyspark.ml.linalg import Vectors

df = sc.parallelize([Row(weight=1.0, features=Vectors.dense(1.0, 1.0, 1.0)),
                     Row(weight=0.0, features=Vectors.dense(1.0, 2.0, 3.0))]).toDF()

# create summarizer for multiple metrics "mean" and "count"
summarizer = Summarizer.metrics("mean", "count")

# compute statistics for multiple metrics with weight
df.select(summarizer.summary(df.features, df.weight)).show(truncate=False)

# compute statistics for multiple metrics without weight
df.select(summarizer.summary(df.features)).show(truncate=False)

# compute statistics for single metric "mean" with weight
df.select(Summarizer.mean(df.features, df.weight)).show(truncate=False)

# compute statistics for single metric "mean" without weight
df.select(Summarizer.mean(df.features)).show(truncate=False)
```
{% endtab %}
{% endtabs %}

