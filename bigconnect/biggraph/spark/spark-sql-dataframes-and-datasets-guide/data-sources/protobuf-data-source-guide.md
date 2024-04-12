# Protobuf Data Source Guide

Since Spark 3.4.0 release, Spark SQL provides built-in support for reading and writing protobuf data.

### Deploying

The `spark-protobuf` module is external and not included in `spark-submit` or `spark-shell` by default.

As with any Spark applications, `spark-submit` is used to launch your application. `spark-protobuf_2.12` and its dependencies can be directly added to `spark-submit` using `--packages`, such as,

```
./bin/spark-submit --packages org.apache.spark:spark-protobuf_2.12:3.5.1 ...
```

For experimenting on `spark-shell`, you can also use `--packages` to add `org.apache.spark:spark-protobuf_2.12` and its dependencies directly,

```
./bin/spark-shell --packages org.apache.spark:spark-protobuf_2.12:3.5.1 ...
```

See Application Submission Guide for more details about submitting applications with external dependencies.

### To\_protobuf() and from\_protobuf()

The spark-protobuf package provides function `to_protobuf` to encode a column as binary in protobuf format, and `from_protobuf()` to decode protobuf binary data into a column. Both functions transform one column to another column, and the input/output SQL data type can be a complex type or a primitive type.

Using protobuf message as columns is useful when reading from or writing to a streaming source like Kafka. Each Kafka key-value record will be augmented with some metadata, such as the ingestion timestamp into Kafka, the offset in Kafka, etc.

* If the “value” field that contains your data is in protobuf, you could use `from_protobuf()` to extract your data, enrich it, clean it, and then push it downstream to Kafka again or write it out to a different sink.
* `to_protobuf()` can be used to turn structs into protobuf message. This method is particularly useful when you would like to re-encode multiple columns into a single one when writing data out to Kafka.

Spark SQL schema is generated based on the protobuf descriptor file or protobuf class passed to `from_protobuf` and `to_protobuf`. The specified protobuf class or protobuf descriptor file must match the data, otherwise, the behavior is undefined: it may fail or return arbitrary results.

* Python

```python
from pyspark.sql.protobuf.functions import from_protobuf, to_protobuf

# from_protobuf and to_protobuf provide two schema choices. Via Protobuf descriptor file,
# or via shaded Java class.
# give input .proto protobuf schema
# syntax = "proto3"
# message AppEvent {
#   string name = 1;
#   int64 id = 2;
#   string context = 3;
# }
df = spark
  .readStream
  .format("kafka")\
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("subscribe", "topic1")
  .load()

# 1. Decode the Protobuf data of schema `AppEvent` into a struct;
# 2. Filter by column `name`;
# 3. Encode the column `event` in Protobuf format.
# The Protobuf protoc command can be used to generate a protobuf descriptor file for give .proto file.
output = df
  .select(from_protobuf("value", "AppEvent", descriptorFilePath).alias("event"))
  .where('event.name == "alice"')
  .select(to_protobuf("event", "AppEvent", descriptorFilePath).alias("event"))

# Alternatively, you can decode and encode the SQL columns into protobuf format using protobuf
# class name. The specified Protobuf class must match the data, otherwise the behavior is undefined:
# it may fail or return arbitrary result. To avoid conflicts, the jar file containing the
# 'com.google.protobuf.*' classes should be shaded. An example of shading can be found at
# https://github.com/rangadi/shaded-protobuf-classes.
output = df
  .select(from_protobuf("value", "org.sparkproject.spark_protobuf.protobuf.AppEvent").alias("event"))
  .where('event.name == "alice"')

output.printSchema()
# root
#  |--event: struct (nullable = true)
#  |   |-- name : string (nullable = true)
#  |   |-- id: long (nullable = true)
#  |   |-- context: string (nullable = true)

output = output
  .select(to_protobuf("event", "org.sparkproject.spark_protobuf.protobuf.AppEvent").alias("event"))

query = output
  .writeStream
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")\
  .option("topic", "topic2")
  .start()
```

* Scala

