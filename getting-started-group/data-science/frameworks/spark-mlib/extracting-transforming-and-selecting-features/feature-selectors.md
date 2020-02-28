# Feature Selectors

## VectorSlicer

`VectorSlicer` is a transformer that takes a feature vector and outputs a new feature vector with a sub-array of the original features. It is useful for extracting features from a vector column.

`VectorSlicer` accepts a vector column with specified indices, then outputs a new vector column whose values are selected via those indices. There are two types of indices,

1. Integer indices that represent the indices into the vector, `setIndices()`.
2. String indices that represent the names of features into the vector, `setNames()`. _This requires the vector column to have an `AttributeGroup` since the implementation matches on the name field of an `Attribute`._

Specification by integer and string are both acceptable. Moreover, you can use integer index and string name simultaneously. At least one feature must be selected. Duplicate features are not allowed, so there can be no overlap between selected indices and names. Note that if names of features are selected, an exception will be thrown if empty input attributes are encountered.

The output vector will order features with the selected indices first \(in the order given\), followed by the selected names \(in the order given\).

**Examples**

Suppose that we have a DataFrame with the column `userFeatures`:

```text
 userFeatures
------------------
 [0.0, 10.0, 0.5]
```

`userFeatures` is a vector column that contains three user features. Assume that the first column of `userFeatures` are all zeros, so we want to remove it and select only the last two columns. The `VectorSlicer` selects the last two elements with `setIndices(1, 2)` then produces a new vector column named `features`:

```text
 userFeatures     | features
------------------|-----------------------------
 [0.0, 10.0, 0.5] | [10.0, 0.5]
```

Suppose also that we have potential input attributes for the `userFeatures`, i.e. `["f1", "f2", "f3"]`, then we can use `setNames("f2", "f3")` to select them.

```text
 userFeatures     | features
------------------|-----------------------------
 [0.0, 10.0, 0.5] | [10.0, 0.5]
 ["f1", "f2", "f3"] | ["f2", "f3"]
```

{% tabs %}
{% tab title="Scala" %}
```scala
import java.util.Arrays

import org.apache.spark.ml.attribute.{Attribute, AttributeGroup, NumericAttribute}
import org.apache.spark.ml.feature.VectorSlicer
import org.apache.spark.ml.linalg.Vectors
import org.apache.spark.sql.{Row, SparkSession}
import org.apache.spark.sql.types.StructType

val data = Arrays.asList(
  Row(Vectors.sparse(3, Seq((0, -2.0), (1, 2.3)))),
  Row(Vectors.dense(-2.0, 2.3, 0.0))
)

val defaultAttr = NumericAttribute.defaultAttr
val attrs = Array("f1", "f2", "f3").map(defaultAttr.withName)
val attrGroup = new AttributeGroup("userFeatures", attrs.asInstanceOf[Array[Attribute]])

val dataset = spark.createDataFrame(data, StructType(Array(attrGroup.toStructField())))

val slicer = new VectorSlicer().setInputCol("userFeatures").setOutputCol("features")

slicer.setIndices(Array(1)).setNames(Array("f3"))
// or slicer.setIndices(Array(1, 2)), or slicer.setNames(Array("f2", "f3"))

val output = slicer.transform(dataset)
output.show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.attribute.Attribute;
import org.apache.spark.ml.attribute.AttributeGroup;
import org.apache.spark.ml.attribute.NumericAttribute;
import org.apache.spark.ml.feature.VectorSlicer;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.*;

Attribute[] attrs = {
  NumericAttribute.defaultAttr().withName("f1"),
  NumericAttribute.defaultAttr().withName("f2"),
  NumericAttribute.defaultAttr().withName("f3")
};
AttributeGroup group = new AttributeGroup("userFeatures", attrs);

List<Row> data = Arrays.asList(
  RowFactory.create(Vectors.sparse(3, new int[]{0, 1}, new double[]{-2.0, 2.3})),
  RowFactory.create(Vectors.dense(-2.0, 2.3, 0.0))
);

Dataset<Row> dataset =
  spark.createDataFrame(data, (new StructType()).add(group.toStructField()));

VectorSlicer vectorSlicer = new VectorSlicer()
  .setInputCol("userFeatures").setOutputCol("features");

vectorSlicer.setIndices(new int[]{1}).setNames(new String[]{"f3"});
// or slicer.setIndices(new int[]{1, 2}), or slicer.setNames(new String[]{"f2", "f3"})

Dataset<Row> output = vectorSlicer.transform(dataset);
output.show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import VectorSlicer
from pyspark.ml.linalg import Vectors
from pyspark.sql.types import Row

df = spark.createDataFrame([
    Row(userFeatures=Vectors.sparse(3, {0: -2.0, 1: 2.3})),
    Row(userFeatures=Vectors.dense([-2.0, 2.3, 0.0]))])

slicer = VectorSlicer(inputCol="userFeatures", outputCol="features", indices=[1])

output = slicer.transform(df)

output.select("userFeatures", "features").show()
```
{% endtab %}
{% endtabs %}

