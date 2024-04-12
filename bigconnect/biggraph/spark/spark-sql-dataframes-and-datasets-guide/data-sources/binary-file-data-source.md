# Binary File Data Source

Since Spark 3.0, Spark supports binary file data source, which reads binary files and converts each file into a single record that contains the raw content and metadata of the file. It produces a DataFrame with the following columns and possibly partition columns:

* `path`: StringType
* `modificationTime`: TimestampType
* `length`: LongType
* `content`: BinaryType

To read whole binary files, you need to specify the data source `format` as `binaryFile`. To load files with paths matching a given glob pattern while keeping the behavior of partition discovery, you can use the general data source option `pathGlobFilter`. For example, the following code reads all PNG files from the input directory:

* Python

```python
spark.read.format("binaryFile").option("pathGlobFilter", "*.png").load("/path/to/data")
```

* Scala

```scala
spark.read.format("binaryFile").option("pathGlobFilter", "*.png").load("/path/to/data")
```

* Java

```java
spark.read().format("binaryFile").option("pathGlobFilter", "*.png").load("/path/to/data");
```

* R

```r
read.df("/path/to/data", source = "binaryFile", pathGlobFilter = "*.png")
```

Binary file data source does not support writing a DataFrame back to the original files.
