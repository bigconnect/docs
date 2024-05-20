# PMML model export - RDD-based API

* spark.mllib supported models
* Examples

### Spark.mllib supported models

`spark.mllib` supports model export to Predictive Model Markup Language ([PMML](http://en.wikipedia.org/wiki/Predictive\_Model\_Markup\_Language)).

The table below outlines the `spark.mllib` models that can be exported to PMML and their equivalent PMML model.

| spark.mllib model              | PMML model                                                                  |
| ------------------------------ | --------------------------------------------------------------------------- |
| KMeansModel                    | ClusteringModel                                                             |
| LinearRegressionModel          | RegressionModel (functionName="regression")                                 |
| RidgeRegressionModel           | RegressionModel (functionName="regression")                                 |
| LassoModel                     | RegressionModel (functionName="regression")                                 |
| SVMModel                       | RegressionModel (functionName="classification" normalizationMethod="none")  |
| Binary LogisticRegressionModel | RegressionModel (functionName="classification" normalizationMethod="logit") |

### Examples

* Scala

To export a supported `model` (see table above) to PMML, simply call `model.toPMML`.

As well as exporting the PMML model to a String (`model.toPMML` as in the example above), you can export the PMML model to other formats.

Refer to the `KMeans` Scala docs and `Vectors` Scala docs for details on the API.

Here a complete example of building a KMeansModel and print it out in PMML format:

```
import org.apache.spark.mllib.clustering.KMeans
import org.apache.spark.mllib.linalg.Vectors

// Load and parse the data
val data = sc.textFile("data/mllib/kmeans_data.txt")
val parsedData = data.map(s => Vectors.dense(s.split(' ').map(_.toDouble))).cache()

// Cluster the data into two classes using KMeans
val numClusters = 2
val numIterations = 20
val clusters = KMeans.train(parsedData, numClusters, numIterations)

// Export to PMML to a String in PMML format
println(s"PMML Model:\n ${clusters.toPMML}")

// Export the model to a local file in PMML format
clusters.toPMML("/tmp/kmeans.xml")

// Export the model to a directory on a distributed file system in PMML format
clusters.toPMML(sc, "/tmp/kmeans")

// Export the model to the OutputStream in PMML format
clusters.toPMML(System.out)
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/mllib/PMMLModelExportExample.scala" in the Spark repo.

For unsupported models, either you will not find a `.toPMML` method or an `IllegalArgumentException` will be thrown.
