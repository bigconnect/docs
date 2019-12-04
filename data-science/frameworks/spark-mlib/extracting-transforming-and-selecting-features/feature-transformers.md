# Feature Transformers

## Tokenizer

[Tokenization](http://en.wikipedia.org/wiki/Lexical_analysis#Tokenization) is the process of taking text \(such as a sentence\) and breaking it into individual terms \(usually words\). A simple [Tokenizer](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.feature.Tokenizer) class provides this functionality. The example below shows how to split sentences into sequences of words.

[RegexTokenizer](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.feature.RegexTokenizer) allows more advanced tokenization based on regular expression \(regex\) matching. By default, the parameter “pattern” \(regex, default: `"\\s+"`\) is used as delimiters to split the input text. Alternatively, users can set parameter “gaps” to false indicating the regex “pattern” denotes “tokens” rather than splitting gaps, and find all matching occurrences as the tokenization result.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.{RegexTokenizer, Tokenizer}
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions._

val sentenceDataFrame = spark.createDataFrame(Seq(
  (0, "Hi I heard about Spark"),
  (1, "I wish Java could use case classes"),
  (2, "Logistic,regression,models,are,neat")
)).toDF("id", "sentence")

val tokenizer = new Tokenizer().setInputCol("sentence").setOutputCol("words")
val regexTokenizer = new RegexTokenizer()
  .setInputCol("sentence")
  .setOutputCol("words")
  .setPattern("\\W") // alternatively .setPattern("\\w+").setGaps(false)

val countTokens = udf { (words: Seq[String]) => words.length }

val tokenized = tokenizer.transform(sentenceDataFrame)
tokenized.select("sentence", "words")
    .withColumn("tokens", countTokens(col("words"))).show(false)

val regexTokenized = regexTokenizer.transform(sentenceDataFrame)
regexTokenized.select("sentence", "words")
    .withColumn("tokens", countTokens(col("words"))).show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import scala.collection.mutable.WrappedArray;

import org.apache.spark.ml.feature.RegexTokenizer;
import org.apache.spark.ml.feature.Tokenizer;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

// col("...") is preferable to df.col("...")
import static org.apache.spark.sql.functions.callUDF;
import static org.apache.spark.sql.functions.col;

List<Row> data = Arrays.asList(
  RowFactory.create(0, "Hi I heard about Spark"),
  RowFactory.create(1, "I wish Java could use case classes"),
  RowFactory.create(2, "Logistic,regression,models,are,neat")
);

StructType schema = new StructType(new StructField[]{
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("sentence", DataTypes.StringType, false, Metadata.empty())
});

Dataset<Row> sentenceDataFrame = spark.createDataFrame(data, schema);

Tokenizer tokenizer = new Tokenizer().setInputCol("sentence").setOutputCol("words");

RegexTokenizer regexTokenizer = new RegexTokenizer()
    .setInputCol("sentence")
    .setOutputCol("words")
    .setPattern("\\W");  // alternatively .setPattern("\\w+").setGaps(false);

spark.udf().register(
  "countTokens", (WrappedArray<?> words) -> words.size(), DataTypes.IntegerType);

Dataset<Row> tokenized = tokenizer.transform(sentenceDataFrame);
tokenized.select("sentence", "words")
    .withColumn("tokens", callUDF("countTokens", col("words")))
    .show(false);

Dataset<Row> regexTokenized = regexTokenizer.transform(sentenceDataFrame);
regexTokenized.select("sentence", "words")
    .withColumn("tokens", callUDF("countTokens", col("words")))
    .show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import Tokenizer, RegexTokenizer
from pyspark.sql.functions import col, udf
from pyspark.sql.types import IntegerType

sentenceDataFrame = spark.createDataFrame([
    (0, "Hi I heard about Spark"),
    (1, "I wish Java could use case classes"),
    (2, "Logistic,regression,models,are,neat")
], ["id", "sentence"])

tokenizer = Tokenizer(inputCol="sentence", outputCol="words")

regexTokenizer = RegexTokenizer(inputCol="sentence", outputCol="words", pattern="\\W")
# alternatively, pattern="\\w+", gaps(False)

countTokens = udf(lambda words: len(words), IntegerType())

tokenized = tokenizer.transform(sentenceDataFrame)
tokenized.select("sentence", "words")\
    .withColumn("tokens", countTokens(col("words"))).show(truncate=False)

regexTokenized = regexTokenizer.transform(sentenceDataFrame)
regexTokenized.select("sentence", "words") \
    .withColumn("tokens", countTokens(col("words"))).show(truncate=False)
```
{% endtab %}
{% endtabs %}

## StopWordsRemover

[Stop words](https://en.wikipedia.org/wiki/Stop_words) are words which should be excluded from the input, typically because the words appear frequently and don’t carry as much meaning.

`StopWordsRemover` takes as input a sequence of strings \(e.g. the output of a [Tokenizer](https://spark.apache.org/docs/2.4.4/ml-features.html#tokenizer)\) and drops all the stop words from the input sequences. The list of stopwords is specified by the `stopWords` parameter. Default stop words for some languages are accessible by calling `StopWordsRemover.loadDefaultStopWords(language)`, for which available options are “danish”, “dutch”, “english”, “finnish”, “french”, “german”, “hungarian”, “italian”, “norwegian”, “portuguese”, “russian”, “spanish”, “swedish” and “turkish”. A boolean parameter `caseSensitive` indicates if the matches should be case sensitive \(false by default\).

**Examples**

Assume that we have the following DataFrame with columns `id` and `raw`:

```text
 id | raw
----|----------
 0  | [I, saw, the, red, baloon]
 1  | [Mary, had, a, little, lamb]
```

Applying `StopWordsRemover` with `raw` as the input column and `filtered` as the output column, we should get the following:

```text
 id | raw                         | filtered
----|-----------------------------|--------------------
 0  | [I, saw, the, red, baloon]  |  [saw, red, baloon]
 1  | [Mary, had, a, little, lamb]|[Mary, little, lamb]
```

In filtered, the stop words “I”, “the”, “had”, and “a” have been filtered out.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.StopWordsRemover

val remover = new StopWordsRemover()
  .setInputCol("raw")
  .setOutputCol("filtered")

val dataSet = spark.createDataFrame(Seq(
  (0, Seq("I", "saw", "the", "red", "balloon")),
  (1, Seq("Mary", "had", "a", "little", "lamb"))
)).toDF("id", "raw")

remover.transform(dataSet).show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.StopWordsRemover;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

StopWordsRemover remover = new StopWordsRemover()
  .setInputCol("raw")
  .setOutputCol("filtered");

List<Row> data = Arrays.asList(
  RowFactory.create(Arrays.asList("I", "saw", "the", "red", "balloon")),
  RowFactory.create(Arrays.asList("Mary", "had", "a", "little", "lamb"))
);

StructType schema = new StructType(new StructField[]{
  new StructField(
    "raw", DataTypes.createArrayType(DataTypes.StringType), false, Metadata.empty())
});

Dataset<Row> dataset = spark.createDataFrame(data, schema);
remover.transform(dataset).show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import StopWordsRemover

sentenceData = spark.createDataFrame([
    (0, ["I", "saw", "the", "red", "balloon"]),
    (1, ["Mary", "had", "a", "little", "lamb"])
], ["id", "raw"])

remover = StopWordsRemover(inputCol="raw", outputCol="filtered")
remover.transform(sentenceData).show(truncate=False)
```
{% endtab %}
{% endtabs %}

## n-gram

An [n-gram](https://en.wikipedia.org/wiki/N-gram) is a sequence of n tokens \(typically words\) for some integer nn. The `NGram` class can be used to transform input features into nn-grams.

`NGram` takes as input a sequence of strings \(e.g. the output of a [Tokenizer](https://spark.apache.org/docs/2.4.4/ml-features.html#tokenizer)\). The parameter `n` is used to determine the number of terms in each nn-gram. The output will consist of a sequence of nn-grams where each nn-gram is represented by a space-delimited string of nn consecutive words. If the input sequence contains fewer than `n` strings, no output is produced.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.NGram

val wordDataFrame = spark.createDataFrame(Seq(
  (0, Array("Hi", "I", "heard", "about", "Spark")),
  (1, Array("I", "wish", "Java", "could", "use", "case", "classes")),
  (2, Array("Logistic", "regression", "models", "are", "neat"))
)).toDF("id", "words")

val ngram = new NGram().setN(2).setInputCol("words").setOutputCol("ngrams")

val ngramDataFrame = ngram.transform(wordDataFrame)
ngramDataFrame.select("ngrams").show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.NGram;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(0, Arrays.asList("Hi", "I", "heard", "about", "Spark")),
  RowFactory.create(1, Arrays.asList("I", "wish", "Java", "could", "use", "case", "classes")),
  RowFactory.create(2, Arrays.asList("Logistic", "regression", "models", "are", "neat"))
);

StructType schema = new StructType(new StructField[]{
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField(
    "words", DataTypes.createArrayType(DataTypes.StringType), false, Metadata.empty())
});

Dataset<Row> wordDataFrame = spark.createDataFrame(data, schema);

NGram ngramTransformer = new NGram().setN(2).setInputCol("words").setOutputCol("ngrams");

Dataset<Row> ngramDataFrame = ngramTransformer.transform(wordDataFrame);
ngramDataFrame.select("ngrams").show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import NGram

wordDataFrame = spark.createDataFrame([
    (0, ["Hi", "I", "heard", "about", "Spark"]),
    (1, ["I", "wish", "Java", "could", "use", "case", "classes"]),
    (2, ["Logistic", "regression", "models", "are", "neat"])
], ["id", "words"])

ngram = NGram(n=2, inputCol="words", outputCol="ngrams")

ngramDataFrame = ngram.transform(wordDataFrame)
ngramDataFrame.select("ngrams").show(truncate=False)
```
{% endtab %}
{% endtabs %}

## Binarizer

Binarization is the process of thresholding numerical features to binary \(0/1\) features.

`Binarizer` takes the common parameters `inputCol` and `outputCol`, as well as the `threshold` for binarization. Feature values greater than the threshold are binarized to 1.0; values equal to or less than the threshold are binarized to 0.0. Both Vector and Double types are supported for `inputCol`.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.Binarizer

val data = Array((0, 0.1), (1, 0.8), (2, 0.2))
val dataFrame = spark.createDataFrame(data).toDF("id", "feature")

val binarizer: Binarizer = new Binarizer()
  .setInputCol("feature")
  .setOutputCol("binarized_feature")
  .setThreshold(0.5)

val binarizedDataFrame = binarizer.transform(dataFrame)

println(s"Binarizer output with Threshold = ${binarizer.getThreshold}")
binarizedDataFrame.show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.Binarizer;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(0, 0.1),
  RowFactory.create(1, 0.8),
  RowFactory.create(2, 0.2)
);
StructType schema = new StructType(new StructField[]{
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("feature", DataTypes.DoubleType, false, Metadata.empty())
});
Dataset<Row> continuousDataFrame = spark.createDataFrame(data, schema);

Binarizer binarizer = new Binarizer()
  .setInputCol("feature")
  .setOutputCol("binarized_feature")
  .setThreshold(0.5);

Dataset<Row> binarizedDataFrame = binarizer.transform(continuousDataFrame);

System.out.println("Binarizer output with Threshold = " + binarizer.getThreshold());
binarizedDataFrame.show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import Binarizer

continuousDataFrame = spark.createDataFrame([
    (0, 0.1),
    (1, 0.8),
    (2, 0.2)
], ["id", "feature"])

binarizer = Binarizer(threshold=0.5, inputCol="feature", outputCol="binarized_feature")

binarizedDataFrame = binarizer.transform(continuousDataFrame)

print("Binarizer output with Threshold = %f" % binarizer.getThreshold())
binarizedDataFrame.show()
```
{% endtab %}
{% endtabs %}

## **PCA**

[PCA](http://en.wikipedia.org/wiki/Principal_component_analysis) is a statistical procedure that uses an orthogonal transformation to convert a set of observations of possibly correlated variables into a set of values of linearly uncorrelated variables called principal components. A [PCA](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.feature.PCA) class trains a model to project vectors to a low-dimensional space using PCA. The example below shows how to project 5-dimensional feature vectors into 3-dimensional principal components.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.PCA
import org.apache.spark.ml.linalg.Vectors

val data = Array(
  Vectors.sparse(5, Seq((1, 1.0), (3, 7.0))),
  Vectors.dense(2.0, 0.0, 3.0, 4.0, 5.0),
  Vectors.dense(4.0, 0.0, 0.0, 6.0, 7.0)
)
val df = spark.createDataFrame(data.map(Tuple1.apply)).toDF("features")

val pca = new PCA()
  .setInputCol("features")
  .setOutputCol("pcaFeatures")
  .setK(3)
  .fit(df)

val result = pca.transform(df).select("pcaFeatures")
result.show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.PCA;
import org.apache.spark.ml.feature.PCAModel;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(Vectors.sparse(5, new int[]{1, 3}, new double[]{1.0, 7.0})),
  RowFactory.create(Vectors.dense(2.0, 0.0, 3.0, 4.0, 5.0)),
  RowFactory.create(Vectors.dense(4.0, 0.0, 0.0, 6.0, 7.0))
);

StructType schema = new StructType(new StructField[]{
  new StructField("features", new VectorUDT(), false, Metadata.empty()),
});

Dataset<Row> df = spark.createDataFrame(data, schema);

PCAModel pca = new PCA()
  .setInputCol("features")
  .setOutputCol("pcaFeatures")
  .setK(3)
  .fit(df);

Dataset<Row> result = pca.transform(df).select("pcaFeatures");
result.show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import PCA
from pyspark.ml.linalg import Vectors

data = [(Vectors.sparse(5, [(1, 1.0), (3, 7.0)]),),
        (Vectors.dense([2.0, 0.0, 3.0, 4.0, 5.0]),),
        (Vectors.dense([4.0, 0.0, 0.0, 6.0, 7.0]),)]
df = spark.createDataFrame(data, ["features"])

pca = PCA(k=3, inputCol="features", outputCol="pcaFeatures")
model = pca.fit(df)

result = model.transform(df).select("pcaFeatures")
result.show(truncate=False)
```
{% endtab %}
{% endtabs %}

## **PolynomialExpansion**

[Polynomial expansion](http://en.wikipedia.org/wiki/Polynomial_expansion) is the process of expanding your features into a polynomial space, which is formulated by an n-degree combination of original dimensions. A [PolynomialExpansion](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.feature.PolynomialExpansion) class provides this functionality. The example below shows how to expand your features into a 3-degree polynomial space.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.PolynomialExpansion
import org.apache.spark.ml.linalg.Vectors

val data = Array(
  Vectors.dense(2.0, 1.0),
  Vectors.dense(0.0, 0.0),
  Vectors.dense(3.0, -1.0)
)
val df = spark.createDataFrame(data.map(Tuple1.apply)).toDF("features")

val polyExpansion = new PolynomialExpansion()
  .setInputCol("features")
  .setOutputCol("polyFeatures")
  .setDegree(3)

val polyDF = polyExpansion.transform(df)
polyDF.show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.PolynomialExpansion;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

PolynomialExpansion polyExpansion = new PolynomialExpansion()
  .setInputCol("features")
  .setOutputCol("polyFeatures")
  .setDegree(3);

List<Row> data = Arrays.asList(
  RowFactory.create(Vectors.dense(2.0, 1.0)),
  RowFactory.create(Vectors.dense(0.0, 0.0)),
  RowFactory.create(Vectors.dense(3.0, -1.0))
);
StructType schema = new StructType(new StructField[]{
  new StructField("features", new VectorUDT(), false, Metadata.empty()),
});
Dataset<Row> df = spark.createDataFrame(data, schema);

Dataset<Row> polyDF = polyExpansion.transform(df);
polyDF.show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import PolynomialExpansion
from pyspark.ml.linalg import Vectors

df = spark.createDataFrame([
    (Vectors.dense([2.0, 1.0]),),
    (Vectors.dense([0.0, 0.0]),),
    (Vectors.dense([3.0, -1.0]),)
], ["features"])

polyExpansion = PolynomialExpansion(degree=3, inputCol="features", outputCol="polyFeatures")
polyDF = polyExpansion.transform(df)

polyDF.show(truncate=False)
```
{% endtab %}
{% endtabs %}

## **Discrete Cosine Transform \(DCT\)**

The [Discrete Cosine Transform](https://en.wikipedia.org/wiki/Discrete_cosine_transform) transforms a length NN real-valued sequence in the time domain into another length NN real-valued sequence in the frequency domain. A [DCT](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.feature.DCT) class provides this functionality, implementing the [DCT-II](https://en.wikipedia.org/wiki/Discrete_cosine_transform#DCT-II) and scaling the result by $$\frac{1}{\sqrt{2}}$$ such that the representing matrix for the transform is unitary. No shift is applied to the transformed sequence \(e.g. the 00th element of the transformed sequence is the 0th DCT coefficient and _not_ the N/2th\).

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.DCT
import org.apache.spark.ml.linalg.Vectors

val data = Seq(
  Vectors.dense(0.0, 1.0, -2.0, 3.0),
  Vectors.dense(-1.0, 2.0, 4.0, -7.0),
  Vectors.dense(14.0, -2.0, -5.0, 1.0))

val df = spark.createDataFrame(data.map(Tuple1.apply)).toDF("features")

val dct = new DCT()
  .setInputCol("features")
  .setOutputCol("featuresDCT")
  .setInverse(false)

val dctDf = dct.transform(df)
dctDf.select("featuresDCT").show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.DCT;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(Vectors.dense(0.0, 1.0, -2.0, 3.0)),
  RowFactory.create(Vectors.dense(-1.0, 2.0, 4.0, -7.0)),
  RowFactory.create(Vectors.dense(14.0, -2.0, -5.0, 1.0))
);
StructType schema = new StructType(new StructField[]{
  new StructField("features", new VectorUDT(), false, Metadata.empty()),
});
Dataset<Row> df = spark.createDataFrame(data, schema);

DCT dct = new DCT()
  .setInputCol("features")
  .setOutputCol("featuresDCT")
  .setInverse(false);

Dataset<Row> dctDf = dct.transform(df);

dctDf.select("featuresDCT").show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import DCT
from pyspark.ml.linalg import Vectors

df = spark.createDataFrame([
    (Vectors.dense([0.0, 1.0, -2.0, 3.0]),),
    (Vectors.dense([-1.0, 2.0, 4.0, -7.0]),),
    (Vectors.dense([14.0, -2.0, -5.0, 1.0]),)], ["features"])

dct = DCT(inverse=False, inputCol="features", outputCol="featuresDCT")

dctDf = dct.transform(df)

dctDf.select("featuresDCT").show(truncate=False)
```
{% endtab %}
{% endtabs %}

## **StringIndexer**

`StringIndexer` encodes a string column of labels to a column of label indices. The indices are in `[0, numLabels)`, and four ordering options are supported: “frequencyDesc”: descending order by label frequency \(most frequent label assigned 0\), “frequencyAsc”: ascending order by label frequency \(least frequent label assigned 0\), “alphabetDesc”: descending alphabetical order, and “alphabetAsc”: ascending alphabetical order \(default = “frequencyDesc”\). The unseen labels will be put at index numLabels if user chooses to keep them. If the input column is numeric, we cast it to string and index the string values. When downstream pipeline components such as `Estimator` or `Transformer` make use of this string-indexed label, you must set the input column of the component to this string-indexed column name. In many cases, you can set the input column with `setInputCol`.

**Examples**

Assume that we have the following DataFrame with columns `id` and `category`:

```text
 id | category
----|----------
 0  | a
 1  | b
 2  | c
 3  | a
 4  | a
 5  | c
```

`category` is a string column with three labels: “a”, “b”, and “c”. Applying `StringIndexer` with `category` as the input column and `categoryIndex` as the output column, we should get the following:

```text
 id | category | categoryIndex
----|----------|---------------
 0  | a        | 0.0
 1  | b        | 2.0
 2  | c        | 1.0
 3  | a        | 0.0
 4  | a        | 0.0
 5  | c        | 1.0
```

“a” gets index `0` because it is the most frequent, followed by “c” with index `1` and “b” with index `2`.

Additionally, there are three strategies regarding how `StringIndexer` will handle unseen labels when you have fit a `StringIndexer` on one dataset and then use it to transform another:

* throw an exception \(which is the default\)
* skip the row containing the unseen label entirely
* put unseen labels in a special additional bucket, at index numLabels

**Examples**

Let’s go back to our previous example but this time reuse our previously defined `StringIndexer` on the following dataset:

```text
 id | category
----|----------
 0  | a
 1  | b
 2  | c
 3  | d
 4  | e
```

If you’ve not set how `StringIndexer` handles unseen labels or set it to “error”, an exception will be thrown. However, if you had called `setHandleInvalid("skip")`, the following dataset will be generated:

```text
 id | category | categoryIndex
----|----------|---------------
 0  | a        | 0.0
 1  | b        | 2.0
 2  | c        | 1.0
```

Notice that the rows containing “d” or “e” are mapped to index “3.0”

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.StringIndexer

val df = spark.createDataFrame(
  Seq((0, "a"), (1, "b"), (2, "c"), (3, "a"), (4, "a"), (5, "c"))
).toDF("id", "category")

val indexer = new StringIndexer()
  .setInputCol("category")
  .setOutputCol("categoryIndex")

val indexed = indexer.fit(df).transform(df)
indexed.show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.StringIndexer;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

import static org.apache.spark.sql.types.DataTypes.*;

List<Row> data = Arrays.asList(
  RowFactory.create(0, "a"),
  RowFactory.create(1, "b"),
  RowFactory.create(2, "c"),
  RowFactory.create(3, "a"),
  RowFactory.create(4, "a"),
  RowFactory.create(5, "c")
);
StructType schema = new StructType(new StructField[]{
  createStructField("id", IntegerType, false),
  createStructField("category", StringType, false)
});
Dataset<Row> df = spark.createDataFrame(data, schema);

StringIndexer indexer = new StringIndexer()
  .setInputCol("category")
  .setOutputCol("categoryIndex");

Dataset<Row> indexed = indexer.fit(df).transform(df);
indexed.show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import StringIndexer

df = spark.createDataFrame(
    [(0, "a"), (1, "b"), (2, "c"), (3, "a"), (4, "a"), (5, "c")],
    ["id", "category"])

indexer = StringIndexer(inputCol="category", outputCol="categoryIndex")
indexed = indexer.fit(df).transform(df)
indexed.show()
```
{% endtab %}
{% endtabs %}

## **IndexToString**

Symmetrically to `StringIndexer`, `IndexToString` maps a column of label indices back to a column containing the original labels as strings. A common use case is to produce indices from labels with `StringIndexer`, train a model with those indices and retrieve the original labels from the column of predicted indices with `IndexToString`. However, you are free to supply your own labels.

**Examples**

Building on the `StringIndexer` example, let’s assume we have the following DataFrame with columns `id` and `categoryIndex`:

```text
 id | categoryIndex
----|---------------
 0  | 0.0
 1  | 2.0
 2  | 1.0
 3  | 0.0
 4  | 0.0
 5  | 1.0
```

Applying `IndexToString` with `categoryIndex` as the input column, `originalCategory` as the output column, we are able to retrieve our original labels \(they will be inferred from the columns’ metadata\):

```text
 id | categoryIndex | originalCategory
----|---------------|-----------------
 0  | 0.0           | a
 1  | 2.0           | b
 2  | 1.0           | c
 3  | 0.0           | a
 4  | 0.0           | a
 5  | 1.0           | c
```

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.attribute.Attribute
import org.apache.spark.ml.feature.{IndexToString, StringIndexer}

val df = spark.createDataFrame(Seq(
  (0, "a"),
  (1, "b"),
  (2, "c"),
  (3, "a"),
  (4, "a"),
  (5, "c")
)).toDF("id", "category")

val indexer = new StringIndexer()
  .setInputCol("category")
  .setOutputCol("categoryIndex")
  .fit(df)
val indexed = indexer.transform(df)

println(s"Transformed string column '${indexer.getInputCol}' " +
    s"to indexed column '${indexer.getOutputCol}'")
indexed.show()

val inputColSchema = indexed.schema(indexer.getOutputCol)
println(s"StringIndexer will store labels in output column metadata: " +
    s"${Attribute.fromStructField(inputColSchema).toString}\n")

val converter = new IndexToString()
  .setInputCol("categoryIndex")
  .setOutputCol("originalCategory")

val converted = converter.transform(indexed)

println(s"Transformed indexed column '${converter.getInputCol}' back to original string " +
    s"column '${converter.getOutputCol}' using labels in metadata")
converted.select("id", "categoryIndex", "originalCategory").show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.attribute.Attribute;
import org.apache.spark.ml.feature.IndexToString;
import org.apache.spark.ml.feature.StringIndexer;
import org.apache.spark.ml.feature.StringIndexerModel;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(0, "a"),
  RowFactory.create(1, "b"),
  RowFactory.create(2, "c"),
  RowFactory.create(3, "a"),
  RowFactory.create(4, "a"),
  RowFactory.create(5, "c")
);
StructType schema = new StructType(new StructField[]{
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("category", DataTypes.StringType, false, Metadata.empty())
});
Dataset<Row> df = spark.createDataFrame(data, schema);

StringIndexerModel indexer = new StringIndexer()
  .setInputCol("category")
  .setOutputCol("categoryIndex")
  .fit(df);
Dataset<Row> indexed = indexer.transform(df);

System.out.println("Transformed string column '" + indexer.getInputCol() + "' " +
    "to indexed column '" + indexer.getOutputCol() + "'");
indexed.show();

StructField inputColSchema = indexed.schema().apply(indexer.getOutputCol());
System.out.println("StringIndexer will store labels in output column metadata: " +
    Attribute.fromStructField(inputColSchema).toString() + "\n");

IndexToString converter = new IndexToString()
  .setInputCol("categoryIndex")
  .setOutputCol("originalCategory");
Dataset<Row> converted = converter.transform(indexed);

System.out.println("Transformed indexed column '" + converter.getInputCol() + "' back to " +
    "original string column '" + converter.getOutputCol() + "' using labels in metadata");
converted.select("id", "categoryIndex", "originalCategory").show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import IndexToString, StringIndexer

df = spark.createDataFrame(
    [(0, "a"), (1, "b"), (2, "c"), (3, "a"), (4, "a"), (5, "c")],
    ["id", "category"])

indexer = StringIndexer(inputCol="category", outputCol="categoryIndex")
model = indexer.fit(df)
indexed = model.transform(df)

print("Transformed string column '%s' to indexed column '%s'"
      % (indexer.getInputCol(), indexer.getOutputCol()))
indexed.show()

print("StringIndexer will store labels in output column metadata\n")

converter = IndexToString(inputCol="categoryIndex", outputCol="originalCategory")
converted = converter.transform(indexed)

print("Transformed indexed column '%s' back to original string column '%s' using "
      "labels in metadata" % (converter.getInputCol(), converter.getOutputCol()))
converted.select("id", "categoryIndex", "originalCategory").show()
```
{% endtab %}
{% endtabs %}

## OneHotEncoderEstimator

[One-hot encoding](http://en.wikipedia.org/wiki/One-hot) maps a categorical feature, represented as a label index, to a binary vector with at most a single one-value indicating the presence of a specific feature value from among the set of all feature values. This encoding allows algorithms which expect continuous features, such as Logistic Regression, to use categorical features. For string type input data, it is common to encode categorical features using [StringIndexer](https://spark.apache.org/docs/2.4.4/ml-features.html#stringindexer) first.

`OneHotEncoderEstimator` can transform multiple columns, returning an one-hot-encoded output vector column for each input column. It is common to merge these vectors into a single feature vector using [VectorAssembler](https://spark.apache.org/docs/2.4.4/ml-features.html#vectorassembler).

`OneHotEncoderEstimator` supports the `handleInvalid` parameter to choose how to handle invalid input during transforming data. Available options include ‘keep’ \(any invalid inputs are assigned to an extra categorical index\) and ‘error’ \(throw an error\).

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.OneHotEncoderEstimator

val df = spark.createDataFrame(Seq(
  (0.0, 1.0),
  (1.0, 0.0),
  (2.0, 1.0),
  (0.0, 2.0),
  (0.0, 1.0),
  (2.0, 0.0)
)).toDF("categoryIndex1", "categoryIndex2")

val encoder = new OneHotEncoderEstimator()
  .setInputCols(Array("categoryIndex1", "categoryIndex2"))
  .setOutputCols(Array("categoryVec1", "categoryVec2"))
val model = encoder.fit(df)

val encoded = model.transform(df)
encoded.show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.OneHotEncoderEstimator;
import org.apache.spark.ml.feature.OneHotEncoderModel;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(0.0, 1.0),
  RowFactory.create(1.0, 0.0),
  RowFactory.create(2.0, 1.0),
  RowFactory.create(0.0, 2.0),
  RowFactory.create(0.0, 1.0),
  RowFactory.create(2.0, 0.0)
);

StructType schema = new StructType(new StructField[]{
  new StructField("categoryIndex1", DataTypes.DoubleType, false, Metadata.empty()),
  new StructField("categoryIndex2", DataTypes.DoubleType, false, Metadata.empty())
});

Dataset<Row> df = spark.createDataFrame(data, schema);

OneHotEncoderEstimator encoder = new OneHotEncoderEstimator()
  .setInputCols(new String[] {"categoryIndex1", "categoryIndex2"})
  .setOutputCols(new String[] {"categoryVec1", "categoryVec2"});

OneHotEncoderModel model = encoder.fit(df);
Dataset<Row> encoded = model.transform(df);
encoded.show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import OneHotEncoderEstimator

df = spark.createDataFrame([
    (0.0, 1.0),
    (1.0, 0.0),
    (2.0, 1.0),
    (0.0, 2.0),
    (0.0, 1.0),
    (2.0, 0.0)
], ["categoryIndex1", "categoryIndex2"])

encoder = OneHotEncoderEstimator(inputCols=["categoryIndex1", "categoryIndex2"],
                                 outputCols=["categoryVec1", "categoryVec2"])
model = encoder.fit(df)
encoded = model.transform(df)
encoded.show()
```
{% endtab %}
{% endtabs %}

## VectorIndexer

`VectorIndexer` helps index categorical features in datasets of `Vector`s. It can both automatically decide which features are categorical and convert original values to category indices. Specifically, it does the following:

1. Take an input column of type [Vector](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.linalg.Vector) and a parameter `maxCategories`.
2. Decide which features should be categorical based on the number of distinct values, where features with at most `maxCategories` are declared categorical.
3. Compute 0-based category indices for each categorical feature.
4. Index categorical features and transform original feature values to indices.

Indexing categorical features allows algorithms such as Decision Trees and Tree Ensembles to treat categorical features appropriately, improving performance.

**Examples**

In the example below, we read in a dataset of labeled points and then use `VectorIndexer` to decide which features should be treated as categorical. We transform the categorical feature values to their indices. This transformed data could then be passed to algorithms such as `DecisionTreeRegressor` that handle categorical features.

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.VectorIndexer

val data = spark.read.format("libsvm").load("data/mllib/sample_libsvm_data.txt")

val indexer = new VectorIndexer()
  .setInputCol("features")
  .setOutputCol("indexed")
  .setMaxCategories(10)

val indexerModel = indexer.fit(data)

val categoricalFeatures: Set[Int] = indexerModel.categoryMaps.keys.toSet
println(s"Chose ${categoricalFeatures.size} " +
  s"categorical features: ${categoricalFeatures.mkString(", ")}")

// Create new column "indexed" with categorical values transformed to indices
val indexedData = indexerModel.transform(data)
indexedData.show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Map;

import org.apache.spark.ml.feature.VectorIndexer;
import org.apache.spark.ml.feature.VectorIndexerModel;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;

Dataset<Row> data = spark.read().format("libsvm").load("data/mllib/sample_libsvm_data.txt");

VectorIndexer indexer = new VectorIndexer()
  .setInputCol("features")
  .setOutputCol("indexed")
  .setMaxCategories(10);
VectorIndexerModel indexerModel = indexer.fit(data);

Map<Integer, Map<Double, Integer>> categoryMaps = indexerModel.javaCategoryMaps();
System.out.print("Chose " + categoryMaps.size() + " categorical features:");

for (Integer feature : categoryMaps.keySet()) {
  System.out.print(" " + feature);
}
System.out.println();

// Create new column "indexed" with categorical values transformed to indices
Dataset<Row> indexedData = indexerModel.transform(data);
indexedData.show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import VectorIndexer

data = spark.read.format("libsvm").load("data/mllib/sample_libsvm_data.txt")

indexer = VectorIndexer(inputCol="features", outputCol="indexed", maxCategories=10)
indexerModel = indexer.fit(data)

categoricalFeatures = indexerModel.categoryMaps
print("Chose %d categorical features: %s" %
      (len(categoricalFeatures), ", ".join(str(k) for k in categoricalFeatures.keys())))

# Create new column "indexed" with categorical values transformed to indices
indexedData = indexerModel.transform(data)
indexedData.show()
```
{% endtab %}
{% endtabs %}

## Interaction

`Interaction` is a `Transformer` which takes vector or double-valued columns, and generates a single vector column that contains the product of all combinations of one value from each input column.

For example, if you have 2 vector type columns each of which has 3 dimensions as input columns, then you’ll get a 9-dimensional vector as the output column.

**Examples**

Assume that we have the following DataFrame with the columns “id1”, “vec1”, and “vec2”:

```text
  id1|vec1          |vec2          
  ---|--------------|--------------
  1  |[1.0,2.0,3.0] |[8.0,4.0,5.0] 
  2  |[4.0,3.0,8.0] |[7.0,9.0,8.0] 
  3  |[6.0,1.0,9.0] |[2.0,3.0,6.0] 
  4  |[10.0,8.0,6.0]|[9.0,4.0,5.0] 
  5  |[9.0,2.0,7.0] |[10.0,7.0,3.0]
  6  |[1.0,1.0,4.0] |[2.0,8.0,4.0]  
```

Applying `Interaction` with those input columns, then `interactedCol` as the output column contains:

```text
  id1|vec1          |vec2          |interactedCol                                         
  ---|--------------|--------------|------------------------------------------------------
  1  |[1.0,2.0,3.0] |[8.0,4.0,5.0] |[8.0,4.0,5.0,16.0,8.0,10.0,24.0,12.0,15.0]            
  2  |[4.0,3.0,8.0] |[7.0,9.0,8.0] |[56.0,72.0,64.0,42.0,54.0,48.0,112.0,144.0,128.0]     
  3  |[6.0,1.0,9.0] |[2.0,3.0,6.0] |[36.0,54.0,108.0,6.0,9.0,18.0,54.0,81.0,162.0]        
  4  |[10.0,8.0,6.0]|[9.0,4.0,5.0] |[360.0,160.0,200.0,288.0,128.0,160.0,216.0,96.0,120.0]
  5  |[9.0,2.0,7.0] |[10.0,7.0,3.0]|[450.0,315.0,135.0,100.0,70.0,30.0,350.0,245.0,105.0] 
  6  |[1.0,1.0,4.0] |[2.0,8.0,4.0] |[12.0,48.0,24.0,12.0,48.0,24.0,48.0,192.0,96.0] 
```

**Examples**

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.Interaction
import org.apache.spark.ml.feature.VectorAssembler

val df = spark.createDataFrame(Seq(
  (1, 1, 2, 3, 8, 4, 5),
  (2, 4, 3, 8, 7, 9, 8),
  (3, 6, 1, 9, 2, 3, 6),
  (4, 10, 8, 6, 9, 4, 5),
  (5, 9, 2, 7, 10, 7, 3),
  (6, 1, 1, 4, 2, 8, 4)
)).toDF("id1", "id2", "id3", "id4", "id5", "id6", "id7")

val assembler1 = new VectorAssembler().
  setInputCols(Array("id2", "id3", "id4")).
  setOutputCol("vec1")

val assembled1 = assembler1.transform(df)

val assembler2 = new VectorAssembler().
  setInputCols(Array("id5", "id6", "id7")).
  setOutputCol("vec2")

val assembled2 = assembler2.transform(assembled1).select("id1", "vec1", "vec2")

val interaction = new Interaction()
  .setInputCols(Array("id1", "vec1", "vec2"))
  .setOutputCol("interactedCol")

val interacted = interaction.transform(assembled2)

interacted.show(truncate = false)
```
{% endtab %}

{% tab title="Java" %}
```java
List<Row> data = Arrays.asList(
  RowFactory.create(1, 1, 2, 3, 8, 4, 5),
  RowFactory.create(2, 4, 3, 8, 7, 9, 8),
  RowFactory.create(3, 6, 1, 9, 2, 3, 6),
  RowFactory.create(4, 10, 8, 6, 9, 4, 5),
  RowFactory.create(5, 9, 2, 7, 10, 7, 3),
  RowFactory.create(6, 1, 1, 4, 2, 8, 4)
);

StructType schema = new StructType(new StructField[]{
  new StructField("id1", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("id2", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("id3", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("id4", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("id5", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("id6", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("id7", DataTypes.IntegerType, false, Metadata.empty())
});

Dataset<Row> df = spark.createDataFrame(data, schema);

VectorAssembler assembler1 = new VectorAssembler()
        .setInputCols(new String[]{"id2", "id3", "id4"})
        .setOutputCol("vec1");

Dataset<Row> assembled1 = assembler1.transform(df);

VectorAssembler assembler2 = new VectorAssembler()
        .setInputCols(new String[]{"id5", "id6", "id7"})
        .setOutputCol("vec2");

Dataset<Row> assembled2 = assembler2.transform(assembled1).select("id1", "vec1", "vec2");

Interaction interaction = new Interaction()
        .setInputCols(new String[]{"id1","vec1","vec2"})
        .setOutputCol("interactedCol");

Dataset<Row> interacted = interaction.transform(assembled2);

interacted.show(false);
```
{% endtab %}
{% endtabs %}

## Normalizer

`Normalizer` is a `Transformer` which transforms a dataset of `Vector` rows, normalizing each `Vector` to have unit norm. It takes parameter `p`, which specifies the [p-norm](http://en.wikipedia.org/wiki/Norm_%28mathematics%29#p-norm) used for normalization. \(p=2p=2 by default.\) This normalization can help standardize your input data and improve the behavior of learning algorithms.

**Examples**

The following example demonstrates how to load a dataset in libsvm format and then normalize each row to have unit L1 norm and unit L∞ norm.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.Normalizer
import org.apache.spark.ml.linalg.Vectors

val dataFrame = spark.createDataFrame(Seq(
  (0, Vectors.dense(1.0, 0.5, -1.0)),
  (1, Vectors.dense(2.0, 1.0, 1.0)),
  (2, Vectors.dense(4.0, 10.0, 2.0))
)).toDF("id", "features")

// Normalize each Vector using $L^1$ norm.
val normalizer = new Normalizer()
  .setInputCol("features")
  .setOutputCol("normFeatures")
  .setP(1.0)

val l1NormData = normalizer.transform(dataFrame)
println("Normalized using L^1 norm")
l1NormData.show()

// Normalize each Vector using $L^\infty$ norm.
val lInfNormData = normalizer.transform(dataFrame, normalizer.p -> Double.PositiveInfinity)
println("Normalized using L^inf norm")
lInfNormData.show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.Normalizer;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
    RowFactory.create(0, Vectors.dense(1.0, 0.1, -8.0)),
    RowFactory.create(1, Vectors.dense(2.0, 1.0, -4.0)),
    RowFactory.create(2, Vectors.dense(4.0, 10.0, 8.0))
);
StructType schema = new StructType(new StructField[]{
    new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
    new StructField("features", new VectorUDT(), false, Metadata.empty())
});
Dataset<Row> dataFrame = spark.createDataFrame(data, schema);

// Normalize each Vector using $L^1$ norm.
Normalizer normalizer = new Normalizer()
  .setInputCol("features")
  .setOutputCol("normFeatures")
  .setP(1.0);

Dataset<Row> l1NormData = normalizer.transform(dataFrame);
l1NormData.show();

// Normalize each Vector using $L^\infty$ norm.
Dataset<Row> lInfNormData =
  normalizer.transform(dataFrame, normalizer.p().w(Double.POSITIVE_INFINITY));
lInfNormData.show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import Normalizer
from pyspark.ml.linalg import Vectors

dataFrame = spark.createDataFrame([
    (0, Vectors.dense([1.0, 0.5, -1.0]),),
    (1, Vectors.dense([2.0, 1.0, 1.0]),),
    (2, Vectors.dense([4.0, 10.0, 2.0]),)
], ["id", "features"])

# Normalize each Vector using $L^1$ norm.
normalizer = Normalizer(inputCol="features", outputCol="normFeatures", p=1.0)
l1NormData = normalizer.transform(dataFrame)
print("Normalized using L^1 norm")
l1NormData.show()

# Normalize each Vector using $L^\infty$ norm.
lInfNormData = normalizer.transform(dataFrame, {normalizer.p: float("inf")})
print("Normalized using L^inf norm")
lInfNormData.show()
```
{% endtab %}
{% endtabs %}

## StandardScaler

`StandardScaler` transforms a dataset of `Vector` rows, normalizing each feature to have unit standard deviation and/or zero mean. It takes parameters:

* `withStd`: True by default. Scales the data to unit standard deviation.
* `withMean`: False by default. Centers the data with mean before scaling. It will build a dense output, so take care when applying to sparse input.

`StandardScaler` is an `Estimator` which can be `fit` on a dataset to produce a `StandardScalerModel`; this amounts to computing summary statistics. The model can then transform a `Vector` column in a dataset to have unit standard deviation and/or zero mean features.

Note that if the standard deviation of a feature is zero, it will return default `0.0` value in the `Vector` for that feature.

**Examples**

The following example demonstrates how to load a dataset in libsvm format and then normalize each feature to have unit standard deviation.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.StandardScaler

val dataFrame = spark.read.format("libsvm").load("data/mllib/sample_libsvm_data.txt")

val scaler = new StandardScaler()
  .setInputCol("features")
  .setOutputCol("scaledFeatures")
  .setWithStd(true)
  .setWithMean(false)

// Compute summary statistics by fitting the StandardScaler.
val scalerModel = scaler.fit(dataFrame)

// Normalize each feature to have unit standard deviation.
val scaledData = scalerModel.transform(dataFrame)
scaledData.show()
```
{% endtab %}

{% tab title="Java" %}
```java
import org.apache.spark.ml.feature.StandardScaler;
import org.apache.spark.ml.feature.StandardScalerModel;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;

Dataset<Row> dataFrame =
  spark.read().format("libsvm").load("data/mllib/sample_libsvm_data.txt");

StandardScaler scaler = new StandardScaler()
  .setInputCol("features")
  .setOutputCol("scaledFeatures")
  .setWithStd(true)
  .setWithMean(false);

// Compute summary statistics by fitting the StandardScaler
StandardScalerModel scalerModel = scaler.fit(dataFrame);

// Normalize each feature to have unit standard deviation.
Dataset<Row> scaledData = scalerModel.transform(dataFrame);
scaledData.show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import StandardScaler

dataFrame = spark.read.format("libsvm").load("data/mllib/sample_libsvm_data.txt")
scaler = StandardScaler(inputCol="features", outputCol="scaledFeatures",
                        withStd=True, withMean=False)

# Compute summary statistics by fitting the StandardScaler
scalerModel = scaler.fit(dataFrame)

# Normalize each feature to have unit standard deviation.
scaledData = scalerModel.transform(dataFrame)
scaledData.show()
```
{% endtab %}
{% endtabs %}

## MinMaxScaler

`MinMaxScaler` transforms a dataset of `Vector` rows, rescaling each feature to a specific range \(often \[0, 1\]\). It takes parameters:

* `min`: 0.0 by default. Lower bound after transformation, shared by all features.
* `max`: 1.0 by default. Upper bound after transformation, shared by all features.

`MinMaxScaler` computes summary statistics on a data set and produces a `MinMaxScalerModel`. The model can then transform each feature individually such that it is in the given range.

Note that since zero values will probably be transformed to non-zero values, output of the transformer will be `DenseVector` even for sparse input.

**Examples**

The following example demonstrates how to load a dataset in libsvm format and then rescale each feature to \[0, 1\].

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.MinMaxScaler
import org.apache.spark.ml.linalg.Vectors

val dataFrame = spark.createDataFrame(Seq(
  (0, Vectors.dense(1.0, 0.1, -1.0)),
  (1, Vectors.dense(2.0, 1.1, 1.0)),
  (2, Vectors.dense(3.0, 10.1, 3.0))
)).toDF("id", "features")

val scaler = new MinMaxScaler()
  .setInputCol("features")
  .setOutputCol("scaledFeatures")

// Compute summary statistics and generate MinMaxScalerModel
val scalerModel = scaler.fit(dataFrame)

// rescale each feature to range [min, max].
val scaledData = scalerModel.transform(dataFrame)
println(s"Features scaled to range: [${scaler.getMin}, ${scaler.getMax}]")
scaledData.select("features", "scaledFeatures").show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.MinMaxScaler;
import org.apache.spark.ml.feature.MinMaxScalerModel;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
    RowFactory.create(0, Vectors.dense(1.0, 0.1, -1.0)),
    RowFactory.create(1, Vectors.dense(2.0, 1.1, 1.0)),
    RowFactory.create(2, Vectors.dense(3.0, 10.1, 3.0))
);
StructType schema = new StructType(new StructField[]{
    new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
    new StructField("features", new VectorUDT(), false, Metadata.empty())
});
Dataset<Row> dataFrame = spark.createDataFrame(data, schema);

MinMaxScaler scaler = new MinMaxScaler()
  .setInputCol("features")
  .setOutputCol("scaledFeatures");

// Compute summary statistics and generate MinMaxScalerModel
MinMaxScalerModel scalerModel = scaler.fit(dataFrame);

// rescale each feature to range [min, max].
Dataset<Row> scaledData = scalerModel.transform(dataFrame);
System.out.println("Features scaled to range: [" + scaler.getMin() + ", "
    + scaler.getMax() + "]");
scaledData.select("features", "scaledFeatures").show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import MinMaxScaler
from pyspark.ml.linalg import Vectors

dataFrame = spark.createDataFrame([
    (0, Vectors.dense([1.0, 0.1, -1.0]),),
    (1, Vectors.dense([2.0, 1.1, 1.0]),),
    (2, Vectors.dense([3.0, 10.1, 3.0]),)
], ["id", "features"])

scaler = MinMaxScaler(inputCol="features", outputCol="scaledFeatures")

# Compute summary statistics and generate MinMaxScalerModel
scalerModel = scaler.fit(dataFrame)

# rescale each feature to range [min, max].
scaledData = scalerModel.transform(dataFrame)
print("Features scaled to range: [%f, %f]" % (scaler.getMin(), scaler.getMax()))
scaledData.select("features", "scaledFeatures").show()
```
{% endtab %}
{% endtabs %}

## **M**axAbsScaler

`MaxAbsScaler` transforms a dataset of `Vector` rows, rescaling each feature to range \[-1, 1\] by dividing through the maximum absolute value in each feature. It does not shift/center the data, and thus does not destroy any sparsity.

`MaxAbsScaler` computes summary statistics on a data set and produces a `MaxAbsScalerModel`. The model can then transform each feature individually to range \[-1, 1\].

**Examples**

The following example demonstrates how to load a dataset in libsvm format and then rescale each feature to \[-1, 1\].

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.MaxAbsScaler
import org.apache.spark.ml.linalg.Vectors

val dataFrame = spark.createDataFrame(Seq(
  (0, Vectors.dense(1.0, 0.1, -8.0)),
  (1, Vectors.dense(2.0, 1.0, -4.0)),
  (2, Vectors.dense(4.0, 10.0, 8.0))
)).toDF("id", "features")

val scaler = new MaxAbsScaler()
  .setInputCol("features")
  .setOutputCol("scaledFeatures")

// Compute summary statistics and generate MaxAbsScalerModel
val scalerModel = scaler.fit(dataFrame)

// rescale each feature to range [-1, 1]
val scaledData = scalerModel.transform(dataFrame)
scaledData.select("features", "scaledFeatures").show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.MaxAbsScaler;
import org.apache.spark.ml.feature.MaxAbsScalerModel;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
    RowFactory.create(0, Vectors.dense(1.0, 0.1, -8.0)),
    RowFactory.create(1, Vectors.dense(2.0, 1.0, -4.0)),
    RowFactory.create(2, Vectors.dense(4.0, 10.0, 8.0))
);
StructType schema = new StructType(new StructField[]{
    new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
    new StructField("features", new VectorUDT(), false, Metadata.empty())
});
Dataset<Row> dataFrame = spark.createDataFrame(data, schema);

MaxAbsScaler scaler = new MaxAbsScaler()
  .setInputCol("features")
  .setOutputCol("scaledFeatures");

// Compute summary statistics and generate MaxAbsScalerModel
MaxAbsScalerModel scalerModel = scaler.fit(dataFrame);

// rescale each feature to range [-1, 1].
Dataset<Row> scaledData = scalerModel.transform(dataFrame);
scaledData.select("features", "scaledFeatures").show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import MaxAbsScaler
from pyspark.ml.linalg import Vectors

dataFrame = spark.createDataFrame([
    (0, Vectors.dense([1.0, 0.1, -8.0]),),
    (1, Vectors.dense([2.0, 1.0, -4.0]),),
    (2, Vectors.dense([4.0, 10.0, 8.0]),)
], ["id", "features"])

scaler = MaxAbsScaler(inputCol="features", outputCol="scaledFeatures")

# Compute summary statistics and generate MaxAbsScalerModel
scalerModel = scaler.fit(dataFrame)

# rescale each feature to range [-1, 1].
scaledData = scalerModel.transform(dataFrame)

scaledData.select("features", "scaledFeatures").show()
```
{% endtab %}
{% endtabs %}

## Bucketizer

`Bucketizer` transforms a column of continuous features to a column of feature buckets, where the buckets are specified by users. It takes a parameter:

* `splits`: Parameter for mapping continuous features into buckets. With n+1 splits, there are n buckets. A bucket defined by splits x,y holds values in the range \[x,y\) except the last bucket, which also includes y. Splits should be strictly increasing. Values at -inf, inf must be explicitly provided to cover all Double values; Otherwise, values outside the splits specified will be treated as errors. Two examples of `splits` are `Array(Double.NegativeInfinity, 0.0, 1.0, Double.PositiveInfinity)` and `Array(0.0, 1.0, 2.0)`.

Note that if you have no idea of the upper and lower bounds of the targeted column, you should add `Double.NegativeInfinity` and `Double.PositiveInfinity` as the bounds of your splits to prevent a potential out of Bucketizer bounds exception.

Note also that the splits that you provided have to be in strictly increasing order, i.e. `s0 < s1 < s2 < ... < sn`.

More details can be found in the API docs for [Bucketizer](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.ml.feature.Bucketizer).

**Examples**

The following example demonstrates how to bucketize a column of `Double`s into another index-wised column.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.Bucketizer

val splits = Array(Double.NegativeInfinity, -0.5, 0.0, 0.5, Double.PositiveInfinity)

val data = Array(-999.9, -0.5, -0.3, 0.0, 0.2, 999.9)
val dataFrame = spark.createDataFrame(data.map(Tuple1.apply)).toDF("features")

val bucketizer = new Bucketizer()
  .setInputCol("features")
  .setOutputCol("bucketedFeatures")
  .setSplits(splits)

// Transform original data into its bucket index.
val bucketedData = bucketizer.transform(dataFrame)

println(s"Bucketizer output with ${bucketizer.getSplits.length-1} buckets")
bucketedData.show()

val splitsArray = Array(
  Array(Double.NegativeInfinity, -0.5, 0.0, 0.5, Double.PositiveInfinity),
  Array(Double.NegativeInfinity, -0.3, 0.0, 0.3, Double.PositiveInfinity))

val data2 = Array(
  (-999.9, -999.9),
  (-0.5, -0.2),
  (-0.3, -0.1),
  (0.0, 0.0),
  (0.2, 0.4),
  (999.9, 999.9))
val dataFrame2 = spark.createDataFrame(data2).toDF("features1", "features2")

val bucketizer2 = new Bucketizer()
  .setInputCols(Array("features1", "features2"))
  .setOutputCols(Array("bucketedFeatures1", "bucketedFeatures2"))
  .setSplitsArray(splitsArray)

// Transform original data into its bucket index.
val bucketedData2 = bucketizer2.transform(dataFrame2)

println(s"Bucketizer output with [" +
  s"${bucketizer2.getSplitsArray(0).length-1}, " +
  s"${bucketizer2.getSplitsArray(1).length-1}] buckets for each input column")
bucketedData2.show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.Bucketizer;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

double[] splits = {Double.NEGATIVE_INFINITY, -0.5, 0.0, 0.5, Double.POSITIVE_INFINITY};

List<Row> data = Arrays.asList(
  RowFactory.create(-999.9),
  RowFactory.create(-0.5),
  RowFactory.create(-0.3),
  RowFactory.create(0.0),
  RowFactory.create(0.2),
  RowFactory.create(999.9)
);
StructType schema = new StructType(new StructField[]{
  new StructField("features", DataTypes.DoubleType, false, Metadata.empty())
});
Dataset<Row> dataFrame = spark.createDataFrame(data, schema);

Bucketizer bucketizer = new Bucketizer()
  .setInputCol("features")
  .setOutputCol("bucketedFeatures")
  .setSplits(splits);

// Transform original data into its bucket index.
Dataset<Row> bucketedData = bucketizer.transform(dataFrame);

System.out.println("Bucketizer output with " + (bucketizer.getSplits().length-1) + " buckets");
bucketedData.show();

// Bucketize multiple columns at one pass.
double[][] splitsArray = {
  {Double.NEGATIVE_INFINITY, -0.5, 0.0, 0.5, Double.POSITIVE_INFINITY},
  {Double.NEGATIVE_INFINITY, -0.3, 0.0, 0.3, Double.POSITIVE_INFINITY}
};

List<Row> data2 = Arrays.asList(
  RowFactory.create(-999.9, -999.9),
  RowFactory.create(-0.5, -0.2),
  RowFactory.create(-0.3, -0.1),
  RowFactory.create(0.0, 0.0),
  RowFactory.create(0.2, 0.4),
  RowFactory.create(999.9, 999.9)
);
StructType schema2 = new StructType(new StructField[]{
  new StructField("features1", DataTypes.DoubleType, false, Metadata.empty()),
  new StructField("features2", DataTypes.DoubleType, false, Metadata.empty())
});
Dataset<Row> dataFrame2 = spark.createDataFrame(data2, schema2);

Bucketizer bucketizer2 = new Bucketizer()
  .setInputCols(new String[] {"features1", "features2"})
  .setOutputCols(new String[] {"bucketedFeatures1", "bucketedFeatures2"})
  .setSplitsArray(splitsArray);
// Transform original data into its bucket index.
Dataset<Row> bucketedData2 = bucketizer2.transform(dataFrame2);

System.out.println("Bucketizer output with [" +
  (bucketizer2.getSplitsArray()[0].length-1) + ", " +
  (bucketizer2.getSplitsArray()[1].length-1) + "] buckets for each input column");
bucketedData2.show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import Bucketizer

splits = [-float("inf"), -0.5, 0.0, 0.5, float("inf")]

data = [(-999.9,), (-0.5,), (-0.3,), (0.0,), (0.2,), (999.9,)]
dataFrame = spark.createDataFrame(data, ["features"])

bucketizer = Bucketizer(splits=splits, inputCol="features", outputCol="bucketedFeatures")

# Transform original data into its bucket index.
bucketedData = bucketizer.transform(dataFrame)

print("Bucketizer output with %d buckets" % (len(bucketizer.getSplits())-1))
bucketedData.show()
```
{% endtab %}
{% endtabs %}

## ElementwiseProduct

ElementwiseProduct multiplies each input vector by a provided “weight” vector, using element-wise multiplication. In other words, it scales each column of the dataset by a scalar multiplier. This represents the [Hadamard product](https://en.wikipedia.org/wiki/Hadamard_product_%28matrices%29) between the input vector, `v` and transforming vector, `w`, to yield a result vector.

**Examples**

This example below demonstrates how to transform vectors using a transforming vector value.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.ElementwiseProduct
import org.apache.spark.ml.linalg.Vectors

// Create some vector data; also works for sparse vectors
val dataFrame = spark.createDataFrame(Seq(
  ("a", Vectors.dense(1.0, 2.0, 3.0)),
  ("b", Vectors.dense(4.0, 5.0, 6.0)))).toDF("id", "vector")

val transformingVector = Vectors.dense(0.0, 1.0, 2.0)
val transformer = new ElementwiseProduct()
  .setScalingVec(transformingVector)
  .setInputCol("vector")
  .setOutputCol("transformedVector")

// Batch transform the vectors to create new column:
transformer.transform(dataFrame).show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.ElementwiseProduct;
import org.apache.spark.ml.linalg.Vector;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

// Create some vector data; also works for sparse vectors
List<Row> data = Arrays.asList(
  RowFactory.create("a", Vectors.dense(1.0, 2.0, 3.0)),
  RowFactory.create("b", Vectors.dense(4.0, 5.0, 6.0))
);

List<StructField> fields = new ArrayList<>(2);
fields.add(DataTypes.createStructField("id", DataTypes.StringType, false));
fields.add(DataTypes.createStructField("vector", new VectorUDT(), false));

StructType schema = DataTypes.createStructType(fields);

Dataset<Row> dataFrame = spark.createDataFrame(data, schema);

Vector transformingVector = Vectors.dense(0.0, 1.0, 2.0);

ElementwiseProduct transformer = new ElementwiseProduct()
  .setScalingVec(transformingVector)
  .setInputCol("vector")
  .setOutputCol("transformedVector");

// Batch transform the vectors to create new column:
transformer.transform(dataFrame).show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import ElementwiseProduct
from pyspark.ml.linalg import Vectors

# Create some vector data; also works for sparse vectors
data = [(Vectors.dense([1.0, 2.0, 3.0]),), (Vectors.dense([4.0, 5.0, 6.0]),)]
df = spark.createDataFrame(data, ["vector"])
transformer = ElementwiseProduct(scalingVec=Vectors.dense([0.0, 1.0, 2.0]),
                                 inputCol="vector", outputCol="transformedVector")
# Batch transform the vectors to create new column:
transformer.transform(df).show()
```
{% endtab %}
{% endtabs %}

## SQLTransformer

`SQLTransformer` implements the transformations which are defined by SQL statement. Currently, we only support SQL syntax like `"SELECT ... FROM __THIS__ ..."` where `"__THIS__"` represents the underlying table of the input dataset. The select clause specifies the fields, constants, and expressions to display in the output, and can be any select clause that Spark SQL supports. Users can also use Spark SQL built-in function and UDFs to operate on these selected columns. For example, `SQLTransformer` supports statements like:

* `SELECT a, a + b AS a_b FROM __THIS__`
* `SELECT a, SQRT(b) AS b_sqrt FROM __THIS__ where a > 5`
* `SELECT a, b, SUM(c) AS c_sum FROM __THIS__ GROUP BY a, b`

**Examples**

Assume that we have the following DataFrame with columns `id`, `v1` and `v2`:

```text
 id |  v1 |  v2
----|-----|-----
 0  | 1.0 | 3.0  
 2  | 2.0 | 5.0
```

This is the output of the `SQLTransformer` with statement `"SELECT *, (v1 + v2) AS v3, (v1 * v2) AS v4 FROM __THIS__"`:

```text
 id |  v1 |  v2 |  v3 |  v4
----|-----|-----|-----|-----
 0  | 1.0 | 3.0 | 4.0 | 3.0
 2  | 2.0 | 5.0 | 7.0 |10.0
```

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.SQLTransformer

val df = spark.createDataFrame(
  Seq((0, 1.0, 3.0), (2, 2.0, 5.0))).toDF("id", "v1", "v2")

val sqlTrans = new SQLTransformer().setStatement(
  "SELECT *, (v1 + v2) AS v3, (v1 * v2) AS v4 FROM __THIS__")

sqlTrans.transform(df).show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.SQLTransformer;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.SparkSession;
import org.apache.spark.sql.types.*;

List<Row> data = Arrays.asList(
  RowFactory.create(0, 1.0, 3.0),
  RowFactory.create(2, 2.0, 5.0)
);
StructType schema = new StructType(new StructField [] {
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("v1", DataTypes.DoubleType, false, Metadata.empty()),
  new StructField("v2", DataTypes.DoubleType, false, Metadata.empty())
});
Dataset<Row> df = spark.createDataFrame(data, schema);

SQLTransformer sqlTrans = new SQLTransformer().setStatement(
  "SELECT *, (v1 + v2) AS v3, (v1 * v2) AS v4 FROM __THIS__");

sqlTrans.transform(df).show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import SQLTransformer

df = spark.createDataFrame([
    (0, 1.0, 3.0),
    (2, 2.0, 5.0)
], ["id", "v1", "v2"])
sqlTrans = SQLTransformer(
    statement="SELECT *, (v1 + v2) AS v3, (v1 * v2) AS v4 FROM __THIS__")
sqlTrans.transform(df).show()
```
{% endtab %}
{% endtabs %}

## VectorAssembler

`VectorAssembler` is a transformer that combines a given list of columns into a single vector column. It is useful for combining raw features and features generated by different feature transformers into a single feature vector, in order to train ML models like logistic regression and decision trees. `VectorAssembler` accepts the following input column types: all numeric types, boolean type, and vector type. In each row, the values of the input columns will be concatenated into a vector in the specified order.

**Examples**

Assume that we have a DataFrame with the columns `id`, `hour`, `mobile`, `userFeatures`, and `clicked`:

```text
 id | hour | mobile | userFeatures     | clicked
----|------|--------|------------------|---------
 0  | 18   | 1.0    | [0.0, 10.0, 0.5] | 1.0
```

`userFeatures` is a vector column that contains three user features. We want to combine `hour`, `mobile`, and `userFeatures` into a single feature vector called `features` and use it to predict `clicked` or not. If we set `VectorAssembler`’s input columns to `hour`, `mobile`, and `userFeatures` and output column to `features`, after transformation we should get the following DataFrame:

```text
 id | hour | mobile | userFeatures     | clicked | features
----|------|--------|------------------|---------|-----------------------------
 0  | 18   | 1.0    | [0.0, 10.0, 0.5] | 1.0     | [18.0, 1.0, 0.0, 10.0, 0.5]
```

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.VectorAssembler
import org.apache.spark.ml.linalg.Vectors

val dataset = spark.createDataFrame(
  Seq((0, 18, 1.0, Vectors.dense(0.0, 10.0, 0.5), 1.0))
).toDF("id", "hour", "mobile", "userFeatures", "clicked")

val assembler = new VectorAssembler()
  .setInputCols(Array("hour", "mobile", "userFeatures"))
  .setOutputCol("features")

val output = assembler.transform(dataset)
println("Assembled columns 'hour', 'mobile', 'userFeatures' to vector column 'features'")
output.select("features", "clicked").show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;

import org.apache.spark.ml.feature.VectorAssembler;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.*;
import static org.apache.spark.sql.types.DataTypes.*;

StructType schema = createStructType(new StructField[]{
  createStructField("id", IntegerType, false),
  createStructField("hour", IntegerType, false),
  createStructField("mobile", DoubleType, false),
  createStructField("userFeatures", new VectorUDT(), false),
  createStructField("clicked", DoubleType, false)
});
Row row = RowFactory.create(0, 18, 1.0, Vectors.dense(0.0, 10.0, 0.5), 1.0);
Dataset<Row> dataset = spark.createDataFrame(Arrays.asList(row), schema);

VectorAssembler assembler = new VectorAssembler()
  .setInputCols(new String[]{"hour", "mobile", "userFeatures"})
  .setOutputCol("features");

Dataset<Row> output = assembler.transform(dataset);
System.out.println("Assembled columns 'hour', 'mobile', 'userFeatures' to vector column " +
    "'features'");
output.select("features", "clicked").show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.linalg import Vectors
from pyspark.ml.feature import VectorAssembler

dataset = spark.createDataFrame(
    [(0, 18, 1.0, Vectors.dense([0.0, 10.0, 0.5]), 1.0)],
    ["id", "hour", "mobile", "userFeatures", "clicked"])

assembler = VectorAssembler(
    inputCols=["hour", "mobile", "userFeatures"],
    outputCol="features")

output = assembler.transform(dataset)
print("Assembled columns 'hour', 'mobile', 'userFeatures' to vector column 'features'")
output.select("features", "clicked").show(truncate=False)
```
{% endtab %}
{% endtabs %}

## VectorSizeHint

It can sometimes be useful to explicitly specify the size of the vectors for a column of `VectorType`. For example, `VectorAssembler` uses size information from its input columns to produce size information and metadata for its output column. While in some cases this information can be obtained by inspecting the contents of the column, in a streaming dataframe the contents are not available until the stream is started. `VectorSizeHint` allows a user to explicitly specify the vector size for a column so that `VectorAssembler`, or other transformers that might need to know vector size, can use that column as an input.

To use `VectorSizeHint` a user must set the `inputCol` and `size` parameters. Applying this transformer to a dataframe produces a new dataframe with updated metadata for `inputCol` specifying the vector size. Downstream operations on the resulting dataframe can get this size using the meatadata.

`VectorSizeHint` can also take an optional `handleInvalid` parameter which controls its behaviour when the vector column contains nulls or vectors of the wrong size. By default `handleInvalid` is set to “error”, indicating an exception should be thrown. This parameter can also be set to “skip”, indicating that rows containing invalid values should be filtered out from the resulting dataframe, or “optimistic”, indicating that the column should not be checked for invalid values and all rows should be kept. Note that the use of “optimistic” can cause the resulting dataframe to be in an inconsistent state, me:aning the metadata for the column `VectorSizeHint` was applied to does not match the contents of that column. Users should take care to avoid this kind of inconsistent state.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.{VectorAssembler, VectorSizeHint}
import org.apache.spark.ml.linalg.Vectors

val dataset = spark.createDataFrame(
  Seq(
    (0, 18, 1.0, Vectors.dense(0.0, 10.0, 0.5), 1.0),
    (0, 18, 1.0, Vectors.dense(0.0, 10.0), 0.0))
).toDF("id", "hour", "mobile", "userFeatures", "clicked")

val sizeHint = new VectorSizeHint()
  .setInputCol("userFeatures")
  .setHandleInvalid("skip")
  .setSize(3)

val datasetWithSize = sizeHint.transform(dataset)
println("Rows where 'userFeatures' is not the right size are filtered out")
datasetWithSize.show(false)

val assembler = new VectorAssembler()
  .setInputCols(Array("hour", "mobile", "userFeatures"))
  .setOutputCol("features")

// This dataframe can be used by downstream transformers as before
val output = assembler.transform(datasetWithSize)
println("Assembled columns 'hour', 'mobile', 'userFeatures' to vector column 'features'")
output.select("features", "clicked").show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;

import org.apache.spark.ml.feature.VectorAssembler;
import org.apache.spark.ml.feature.VectorSizeHint;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;
import static org.apache.spark.sql.types.DataTypes.*;

StructType schema = createStructType(new StructField[]{
  createStructField("id", IntegerType, false),
  createStructField("hour", IntegerType, false),
  createStructField("mobile", DoubleType, false),
  createStructField("userFeatures", new VectorUDT(), false),
  createStructField("clicked", DoubleType, false)
});
Row row0 = RowFactory.create(0, 18, 1.0, Vectors.dense(0.0, 10.0, 0.5), 1.0);
Row row1 = RowFactory.create(0, 18, 1.0, Vectors.dense(0.0, 10.0), 0.0);
Dataset<Row> dataset = spark.createDataFrame(Arrays.asList(row0, row1), schema);

VectorSizeHint sizeHint = new VectorSizeHint()
  .setInputCol("userFeatures")
  .setHandleInvalid("skip")
  .setSize(3);

Dataset<Row> datasetWithSize = sizeHint.transform(dataset);
System.out.println("Rows where 'userFeatures' is not the right size are filtered out");
datasetWithSize.show(false);

VectorAssembler assembler = new VectorAssembler()
  .setInputCols(new String[]{"hour", "mobile", "userFeatures"})
  .setOutputCol("features");

// This dataframe can be used by downstream transformers as before
Dataset<Row> output = assembler.transform(datasetWithSize);
System.out.println("Assembled columns 'hour', 'mobile', 'userFeatures' to vector column " +
    "'features'");
output.select("features", "clicked").show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.linalg import Vectors
from pyspark.ml.feature import (VectorSizeHint, VectorAssembler)

dataset = spark.createDataFrame(
    [(0, 18, 1.0, Vectors.dense([0.0, 10.0, 0.5]), 1.0),
     (0, 18, 1.0, Vectors.dense([0.0, 10.0]), 0.0)],
    ["id", "hour", "mobile", "userFeatures", "clicked"])

sizeHint = VectorSizeHint(
    inputCol="userFeatures",
    handleInvalid="skip",
    size=3)

datasetWithSize = sizeHint.transform(dataset)
print("Rows where 'userFeatures' is not the right size are filtered out")
datasetWithSize.show(truncate=False)

assembler = VectorAssembler(
    inputCols=["hour", "mobile", "userFeatures"],
    outputCol="features")

# This dataframe can be used by downstream transformers as before
output = assembler.transform(datasetWithSize)
print("Assembled columns 'hour', 'mobile', 'userFeatures' to vector column 'features'")
output.select("features", "clicked").show(truncate=False)
```
{% endtab %}
{% endtabs %}

## QuantileDiscretizer

`QuantileDiscretizer` takes a column with continuous features and outputs a column with binned categorical features. The number of bins is set by the `numBuckets` parameter. It is possible that the number of buckets used will be smaller than this value, for example, if there are too few distinct values of the input to create enough distinct quantiles.

NaN values: NaN values will be removed from the column during `QuantileDiscretizer` fitting. This will produce a `Bucketizer` model for making predictions. During the transformation, `Bucketizer` will raise an error when it finds NaN values in the dataset, but the user can also choose to either keep or remove NaN values within the dataset by setting `handleInvalid`. If the user chooses to keep NaN values, they will be handled specially and placed into their own bucket, for example, if 4 buckets are used, then non-NaN data will be put into buckets\[0-3\], but NaNs will be counted in a special bucket\[4\].

Algorithm: The bin ranges are chosen using an approximate algorithm \(see the documentation for [approxQuantile](https://spark.apache.org/docs/2.4.4/api/scala/index.html#org.apache.spark.sql.DataFrameStatFunctions) for a detailed description\). The precision of the approximation can be controlled with the `relativeError` parameter. When set to zero, exact quantiles are calculated \(**Note:** Computing exact quantiles is an expensive operation\). The lower and upper bin bounds will be `-Infinity` and `+Infinity` covering all real values.

**Examples**

Assume that we have a DataFrame with the columns `id`, `hour`:

```text
 id | hour
----|------
 0  | 18.0
----|------
 1  | 19.0
----|------
 2  | 8.0
----|------
 3  | 5.0
----|------
 4  | 2.2
```

`hour` is a continuous feature with `Double` type. We want to turn the continuous feature into a categorical one. Given `numBuckets = 3`, we should get the following DataFrame:

```text
 id | hour | result
----|------|------
 0  | 18.0 | 2.0
----|------|------
 1  | 19.0 | 2.0
----|------|------
 2  | 8.0  | 1.0
----|------|------
 3  | 5.0  | 1.0
----|------|------
 4  | 2.2  | 0.0
```

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.QuantileDiscretizer

val data = Array((0, 18.0), (1, 19.0), (2, 8.0), (3, 5.0), (4, 2.2))
val df = spark.createDataFrame(data).toDF("id", "hour")

val discretizer = new QuantileDiscretizer()
  .setInputCol("hour")
  .setOutputCol("result")
  .setNumBuckets(3)

val result = discretizer.fit(df).transform(df)
result.show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.QuantileDiscretizer;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(0, 18.0),
  RowFactory.create(1, 19.0),
  RowFactory.create(2, 8.0),
  RowFactory.create(3, 5.0),
  RowFactory.create(4, 2.2)
);

StructType schema = new StructType(new StructField[]{
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("hour", DataTypes.DoubleType, false, Metadata.empty())
});

Dataset<Row> df = spark.createDataFrame(data, schema);

QuantileDiscretizer discretizer = new QuantileDiscretizer()
  .setInputCol("hour")
  .setOutputCol("result")
  .setNumBuckets(3);

Dataset<Row> result = discretizer.fit(df).transform(df);
result.show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import QuantileDiscretizer

data = [(0, 18.0), (1, 19.0), (2, 8.0), (3, 5.0), (4, 2.2)]
df = spark.createDataFrame(data, ["id", "hour"])

discretizer = QuantileDiscretizer(numBuckets=3, inputCol="hour", outputCol="result")

result = discretizer.fit(df).transform(df)
result.show()
```
{% endtab %}
{% endtabs %}

## Imputer

The `Imputer` estimator completes missing values in a dataset, either using the mean or the median of the columns in which the missing values are located. The input columns should be of `DoubleType` or `FloatType`. Currently `Imputer` does not support categorical features and possibly creates incorrect values for columns containing categorical features. Imputer can impute custom values other than ‘NaN’ by `.setMissingValue(custom_value)`. For example, `.setMissingValue(0)` will impute all occurrences of \(0\).

**Note** all `null` values in the input columns are treated as missing, and so are also imputed.

**Examples**

Suppose that we have a DataFrame with the columns `a` and `b`:

```text
      a     |      b      
------------|-----------
     1.0    | Double.NaN
     2.0    | Double.NaN
 Double.NaN |     3.0   
     4.0    |     4.0   
     5.0    |     5.0   
```

In this example, Imputer will replace all occurrences of `Double.NaN` \(the default for the missing value\) with the mean \(the default imputation strategy\) computed from the other values in the corresponding columns. In this example, the surrogate values for columns `a` and `b` are 3.0 and 4.0 respectively. After transformation, the missing values in the output columns will be replaced by the surrogate value for the relevant column.

```text
      a     |      b     | out_a | out_b   
------------|------------|-------|-------
     1.0    | Double.NaN |  1.0  |  4.0 
     2.0    | Double.NaN |  2.0  |  4.0 
 Double.NaN |     3.0    |  3.0  |  3.0 
     4.0    |     4.0    |  4.0  |  4.0
     5.0    |     5.0    |  5.0  |  5.0 
```

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.Imputer

val df = spark.createDataFrame(Seq(
  (1.0, Double.NaN),
  (2.0, Double.NaN),
  (Double.NaN, 3.0),
  (4.0, 4.0),
  (5.0, 5.0)
)).toDF("a", "b")

val imputer = new Imputer()
  .setInputCols(Array("a", "b"))
  .setOutputCols(Array("out_a", "out_b"))

val model = imputer.fit(df)
model.transform(df).show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.Imputer;
import org.apache.spark.ml.feature.ImputerModel;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.SparkSession;
import org.apache.spark.sql.types.*;

List<Row> data = Arrays.asList(
  RowFactory.create(1.0, Double.NaN),
  RowFactory.create(2.0, Double.NaN),
  RowFactory.create(Double.NaN, 3.0),
  RowFactory.create(4.0, 4.0),
  RowFactory.create(5.0, 5.0)
);
StructType schema = new StructType(new StructField[]{
  createStructField("a", DoubleType, false),
  createStructField("b", DoubleType, false)
});
Dataset<Row> df = spark.createDataFrame(data, schema);

Imputer imputer = new Imputer()
  .setInputCols(new String[]{"a", "b"})
  .setOutputCols(new String[]{"out_a", "out_b"});

ImputerModel model = imputer.fit(df);
model.transform(df).show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import Imputer

df = spark.createDataFrame([
    (1.0, float("nan")),
    (2.0, float("nan")),
    (float("nan"), 3.0),
    (4.0, 4.0),
    (5.0, 5.0)
], ["a", "b"])

imputer = Imputer(inputCols=["a", "b"], outputCols=["out_a", "out_b"])
model = imputer.fit(df)

model.transform(df).show()
```
{% endtab %}
{% endtabs %}



