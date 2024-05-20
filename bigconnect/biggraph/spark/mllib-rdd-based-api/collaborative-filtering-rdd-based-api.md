# Collaborative Filtering - RDD-based API

* Collaborative filtering
  * Explicit vs. implicit feedback
  * Scaling of the regularization parameter
* Examples
* Tutorial

### Collaborative filtering

Collaborative filtering is commonly used for recommender systems. These techniques aim to fill in the missing entries of a user-item association matrix. `spark.mllib` currently supports model-based collaborative filtering, in which users and products are described by a small set of latent factors that can be used to predict missing entries. `spark.mllib` uses the [alternating least squares (ALS)](http://dl.acm.org/citation.cfm?id=1608614) algorithm to learn these latent factors. The implementation in `spark.mllib` has the following parameters:

* _numBlocks_ is the number of blocks used to parallelize computation (set to -1 to auto-configure).
* _rank_ is the number of features to use (also referred to as the number of latent factors).
* _iterations_ is the number of iterations of ALS to run. ALS typically converges to a reasonable solution in 20 iterations or less.
* _lambda_ specifies the regularization parameter in ALS.
* _implicitPrefs_ specifies whether to use the _explicit feedback_ ALS variant or one adapted for _implicit feedback_ data.
* _alpha_ is a parameter applicable to the implicit feedback variant of ALS that governs the _baseline_ confidence in preference observations.

#### Explicit vs. implicit feedback

The standard approach to matrix factorization-based collaborative filtering treats the entries in the user-item matrix as _explicit_ preferences given by the user to the item, for example, users giving ratings to movies.

It is common in many real-world use cases to only have access to _implicit feedback_ (e.g. views, clicks, purchases, likes, shares etc.). The approach used in `spark.mllib` to deal with such data is taken from [Collaborative Filtering for Implicit Feedback Datasets](https://doi.org/10.1109/ICDM.2008.22). Essentially, instead of trying to model the matrix of ratings directly, this approach treats the data as numbers representing the _strength_ in observations of user actions (such as the number of clicks, or the cumulative duration someone spent viewing a movie). Those numbers are then related to the level of confidence in observed user preferences, rather than explicit ratings given to items. The model then tries to find latent factors that can be used to predict the expected preference of a user for an item.

#### Scaling of the regularization parameter

Since v1.1, we scale the regularization parameter `lambda` in solving each least squares problem by the number of ratings the user generated in updating user factors, or the number of ratings the product received in updating product factors. This approach is named “ALS-WR” and discussed in the paper “[Large-Scale Parallel Collaborative Filtering for the Netflix Prize](https://doi.org/10.1007/978-3-540-68880-8\_32)”. It makes `lambda` less dependent on the scale of the dataset, so we can apply the best parameter learned from a sampled subset to the full dataset and expect similar performance.

### Examples

* Python

In the following example we load rating data. Each row consists of a user, a product and a rating. We use the default ALS.train() method which assumes ratings are explicit. We evaluate the recommendation by measuring the Mean Squared Error of rating prediction.

Refer to the `ALS` Python docs for more details on the API.

```
from pyspark.mllib.recommendation import ALS, MatrixFactorizationModel, Rating

# Load and parse the data
data = sc.textFile("data/mllib/als/test.data")
ratings = data.map(lambda l: l.split(','))\
    .map(lambda l: Rating(int(l[0]), int(l[1]), float(l[2])))

# Build the recommendation model using Alternating Least Squares
rank = 10
numIterations = 10
model = ALS.train(ratings, rank, numIterations)

# Evaluate the model on training data
testdata = ratings.map(lambda p: (p[0], p[1]))
predictions = model.predictAll(testdata).map(lambda r: ((r[0], r[1]), r[2]))
ratesAndPreds = ratings.map(lambda r: ((r[0], r[1]), r[2])).join(predictions)
MSE = ratesAndPreds.map(lambda r: (r[1][0] - r[1][1])**2).mean()
print("Mean Squared Error = " + str(MSE))

# Save and load model
model.save(sc, "target/tmp/myCollaborativeFilter")
sameModel = MatrixFactorizationModel.load(sc, "target/tmp/myCollaborativeFilter")
```

Find full example code at "examples/src/main/python/mllib/recommendation\_example.py" in the Spark repo.

If the rating matrix is derived from other source of information (i.e. it is inferred from other signals), you can use the trainImplicit method to get better results.

```python
# Build the recommendation model using Alternating Least Squares based on implicit ratings
model = ALS.trainImplicit(ratings, rank, numIterations, alpha=0.01)
```

In order to run the above application, follow the instructions provided in the [Self-Contained Applications](https://spark.apache.org/docs/latest/quick-start.html#self-contained-applications) section of the Spark Quick Start guide. Be sure to also include _spark-mllib_ to your build file as a dependency.

* Scala

In the following example, we load rating data. Each row consists of a user, a product and a rating. We use the default ALS.train() method which assumes ratings are explicit. We evaluate the recommendation model by measuring the Mean Squared Error of rating prediction.

Refer to the `ALS` Scala docs for more details on the API.

```
import org.apache.spark.mllib.recommendation.ALS
import org.apache.spark.mllib.recommendation.MatrixFactorizationModel
import org.apache.spark.mllib.recommendation.Rating

// Load and parse the data
val data = sc.textFile("data/mllib/als/test.data")
val ratings = data.map(_.split(',') match { case Array(user, item, rate) =>
  Rating(user.toInt, item.toInt, rate.toDouble)
})

// Build the recommendation model using ALS
val rank = 10
val numIterations = 10
val model = ALS.train(ratings, rank, numIterations, 0.01)

// Evaluate the model on rating data
val usersProducts = ratings.map { case Rating(user, product, rate) =>
  (user, product)
}
val predictions =
  model.predict(usersProducts).map { case Rating(user, product, rate) =>
    ((user, product), rate)
  }
val ratesAndPreds = ratings.map { case Rating(user, product, rate) =>
  ((user, product), rate)
}.join(predictions)
val MSE = ratesAndPreds.map { case ((user, product), (r1, r2)) =>
  val err = (r1 - r2)
  err * err
}.mean()
println(s"Mean Squared Error = $MSE")

// Save and load model
model.save(sc, "target/tmp/myCollaborativeFilter")
val sameModel = MatrixFactorizationModel.load(sc, "target/tmp/myCollaborativeFilter")
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/RecommendationExample.scala" in the Spark repo.

* Java

All of MLlib’s methods use Java-friendly types, so you can import and call them there the same way you do in Scala. The only caveat is that the methods take Scala RDD objects, while the Spark Java API uses a separate `JavaRDD` class. You can convert a Java RDD to a Scala one by calling `.rdd()` on your `JavaRDD` object. A self-contained application example that is equivalent to the provided example in Scala is given below:

Refer to the `ALS` Java docs for more details on the API.

```
import scala.Tuple2;

import org.apache.spark.api.java.*;
import org.apache.spark.mllib.recommendation.ALS;
import org.apache.spark.mllib.recommendation.MatrixFactorizationModel;
import org.apache.spark.mllib.recommendation.Rating;
import org.apache.spark.SparkConf;

SparkConf conf = new SparkConf().setAppName("Java Collaborative Filtering Example");
JavaSparkContext jsc = new JavaSparkContext(conf);

// Load and parse the data
String path = "data/mllib/als/test.data";
JavaRDD<String> data = jsc.textFile(path);
JavaRDD<Rating> ratings = data.map(s -> {
  String[] sarray = s.split(",");
  return new Rating(Integer.parseInt(sarray[0]),
    Integer.parseInt(sarray[1]),
    Double.parseDouble(sarray[2]));
});

// Build the recommendation model using ALS
int rank = 10;
int numIterations = 10;
MatrixFactorizationModel model = ALS.train(JavaRDD.toRDD(ratings), rank, numIterations, 0.01);

// Evaluate the model on rating data
JavaRDD<Tuple2<Object, Object>> userProducts =
  ratings.map(r -> new Tuple2<>(r.user(), r.product()));
JavaPairRDD<Tuple2<Integer, Integer>, Double> predictions = JavaPairRDD.fromJavaRDD(
  model.predict(JavaRDD.toRDD(userProducts)).toJavaRDD()
      .map(r -> new Tuple2<>(new Tuple2<>(r.user(), r.product()), r.rating()))
);
JavaRDD<Tuple2<Double, Double>> ratesAndPreds = JavaPairRDD.fromJavaRDD(
    ratings.map(r -> new Tuple2<>(new Tuple2<>(r.user(), r.product()), r.rating())))
  .join(predictions).values();
double MSE = ratesAndPreds.mapToDouble(pair -> {
  double err = pair._1() - pair._2();
  return err * err;
}).mean();
System.out.println("Mean Squared Error = " + MSE);

// Save and load model
model.save(jsc.sc(), "target/tmp/myCollaborativeFilter");
MatrixFactorizationModel sameModel = MatrixFactorizationModel.load(jsc.sc(),
  "target/tmp/myCollaborativeFilter");
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/mllib/JavaRecommendationExample.java" in the Spark repo.

### Tutorial

The training exercises from the Spark Summit 2014 include a hands-on tutorial for personalized movie recommendation with `spark.mllib`.
