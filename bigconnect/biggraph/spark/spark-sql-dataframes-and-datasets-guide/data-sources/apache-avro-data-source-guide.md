# Apache Avro Data Source Guide

Since Spark 2.4 release, Spark SQL provides built-in support for reading and writing Apache Avro data.

### Deploying

The `spark-avro` module is external and not included in `spark-submit` or `spark-shell` by default.

As with any Spark applications, `spark-submit` is used to launch your application. `spark-avro_2.12` and its dependencies can be directly added to `spark-submit` using `--packages`, such as,

```
./bin/spark-submit --packages org.apache.spark:spark-avro_2.12:3.5.1 ...
```

For experimenting on `spark-shell`, you can also use `--packages` to add `org.apache.spark:spark-avro_2.12` and its dependencies directly,

```
./bin/spark-shell --packages org.apache.spark:spark-avro_2.12:3.5.1 ...
```

See Application Submission Guide for more details about submitting applications with external dependencies.

### Load and Save Functions

Since `spark-avro` module is external, there is no `.avro` API in `DataFrameReader` or `DataFrameWriter`.

To load/save data in Avro format, you need to specify the data source option `format` as `avro`(or `org.apache.spark.sql.avro`).

* Python

```python
df = spark.read.format("avro").load("examples/src/main/resources/users.avro")
df.select("name", "favorite_color").write.format("avro").save("namesAndFavColors.avro")
```

* Scala

```scala
val usersDF = spark.read.format("avro").load("examples/src/main/resources/users.avro")
usersDF.select("name", "favorite_color").write.format("avro").save("namesAndFavColors.avro")
```

* Java

```java
Dataset<Row> usersDF = spark.read().format("avro").load("examples/src/main/resources/users.avro");
usersDF.select("name", "favorite_color").write().format("avro").save("namesAndFavColors.avro");
```

* R

```r
df <- read.df("examples/src/main/resources/users.avro", "avro")
write.df(select(df, "name", "favorite_color"), "namesAndFavColors.avro", "avro")
```

### To\_avro() and from\_avro()

The Avro package provides function `to_avro` to encode a column as binary in Avro format, and `from_avro()` to decode Avro binary data into a column. Both functions transform one column to another column, and the input/output SQL data type can be a complex type or a primitive type.

Using Avro record as columns is useful when reading from or writing to a streaming source like Kafka. Each Kafka key-value record will be augmented with some metadata, such as the ingestion timestamp into Kafka, the offset in Kafka, etc.

* If the “value” field that contains your data is in Avro, you could use `from_avro()` to extract your data, enrich it, clean it, and then push it downstream to Kafka again or write it out to a file.
* `to_avro()` can be used to turn structs into Avro records. This method is particularly useful when you would like to re-encode multiple columns into a single one when writing data out to Kafka.
* Python

```python
from pyspark.sql.avro.functions import from_avro, to_avro

# `from_avro` requires Avro schema in JSON string format.
jsonFormatSchema = open("examples/src/main/resources/user.avsc", "r").read()

df = spark\
  .readStream\
  .format("kafka")\
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")\
  .option("subscribe", "topic1")\
  .load()

# 1. Decode the Avro data into a struct;
# 2. Filter by column `favorite_color`;
# 3. Encode the column `name` in Avro format.
output = df\
  .select(from_avro("value", jsonFormatSchema).alias("user"))\
  .where('user.favorite_color == "red"')\
  .select(to_avro("user.name").alias("value"))

query = output\
  .writeStream\
  .format("kafka")\
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")\
  .option("topic", "topic2")\
  .start()
```

* Scala

```scala
import org.apache.spark.sql.avro.functions._

// `from_avro` requires Avro schema in JSON string format.
val jsonFormatSchema = new String(Files.readAllBytes(Paths.get("./examples/src/main/resources/user.avsc")))

val df = spark
  .readStream
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("subscribe", "topic1")
  .load()

// 1. Decode the Avro data into a struct;
// 2. Filter by column `favorite_color`;
// 3. Encode the column `name` in Avro format.
val output = df
  .select(from_avro($"value", jsonFormatSchema) as $"user")
  .where("user.favorite_color == \"red\"")
  .select(to_avro($"user.name") as $"value")

val query = output
  .writeStream
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("topic", "topic2")
  .start()
```

