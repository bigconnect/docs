# SparkR (R on Spark)

* Overview
* SparkDataFrame
  * Starting Up: SparkSession
  * Starting Up from RStudio
  * Creating SparkDataFrames
    * From local data frames
    * From Data Sources
    * From Hive tables
  * SparkDataFrame Operations
    * Selecting rows, columns
    * Grouping, Aggregation
    * Operating on Columns
    * Applying User-Defined Function
      * Run a given function on a large dataset using `dapply` or `dapplyCollect`
        * dapply
        * dapplyCollect
      * Run a given function on a large dataset grouping by input column(s) and using `gapply` or `gapplyCollect`
        * gapply
        * gapplyCollect
      * Run local R functions distributed using `spark.lapply`
        * spark.lapply
    * Eager execution
  * Running SQL Queries from SparkR
* Machine Learning
  * Algorithms
    * Classification
    * Regression
    * Tree
    * Clustering
    * Collaborative Filtering
    * Frequent Pattern Mining
    * Statistics
  * Model persistence
* Data type mapping between R and Spark
* Structured Streaming
* Apache Arrow in SparkR
  * Ensure Arrow Installed
  * Enabling for Conversion to/from R DataFrame, `dapply` and `gapply`
  * Supported SQL Types
* R Function Name Conflicts
* Migration Guide

## Overview

