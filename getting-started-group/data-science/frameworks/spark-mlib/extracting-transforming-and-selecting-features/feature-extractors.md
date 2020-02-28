# Feature Extractors

## TF-IDF

[Term frequency-inverse document frequency \(TF-IDF\)](http://en.wikipedia.org/wiki/Tf%E2%80%93idf) is a feature vectorization method widely used in text mining to reflect the importance of a term to a document in the corpus. Denote a term by `t`, a document by `d`, and the corpus by `D`. Term frequency `TF(t,d)` is the number of times that term `t` appears in document `d`, while document frequency `DF(t,D)` is the number of documents that contains term `t`. If we only use term frequency to measure the importance, it is very easy to over-emphasize terms that appear very often but carry little information about the document, e.g. “a”, “the”, and “of”. If a term appears very often across the corpus, it means it doesn’t carry special information about a particular document. Inverse document frequency is a numerical measure of how much information a term provides:

$$
IDF(t,D)=log \frac{|D|+1}{DF(t,D)+1}
$$

where `|D|` is the total number of documents in the corpus. Since logarithm is used, if a term appears in all documents, its IDF value becomes 0. Note that a smoothing term is applied to avoid dividing by zero for terms outside the corpus. The TF-IDF measure is simply the product of TF and IDF:

$$
TFIDF(t,d,D) = TF(t,d)⋅IDF(t,D)
$$

There are several variants on the definition of term frequency and document frequency. In MLlib, we separate TF and IDF to make them flexible.

**TF**: Both `HashingTF` and `CountVectorizer` can be used to generate the term frequency vectors.

`HashingTF` is a `Transformer` which takes sets of terms and converts those sets into fixed-length feature vectors. In text processing, a “set of terms” might be a bag of words. `HashingTF` utilizes the [hashing trick](http://en.wikipedia.org/wiki/Feature_hashing). A raw feature is mapped into an index \(term\) by applying a hash function. The hash function used here is [MurmurHash 3](https://en.wikipedia.org/wiki/MurmurHash). Then term frequencies are calculated based on the mapped indices. This approach avoids the need to compute a global term-to-index map, which can be expensive for a large corpus, but it suffers from potential hash collisions, where different raw features may become the same term after hashing. To reduce the chance of collision, we can increase the target feature dimension, i.e. the number of buckets of the hash table. Since a simple modulo on the hashed value is used to determine the vector index, it is advisable to use a power of two as the feature dimension, otherwise the features will not be mapped evenly to the vector indices. The default feature dimension is `218=262,144218=262,144`. An optional binary toggle parameter controls term frequency counts. When set to true all nonzero frequency counts are set to 1. This is especially useful for discrete probabilistic models that model binary, rather than integer, counts.

`CountVectorizer` converts text documents to vectors of term counts. Refer to [CountVectorizer ](https://spark.apache.org/docs/2.4.4/ml-features.html#countvectorizer)for more details.

**IDF**: `IDF` is an `Estimator` which is fit on a dataset and produces an `IDFModel`. The `IDFModel` takes feature vectors \(generally created from `HashingTF` or `CountVectorizer`\) and scales each feature. Intuitively, it down-weights features which appear frequently in a corpus.

**Note:** `spark.ml` doesn’t provide tools for text segmentation. We refer users to the [Stanford NLP Group](http://nlp.stanford.edu/) and [scalanlp/chalk](https://github.com/scalanlp/chalk).

**Examples**

In the following code segment, we start with a set of sentences. We split each sentence into words using `Tokenizer`. For each sentence \(bag of words\), we use `HashingTF` to hash the sentence into a feature vector. We use `IDF` to rescale the feature vectors; this generally improves performance when using text as features. Our feature vectors could then be passed to a learning algorithm.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.{HashingTF, IDF, Tokenizer}

val sentenceData = spark.createDataFrame(Seq(
  (0.0, "Hi I heard about Spark"),
  (0.0, "I wish Java could use case classes"),
  (1.0, "Logistic regression models are neat")
)).toDF("label", "sentence")

val tokenizer = new Tokenizer().setInputCol("sentence").setOutputCol("words")
val wordsData = tokenizer.transform(sentenceData)

val hashingTF = new HashingTF()
  .setInputCol("words").setOutputCol("rawFeatures").setNumFeatures(20)

val featurizedData = hashingTF.transform(wordsData)
// alternatively, CountVectorizer can also be used to get term frequency vectors

val idf = new IDF().setInputCol("rawFeatures").setOutputCol("features")
val idfModel = idf.fit(featurizedData)

val rescaledData = idfModel.transform(featurizedData)
rescaledData.select("label", "features").show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.HashingTF;
import org.apache.spark.ml.feature.IDF;
import org.apache.spark.ml.feature.IDFModel;
import org.apache.spark.ml.feature.Tokenizer;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.SparkSession;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(0.0, "Hi I heard about Spark"),
  RowFactory.create(0.0, "I wish Java could use case classes"),
  RowFactory.create(1.0, "Logistic regression models are neat")
);
StructType schema = new StructType(new StructField[]{
  new StructField("label", DataTypes.DoubleType, false, Metadata.empty()),
  new StructField("sentence", DataTypes.StringType, false, Metadata.empty())
});
Dataset<Row> sentenceData = spark.createDataFrame(data, schema);

Tokenizer tokenizer = new Tokenizer().setInputCol("sentence").setOutputCol("words");
Dataset<Row> wordsData = tokenizer.transform(sentenceData);

int numFeatures = 20;
HashingTF hashingTF = new HashingTF()
  .setInputCol("words")
  .setOutputCol("rawFeatures")
  .setNumFeatures(numFeatures);

Dataset<Row> featurizedData = hashingTF.transform(wordsData);
// alternatively, CountVectorizer can also be used to get term frequency vectors

IDF idf = new IDF().setInputCol("rawFeatures").setOutputCol("features");
IDFModel idfModel = idf.fit(featurizedData);

Dataset<Row> rescaledData = idfModel.transform(featurizedData);
rescaledData.select("label", "features").show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import HashingTF, IDF, Tokenizer

sentenceData = spark.createDataFrame([
    (0.0, "Hi I heard about Spark"),
    (0.0, "I wish Java could use case classes"),
    (1.0, "Logistic regression models are neat")
], ["label", "sentence"])

tokenizer = Tokenizer(inputCol="sentence", outputCol="words")
wordsData = tokenizer.transform(sentenceData)

hashingTF = HashingTF(inputCol="words", outputCol="rawFeatures", numFeatures=20)
featurizedData = hashingTF.transform(wordsData)
# alternatively, CountVectorizer can also be used to get term frequency vectors

idf = IDF(inputCol="rawFeatures", outputCol="features")
idfModel = idf.fit(featurizedData)
rescaledData = idfModel.transform(featurizedData)

rescaledData.select("label", "features").show()
```
{% endtab %}
{% endtabs %}

## Word2Vec

`Word2Vec` is an `Estimator` which takes sequences of words representing documents and trains a `Word2VecModel`. The model maps each word to a unique fixed-size vector. The `Word2VecModel` transforms each document into a vector using the average of all words in the document; this vector can then be used as features for prediction, document similarity calculations, etc. Please refer to the [MLlib user guide on Word2Vec](https://spark.apache.org/docs/2.4.4/mllib-feature-extraction.html#word2vec) for more details.

**Examples**

In the following code segment, we start with a set of documents, each of which is represented as a sequence of words. For each document, we transform it into a feature vector. This feature vector could then be passed to a learning algorithm.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.Word2Vec
import org.apache.spark.ml.linalg.Vector
import org.apache.spark.sql.Row

// Input data: Each row is a bag of words from a sentence or document.
val documentDF = spark.createDataFrame(Seq(
  "Hi I heard about Spark".split(" "),
  "I wish Java could use case classes".split(" "),
  "Logistic regression models are neat".split(" ")
).map(Tuple1.apply)).toDF("text")

// Learn a mapping from words to Vectors.
val word2Vec = new Word2Vec()
  .setInputCol("text")
  .setOutputCol("result")
  .setVectorSize(3)
  .setMinCount(0)
val model = word2Vec.fit(documentDF)

val result = model.transform(documentDF)
result.collect().foreach { case Row(text: Seq[_], features: Vector) =>
  println(s"Text: [${text.mkString(", ")}] => \nVector: $features\n") }
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.Word2Vec;
import org.apache.spark.ml.feature.Word2VecModel;
import org.apache.spark.ml.linalg.Vector;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.SparkSession;
import org.apache.spark.sql.types.*;

// Input data: Each row is a bag of words from a sentence or document.
List<Row> data = Arrays.asList(
  RowFactory.create(Arrays.asList("Hi I heard about Spark".split(" "))),
  RowFactory.create(Arrays.asList("I wish Java could use case classes".split(" "))),
  RowFactory.create(Arrays.asList("Logistic regression models are neat".split(" ")))
);
StructType schema = new StructType(new StructField[]{
  new StructField("text", new ArrayType(DataTypes.StringType, true), false, Metadata.empty())
});
Dataset<Row> documentDF = spark.createDataFrame(data, schema);

// Learn a mapping from words to Vectors.
Word2Vec word2Vec = new Word2Vec()
  .setInputCol("text")
  .setOutputCol("result")
  .setVectorSize(3)
  .setMinCount(0);

Word2VecModel model = word2Vec.fit(documentDF);
Dataset<Row> result = model.transform(documentDF);

for (Row row : result.collectAsList()) {
  List<String> text = row.getList(0);
  Vector vector = (Vector) row.get(1);
  System.out.println("Text: " + text + " => \nVector: " + vector + "\n");
}
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import Word2Vec

# Input data: Each row is a bag of words from a sentence or document.
documentDF = spark.createDataFrame([
    ("Hi I heard about Spark".split(" "), ),
    ("I wish Java could use case classes".split(" "), ),
    ("Logistic regression models are neat".split(" "), )
], ["text"])

# Learn a mapping from words to Vectors.
word2Vec = Word2Vec(vectorSize=3, minCount=0, inputCol="text", outputCol="result")
model = word2Vec.fit(documentDF)

result = model.transform(documentDF)
for row in result.collect():
    text, vector = row
    print("Text: [%s] => \nVector: %s\n" % (", ".join(text), str(vector)))
```
{% endtab %}
{% endtabs %}

## CountVectorizer

`CountVectorizer` and `CountVectorizerModel` aim to help convert a collection of text documents to vectors of token counts. When an a-priori dictionary is not available, `CountVectorizer` can be used as an `Estimator` to extract the vocabulary, and generates a `CountVectorizerModel`. The model produces sparse representations for the documents over the vocabulary, which can then be passed to other algorithms like LDA.

During the fitting process, `CountVectorizer` will select the top `vocabSize` words ordered by term frequency across the corpus. An optional parameter `minDF` also affects the fitting process by specifying the minimum number \(or fraction if &lt; 1.0\) of documents a term must appear in to be included in the vocabulary. Another optional binary toggle parameter controls the output vector. If set to true all nonzero counts are set to 1. This is especially useful for discrete probabilistic models that model binary, rather than integer, counts.

**Examples**

Assume that we have the following DataFrame with columns `id` and `texts`:

| id | texts |
| :--- | :--- |
| 0 | Array\("a", "b", "c"\) |
| 1 | Array\("a", "b", "b", "c", "a"\) |

each row in `texts` is a document of type Array\[String\]. Invoking fit of `CountVectorizer` produces a `CountVectorizerModel` with vocabulary \(a, b, c\). Then the output column “vector” after transformation contains:

| id | texts | vector |
| :--- | :--- | :--- |
| 0 | Array\("a", "b", "c"\) | \(3,\[0,1,2\],\[1.0,1.0,1.0\]\) |
| 1 | Array\("a", "b", "b", "c", "a"\) | \(3,\[0,1,2\],\[2.0,2.0,1.0\]\) |

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.{CountVectorizer, CountVectorizerModel}

val df = spark.createDataFrame(Seq(
  (0, Array("a", "b", "c")),
  (1, Array("a", "b", "b", "c", "a"))
)).toDF("id", "words")

// fit a CountVectorizerModel from the corpus
val cvModel: CountVectorizerModel = new CountVectorizer()
  .setInputCol("words")
  .setOutputCol("features")
  .setVocabSize(3)
  .setMinDF(2)
  .fit(df)

// alternatively, define CountVectorizerModel with a-priori vocabulary
val cvm = new CountVectorizerModel(Array("a", "b", "c"))
  .setInputCol("words")
  .setOutputCol("features")

cvModel.transform(df).show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.CountVectorizer;
import org.apache.spark.ml.feature.CountVectorizerModel;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.SparkSession;
import org.apache.spark.sql.types.*;

// Input data: Each row is a bag of words from a sentence or document.
List<Row> data = Arrays.asList(
  RowFactory.create(Arrays.asList("a", "b", "c")),
  RowFactory.create(Arrays.asList("a", "b", "b", "c", "a"))
);
StructType schema = new StructType(new StructField [] {
  new StructField("text", new ArrayType(DataTypes.StringType, true), false, Metadata.empty())
});
Dataset<Row> df = spark.createDataFrame(data, schema);

// fit a CountVectorizerModel from the corpus
CountVectorizerModel cvModel = new CountVectorizer()
  .setInputCol("text")
  .setOutputCol("feature")
  .setVocabSize(3)
  .setMinDF(2)
  .fit(df);

// alternatively, define CountVectorizerModel with a-priori vocabulary
CountVectorizerModel cvm = new CountVectorizerModel(new String[]{"a", "b", "c"})
  .setInputCol("text")
  .setOutputCol("feature");

cvModel.transform(df).show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import CountVectorizer

# Input data: Each row is a bag of words with a ID.
df = spark.createDataFrame([
    (0, "a b c".split(" ")),
    (1, "a b b c a".split(" "))
], ["id", "words"])

# fit a CountVectorizerModel from the corpus.
cv = CountVectorizer(inputCol="words", outputCol="features", vocabSize=3, minDF=2.0)

model = cv.fit(df)

result = model.transform(df)
result.show(truncate=False)
```
{% endtab %}
{% endtabs %}

## FeatureHasher

Feature hashing projects a set of categorical or numerical features into a feature vector of specified dimension \(typically substantially smaller than that of the original feature space\). This is done using the [hashing trick](https://en.wikipedia.org/wiki/Feature_hashing) to map features to indices in the feature vector.

The `FeatureHasher` transformer operates on multiple columns. Each column may contain either numeric or categorical features. Behavior and handling of column data types is as follows:

* Numeric columns: For numeric features, the hash value of the column name is used to map the feature value to its index in the feature vector. By default, numeric features are not treated as categorical \(even when they are integers\). To treat them as categorical, specify the relevant columns using the `categoricalCols` parameter.
* String columns: For categorical features, the hash value of the string “column\_name=value” is used to map to the vector index, with an indicator value of `1.0`. Thus, categorical features are “one-hot” encoded \(similarly to using [OneHotEncoder](https://spark.apache.org/docs/2.4.4/ml-features.html#onehotencoder) with `dropLast=false`\).
* Boolean columns: Boolean values are treated in the same way as string columns. That is, boolean features are represented as “column\_name=true” or “column\_name=false”, with an indicator value of `1.0`.

Null \(missing\) values are ignored \(implicitly zero in the resulting feature vector\).

The hash function used here is also the [MurmurHash 3](https://en.wikipedia.org/wiki/MurmurHash) used in [HashingTF](https://spark.apache.org/docs/2.4.4/ml-features.html#tf-idf). Since a simple modulo on the hashed value is used to determine the vector index, it is advisable to use a power of two as the numFeatures parameter; otherwise the features will not be mapped evenly to the vector indices.

**Examples**

Assume that we have a DataFrame with 4 input columns `real`, `bool`, `stringNum`, and `string`. These different data types as input will illustrate the behavior of the transform to produce a column of feature vectors.

`real| bool|stringNum|string  
----|-----|---------|------  
 2.2| true|        1|   foo  
 3.3|false|        2|   bar  
 4.4|false|        3|   baz  
 5.5|false|        4|   foo`

Then the output of `FeatureHasher.transform` on this DataFrame is:

`real|bool |stringNum|string|features  
----|-----|---------|------|-------------------------------------------------------  
2.2 |true |1        |foo   |(262144,[51871, 63643,174475,253195],[1.0,1.0,2.2,1.0])  
3.3 |false|2        |bar   |(262144,[6031,  80619,140467,174475],[1.0,1.0,1.0,3.3])  
4.4 |false|3        |baz   |(262144,[24279,140467,174475,196810],[1.0,1.0,4.4,1.0])  
5.5 |false|4        |foo   |(262144,[63643,140467,168512,174475],[1.0,1.0,1.0,5.5])`

The resulting feature vectors could then be passed to a learning algorithm.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.FeatureHasher

val dataset = spark.createDataFrame(Seq(
  (2.2, true, "1", "foo"),
  (3.3, false, "2", "bar"),
  (4.4, false, "3", "baz"),
  (5.5, false, "4", "foo")
)).toDF("real", "bool", "stringNum", "string")

val hasher = new FeatureHasher()
  .setInputCols("real", "bool", "stringNum", "string")
  .setOutputCol("features")

val featurized = hasher.transform(dataset)
featurized.show(false)
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.FeatureHasher;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

List<Row> data = Arrays.asList(
  RowFactory.create(2.2, true, "1", "foo"),
  RowFactory.create(3.3, false, "2", "bar"),
  RowFactory.create(4.4, false, "3", "baz"),
  RowFactory.create(5.5, false, "4", "foo")
);
StructType schema = new StructType(new StructField[]{
  new StructField("real", DataTypes.DoubleType, false, Metadata.empty()),
  new StructField("bool", DataTypes.BooleanType, false, Metadata.empty()),
  new StructField("stringNum", DataTypes.StringType, false, Metadata.empty()),
  new StructField("string", DataTypes.StringType, false, Metadata.empty())
});
Dataset<Row> dataset = spark.createDataFrame(data, schema);

FeatureHasher hasher = new FeatureHasher()
  .setInputCols(new String[]{"real", "bool", "stringNum", "string"})
  .setOutputCol("features");

Dataset<Row> featurized = hasher.transform(dataset);

featurized.show(false);
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import FeatureHasher

dataset = spark.createDataFrame([
    (2.2, True, "1", "foo"),
    (3.3, False, "2", "bar"),
    (4.4, False, "3", "baz"),
    (5.5, False, "4", "foo")
], ["real", "bool", "stringNum", "string"])

hasher = FeatureHasher(inputCols=["real", "bool", "stringNum", "string"],
                       outputCol="features")

featurized = hasher.transform(dataset)
featurized.show(truncate=False)
```
{% endtab %}
{% endtabs %}



