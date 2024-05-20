# Clustering - RDD-based API

Clustering is an unsupervised learning problem whereby we aim to group subsets of entities with one another based on some notion of similarity. Clustering is often used for exploratory analysis and/or as a component of a hierarchical supervised learning pipeline (in which distinct classifiers or regression models are trained for each cluster).

The `spark.mllib` package supports the following models:

* K-means
* Gaussian mixture
* Power iteration clustering (PIC)
* Latent Dirichlet allocation (LDA)
* Bisecting k-means
* Streaming k-means

### K-means

[K-means](http://en.wikipedia.org/wiki/K-means\_clustering) is one of the most commonly used clustering algorithms that clusters the data points into a predefined number of clusters. The `spark.mllib` implementation includes a parallelized variant of the [k-means++](http://en.wikipedia.org/wiki/K-means%2B%2B) method called [kmeans||](http://theory.stanford.edu/\~sergei/papers/vldb12-kmpar.pdf). The implementation in `spark.mllib` has the following parameters:

* _k_ is the number of desired clusters. Note that it is possible for fewer than k clusters to be returned, for example, if there are fewer than k distinct points to cluster.
* _maxIterations_ is the maximum number of iterations to run.
* _initializationMode_ specifies either random initialization or initialization via k-means||.
* _runs_ This param has no effect since Spark 2.0.0.
* _initializationSteps_ determines the number of steps in the k-means|| algorithm.
* _epsilon_ determines the distance threshold within which we consider k-means to have converged.
* _initialModel_ is an optional set of cluster centers used for initialization. If this parameter is supplied, only one run is performed.

**Examples**

* Python

The following examples can be tested in the PySpark shell.

In the following example after loading and parsing data, we use the KMeans object to cluster the data into two clusters. The number of desired clusters is passed to the algorithm. We then compute Within Set Sum of Squared Error (WSSSE). You can reduce this error measure by increasing _k_. In fact the optimal _k_ is usually one where there is an “elbow” in the WSSSE graph.

Refer to the `KMeans` Python docs and `KMeansModel` Python docs for more details on the API.

```
from numpy import array
from math import sqrt

from pyspark.mllib.clustering import KMeans, KMeansModel

# Load and parse the data
data = sc.textFile("data/mllib/kmeans_data.txt")
parsedData = data.map(lambda line: array([float(x) for x in line.split(' ')]))

# Build the model (cluster the data)
clusters = KMeans.train(parsedData, 2, maxIterations=10, initializationMode="random")

# Evaluate clustering by computing Within Set Sum of Squared Errors
def error(point):
    center = clusters.centers[clusters.predict(point)]
    return sqrt(sum([x**2 for x in (point - center)]))

WSSSE = parsedData.map(lambda point: error(point)).reduce(lambda x, y: x + y)
print("Within Set Sum of Squared Error = " + str(WSSSE))

# Save and load model
clusters.save(sc, "target/org/apache/spark/PythonKMeansExample/KMeansModel")
sameModel = KMeansModel.load(sc, "target/org/apache/spark/PythonKMeansExample/KMeansModel")
```

Find full example code at "examples/src/main/python/mllib/k\_means\_example.py" in the Spark repo.

* Scala

The following code snippets can be executed in `spark-shell`.

In the following example after loading and parsing data, we use the [`KMeans`](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/mllib/clustering/KMeans.html) object to cluster the data into two clusters. The number of desired clusters is passed to the algorithm. We then compute Within Set Sum of Squared Error (WSSSE). You can reduce this error measure by increasing _k_. In fact, the optimal _k_ is usually one where there is an “elbow” in the WSSSE graph.

Refer to the `KMeans` Scala docs and `KMeansModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.clustering.{KMeans, KMeansModel}
import org.apache.spark.mllib.linalg.Vectors

// Load and parse the data
val data = sc.textFile("data/mllib/kmeans_data.txt")
val parsedData = data.map(s => Vectors.dense(s.split(' ').map(_.toDouble))).cache()

// Cluster the data into two classes using KMeans
val numClusters = 2
val numIterations = 20
val clusters = KMeans.train(parsedData, numClusters, numIterations)

// Evaluate clustering by computing Within Set Sum of Squared Errors
val WSSSE = clusters.computeCost(parsedData)
println(s"Within Set Sum of Squared Errors = $WSSSE")

// Save and load model
clusters.save(sc, "target/org/apache/spark/KMeansExample/KMeansModel")
val sameModel = KMeansModel.load(sc, "target/org/apache/spark/KMeansExample/KMeansModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/KMeansExample.scala" in the Spark repo.

* Java

All of MLlib’s methods use Java-friendly types, so you can import and call them there the same way you do in Scala. The only caveat is that the methods take Scala RDD objects, while the Spark Java API uses a separate `JavaRDD` class. You can convert a Java RDD to a Scala one by calling `.rdd()` on your `JavaRDD` object. A self-contained application example that is equivalent to the provided example in Scala is given below:

Refer to the `KMeans` Java docs and `KMeansModel` Java docs for details on the API.

```
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.clustering.KMeans;
import org.apache.spark.mllib.clustering.KMeansModel;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;

// Load and parse data
String path = "data/mllib/kmeans_data.txt";
JavaRDD<String> data = jsc.textFile(path);
JavaRDD<Vector> parsedData = data.map(s -> {
  String[] sarray = s.split(" ");
  double[] values = new double[sarray.length];
  for (int i = 0; i < sarray.length; i++) {
    values[i] = Double.parseDouble(sarray[i]);
  }
  return Vectors.dense(values);
});
parsedData.cache();

// Cluster the data into two classes using KMeans
int numClusters = 2;
int numIterations = 20;
KMeansModel clusters = KMeans.train(parsedData.rdd(), numClusters, numIterations);

System.out.println("Cluster centers:");
for (Vector center: clusters.clusterCenters()) {
  System.out.println(" " + center);
}
double cost = clusters.computeCost(parsedData.rdd());
System.out.println("Cost: " + cost);

// Evaluate clustering by computing Within Set Sum of Squared Errors
double WSSSE = clusters.computeCost(parsedData.rdd());
System.out.println("Within Set Sum of Squared Errors = " + WSSSE);

// Save and load model
clusters.save(jsc.sc(), "target/org/apache/spark/JavaKMeansExample/KMeansModel");
KMeansModel sameModel = KMeansModel.load(jsc.sc(),
  "target/org/apache/spark/JavaKMeansExample/KMeansModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaKMeansExample.java" in the Spark repo.

### Gaussian mixture

A [Gaussian Mixture Model](http://en.wikipedia.org/wiki/Mixture\_model#Multivariate\_Gaussian\_mixture\_model) represents a composite distribution whereby points are drawn from one of _k_ Gaussian sub-distributions, each with its own probability. The `spark.mllib` implementation uses the [expectation-maximization](http://en.wikipedia.org/wiki/Expectation%E2%80%93maximization\_algorithm) algorithm to induce the maximum-likelihood model given a set of samples. The implementation has the following parameters:

* _k_ is the number of desired clusters.
* _convergenceTol_ is the maximum change in log-likelihood at which we consider convergence achieved.
* _maxIterations_ is the maximum number of iterations to perform without reaching convergence.
* _initialModel_ is an optional starting point from which to start the EM algorithm. If this parameter is omitted, a random starting point will be constructed from the data.

**Examples**

* Python

In the following example after loading and parsing data, we use a [GaussianMixture](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.mllib.clustering.GaussianMixture.html) object to cluster the data into two clusters. The number of desired clusters is passed to the algorithm. We then output the parameters of the mixture model.

Refer to the `GaussianMixture` Python docs and `GaussianMixtureModel` Python docs for more details on the API.

```
from numpy import array

from pyspark.mllib.clustering import GaussianMixture, GaussianMixtureModel

# Load and parse the data
data = sc.textFile("data/mllib/gmm_data.txt")
parsedData = data.map(lambda line: array([float(x) for x in line.strip().split(' ')]))

# Build the model (cluster the data)
gmm = GaussianMixture.train(parsedData, 2)

# Save and load model
gmm.save(sc, "target/org/apache/spark/PythonGaussianMixtureExample/GaussianMixtureModel")
sameModel = GaussianMixtureModel\
    .load(sc, "target/org/apache/spark/PythonGaussianMixtureExample/GaussianMixtureModel")

# output parameters of model
for i in range(2):
    print("weight = ", gmm.weights[i], "mu = ", gmm.gaussians[i].mu,
          "sigma = ", gmm.gaussians[i].sigma.toArray())
```

Find full example code at "examples/src/main/python/mllib/gaussian\_mixture\_example.py" in the Spark repo.

* Scala

In the following example after loading and parsing data, we use a [GaussianMixture](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/mllib/clustering/GaussianMixture.html) object to cluster the data into two clusters. The number of desired clusters is passed to the algorithm. We then output the parameters of the mixture model.

Refer to the `GaussianMixture` Scala docs and `GaussianMixtureModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.clustering.{GaussianMixture, GaussianMixtureModel}
import org.apache.spark.mllib.linalg.Vectors

// Load and parse the data
val data = sc.textFile("data/mllib/gmm_data.txt")
val parsedData = data.map(s => Vectors.dense(s.trim.split(' ').map(_.toDouble))).cache()

// Cluster the data into two classes using GaussianMixture
val gmm = new GaussianMixture().setK(2).run(parsedData)

// Save and load model
gmm.save(sc, "target/org/apache/spark/GaussianMixtureExample/GaussianMixtureModel")
val sameModel = GaussianMixtureModel.load(sc,
  "target/org/apache/spark/GaussianMixtureExample/GaussianMixtureModel")

// output parameters of max-likelihood model
for (i <- 0 until gmm.k) {
  println("weight=%f\nmu=%s\nsigma=\n%s\n" format
    (gmm.weights(i), gmm.gaussians(i).mu, gmm.gaussians(i).sigma))
}
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/GaussianMixtureExample.scala" in the Spark repo.

* Java

All of MLlib’s methods use Java-friendly types, so you can import and call them there the same way you do in Scala. The only caveat is that the methods take Scala RDD objects, while the Spark Java API uses a separate `JavaRDD` class. You can convert a Java RDD to a Scala one by calling `.rdd()` on your `JavaRDD` object. A self-contained application example that is equivalent to the provided example in Scala is given below:

Refer to the `GaussianMixture` Java docs and `GaussianMixtureModel` Java docs for details on the API.

```
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.clustering.GaussianMixture;
import org.apache.spark.mllib.clustering.GaussianMixtureModel;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;

// Load and parse data
String path = "data/mllib/gmm_data.txt";
JavaRDD<String> data = jsc.textFile(path);
JavaRDD<Vector> parsedData = data.map(s -> {
  String[] sarray = s.trim().split(" ");
  double[] values = new double[sarray.length];
  for (int i = 0; i < sarray.length; i++) {
    values[i] = Double.parseDouble(sarray[i]);
  }
  return Vectors.dense(values);
});
parsedData.cache();

// Cluster the data into two classes using GaussianMixture
GaussianMixtureModel gmm = new GaussianMixture().setK(2).run(parsedData.rdd());

// Save and load GaussianMixtureModel
gmm.save(jsc.sc(), "target/org/apache/spark/JavaGaussianMixtureExample/GaussianMixtureModel");
GaussianMixtureModel sameModel = GaussianMixtureModel.load(jsc.sc(),
  "target/org.apache.spark.JavaGaussianMixtureExample/GaussianMixtureModel");

// Output the parameters of the mixture model
for (int j = 0; j < gmm.k(); j++) {
  System.out.printf("weight=%f\nmu=%s\nsigma=\n%s\n",
    gmm.weights()[j], gmm.gaussians()[j].mu(), gmm.gaussians()[j].sigma());
}
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaGaussianMixtureExample.java" in the Spark repo.

### Power iteration clustering (PIC)

Power iteration clustering (PIC) is a scalable and efficient algorithm for clustering vertices of a graph given pairwise similarities as edge properties, described in [Lin and Cohen, Power Iteration Clustering](http://www.cs.cmu.edu/\~frank/papers/icml2010-pic-final.pdf). It computes a pseudo-eigenvector of the normalized affinity matrix of the graph via [power iteration](http://en.wikipedia.org/wiki/Power\_iteration) and uses it to cluster vertices. `spark.mllib` includes an implementation of PIC using GraphX as its backend. It takes an `RDD` of `(srcId, dstId, similarity)` tuples and outputs a model with the clustering assignments. The similarities must be nonnegative. PIC assumes that the similarity measure is symmetric. A pair `(srcId, dstId)` regardless of the ordering should appear at most once in the input data. If a pair is missing from input, their similarity is treated as zero. `spark.mllib`’s PIC implementation takes the following (hyper-)parameters:

* `k`: number of clusters
* `maxIterations`: maximum number of power iterations
* `initializationMode`: initialization model. This can be either “random”, which is the default, to use a random vector as vertex properties, or “degree” to use normalized sum similarities.

**Examples**

In the following, we show code snippets to demonstrate how to use PIC in `spark.mllib`.

* Python

`PowerIterationClustering` implements the PIC algorithm. It takes an `RDD` of `(srcId: Long, dstId: Long, similarity: Double)` tuples representing the affinity matrix. Calling `PowerIterationClustering.run` returns a `PowerIterationClusteringModel`, which contains the computed clustering assignments.

Refer to the `PowerIterationClustering` Python docs and `PowerIterationClusteringModel` Python docs for more details on the API.

```
from pyspark.mllib.clustering import PowerIterationClustering, PowerIterationClusteringModel

# Load and parse the data
data = sc.textFile("data/mllib/pic_data.txt")
similarities = data.map(lambda line: tuple([float(x) for x in line.split(' ')]))

# Cluster the data into two classes using PowerIterationClustering
model = PowerIterationClustering.train(similarities, 2, 10)

model.assignments().foreach(lambda x: print(str(x.id) + " -> " + str(x.cluster)))

# Save and load model
model.save(sc, "target/org/apache/spark/PythonPowerIterationClusteringExample/PICModel")
sameModel = PowerIterationClusteringModel\
    .load(sc, "target/org/apache/spark/PythonPowerIterationClusteringExample/PICModel")
```

Find full example code at "examples/src/main/python/mllib/power\_iteration\_clustering\_example.py" in the Spark repo.

* Scala

`PowerIterationClustering` implements the PIC algorithm. It takes an `RDD` of `(srcId: Long, dstId: Long, similarity: Double)` tuples representing the affinity matrix. Calling `PowerIterationClustering.run` returns a `PowerIterationClusteringModel`, which contains the computed clustering assignments.

Refer to the `PowerIterationClustering` Scala docs and `PowerIterationClusteringModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.clustering.PowerIterationClustering

val circlesRdd = generateCirclesRdd(sc, params.k, params.numPoints)
val model = new PowerIterationClustering()
  .setK(params.k)
  .setMaxIterations(params.maxIterations)
  .setInitializationMode("degree")
  .run(circlesRdd)

val clusters = model.assignments.collect().groupBy(_.cluster).mapValues(_.map(_.id))
val assignments = clusters.toList.sortBy { case (k, v) => v.length }
val assignmentsStr = assignments
  .map { case (k, v) =>
    s"$k -> ${v.sorted.mkString("[", ",", "]")}"
  }.mkString(", ")
val sizesStr = assignments.map {
  _._2.length
}.sorted.mkString("(", ",", ")")
println(s"Cluster assignments: $assignmentsStr\ncluster sizes: $sizesStr")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/PowerIterationClusteringExample.scala" in the Spark repo.

* Java

`PowerIterationClustering` implements the PIC algorithm. It takes a `JavaRDD` of `(srcId: Long, dstId: Long, similarity: Double)` tuples representing the affinity matrix. Calling `PowerIterationClustering.run` returns a `PowerIterationClusteringModel` which contains the computed clustering assignments.

Refer to the `PowerIterationClustering` Java docs and `PowerIterationClusteringModel` Java docs for details on the API.

```
import org.apache.spark.mllib.clustering.PowerIterationClustering;
import org.apache.spark.mllib.clustering.PowerIterationClusteringModel;

JavaRDD<Tuple3<Long, Long, Double>> similarities = sc.parallelize(Arrays.asList(
  new Tuple3<>(0L, 1L, 0.9),
  new Tuple3<>(1L, 2L, 0.9),
  new Tuple3<>(2L, 3L, 0.9),
  new Tuple3<>(3L, 4L, 0.1),
  new Tuple3<>(4L, 5L, 0.9)));

PowerIterationClustering pic = new PowerIterationClustering()
  .setK(2)
  .setMaxIterations(10);
PowerIterationClusteringModel model = pic.run(similarities);

for (PowerIterationClustering.Assignment a: model.assignments().toJavaRDD().collect()) {
  System.out.println(a.id() + " -> " + a.cluster());
}
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaPowerIterationClusteringExample.java" in the Spark repo.

### Latent Dirichlet allocation (LDA)

[Latent Dirichlet allocation (LDA)](http://en.wikipedia.org/wiki/Latent\_Dirichlet\_allocation) is a topic model which infers topics from a collection of text documents. LDA can be thought of as a clustering algorithm as follows:

* Topics correspond to cluster centers, and documents correspond to examples (rows) in a dataset.
* Topics and documents both exist in a feature space, where feature vectors are vectors of word counts (bag of words).
* Rather than estimating a clustering using a traditional distance, LDA uses a function based on a statistical model of how text documents are generated.

LDA supports different inference algorithms via `setOptimizer` function. `EMLDAOptimizer` learns clustering using [expectation-maximization](http://en.wikipedia.org/wiki/Expectation%E2%80%93maximization\_algorithm) on the likelihood function and yields comprehensive results, while `OnlineLDAOptimizer` uses iterative mini-batch sampling for [online variational inference](https://mimno.infosci.cornell.edu/info6150/readings/HoffmanBleiBach2010b.pdf) and is generally memory friendly.

LDA takes in a collection of documents as vectors of word counts and the following parameters (set using the builder pattern):

* `k`: Number of topics (i.e., cluster centers)
* `optimizer`: Optimizer to use for learning the LDA model, either `EMLDAOptimizer` or `OnlineLDAOptimizer`
* `docConcentration`: Dirichlet parameter for prior over documents’ distributions over topics. Larger values encourage smoother inferred distributions.
* `topicConcentration`: Dirichlet parameter for prior over topics’ distributions over terms (words). Larger values encourage smoother inferred distributions.
* `maxIterations`: Limit on the number of iterations.
* `checkpointInterval`: If using checkpointing (set in the Spark configuration), this parameter specifies the frequency with which checkpoints will be created. If `maxIterations` is large, using checkpointing can help reduce shuffle file sizes on disk and help with failure recovery.

All of `spark.mllib`’s LDA models support:

* `describeTopics`: Returns topics as arrays of most important terms and term weights
* `topicsMatrix`: Returns a `vocabSize` by `k` matrix where each column is a topic

_Note_: LDA is still an experimental feature under active development. As a result, certain features are only available in one of the two optimizers / models generated by the optimizer. Currently, a distributed model can be converted into a local model, but not vice-versa.

The following discussion will describe each optimizer/model pair separately.

**Expectation Maximization**

Implemented in `EMLDAOptimizer` and `DistributedLDAModel`.

For the parameters provided to `LDA`:

* `docConcentration`: Only symmetric priors are supported, so all values in the provided `k`-dimensional vector must be identical. All values must also be $$>1.0$$>1.0. Providing `Vector(-1)` results in default behavior (uniform `k` dimensional vector with value $$(50/k)+1$$
* `topicConcentration`: Only symmetric priors supported. Values must be $$>1.0$$>1.0. Providing `-1` results in defaulting to a value of $$0.1+1$$0.1+1.
* `maxIterations`: The maximum number of EM iterations.

_Note_: It is important to do enough iterations. In early iterations, EM often has useless topics, but those topics improve dramatically after more iterations. Using at least 20 and possibly 50-100 iterations is often reasonable, depending on your dataset.

`EMLDAOptimizer` produces a `DistributedLDAModel`, which stores not only the inferred topics but also the full training corpus and topic distributions for each document in the training corpus. A `DistributedLDAModel` supports:

* `topTopicsPerDocument`: The top topics and their weights for each document in the training corpus
* `topDocumentsPerTopic`: The top documents for each topic and the corresponding weight of the topic in the documents.
* `logPrior`: log probability of the estimated topics and document-topic distributions given the hyperparameters `docConcentration` and `topicConcentration`
* `logLikelihood`: log likelihood of the training corpus, given the inferred topics and document-topic distributions

**Online Variational Bayes**

Implemented in `OnlineLDAOptimizer` and `LocalLDAModel`.

For the parameters provided to `LDA`:

* `docConcentration`: Asymmetric priors can be used by passing in a vector with values equal to the Dirichlet parameter in each of the `k` dimensions. Values should be $$>=0$$>=0. Providing `Vector(-1)` results in default behavior (uniform `k` dimensional vector with value $$(1.0/k)$$
* `topicConcentration`: Only symmetric priors supported. Values must be $$>=0$$>=0. Providing `-1` results in defaulting to a value of $$(1.0/k)$$.
* `maxIterations`: Maximum number of minibatches to submit.

In addition, `OnlineLDAOptimizer` accepts the following parameters:

* `miniBatchFraction`: Fraction of corpus sampled and used at each iteration
* `optimizeDocConcentration`: If set to true, performs maximum-likelihood estimation of the hyperparameter `docConcentration` (aka `alpha`) after each minibatch and sets the optimized `docConcentration` in the returned `LocalLDAModel`
* `tau0` and `kappa`: Used for learning-rate decay, which is computed by $$(τ0+iter)−κ$$ where $$iter$$ is the current number of iterations.

`OnlineLDAOptimizer` produces a `LocalLDAModel`, which only stores the inferred topics. A `LocalLDAModel` supports:

* `logLikelihood(documents)`: Calculates a lower bound on the provided `documents` given the inferred topics.
* `logPerplexity(documents)`: Calculates an upper bound on the perplexity of the provided `documents` given the inferred topics.

**Examples**

In the following example, we load word count vectors representing a corpus of documents. We then use [LDA](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/mllib/clustering/LDA.html) to infer three topics from the documents. The number of desired clusters is passed to the algorithm. We then output the topics, represented as probability distributions over words.

* Python

Refer to the `LDA` Python docs and `LDAModel` Python docs for more details on the API.

```
from pyspark.mllib.clustering import LDA, LDAModel
from pyspark.mllib.linalg import Vectors

# Load and parse the data
data = sc.textFile("data/mllib/sample_lda_data.txt")
parsedData = data.map(lambda line: Vectors.dense([float(x) for x in line.strip().split(' ')]))
# Index documents with unique IDs
corpus = parsedData.zipWithIndex().map(lambda x: [x[1], x[0]]).cache()

# Cluster the documents into three topics using LDA
ldaModel = LDA.train(corpus, k=3)

# Output topics. Each is a distribution over words (matching word count vectors)
print("Learned topics (as distributions over vocab of " + str(ldaModel.vocabSize())
      + " words):")
topics = ldaModel.topicsMatrix()
for topic in range(3):
    print("Topic " + str(topic) + ":")
    for word in range(0, ldaModel.vocabSize()):
        print(" " + str(topics[word][topic]))

# Save and load model
ldaModel.save(sc, "target/org/apache/spark/PythonLatentDirichletAllocationExample/LDAModel")
sameModel = LDAModel\
    .load(sc, "target/org/apache/spark/PythonLatentDirichletAllocationExample/LDAModel")
```

Find full example code at "examples/src/main/python/mllib/latent\_dirichlet\_allocation\_example.py" in the Spark repo.

* Scala

Refer to the `LDA` Scala docs and `DistributedLDAModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.clustering.{DistributedLDAModel, LDA}
import org.apache.spark.mllib.linalg.Vectors

// Load and parse the data
val data = sc.textFile("data/mllib/sample_lda_data.txt")
val parsedData = data.map(s => Vectors.dense(s.trim.split(' ').map(_.toDouble)))
// Index documents with unique IDs
val corpus = parsedData.zipWithIndex.map(_.swap).cache()

// Cluster the documents into three topics using LDA
val ldaModel = new LDA().setK(3).run(corpus)

// Output topics. Each is a distribution over words (matching word count vectors)
println(s"Learned topics (as distributions over vocab of ${ldaModel.vocabSize} words):")
val topics = ldaModel.topicsMatrix
for (topic <- Range(0, 3)) {
  print(s"Topic $topic :")
  for (word <- Range(0, ldaModel.vocabSize)) {
    print(s"${topics(word, topic)}")
  }
  println()
}

// Save and load model.
ldaModel.save(sc, "target/org/apache/spark/LatentDirichletAllocationExample/LDAModel")
val sameModel = DistributedLDAModel.load(sc,
  "target/org/apache/spark/LatentDirichletAllocationExample/LDAModel")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/LatentDirichletAllocationExample.scala" in the Spark repo.

* Java

Refer to the `LDA` Java docs and `DistributedLDAModel` Java docs for details on the API.

```
import scala.Tuple2;

import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.clustering.DistributedLDAModel;
import org.apache.spark.mllib.clustering.LDA;
import org.apache.spark.mllib.clustering.LDAModel;
import org.apache.spark.mllib.linalg.Matrix;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;

// Load and parse the data
String path = "data/mllib/sample_lda_data.txt";
JavaRDD<String> data = jsc.textFile(path);
JavaRDD<Vector> parsedData = data.map(s -> {
  String[] sarray = s.trim().split(" ");
  double[] values = new double[sarray.length];
  for (int i = 0; i < sarray.length; i++) {
    values[i] = Double.parseDouble(sarray[i]);
  }
  return Vectors.dense(values);
});
// Index documents with unique IDs
JavaPairRDD<Long, Vector> corpus =
  JavaPairRDD.fromJavaRDD(parsedData.zipWithIndex().map(Tuple2::swap));
corpus.cache();

// Cluster the documents into three topics using LDA
LDAModel ldaModel = new LDA().setK(3).run(corpus);

// Output topics. Each is a distribution over words (matching word count vectors)
System.out.println("Learned topics (as distributions over vocab of " + ldaModel.vocabSize()
  + " words):");
Matrix topics = ldaModel.topicsMatrix();
for (int topic = 0; topic < 3; topic++) {
  System.out.print("Topic " + topic + ":");
  for (int word = 0; word < ldaModel.vocabSize(); word++) {
    System.out.print(" " + topics.apply(word, topic));
  }
  System.out.println();
}

ldaModel.save(jsc.sc(),
  "target/org/apache/spark/JavaLatentDirichletAllocationExample/LDAModel");
DistributedLDAModel sameModel = DistributedLDAModel.load(jsc.sc(),
  "target/org/apache/spark/JavaLatentDirichletAllocationExample/LDAModel");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaLatentDirichletAllocationExample.java" in the Spark repo.

### Bisecting k-means

Bisecting K-means can often be much faster than regular K-means, but it will generally produce a different clustering.

Bisecting k-means is a kind of [hierarchical clustering](https://en.wikipedia.org/wiki/Hierarchical\_clustering). Hierarchical clustering is one of the most commonly used method of cluster analysis which seeks to build a hierarchy of clusters. Strategies for hierarchical clustering generally fall into two types:

* Agglomerative: This is a “bottom up” approach: each observation starts in its own cluster, and pairs of clusters are merged as one moves up the hierarchy.
* Divisive: This is a “top down” approach: all observations start in one cluster, and splits are performed recursively as one moves down the hierarchy.

Bisecting k-means algorithm is a kind of divisive algorithms. The implementation in MLlib has the following parameters:

* _k_: the desired number of leaf clusters (default: 4). The actual number could be smaller if there are no divisible leaf clusters.
* _maxIterations_: the max number of k-means iterations to split clusters (default: 20)
* _minDivisibleClusterSize_: the minimum number of points (if >= 1.0) or the minimum proportion of points (if < 1.0) of a divisible cluster (default: 1)
* _seed_: a random seed (default: hash value of the class name)

**Examples**

* Python

Refer to the `BisectingKMeans` Python docs and `BisectingKMeansModel` Python docs for more details on the API.

```
from numpy import array

from pyspark.mllib.clustering import BisectingKMeans

# Load and parse the data
data = sc.textFile("data/mllib/kmeans_data.txt")
parsedData = data.map(lambda line: array([float(x) for x in line.split(' ')]))

# Build the model (cluster the data)
model = BisectingKMeans.train(parsedData, 2, maxIterations=5)

# Evaluate clustering
cost = model.computeCost(parsedData)
print("Bisecting K-means Cost = " + str(cost))
```

Find full example code at "examples/src/main/python/mllib/bisecting\_k\_means\_example.py" in the Spark repo.

* Scala

Refer to the `BisectingKMeans` Scala docs and `BisectingKMeansModel` Scala docs for details on the API.

```
import org.apache.spark.mllib.clustering.BisectingKMeans
import org.apache.spark.mllib.linalg.{Vector, Vectors}

// Loads and parses data
def parse(line: String): Vector = Vectors.dense(line.split(" ").map(_.toDouble))
val data = sc.textFile("data/mllib/kmeans_data.txt").map(parse).cache()

// Clustering the data into 6 clusters by BisectingKMeans.
val bkm = new BisectingKMeans().setK(6)
val model = bkm.run(data)

// Show the compute cost and the cluster centers
println(s"Compute Cost: ${model.computeCost(data)}")
model.clusterCenters.zipWithIndex.foreach { case (center, idx) =>
  println(s"Cluster Center ${idx}: ${center}")
}
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/BisectingKMeansExample.scala" in the Spark repo.

* Java

Refer to the `BisectingKMeans` Java docs and `BisectingKMeansModel` Java docs for details on the API.

```
import java.util.Arrays;
import java.util.List;

import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.mllib.clustering.BisectingKMeans;
import org.apache.spark.mllib.clustering.BisectingKMeansModel;
import org.apache.spark.mllib.linalg.Vector;
import org.apache.spark.mllib.linalg.Vectors;

List<Vector> localData = Arrays.asList(
  Vectors.dense(0.1, 0.1),   Vectors.dense(0.3, 0.3),
  Vectors.dense(10.1, 10.1), Vectors.dense(10.3, 10.3),
  Vectors.dense(20.1, 20.1), Vectors.dense(20.3, 20.3),
  Vectors.dense(30.1, 30.1), Vectors.dense(30.3, 30.3)
);
JavaRDD<Vector> data = sc.parallelize(localData, 2);

BisectingKMeans bkm = new BisectingKMeans()
  .setK(4);
BisectingKMeansModel model = bkm.run(data);

System.out.println("Compute Cost: " + model.computeCost(data));

Vector[] clusterCenters = model.clusterCenters();
for (int i = 0; i < clusterCenters.length; i++) {
  Vector clusterCenter = clusterCenters[i];
  System.out.println("Cluster Center " + i + ": " + clusterCenter);
}
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaBisectingKMeansExample.java" in the Spark repo.

### Streaming k-means

When data arrive in a stream, we may want to estimate clusters dynamically, updating them as new data arrive. `spark.mllib` provides support for streaming k-means clustering, with parameters to control the decay (or “forgetfulness”) of the estimates. The algorithm uses a generalization of the mini-batch k-means update rule. For each batch of data, we assign all points to their nearest cluster, compute new cluster centers, then update each cluster using:

$$ct+1=ctntα+xtmtntα+mt(1)$$`(2)`

Where $$ct$$ is the previous center for the cluster, $$nt$$ is the number of points assigned to the cluster thus far, $$xt$$ is the new cluster center from the current batch, and $$mt$$ is the number of points added to the cluster in the current batch. The decay factor $$α$$ can be used to ignore the past: with $$α$$`=1` all data will be used from the beginning; with $$α$$`=0` only the most recent data will be used. This is analogous to an exponentially-weighted moving average.

The decay can be specified using a `halfLife` parameter, which determines the correct decay factor `a` such that, for data acquired at time `t`, its contribution by time `t + halfLife` will have dropped to 0.5. The unit of time can be specified either as `batches` or `points` and the update rule will be adjusted accordingly.

**Examples**

This example shows how to estimate clusters on streaming data.

* Python

Refer to the `StreamingKMeans` Python docs for more details on the API. And Refer to Spark Streaming Programming Guide for details on StreamingContext.

```
from pyspark.mllib.linalg import Vectors
from pyspark.mllib.regression import LabeledPoint
from pyspark.mllib.clustering import StreamingKMeans

# we make an input stream of vectors for training,
# as well as a stream of vectors for testing
def parse(lp):
    label = float(lp[lp.find('(') + 1: lp.find(')')])
    vec = Vectors.dense(lp[lp.find('[') + 1: lp.find(']')].split(','))

    return LabeledPoint(label, vec)

trainingData = sc.textFile("data/mllib/kmeans_data.txt")\
    .map(lambda line: Vectors.dense([float(x) for x in line.strip().split(' ')]))

testingData = sc.textFile("data/mllib/streaming_kmeans_data_test.txt").map(parse)

trainingQueue = [trainingData]
testingQueue = [testingData]

trainingStream = ssc.queueStream(trainingQueue)
testingStream = ssc.queueStream(testingQueue)

# We create a model with random clusters and specify the number of clusters to find
model = StreamingKMeans(k=2, decayFactor=1.0).setRandomCenters(3, 1.0, 0)

# Now register the streams for training and testing and start the job,
# printing the predicted cluster assignments on new data points as they arrive.
model.trainOn(trainingStream)

result = model.predictOnValues(testingStream.map(lambda lp: (lp.label, lp.features)))
result.pprint()

ssc.start()
ssc.stop(stopSparkContext=True, stopGraceFully=True)
```

Find full example code at "examples/src/main/python/mllib/streaming\_k\_means\_example.py" in the Spark repo.

* Scala

Refer to the `StreamingKMeans` Scala docs for details on the API. And Refer to Spark Streaming Programming Guide for details on StreamingContext.

```
import org.apache.spark.mllib.clustering.StreamingKMeans
import org.apache.spark.mllib.linalg.Vectors
import org.apache.spark.mllib.regression.LabeledPoint
import org.apache.spark.streaming.{Seconds, StreamingContext}

val conf = new SparkConf().setAppName("StreamingKMeansExample")
val ssc = new StreamingContext(conf, Seconds(args(2).toLong))

val trainingData = ssc.textFileStream(args(0)).map(Vectors.parse)
val testData = ssc.textFileStream(args(1)).map(LabeledPoint.parse)

val model = new StreamingKMeans()
  .setK(args(3).toInt)
  .setDecayFactor(1.0)
  .setRandomCenters(args(4).toInt, 0.0)

model.trainOn(trainingData)
model.predictOnValues(testData.map(lp => (lp.label, lp.features))).print()

ssc.start()
ssc.awaitTermination()
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/StreamingKMeansExample.scala" in the Spark repo.



As you add new text files with data the cluster centers will update. Each training point should be formatted as `[x1, x2, x3]`, and each test data point should be formatted as `(y, [x1, x2, x3])`, where `y` is some useful label or identifier (e.g. a true category assignment). Anytime a text file is placed in `/training/data/dir` the model will update. Anytime a text file is placed in `/testing/data/dir` you will see predictions. With new data, the cluster centers will change!
