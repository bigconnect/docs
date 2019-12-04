# Locality Sensitive Hashing

[Locality Sensitive Hashing \(LSH\)](https://en.wikipedia.org/wiki/Locality-sensitive_hashing) is an important class of hashing techniques, which is commonly used in clustering, approximate nearest neighbor search and outlier detection with large datasets.

The general idea of LSH is to use a family of functions \(“LSH families”\) to hash data points into buckets, so that the data points which are close to each other are in the same buckets with high probability, while data points that are far away from each other are very likely in different buckets.

In Spark, different LSH families are implemented in separate classes \(e.g., `MinHash`\), and APIs for feature transformation, approximate similarity join and approximate nearest neighbor are provided in each class.

## LSH Operations

We describe the major types of operations which LSH can be used for. A fitted LSH model has methods for each of these operations.

### Feature Transformation

Feature transformation is the basic functionality to add hashed values as a new column. This can be useful for dimensionality reduction. Users can specify input and output column names by setting `inputCol` and `outputCol`.

LSH also supports multiple LSH hash tables. Users can specify the number of hash tables by setting `numHashTables`. This is also used for [OR-amplification](https://en.wikipedia.org/wiki/Locality-sensitive_hashing#Amplification) in approximate similarity join and approximate nearest neighbor. Increasing the number of hash tables will increase the accuracy but will also increase communication cost and running time.

The type of `outputCol` is `Seq[Vector]` where the dimension of the array equals `numHashTables`, and the dimensions of the vectors are currently set to 1. In future releases, we will implement AND-amplification so that users can specify the dimensions of these vectors.

### Approximate Similarity Join

Approximate similarity join takes two datasets and approximately returns pairs of rows in the datasets whose distance is smaller than a user-defined threshold. Approximate similarity join supports both joining two different datasets and self-joining. Self-joining will produce some duplicate pairs.

Approximate similarity join accepts both transformed and untransformed datasets as input. If an untransformed dataset is used, it will be transformed automatically. In this case, the hash signature will be created as `outputCol`.

In the joined dataset, the origin datasets can be queried in `datasetA` and `datasetB`. A distance column will be added to the output dataset to show the true distance between each pair of rows returned.

### Approximate Nearest Neighbor Search

Approximate nearest neighbor search takes a dataset \(of feature vectors\) and a key \(a single feature vector\), and it approximately returns a specified number of rows in the dataset that are closest to the vector.

Approximate nearest neighbor search accepts both transformed and untransformed datasets as input. If an untransformed dataset is used, it will be transformed automatically. In this case, the hash signature will be created as `outputCol`.

A distance column will be added to the output dataset to show the true distance between each output row and the searched key.

**Note:** Approximate nearest neighbor search will return fewer than `k` rows when there are not enough candidates in the hash bucket.

## LSH Algorithms

### Bucketed Random Projection for Euclidean Distance

[Bucketed Random Projection](https://en.wikipedia.org/wiki/Locality-sensitive_hashing#Stable_distributions) is an LSH family for Euclidean distance.  Bucketed Random Projection accepts arbitrary vectors as input features, and supports both sparse and dense vectors.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.BucketedRandomProjectionLSH
import org.apache.spark.ml.linalg.Vectors
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions.col

val dfA = spark.createDataFrame(Seq(
  (0, Vectors.dense(1.0, 1.0)),
  (1, Vectors.dense(1.0, -1.0)),
  (2, Vectors.dense(-1.0, -1.0)),
  (3, Vectors.dense(-1.0, 1.0))
)).toDF("id", "features")

val dfB = spark.createDataFrame(Seq(
  (4, Vectors.dense(1.0, 0.0)),
  (5, Vectors.dense(-1.0, 0.0)),
  (6, Vectors.dense(0.0, 1.0)),
  (7, Vectors.dense(0.0, -1.0))
)).toDF("id", "features")

val key = Vectors.dense(1.0, 0.0)

val brp = new BucketedRandomProjectionLSH()
  .setBucketLength(2.0)
  .setNumHashTables(3)
  .setInputCol("features")
  .setOutputCol("hashes")

val model = brp.fit(dfA)

// Feature Transformation
println("The hashed dataset where hashed values are stored in the column 'hashes':")
model.transform(dfA).show()

// Compute the locality sensitive hashes for the input rows, then perform approximate
// similarity join.
// We could avoid computing hashes by passing in the already-transformed dataset, e.g.
// `model.approxSimilarityJoin(transformedA, transformedB, 1.5)`
println("Approximately joining dfA and dfB on Euclidean distance smaller than 1.5:")
model.approxSimilarityJoin(dfA, dfB, 1.5, "EuclideanDistance")
  .select(col("datasetA.id").alias("idA"),
    col("datasetB.id").alias("idB"),
    col("EuclideanDistance")).show()

// Compute the locality sensitive hashes for the input rows, then perform approximate nearest
// neighbor search.
// We could avoid computing hashes by passing in the already-transformed dataset, e.g.
// `model.approxNearestNeighbors(transformedA, key, 2)`
println("Approximately searching dfA for 2 nearest neighbors of the key:")
model.approxNearestNeighbors(dfA, key, 2).show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.BucketedRandomProjectionLSH;
import org.apache.spark.ml.feature.BucketedRandomProjectionLSHModel;
import org.apache.spark.ml.linalg.Vector;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

import static org.apache.spark.sql.functions.col;

List<Row> dataA = Arrays.asList(
  RowFactory.create(0, Vectors.dense(1.0, 1.0)),
  RowFactory.create(1, Vectors.dense(1.0, -1.0)),
  RowFactory.create(2, Vectors.dense(-1.0, -1.0)),
  RowFactory.create(3, Vectors.dense(-1.0, 1.0))
);

List<Row> dataB = Arrays.asList(
    RowFactory.create(4, Vectors.dense(1.0, 0.0)),
    RowFactory.create(5, Vectors.dense(-1.0, 0.0)),
    RowFactory.create(6, Vectors.dense(0.0, 1.0)),
    RowFactory.create(7, Vectors.dense(0.0, -1.0))
);

StructType schema = new StructType(new StructField[]{
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("features", new VectorUDT(), false, Metadata.empty())
});
Dataset<Row> dfA = spark.createDataFrame(dataA, schema);
Dataset<Row> dfB = spark.createDataFrame(dataB, schema);

Vector key = Vectors.dense(1.0, 0.0);

BucketedRandomProjectionLSH mh = new BucketedRandomProjectionLSH()
  .setBucketLength(2.0)
  .setNumHashTables(3)
  .setInputCol("features")
  .setOutputCol("hashes");

BucketedRandomProjectionLSHModel model = mh.fit(dfA);

// Feature Transformation
System.out.println("The hashed dataset where hashed values are stored in the column 'hashes':");
model.transform(dfA).show();

// Compute the locality sensitive hashes for the input rows, then perform approximate
// similarity join.
// We could avoid computing hashes by passing in the already-transformed dataset, e.g.
// `model.approxSimilarityJoin(transformedA, transformedB, 1.5)`
System.out.println("Approximately joining dfA and dfB on distance smaller than 1.5:");
model.approxSimilarityJoin(dfA, dfB, 1.5, "EuclideanDistance")
  .select(col("datasetA.id").alias("idA"),
    col("datasetB.id").alias("idB"),
    col("EuclideanDistance")).show();

// Compute the locality sensitive hashes for the input rows, then perform approximate nearest
// neighbor search.
// We could avoid computing hashes by passing in the already-transformed dataset, e.g.
// `model.approxNearestNeighbors(transformedA, key, 2)`
System.out.println("Approximately searching dfA for 2 nearest neighbors of the key:");
model.approxNearestNeighbors(dfA, key, 2).show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import BucketedRandomProjectionLSH
from pyspark.ml.linalg import Vectors
from pyspark.sql.functions import col

dataA = [(0, Vectors.dense([1.0, 1.0]),),
         (1, Vectors.dense([1.0, -1.0]),),
         (2, Vectors.dense([-1.0, -1.0]),),
         (3, Vectors.dense([-1.0, 1.0]),)]
dfA = spark.createDataFrame(dataA, ["id", "features"])

dataB = [(4, Vectors.dense([1.0, 0.0]),),
         (5, Vectors.dense([-1.0, 0.0]),),
         (6, Vectors.dense([0.0, 1.0]),),
         (7, Vectors.dense([0.0, -1.0]),)]
dfB = spark.createDataFrame(dataB, ["id", "features"])

key = Vectors.dense([1.0, 0.0])

brp = BucketedRandomProjectionLSH(inputCol="features", outputCol="hashes", bucketLength=2.0,
                                  numHashTables=3)
model = brp.fit(dfA)

# Feature Transformation
print("The hashed dataset where hashed values are stored in the column 'hashes':")
model.transform(dfA).show()

# Compute the locality sensitive hashes for the input rows, then perform approximate
# similarity join.
# We could avoid computing hashes by passing in the already-transformed dataset, e.g.
# `model.approxSimilarityJoin(transformedA, transformedB, 1.5)`
print("Approximately joining dfA and dfB on Euclidean distance smaller than 1.5:")
model.approxSimilarityJoin(dfA, dfB, 1.5, distCol="EuclideanDistance")\
    .select(col("datasetA.id").alias("idA"),
            col("datasetB.id").alias("idB"),
            col("EuclideanDistance")).show()

# Compute the locality sensitive hashes for the input rows, then perform approximate nearest
# neighbor search.
# We could avoid computing hashes by passing in the already-transformed dataset, e.g.
# `model.approxNearestNeighbors(transformedA, key, 2)`
print("Approximately searching dfA for 2 nearest neighbors of the key:")
model.approxNearestNeighbors(dfA, key, 2).show()
```
{% endtab %}
{% endtabs %}

### MinHash for Jaccard Distance

[MinHash](https://en.wikipedia.org/wiki/MinHash) is an LSH family for Jaccard distance where input features are sets of natural numbers. Jaccard distance of two sets is defined by the cardinality of their intersection and union. 

MinHash applies a random hash function `g` to each element in the set and take the minimum of all hashed values.

The input sets for MinHash are represented as binary vectors, where the vector indices represent the elements themselves and the non-zero values in the vector represent the presence of that element in the set. While both dense and sparse vectors are supported, typically sparse vectors are recommended for efficiency. For example, `Vectors.sparse(10, Array[(2, 1.0), (3, 1.0), (5, 1.0)])` means there are 10 elements in the space. This set contains elem 2, elem 3 and elem 5. All non-zero values are treated as binary “1” values.

**Note:** Empty sets cannot be transformed by MinHash, which means any input vector must have at least 1 non-zero entry.

{% tabs %}
{% tab title="Scala" %}
```scala
import org.apache.spark.ml.feature.MinHashLSH
import org.apache.spark.ml.linalg.Vectors
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions.col

val dfA = spark.createDataFrame(Seq(
  (0, Vectors.sparse(6, Seq((0, 1.0), (1, 1.0), (2, 1.0)))),
  (1, Vectors.sparse(6, Seq((2, 1.0), (3, 1.0), (4, 1.0)))),
  (2, Vectors.sparse(6, Seq((0, 1.0), (2, 1.0), (4, 1.0))))
)).toDF("id", "features")

val dfB = spark.createDataFrame(Seq(
  (3, Vectors.sparse(6, Seq((1, 1.0), (3, 1.0), (5, 1.0)))),
  (4, Vectors.sparse(6, Seq((2, 1.0), (3, 1.0), (5, 1.0)))),
  (5, Vectors.sparse(6, Seq((1, 1.0), (2, 1.0), (4, 1.0))))
)).toDF("id", "features")

val key = Vectors.sparse(6, Seq((1, 1.0), (3, 1.0)))

val mh = new MinHashLSH()
  .setNumHashTables(5)
  .setInputCol("features")
  .setOutputCol("hashes")

val model = mh.fit(dfA)

// Feature Transformation
println("The hashed dataset where hashed values are stored in the column 'hashes':")
model.transform(dfA).show()

// Compute the locality sensitive hashes for the input rows, then perform approximate
// similarity join.
// We could avoid computing hashes by passing in the already-transformed dataset, e.g.
// `model.approxSimilarityJoin(transformedA, transformedB, 0.6)`
println("Approximately joining dfA and dfB on Jaccard distance smaller than 0.6:")
model.approxSimilarityJoin(dfA, dfB, 0.6, "JaccardDistance")
  .select(col("datasetA.id").alias("idA"),
    col("datasetB.id").alias("idB"),
    col("JaccardDistance")).show()

// Compute the locality sensitive hashes for the input rows, then perform approximate nearest
// neighbor search.
// We could avoid computing hashes by passing in the already-transformed dataset, e.g.
// `model.approxNearestNeighbors(transformedA, key, 2)`
// It may return less than 2 rows when not enough approximate near-neighbor candidates are
// found.
println("Approximately searching dfA for 2 nearest neighbors of the key:")
model.approxNearestNeighbors(dfA, key, 2).show()
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.Arrays;
import java.util.List;

import org.apache.spark.ml.feature.MinHashLSH;
import org.apache.spark.ml.feature.MinHashLSHModel;
import org.apache.spark.ml.linalg.Vector;
import org.apache.spark.ml.linalg.VectorUDT;
import org.apache.spark.ml.linalg.Vectors;
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.RowFactory;
import org.apache.spark.sql.types.DataTypes;
import org.apache.spark.sql.types.Metadata;
import org.apache.spark.sql.types.StructField;
import org.apache.spark.sql.types.StructType;

import static org.apache.spark.sql.functions.col;

List<Row> dataA = Arrays.asList(
  RowFactory.create(0, Vectors.sparse(6, new int[]{0, 1, 2}, new double[]{1.0, 1.0, 1.0})),
  RowFactory.create(1, Vectors.sparse(6, new int[]{2, 3, 4}, new double[]{1.0, 1.0, 1.0})),
  RowFactory.create(2, Vectors.sparse(6, new int[]{0, 2, 4}, new double[]{1.0, 1.0, 1.0}))
);

List<Row> dataB = Arrays.asList(
  RowFactory.create(0, Vectors.sparse(6, new int[]{1, 3, 5}, new double[]{1.0, 1.0, 1.0})),
  RowFactory.create(1, Vectors.sparse(6, new int[]{2, 3, 5}, new double[]{1.0, 1.0, 1.0})),
  RowFactory.create(2, Vectors.sparse(6, new int[]{1, 2, 4}, new double[]{1.0, 1.0, 1.0}))
);

StructType schema = new StructType(new StructField[]{
  new StructField("id", DataTypes.IntegerType, false, Metadata.empty()),
  new StructField("features", new VectorUDT(), false, Metadata.empty())
});
Dataset<Row> dfA = spark.createDataFrame(dataA, schema);
Dataset<Row> dfB = spark.createDataFrame(dataB, schema);

int[] indices = {1, 3};
double[] values = {1.0, 1.0};
Vector key = Vectors.sparse(6, indices, values);

MinHashLSH mh = new MinHashLSH()
  .setNumHashTables(5)
  .setInputCol("features")
  .setOutputCol("hashes");

MinHashLSHModel model = mh.fit(dfA);

// Feature Transformation
System.out.println("The hashed dataset where hashed values are stored in the column 'hashes':");
model.transform(dfA).show();

// Compute the locality sensitive hashes for the input rows, then perform approximate
// similarity join.
// We could avoid computing hashes by passing in the already-transformed dataset, e.g.
// `model.approxSimilarityJoin(transformedA, transformedB, 0.6)`
System.out.println("Approximately joining dfA and dfB on Jaccard distance smaller than 0.6:");
model.approxSimilarityJoin(dfA, dfB, 0.6, "JaccardDistance")
  .select(col("datasetA.id").alias("idA"),
    col("datasetB.id").alias("idB"),
    col("JaccardDistance")).show();

// Compute the locality sensitive hashes for the input rows, then perform approximate nearest
// neighbor search.
// We could avoid computing hashes by passing in the already-transformed dataset, e.g.
// `model.approxNearestNeighbors(transformedA, key, 2)`
// It may return less than 2 rows when not enough approximate near-neighbor candidates are
// found.
System.out.println("Approximately searching dfA for 2 nearest neighbors of the key:");
model.approxNearestNeighbors(dfA, key, 2).show();
```
{% endtab %}

{% tab title="Python" %}
```python
from pyspark.ml.feature import MinHashLSH
from pyspark.ml.linalg import Vectors
from pyspark.sql.functions import col

dataA = [(0, Vectors.sparse(6, [0, 1, 2], [1.0, 1.0, 1.0]),),
         (1, Vectors.sparse(6, [2, 3, 4], [1.0, 1.0, 1.0]),),
         (2, Vectors.sparse(6, [0, 2, 4], [1.0, 1.0, 1.0]),)]
dfA = spark.createDataFrame(dataA, ["id", "features"])

dataB = [(3, Vectors.sparse(6, [1, 3, 5], [1.0, 1.0, 1.0]),),
         (4, Vectors.sparse(6, [2, 3, 5], [1.0, 1.0, 1.0]),),
         (5, Vectors.sparse(6, [1, 2, 4], [1.0, 1.0, 1.0]),)]
dfB = spark.createDataFrame(dataB, ["id", "features"])

key = Vectors.sparse(6, [1, 3], [1.0, 1.0])

mh = MinHashLSH(inputCol="features", outputCol="hashes", numHashTables=5)
model = mh.fit(dfA)

# Feature Transformation
print("The hashed dataset where hashed values are stored in the column 'hashes':")
model.transform(dfA).show()

# Compute the locality sensitive hashes for the input rows, then perform approximate
# similarity join.
# We could avoid computing hashes by passing in the already-transformed dataset, e.g.
# `model.approxSimilarityJoin(transformedA, transformedB, 0.6)`
print("Approximately joining dfA and dfB on distance smaller than 0.6:")
model.approxSimilarityJoin(dfA, dfB, 0.6, distCol="JaccardDistance")\
    .select(col("datasetA.id").alias("idA"),
            col("datasetB.id").alias("idB"),
            col("JaccardDistance")).show()

# Compute the locality sensitive hashes for the input rows, then perform approximate nearest
# neighbor search.
# We could avoid computing hashes by passing in the already-transformed dataset, e.g.
# `model.approxNearestNeighbors(transformedA, key, 2)`
# It may return less than 2 rows when not enough approximate near-neighbor candidates are
# found.
print("Approximately searching dfA for 2 nearest neighbors of the key:")
model.approxNearestNeighbors(dfA, key, 2).show()
```
{% endtab %}
{% endtabs %}