SparkR is an R package that provides a light-weight frontend to use Apache Spark from R. In Spark 3.5.1, SparkR provides a distributed data frame implementation that supports operations like selection, filtering, aggregation etc. (similar to R data frames, [dplyr](https://github.com/hadley/dplyr)) but on large datasets. SparkR also supports distributed machine learning using MLlib.

## SparkDataFrame

A SparkDataFrame is a distributed collection of data organized into named columns. It is conceptually equivalent to a table in a relational database or a data frame in R, but with richer optimizations under the hood. SparkDataFrames can be constructed from a wide array of sources such as: structured data files, tables in Hive, external databases, or existing local R data frames.

All of the examples on this page use sample data included in R or the Spark distribution and can be run using the `./bin/sparkR` shell.

### Starting Up: SparkSession

The entry point into SparkR is the `SparkSession` which connects your R program to a Spark cluster. You can create a `SparkSession` using `sparkR.session` and pass in options such as the application name, any spark packages depended on, etc. Further, you can also work with SparkDataFrames via `SparkSession`. If you are working from the `sparkR` shell, the `SparkSession` should already be created for you, and you would not need to call `sparkR.session`.

### Starting Up from RStudio

You can also start SparkR from RStudio. You can connect your R program to a Spark cluster from RStudio, R shell, Rscript or other R IDEs. To start, make sure SPARK\_HOME is set in environment (you can check Sys.getenv), load the SparkR package, and call `sparkR.session` as below. It will check for the Spark installation, and, if not found, it will be downloaded and cached automatically. Alternatively, you can also run `install.spark` manually.

In addition to calling `sparkR.session`, you could also specify certain Spark driver properties. Normally these Application properties and Runtime Environment cannot be set programmatically, as the driver JVM process would have been started, in this case SparkR takes care of this for you. To set them, pass them as you would other configuration properties in the `sparkConfig` argument to `sparkR.session()`.

The following Spark driver properties can be set in `sparkConfig` with `sparkR.session` from RStudio:

| Property Name                   | Property group         | `spark-submit` equivalent |
| ------------------------------- | ---------------------- | ------------------------- |
| `spark.master`                  | Application Properties | `--master`                |
| `spark.kerberos.keytab`         | Application Properties | `--keytab`                |
| `spark.kerberos.principal`      | Application Properties | `--principal`             |
| `spark.driver.memory`           | Application Properties | `--driver-memory`         |
| `spark.driver.extraClassPath`   | Runtime Environment    | `--driver-class-path`     |
| `spark.driver.extraJavaOptions` | Runtime Environment    | `--driver-java-options`   |
| `spark.driver.extraLibraryPath` | Runtime Environment    | `--driver-library-path`   |

### Creating SparkDataFrames

With a `SparkSession`, applications can create `SparkDataFrame`s from a local R data frame, from a Hive table, or from other data sources.

#### From local data frames

The simplest way to create a data frame is to convert a local R data frame into a SparkDataFrame. Specifically, we can use `as.DataFrame` or `createDataFrame` and pass in the local R data frame to create a SparkDataFrame. As an example, the following creates a `SparkDataFrame` based using the `faithful` dataset from R.

#### From Data Sources

SparkR supports operating on a variety of data sources through the `SparkDataFrame` interface. This section describes the general methods for loading and saving data using Data Sources. You can check the Spark SQL programming guide for more specific options that are available for the built-in data sources.

The general method for creating SparkDataFrames from data sources is `read.df`. This method takes in the path for the file to load and the type of data source, and the currently active SparkSession will be used automatically. SparkR supports reading JSON, CSV and Parquet files natively, and through packages available from sources like Third Party Projects, you can find data source connectors for popular file formats like Avro. These packages can either be added by specifying `--packages` with `spark-submit` or `sparkR` commands, or if initializing SparkSession with `sparkPackages` parameter when in an interactive R shell or from RStudio.

We can see how to use data sources using an example JSON input file. Note that the file that is used here is _not_ a typical JSON file. Each line in the file must contain a separate, self-contained valid JSON object. For more information, please see JSON Lines text format, also called newline-delimited JSON. As a consequence, a regular multi-line JSON file will most often fail.

The data sources API natively supports CSV formatted input files. For more information please refer to SparkR read.df API documentation.

The data sources API can also be used to save out SparkDataFrames into multiple file formats. For example, we can save the SparkDataFrame from the previous example to a Parquet file using `write.df`.

#### From Hive tables

You can also create SparkDataFrames from Hive tables. To do this we will need to create a SparkSession with Hive support which can access tables in the Hive MetaStore. Note that Spark should have been built with Hive support and more details can be found in the SQL programming guide. In SparkR, by default it will attempt to create a SparkSession with Hive support enabled (`enableHiveSupport = TRUE`).

### SparkDataFrame Operations

SparkDataFrames support a number of functions to do structured data processing. Here we include some basic examples and a complete list can be found in the [API](https://spark.apache.org/docs/latest/api/R/index.html) docs:

#### Selecting rows, columns

#### Grouping, Aggregation

SparkR data frames support a number of commonly used functions to aggregate data after grouping. For example, we can compute a histogram of the `waiting` time in the `faithful` dataset as shown below

In addition to standard aggregations, SparkR supports OLAP cube operators `cube`:

and `rollup`:

#### Operating on Columns

SparkR also provides a number of functions that can be directly applied to columns for data processing and during aggregation. The example below shows the use of basic arithmetic functions.

#### Applying User-Defined Function

In SparkR, we support several kinds of User-Defined Functions:

**Run a given function on a large dataset using `dapply` or `dapplyCollect`**

**Dapply**

Apply a function to each partition of a `SparkDataFrame`. The function to be applied to each partition of the `SparkDataFrame` and should have only one parameter, to which a `data.frame` corresponds to each partition will be passed. The output of function should be a `data.frame`. Schema specifies the row format of the resulting a `SparkDataFrame`. It must match to [data types](https://spark.apache.org/docs/latest/sparkr.html#data-type-mapping-between-r-and-spark) of returned value.

**DapplyCollect**

Like `dapply`, apply a function to each partition of a `SparkDataFrame` and collect the result back. The output of function should be a `data.frame`. But, Schema is not required to be passed. Note that `dapplyCollect` can fail if the output of UDF run on all the partition cannot be pulled to the driver and fit in driver memory.

**Run a given function on a large dataset grouping by input column(s) and using `gapply` or `gapplyCollect`**

**Gapply**

Apply a function to each group of a `SparkDataFrame`. The function is to be applied to each group of the `SparkDataFrame` and should have only two parameters: grouping key and R `data.frame` corresponding to that key. The groups are chosen from `SparkDataFrame`s column(s). The output of function should be a `data.frame`. Schema specifies the row format of the resulting `SparkDataFrame`. It must represent R function’s output schema on the basis of Spark data types. The column names of the returned `data.frame` are set by user.

**GapplyCollect**

Like `gapply`, applies a function to each partition of a `SparkDataFrame` and collect the result back to R data.frame. The output of the function should be a `data.frame`. But, the schema is not required to be passed. Note that `gapplyCollect` can fail if the output of UDF run on all the partition cannot be pulled to the driver and fit in driver memory.

**Run local R functions distributed using `spark.lapply`**

**Spark.lapply**

Similar to `lapply` in native R, `spark.lapply` runs a function over a list of elements and distributes the computations with Spark. Applies a function in a manner that is similar to `doParallel` or `lapply` to elements of a list. The results of all the computations should fit in a single machine. If that is not the case they can do something like `df <- createDataFrame(list)` and then use `dapply`

#### Eager execution

If eager execution is enabled, the data will be returned to R client immediately when the `SparkDataFrame` is created. By default, eager execution is not enabled and can be enabled by setting the configuration property `spark.sql.repl.eagerEval.enabled` to `true` when the `SparkSession` is started up.

Maximum number of rows and maximum number of characters per column of data to display can be controlled by `spark.sql.repl.eagerEval.maxNumRows` and `spark.sql.repl.eagerEval.truncate` configuration properties, respectively. These properties are only effective when eager execution is enabled. If these properties are not set explicitly, by default, data up to 20 rows and up to 20 characters per column will be showed.

Note that to enable eager execution in `sparkR` shell, add `spark.sql.repl.eagerEval.enabled=true` configuration property to the `--conf` option.

### Running SQL Queries from SparkR

A SparkDataFrame can also be registered as a temporary view in Spark SQL and that allows you to run SQL queries over its data. The `sql` function enables applications to run SQL queries programmatically and returns the result as a `SparkDataFrame`.

## Machine Learning

### Algorithms

SparkR supports the following machine learning algorithms currently:

**Classification**

* `spark.logit`: `Logistic Regression`
* `spark.mlp`: `Multilayer Perceptron (MLP)`
* `spark.naiveBayes`: `Naive Bayes`
* `spark.svmLinear`: `Linear Support Vector Machine`
* `spark.fmClassifier`: `Factorization Machines classifier`

**Regression**

* `spark.survreg`: `Accelerated Failure Time (AFT) Survival Model`
* `spark.glm` or `glm`: `Generalized Linear Model (GLM)`
* `spark.isoreg`: `Isotonic Regression`
* `spark.lm`: `Linear Regression`
* `spark.fmRegressor`: `Factorization Machines regressor`

**Tree**

* `spark.decisionTree`: `Decision Tree for` `Regression` `and` `Classification`
* `spark.gbt`: `Gradient Boosted Trees for` `Regression` `and` `Classification`
* `spark.randomForest`: `Random Forest for` `Regression` `and` `Classification`

**Clustering**

* `spark.bisectingKmeans`: `Bisecting k-means`
* `spark.gaussianMixture`: `Gaussian Mixture Model (GMM)`
* `spark.kmeans`: `K-Means`
* `spark.lda`: `Latent Dirichlet Allocation (LDA)`
* `spark.powerIterationClustering (PIC)`: `Power Iteration Clustering (PIC)`

**Collaborative Filtering**

* `spark.als`: `Alternating Least Squares (ALS)`

**Frequent Pattern Mining**

* `spark.fpGrowth` : `FP-growth`
* `spark.prefixSpan` : `PrefixSpan`

**Statistics**

* `spark.kstest`: `Kolmogorov-Smirnov Test`

Under the hood, SparkR uses MLlib to train the model. Please refer to the corresponding section of MLlib user guide for example code. Users can call `summary` to print a summary of the fitted model, predict to make predictions on new data, and write.ml/read.ml to save/load fitted models. SparkR supports a subset of the available R formula operators for model fitting, including ‘\~’, ‘.’, ‘:’, ‘+’, and ‘-‘.

### Model persistence

The following example shows how to save/load a MLlib model by SparkR.

```
training <- read.df("data/mllib/sample_multiclass_classification_data.txt", source = "libsvm")
# Fit a generalized linear model of family "gaussian" with spark.glm
df_list <- randomSplit(training, c(7,3), 2)
gaussianDF <- df_list[[1]]
gaussianTestDF <- df_list[[2]]
gaussianGLM <- spark.glm(gaussianDF, label ~ features, family = "gaussian")

# Save and then load a fitted MLlib model
modelPath <- tempfile(pattern = "ml", fileext = ".tmp")
write.ml(gaussianGLM, modelPath)
gaussianGLM2 <- read.ml(modelPath)

# Check model summary
summary(gaussianGLM2)

# Check model prediction
gaussianPredictions <- predict(gaussianGLM2, gaussianTestDF)
head(gaussianPredictions)

unlink(modelPath)
```

Find full example code at "examples/src/main/r/ml/ml.R" in the Spark repo.

## Data type mapping between R and Spark

| R                                                                                       | Spark     |
| --------------------------------------------------------------------------------------- | --------- |
| byte                                                                                    | byte      |
| integer                                                                                 | integer   |
| float                                                                                   | float     |
| double                                                                                  | double    |
| numeric                                                                                 | double    |
| character                                                                               | string    |
| string                                                                                  | string    |
| binary                                                                                  | binary    |
| raw                                                                                     | binary    |
| logical                                                                                 | boolean   |
| [POSIXct](https://stat.ethz.ch/R-manual/R-devel/library/base/html/DateTimeClasses.html) | timestamp |
| [POSIXlt](https://stat.ethz.ch/R-manual/R-devel/library/base/html/DateTimeClasses.html) | timestamp |
| [Date](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Dates.html)              | date      |
| array                                                                                   | array     |
| list                                                                                    | array     |
| env                                                                                     | map       |

## Structured Streaming

SparkR supports the Structured Streaming API. Structured Streaming is a scalable and fault-tolerant stream processing engine built on the Spark SQL engine. For more information see the R API on the Structured Streaming Programming Guide

## Apache Arrow in SparkR

Apache Arrow is an in-memory columnar data format that is used in Spark to efficiently transfer data between JVM and R processes. See also PySpark optimization done, PySpark Usage Guide for Pandas with Apache Arrow. This guide targets to explain how to use Arrow optimization in SparkR with some key points.

### Ensure Arrow Installed

Arrow R library is available on CRAN and it can be installed as below.

```
Rscript -e 'install.packages("arrow", repos="https://cloud.r-project.org/")'
```

Please refer the official documentation of Apache Arrow for more details.

Note that you must ensure that Arrow R package is installed and available on all cluster nodes. The current supported minimum version is 1.0.0; however, this might change between the minor releases since Arrow optimization in SparkR is experimental.

### Enabling for Conversion to/from R DataFrame, `dapply` and `gapply`

Arrow optimization is available when converting a Spark DataFrame to an R DataFrame using the call `collect(spark_df)`, when creating a Spark DataFrame from an R DataFrame with `createDataFrame(r_df)`, when applying an R native function to each partition via `dapply(...)` and when applying an R native function to grouped data via `gapply(...)`. To use Arrow when executing these, users need to set the Spark configuration ‘spark.sql.execution.arrow.sparkr.enabled’ to ‘true’ first. This is disabled by default.

Whether the optimization is enabled or not, SparkR produces the same results. In addition, the conversion between Spark DataFrame and R DataFrame falls back automatically to non-Arrow optimization implementation when the optimization fails for any reasons before the actual computation.

Note that even with Arrow, `collect(spark_df)` results in the collection of all records in the DataFrame to the driver program and should be done on a small subset of the data. In addition, the specified output schema in `gapply(...)` and `dapply(...)` should be matched to the R DataFrame’s returned by the given function.

### Supported SQL Types

Currently, all Spark SQL data types are supported by Arrow-based conversion except `FloatType`, `BinaryType`, `ArrayType`, `StructType` and `MapType`.

## R Function Name Conflicts

When loading and attaching a new package in R, it is possible to have a name [conflict](https://stat.ethz.ch/R-manual/R-devel/library/base/html/library.html), where a function is masking another function.

The following functions are masked by the SparkR package:

| Masked function             | How to Access                                                                                                                              |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `cov` in `package:stats`    | <pre><code>stats::cov(x, y = NULL, use = "everything",
           method = c("pearson", "kendall", "spearman"))
</code></pre>              |
| `filter` in `package:stats` | <pre><code>stats::filter(x, filter, method = c("convolution", "recursive"),
              sides = 2, circular = FALSE, init)
</code></pre> |
| `sample` in `package:base`  | `base::sample(x, size, replace = FALSE, prob = NULL)`                                                                                      |

Since part of SparkR is modeled on the `dplyr` package, certain functions in SparkR share the same names with those in `dplyr`. Depending on the load order of the two packages, some functions from the package loaded first are masked by those in the package loaded after. In such case, prefix such calls with the package name, for instance, `SparkR::cume_dist(x)` or `dplyr::cume_dist(x)`.

You can inspect the search path in R with `search()`