* Java

```java
import static org.apache.spark.sql.functions.col;
import static org.apache.spark.sql.avro.functions.*;

// `from_avro` requires Avro schema in JSON string format.
String jsonFormatSchema = new String(Files.readAllBytes(Paths.get("./examples/src/main/resources/user.avsc")));

Dataset<Row> df = spark
  .readStream()
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("subscribe", "topic1")
  .load();

// 1. Decode the Avro data into a struct;
// 2. Filter by column `favorite_color`;
// 3. Encode the column `name` in Avro format.
Dataset<Row> output = df
  .select(from_avro(col("value"), jsonFormatSchema).as("user"))
  .where("user.favorite_color == \"red\"")
  .select(to_avro(col("user.name")).as("value"));

StreamingQuery query = output
  .writeStream()
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("topic", "topic2")
  .start();
```

* R

```r
# `from_avro` requires Avro schema in JSON string format.
jsonFormatSchema <- paste0(readLines("examples/src/main/resources/user.avsc"), collapse=" ")

df <- read.stream(
  "kafka",
  kafka.bootstrap.servers = "host1:port1,host2:port2",
  subscribe = "topic1"
)

# 1. Decode the Avro data into a struct;
# 2. Filter by column `favorite_color`;
# 3. Encode the column `name` in Avro format.

output <- select(
  filter(
    select(df, alias(from_avro("value", jsonFormatSchema), "user")),
    column("user.favorite_color") == "red"
  ),
  alias(to_avro("user.name"), "value")
)

write.stream(
  output,
  "kafka",
  kafka.bootstrap.servers = "host1:port1,host2:port2",
  topic = "topic2"
)
```

### Data Source Option

Data source options of Avro can be set via:

* the `.option` method on `DataFrameReader` or `DataFrameWriter`.
* the `options` parameter in function `from_avro`.