```scala
import org.apache.spark.sql.protobuf.functions._

// `from_protobuf` and `to_protobuf` provides two schema choices. Via the protobuf descriptor file,
// or via shaded Java class.
// give input .proto protobuf schema
// syntax = "proto3"
// message AppEvent {
//   string name = 1;
//   int64 id = 2;
//   string context = 3;
// }

val df = spark
  .readStream
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("subscribe", "topic1")
  .load()

// 1. Decode the Protobuf data of schema `AppEvent` into a struct;
// 2. Filter by column `name`;
// 3. Encode the column `event` in Protobuf format.
// The Protobuf protoc command can be used to generate a protobuf descriptor file for give .proto file.
val output = df
  .select(from_protobuf($"value", "AppEvent", descriptorFilePath) as $"event")
  .where("event.name == \"alice\"")
  .select(to_protobuf($"user", "AppEvent", descriptorFilePath) as $"event")

val query = output
  .writeStream
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("topic", "topic2")
  .start()

// Alternatively, you can decode and encode the SQL columns into protobuf format using protobuf
// class name. The specified Protobuf class must match the data, otherwise the behavior is undefined:
// it may fail or return arbitrary result. To avoid conflicts, the jar file containing the
// 'com.google.protobuf.*' classes should be shaded. An example of shading can be found at
// https://github.com/rangadi/shaded-protobuf-classes.
var output = df
  .select(from_protobuf($"value", "org.example.protos..AppEvent") as $"event")
  .where("event.name == \"alice\"")

output.printSchema()
// root
//  |--event: struct (nullable = true)
//  |    |-- name : string (nullable = true)
//  |    |-- id: long (nullable = true)
//  |    |-- context: string (nullable = true)

output = output.select(to_protobuf($"event", "org.sparkproject.spark_protobuf.protobuf.AppEvent") as $"event")

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
import static org.apache.spark.sql.protobuf.functions.*;

// `from_protobuf` and `to_protobuf` provides two schema choices. Via the protobuf descriptor file,
// or via shaded Java class.
// give input .proto protobuf schema
// syntax = "proto3"
// message AppEvent {
//   string name = 1;
//   int64 id = 2;
//   string context = 3;
// }

Dataset<Row> df = spark
  .readStream()
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("subscribe", "topic1")
  .load();

// 1. Decode the Protobuf data of schema `AppEvent` into a struct;
// 2. Filter by column `name`;
// 3. Encode the column `event` in Protobuf format.
// The Protobuf protoc command can be used to generate a protobuf descriptor file for give .proto file.
Dataset<Row> output = df
  .select(from_protobuf(col("value"), "AppEvent", descriptorFilePath).as("event"))
  .where("event.name == \"alice\"")
  .select(to_protobuf(col("event"), "AppEvent", descriptorFilePath).as("event"));

// Alternatively, you can decode and encode the SQL columns into protobuf format using protobuf
// class name. The specified Protobuf class must match the data, otherwise the behavior is undefined:
// it may fail or return arbitrary result. To avoid conflicts, the jar file containing the
// 'com.google.protobuf.*' classes should be shaded. An example of shading can be found at
// https://github.com/rangadi/shaded-protobuf-classes.
Dataset<Row> output = df
  .select(
    from_protobuf(col("value"),
    "org.sparkproject.spark_protobuf.protobuf.AppEvent").as("event"))
  .where("event.name == \"alice\"")

output.printSchema()
// root
//  |--event: struct (nullable = true)
//  |    |-- name : string (nullable = true)
//  |    |-- id: long (nullable = true)
//  |    |-- context: string (nullable = true)

output = output.select(
  to_protobuf(col("event"),
  "org.sparkproject.spark_protobuf.protobuf.AppEvent").as("event"));

StreamingQuery query = output
  .writeStream()
  .format("kafka")
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2")
  .option("topic", "topic2")
  .start();
```

### Supported types for Protobuf -> Spark SQL conversion

Currently Spark supports reading [protobuf scalar types](https://developers.google.com/protocol-buffers/docs/proto3#scalar), [enum types](https://developers.google.com/protocol-buffers/docs/proto3#enum), [nested type](https://developers.google.com/protocol-buffers/docs/proto3#nested), and [maps type](https://developers.google.com/protocol-buffers/docs/proto3#maps) under messages of Protobuf. In addition to the these types, `spark-protobuf` also introduces support for Protobuf `OneOf` fields. which allows you to handle messages that can have multiple possible sets of fields, but only one set can be present at a time. This is useful for situations where the data you are working with is not always in the same format, and you need to be able to handle messages with different sets of fields without encountering errors.

| Protobuf type | Spark SQL type |
| ------------- | -------------- |
| boolean       | BooleanType    |
| int           | IntegerType    |
| long          | LongType       |
| float         | FloatType      |
| double        | DoubleType     |
| string        | StringType     |
| enum          | StringType     |
| bytes         | BinaryType     |
| Message       | StructType     |
| repeated      | ArrayType      |
| map           | MapType        |
| OneOf         | Struct         |

It also supports reading the following Protobuf types [Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp) and [Duration](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#duration)

| Protobuf logical type | Protobuf schema                        | Spark SQL type      |
| --------------------- | -------------------------------------- | ------------------- |
| duration              | MessageType{seconds: Long, nanos: Int} | DayTimeIntervalType |
| timestamp             | MessageType{seconds: Long, nanos: Int} | TimestampType       |

### Supported types for Spark SQL -> Protobuf conversion

Spark supports the writing of all Spark SQL types into Protobuf. For most types, the mapping from Spark types to Protobuf types is straightforward (e.g. IntegerType gets converted to int);

| Spark SQL type | Protobuf type |
| -------------- | ------------- |
| BooleanType    | boolean       |
| IntegerType    | int           |
| LongType       | long          |
| FloatType      | float         |
| DoubleType     | double        |
| StringType     | string        |
| StringType     | enum          |
| BinaryType     | bytes         |
| StructType     | message       |
| ArrayType      | repeated      |
| MapType        | map           |

### Handling circular references protobuf fields

One common issue that can arise when working with Protobuf data is the presence of circular references. In Protobuf, a circular reference occurs when a field refers back to itself or to another field that refers back to the original field. This can cause issues when parsing the data, as it can result in infinite loops or other unexpected behavior. To address this issue, the latest version of spark-protobuf introduces a new feature: the ability to check for circular references through field types. This allows users use the `recursive.fields.max.depth` option to specify the maximum number of levels of recursion to allow when parsing the schema. By default, `spark-protobuf` will not permit recursive fields by setting `recursive.fields.max.depth` to -1. However, you can set this option to 0 to 10 if needed.

Setting `recursive.fields.max.depth` to 0 drops all recursive fields, setting it to 1 allows it to be recursed once, and setting it to 2 allows it to be recursed twice. A `recursive.fields.max.depth` value greater than 10 is not allowed, as it can lead to performance issues and even stack overflows.

SQL Schema for the below protobuf message will vary based on the value of `recursive.fields.max.depth`.