## RFormula

`RFormula` selects columns specified by an [R model formula](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/formula.html). Currently we support a limited subset of the R operators, including ‘~’, ‘.’, ‘:’, ‘+’, and ‘-‘. The basic operators are:

* `~` separate target and terms
* `+` concat terms, “+ 0” means removing intercept
* `-` remove a term, “- 1” means removing intercept
* `:` interaction \(multiplication for numeric values, or binarized categorical values\)
* `.` all columns except target

Suppose `a` and `b` are double columns, we use the following simple examples to illustrate the effect of `RFormula`:

* `y ~ a + b` means model `y ~ w0 + w1 * a + w2 * b` where `w0` is the intercept and `w1, w2` are coefficients.
* `y ~ a + b + a:b - 1` means model `y ~ w1 * a + w2 * b + w3 * a * b` where `w1, w2, w3` are coefficients.

`RFormula` produces a vector column of features and a double or string column of label. Like when formulas are used in R for linear regression, numeric columns will be cast to doubles. As to string input columns, they will first be transformed with [StringIndexer](https://spark.apache.org/docs/2.4.4/ml-features.html#stringindexer) using ordering determined by `stringOrderType`, and the last category after ordering is dropped, then the doubles will be one-hot encoded.

Suppose a string feature column containing values `{'b', 'a', 'b', 'a', 'c', 'b'}`, we set `stringOrderType` to control the encoding:

```text
stringOrderType | Category mapped to 0 by StringIndexer |  Category dropped by RFormula
----------------|---------------------------------------|---------------------------------
'frequencyDesc' | most frequent category ('b')          | least frequent category ('c')
'frequencyAsc'  | least frequent category ('c')         | most frequent category ('b')
'alphabetDesc'  | last alphabetical category ('c')      | first alphabetical category ('a')
'alphabetAsc'   | first alphabetical category ('a')     | last alphabetical category ('c')
```

If the label column is of type string, it will be first transformed to double with [StringIndexer](https://spark.apache.org/docs/2.4.4/ml-features.html#stringindexer) using `frequencyDesc` ordering. If the label column does not exist in the DataFrame, the output label column will be created from the specified response variable in the formula.

**Note:** The ordering option `stringOrderType` is NOT used for the label column. When the label column is indexed, it uses the default descending frequency ordering in `StringIndexer`.

**Examples**

Assume that we have a DataFrame with the columns `id`, `country`, `hour`, and `clicked`:

```text
id | country | hour | clicked
---|---------|------|---------
 7 | "US"    | 18   | 1.0
 8 | "CA"    | 12   | 0.0
 9 | "NZ"    | 15   | 0.0
```

If we use `RFormula` with a formula string of `clicked ~ country + hour`, which indicates that we want to predict `clicked` based on `country` and `hour`, after transformation we should get the following DataFrame:

```text
id | country | hour | clicked | features         | label
---|---------|------|---------|------------------|-------
 7 | "US"    | 18   | 1.0     | [0.0, 0.0, 18.0] | 1.0
 8 | "CA"    | 12   | 0.0     | [0.0, 1.0, 12.0] | 0.0
 9 | "NZ"    | 15   | 0.0     | [1.0, 0.0, 15.0] | 0.0
```

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.RFormula

val dataset = spark.createDataFrame(Seq(
  (7, "US", 18, 1.0),
  (8, "CA", 12, 0.0),
  (9, "NZ", 15, 0.0)
)).toDF("id", "country", "hour", "clicked")

val formula = new RFormula()
  .setFormula("clicked ~ country + hour")
  .setFeaturesCol("features")
  .setLabelCol("label")

val output = formula.fit(dataset).transform(dataset)
output.select("features", "label").show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.RFormula;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

import static org.apache.spark.sql.types.DataTypes.*;

StructType schema = createStructType(new StructField[]{
  createStructField("id", IntegerType, false),
  createStructField("country", StringType, false),
  createStructField("hour", IntegerType, false),
  createStructField("clicked", DoubleType, false)
});

List<Row> data = Arrays.asList(
  RowFactory.create(7, "US", 18, 1.0),
  RowFactory.create(8, "CA", 12, 0.0),
  RowFactory.create(9, "NZ", 15, 0.0)
);

Dataset<Row> dataset = spark.createDataFrame(data, schema);
RFormula formula = new RFormula()
  .setFormula("clicked ~ country + hour")
  .setFeaturesCol("features")
  .setLabelCol("label");
Dataset<Row> output = formula.fit(dataset).transform(dataset);
output.select("features", "label").show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import RFormula

dataset = spark.createDataFrame(
    [(7, "US", 18, 1.0),
     (8, "CA", 12, 0.0),
     (9, "NZ", 15, 0.0)],
    ["id", "country", "hour", "clicked"])

formula = RFormula(
    formula="clicked ~ country + hour",
    featuresCol="features",
    labelCol="label")

output = formula.fit(dataset).transform(dataset)
output.select("features", "label").show()
```
{% endtab %}
{% endtabs %}

## ChiSqSelector

`ChiSqSelector` stands for Chi-Squared feature selection. It operates on labeled data with categorical features. ChiSqSelector uses the [Chi-Squared test of independence](https://en.wikipedia.org/wiki/Chi-squared_test) to decide which features to choose. It supports five selection methods: `numTopFeatures`, `percentile`, `fpr`, `fdr`, `fwe`:

* `numTopFeatures` chooses a fixed number of top features according to a chi-squared test. This is akin to yielding the features with the most predictive power.
* `percentile` is similar to `numTopFeatures` but chooses a fraction of all features instead of a fixed number.
* `fpr` chooses all features whose p-values are below a threshold, thus controlling the false positive rate of selection.
* `fdr` uses the [Benjamini-Hochberg procedure](https://en.wikipedia.org/wiki/False_discovery_rate#Benjamini.E2.80.93Hochberg_procedure) to choose all features whose false discovery rate is below a threshold.
* `fwe` chooses all features whose p-values are below a threshold. The threshold is scaled by 1/numFeatures, thus controlling the family-wise error rate of selection. By default, the selection method is `numTopFeatures`, with the default number of top features set to 50. The user can choose a selection method using `setSelectorType`.

**Examples**

Assume that we have a DataFrame with the columns `id`, `features`, and `clicked`, which is used as our target to be predicted:

```text
id | features              | clicked
---|-----------------------|---------
 7 | [0.0, 0.0, 18.0, 1.0] | 1.0
 8 | [0.0, 1.0, 12.0, 0.0] | 0.0
 9 | [1.0, 0.0, 15.0, 0.1] | 0.0
```

If we use `ChiSqSelector` with `numTopFeatures = 1`, then according to our label `clicked` the last column in our `features` is chosen as the most useful feature:

```text
id | features              | clicked | selectedFeatures
---|-----------------------|---------|------------------
 7 | [0.0, 0.0, 18.0, 1.0] | 1.0     | [1.0]
 8 | [0.0, 1.0, 12.0, 0.0] | 0.0     | [0.0]
 9 | [1.0, 0.0, 15.0, 0.1] | 0.0     | [0.1]
```

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.ChiSqSelector
import org.apache.spark.ml.linalg.Vectors

val data = Seq(
  (7, Vectors.dense(0.0, 0.0, 18.0, 1.0), 1.0),
  (8, Vectors.dense(0.0, 1.0, 12.0, 0.0), 0.0),
  (9, Vectors.dense(1.0, 0.0, 15.0, 0.1), 0.0)
)

val df = spark.createDataset(data).toDF("id", "features", "clicked")

val selector = new ChiSqSelector()
  .setNumTopFeatures(1)
  .setFeaturesCol("features")
  .setLabelCol("clicked")
  .setOutputCol("selectedFeatures")

val result = selector.fit(df).transform(df)

println(s"ChiSqSelector output with top ${selector.getNumTopFeatures} features selected")
result.show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.ChiSqSelector;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(7, Vectors.dense(0.0, 0.0, 18.0, 1.0), 1.0),
  RowFactory.create(8, Vectors.dense(0.0, 1.0, 12.0, 0.0), 0.0),
  RowFactory.create(9, Vectors.dense(1.0, 0.0, 15.0, 0.1), 0.0)
);
StructType schema = new StructType(new StructField[]{
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("features", new VectorUDT(), false, Metadata.empty()),
  new StructField("clicked", DataTypes.DoubleType, false, Metadata.empty())
});

Dataset<Row> df = spark.createDataFrame(data, schema);

ChiSqSelector selector = new ChiSqSelector()
  .setNumTopFeatures(1)
  .setFeaturesCol("features")
  .setLabelCol("clicked")
  .setOutputCol("selectedFeatures");

Dataset<Row> result = selector.fit(df).transform(df);

System.out.println("ChiSqSelector output with top " + selector.getNumTopFeatures()
    + " features selected");
result.show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import ChiSqSelector
from pyspark.ml.linalg import Vectors

df = spark.createDataFrame([
    (7, Vectors.dense([0.0, 0.0, 18.0, 1.0]), 1.0,),
    (8, Vectors.dense([0.0, 1.0, 12.0, 0.0]), 0.0,),
    (9, Vectors.dense([1.0, 0.0, 15.0, 0.1]), 0.0,)], ["id", "features", "clicked"])

selector = ChiSqSelector(numTopFeatures=1, featuresCol="features",
                         outputCol="selectedFeatures", labelCol="clicked")

result = selector.fit(df).transform(df)

print("ChiSqSelector output with top %d features selected" % selector.getNumTopFeatures())
result.show()
```
{% endtab %}
{% endtabs %}