| Property Name                         | Default                                                            | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Scope                                | Since Version |
| ------------------------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ | ------------- |
| `avroSchema`                          | None                                                               | <p>Optional schema provided by a user in JSON format.</p><ul><li>When reading Avro files or calling function <code>from_avro</code>, this option can be set to an evolved schema, which is compatible but different with the actual Avro schema. The deserialization schema will be consistent with the evolved schema. For example, if we set an evolved schema containing one additional column with a default value, the reading result in Spark will contain the new column too. Note that when using this option with <code>from_avro</code>, you still need to pass the actual Avro schema as a parameter to the function.</li><li>When writing Avro, this option can be set if the expected output Avro schema doesn't match the schema converted by Spark. For example, the expected schema of one column is of "enum" type, instead of "string" type in the default converted schema.</li></ul> | read, write and function `from_avro` | 2.4.0         |
| `recordName`                          | topLevelRecord                                                     | Top level record name in write result, which is required in Avro spec.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | write                                | 2.4.0         |
| `recordNamespace`                     | ""                                                                 | Record namespace in write result.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | write                                | 2.4.0         |
| `ignoreExtension`                     | true                                                               | <p>The option controls ignoring of files without <code>.avro</code> extensions in read.<br>If the option is enabled, all files (with and without <code>.avro</code> extension) are loaded.<br>The option has been deprecated, and it will be removed in the future releases. Please use the general data source option <a href="https://spark.apache.org/docs/latest/sql-data-sources-generic-options.html#path-global-filter">pathGlobFilter</a> for filtering file names.</p>                                                                                                                                                                                                                                                                                                                                                                                                                          | read                                 | 2.4.0         |
| `compression`                         | snappy                                                             | <p>The <code>compression</code> option allows to specify a compression codec used in write.<br>Currently supported codecs are <code>uncompressed</code>, <code>snappy</code>, <code>deflate</code>, <code>bzip2</code>, <code>xz</code> and <code>zstandard</code>.<br>If the option is not set, the configuration <code>spark.sql.avro.compression.codec</code> config is taken into account.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | write                                | 2.4.0         |
| `mode`                                | FAILFAST                                                           | <p>The <code>mode</code> option allows to specify parse mode for function <code>from_avro</code>.<br>Currently supported modes are:</p><ul><li><code>FAILFAST</code>: Throws an exception on processing corrupted record.</li><li><code>PERMISSIVE</code>: Corrupt records are processed as null result. Therefore, the data schema is forced to be fully nullable, which might be different from the one user provided.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | function `from_avro`                 | 2.4.0         |
| `datetimeRebaseMode`                  | (value of `spark.sql.avro.datetimeRebaseModeInRead` configuration) | <p>The <code>datetimeRebaseMode</code> option allows to specify the rebasing mode for the values of the <code>date</code>, <code>timestamp-micros</code>, <code>timestamp-millis</code> logical types from the Julian to Proleptic Gregorian calendar.<br>Currently supported modes are:</p><ul><li><code>EXCEPTION</code>: fails in reads of ancient dates/timestamps that are ambiguous between the two calendars.</li><li><code>CORRECTED</code>: loads dates/timestamps without rebasing.</li><li><code>LEGACY</code>: performs rebasing of ancient dates/timestamps from the Julian to Proleptic Gregorian calendar.</li></ul>                                                                                                                                                                                                                                                                      | read and function `from_avro`        | 3.2.0         |
| `positionalFieldMatching`             | false                                                              | This can be used in tandem with the \`avroSchema\` option to adjust the behavior for matching the fields in the provided Avro schema with those in the SQL schema. By default, the matching will be performed using field names, ignoring their positions. If this option is set to "true", the matching will be based on the position of the fields.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | read and write                       | 3.2.0         |
| `enableStableIdentifiersForUnionType` | false                                                              | If it is set to true, Avro schema is deserialized into Spark SQL schema, and the Avro Union type is transformed into a structure where the field names remain consistent with their respective types. The resulting field names are converted to lowercase, e.g. member\_int or member\_string. If two user-defined type names or a user-defined type name and a built-in type name are identical regardless of case, an exception will be raised. However, in other cases, the field names can be uniquely identified.                                                                                                                                                                                                                                                                                                                                                                                  | read                                 | 3.5.0         |

### Configuration

Configuration of Avro can be done using the `setConf` method on SparkSession or by running `SET key=value` commands using SQL.

| Property Name                                       | Default     | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Since Version |
| --------------------------------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| spark.sql.legacy.replaceDatabricksSparkAvro.enabled | true        | <p>If it is set to true, the data source provider <code>com.databricks.spark.avro</code> is mapped to the built-in but external Avro data source module for backward compatibility.<br>Note: the SQL config has been deprecated in Spark 3.2 and might be removed in the future.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 2.4.0         |
| spark.sql.avro.compression.codec                    | snappy      | Compression codec used in writing of AVRO files. Supported codecs: uncompressed, deflate, snappy, bzip2, xz and zstandard. Default codec is snappy.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 2.4.0         |
| spark.sql.avro.deflate.level                        | -1          | Compression level for the deflate codec used in writing of AVRO files. Valid value must be in the range of from 1 to 9 inclusive or -1. The default value is -1 which corresponds to 6 level in the current implementation.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 2.4.0         |
| spark.sql.avro.datetimeRebaseModeInRead             | `EXCEPTION` | <p>The rebasing mode for the values of the <code>date</code>, <code>timestamp-micros</code>, <code>timestamp-millis</code> logical types from the Julian to Proleptic Gregorian calendar:<br></p><ul><li><code>EXCEPTION</code>: Spark will fail the reading if it sees ancient dates/timestamps that are ambiguous between the two calendars.</li><li><code>CORRECTED</code>: Spark will not do rebase and read the dates/timestamps as it is.</li><li><code>LEGACY</code>: Spark will rebase dates/timestamps from the legacy hybrid (Julian + Gregorian) calendar to Proleptic Gregorian calendar when reading Avro files.</li></ul><p>This config is only effective if the writer info (like Spark, Hive) of the Avro files is unknown.</p> | 3.0.0         |
| spark.sql.avro.datetimeRebaseModeInWrite            | `EXCEPTION` | <p>The rebasing mode for the values of the <code>date</code>, <code>timestamp-micros</code>, <code>timestamp-millis</code> logical types from the Proleptic Gregorian to Julian calendar:<br></p><ul><li><code>EXCEPTION</code>: Spark will fail the writing if it sees ancient dates/timestamps that are ambiguous between the two calendars.</li><li><code>CORRECTED</code>: Spark will not do rebase and write the dates/timestamps as it is.</li><li><code>LEGACY</code>: Spark will rebase dates/timestamps from Proleptic Gregorian calendar to the legacy hybrid (Julian + Gregorian) calendar when writing Avro files.</li></ul>                                                                                                        | 3.0.0         |
| spark.sql.avro.filterPushdown.enabled               | true        | When true, enable filter pushdown to Avro datasource.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 3.1.0         |

### Compatibility with Databricks spark-avro

This Avro data source module is originally from and compatible with Databricks’s open source repository [spark-avro](https://github.com/databricks/spark-avro).

By default with the SQL configuration `spark.sql.legacy.replaceDatabricksSparkAvro.enabled` enabled, the data source provider `com.databricks.spark.avro` is mapped to this built-in Avro module. For the Spark tables created with `Provider` property as `com.databricks.spark.avro` in catalog meta store, the mapping is essential to load these tables if you are using this built-in Avro module.

Note in Databricks’s [spark-avro](https://github.com/databricks/spark-avro), implicit classes `AvroDataFrameWriter` and `AvroDataFrameReader` were created for shortcut function `.avro()`. In this built-in but external module, both implicit classes are removed. Please use `.format("avro")` in `DataFrameWriter` or `DataFrameReader` instead, which should be clean and good enough.

If you prefer using your own build of `spark-avro` jar file, you can simply disable the configuration `spark.sql.legacy.replaceDatabricksSparkAvro.enabled`, and use the option `--jars` on deploying your applications. Read the Advanced Dependency Management section in Application Submission Guide for more details.

### Supported types for Avro -> Spark SQL conversion

Currently Spark supports reading all [primitive types](https://avro.apache.org/docs/1.11.2/specification/#primitive-types) and [complex types](https://avro.apache.org/docs/1.11.2/specification/#complex-types) under records of Avro.

| Avro type | Spark SQL type |
| --------- | -------------- |
| boolean   | BooleanType    |
| int       | IntegerType    |
| long      | LongType       |
| float     | FloatType      |
| double    | DoubleType     |
| string    | StringType     |
| enum      | StringType     |
| fixed     | BinaryType     |
| bytes     | BinaryType     |
| record    | StructType     |
| array     | ArrayType      |
| map       | MapType        |
| union     | See below      |

In addition to the types listed above, it supports reading `union` types. The following three types are considered basic `union` types:

1. `union(int, long)` will be mapped to LongType.
2. `union(float, double)` will be mapped to DoubleType.
3. `union(something, null)`, where something is any supported Avro type. This will be mapped to the same Spark SQL type as that of something, with nullable set to true. All other union types are considered complex. They will be mapped to StructType where field names are member0, member1, etc., in accordance with members of the union. This is consistent with the behavior when converting between Avro and Parquet.

It also supports reading the following Avro [logical types](https://avro.apache.org/docs/1.11.2/specification/#logical-types):

| Avro logical type | Avro type | Spark SQL type |
| ----------------- | --------- | -------------- |
| date              | int       | DateType       |
| timestamp-millis  | long      | TimestampType  |
| timestamp-micros  | long      | TimestampType  |
| decimal           | fixed     | DecimalType    |
| decimal           | bytes     | DecimalType    |

At the moment, it ignores docs, aliases and other properties present in the Avro file.

### Supported types for Spark SQL -> Avro conversion

Spark supports writing of all Spark SQL types into Avro. For most types, the mapping from Spark types to Avro types is straightforward (e.g. IntegerType gets converted to int); however, there are a few special cases which are listed below:

| Spark SQL type | Avro type | Avro logical type |
| -------------- | --------- | ----------------- |
| ByteType       | int       |                   |
| ShortType      | int       |                   |
| BinaryType     | bytes     |                   |
| DateType       | int       | date              |
| TimestampType  | long      | timestamp-micros  |
| DecimalType    | fixed     | decimal           |

You can also specify the whole output Avro schema with the option `avroSchema`, so that Spark SQL types can be converted into other Avro types. The following conversions are not applied by default and require user specified Avro schema:

| Spark SQL type | Avro type | Avro logical type |
| -------------- | --------- | ----------------- |
| BinaryType     | fixed     |                   |
| StringType     | enum      |                   |
| TimestampType  | long      | timestamp-millis  |
| DecimalType    | bytes     | decimal           |
