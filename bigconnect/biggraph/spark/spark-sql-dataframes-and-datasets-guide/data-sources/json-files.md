# JSON Files

* Python

Spark SQL can automatically infer the schema of a JSON dataset and load it as a DataFrame. This conversion can be done using `SparkSession.read.json` on a JSON file.

Note that the file that is offered as _a json file_ is not a typical JSON file. Each line must contain a separate, self-contained valid JSON object. For more information, please see [JSON Lines text format, also called newline-delimited JSON](http://jsonlines.org/).

For a regular multi-line JSON file, set the `multiLine` parameter to `True`.

```
# spark is from the previous example.
sc = spark.sparkContext

# A JSON dataset is pointed to by path.
# The path can be either a single text file or a directory storing text files
path = "examples/src/main/resources/people.json"
peopleDF = spark.read.json(path)

# The inferred schema can be visualized using the printSchema() method
peopleDF.printSchema()
# root
#  |-- age: long (nullable = true)
#  |-- name: string (nullable = true)

# Creates a temporary view using the DataFrame
peopleDF.createOrReplaceTempView("people")

# SQL statements can be run by using the sql methods provided by spark
teenagerNamesDF = spark.sql("SELECT name FROM people WHERE age BETWEEN 13 AND 19")
teenagerNamesDF.show()
# +------+
# |  name|
# +------+
# |Justin|
# +------+

# Alternatively, a DataFrame can be created for a JSON dataset represented by
# an RDD[String] storing one JSON object per string
jsonStrings = ['{"name":"Yin","address":{"city":"Columbus","state":"Ohio"}}']
otherPeopleRDD = sc.parallelize(jsonStrings)
otherPeople = spark.read.json(otherPeopleRDD)
otherPeople.show()
# +---------------+----+
# |        address|name|
# +---------------+----+
# |[Columbus,Ohio]| Yin|
# +---------------+----+
```

Find full example code at "examples/src/main/python/sql/datasource.py" in the Spark repo.

* Scala

Spark SQL can automatically infer the schema of a JSON dataset and load it as a `Dataset[Row]`. This conversion can be done using `SparkSession.read.json()` on either a `Dataset[String]`, or a JSON file.

Note that the file that is offered as _a json file_ is not a typical JSON file. Each line must contain a separate, self-contained valid JSON object. For more information, please see [JSON Lines text format, also called newline-delimited JSON](http://jsonlines.org/).

For a regular multi-line JSON file, set the `multiLine` option to `true`.

```
// Primitive types (Int, String, etc) and Product types (case classes) encoders are
// supported by importing this when creating a Dataset.
import spark.implicits._

// A JSON dataset is pointed to by path.
// The path can be either a single text file or a directory storing text files
val path = "examples/src/main/resources/people.json"
val peopleDF = spark.read.json(path)

// The inferred schema can be visualized using the printSchema() method
peopleDF.printSchema()
// root
//  |-- age: long (nullable = true)
//  |-- name: string (nullable = true)

// Creates a temporary view using the DataFrame
peopleDF.createOrReplaceTempView("people")

// SQL statements can be run by using the sql methods provided by spark
val teenagerNamesDF = spark.sql("SELECT name FROM people WHERE age BETWEEN 13 AND 19")
teenagerNamesDF.show()
// +------+
// |  name|
// +------+
// |Justin|
// +------+

// Alternatively, a DataFrame can be created for a JSON dataset represented by
// a Dataset[String] storing one JSON object per string
val otherPeopleDataset = spark.createDataset(
  """{"name":"Yin","address":{"city":"Columbus","state":"Ohio"}}""" :: Nil)
val otherPeople = spark.read.json(otherPeopleDataset)
otherPeople.show()
// +---------------+----+
// |        address|name|
// +---------------+----+
// |[Columbus,Ohio]| Yin|
// +---------------+----+
```

Find full example code at "examples/src/main/scala/org/apache/spark/examples/sql/SQLDataSourceExample.scala" in the Spark repo.

* Java

Spark SQL can automatically infer the schema of a JSON dataset and load it as a `Dataset<Row>`. This conversion can be done using `SparkSession.read().json()` on either a `Dataset<String>`, or a JSON file.

Note that the file that is offered as _a json file_ is not a typical JSON file. Each line must contain a separate, self-contained valid JSON object. For more information, please see [JSON Lines text format, also called newline-delimited JSON](http://jsonlines.org/).

For a regular multi-line JSON file, set the `multiLine` option to `true`.

```
import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;

// A JSON dataset is pointed to by path.
// The path can be either a single text file or a directory storing text files
Dataset<Row> people = spark.read().json("examples/src/main/resources/people.json");

// The inferred schema can be visualized using the printSchema() method
people.printSchema();
// root
//  |-- age: long (nullable = true)
//  |-- name: string (nullable = true)

// Creates a temporary view using the DataFrame
people.createOrReplaceTempView("people");

// SQL statements can be run by using the sql methods provided by spark
Dataset<Row> namesDF = spark.sql("SELECT name FROM people WHERE age BETWEEN 13 AND 19");
namesDF.show();
// +------+
// |  name|
// +------+
// |Justin|
// +------+

// Alternatively, a DataFrame can be created for a JSON dataset represented by
// a Dataset<String> storing one JSON object per string.
List<String> jsonData = Arrays.asList(
        "{\"name\":\"Yin\",\"address\":{\"city\":\"Columbus\",\"state\":\"Ohio\"}}");
Dataset<String> anotherPeopleDataset = spark.createDataset(jsonData, Encoders.STRING());
Dataset<Row> anotherPeople = spark.read().json(anotherPeopleDataset);
anotherPeople.show();
// +---------------+----+
// |        address|name|
// +---------------+----+
// |[Columbus,Ohio]| Yin|
// +---------------+----+
```

Find full example code at "examples/src/main/java/org/apache/spark/examples/sql/JavaSQLDataSourceExample.java" in the Spark repo.

* R

Spark SQL can automatically infer the schema of a JSON dataset and load it as a DataFrame. using the `read.json()` function, which loads data from a directory of JSON files where each line of the files is a JSON object.

Note that the file that is offered as _a json file_ is not a typical JSON file. Each line must contain a separate, self-contained valid JSON object. For more information, please see [JSON Lines text format, also called newline-delimited JSON](http://jsonlines.org/).

For a regular multi-line JSON file, set a named parameter `multiLine` to `TRUE`.

```
# A JSON dataset is pointed to by path.
# The path can be either a single text file or a directory storing text files.
path <- "examples/src/main/resources/people.json"
# Create a DataFrame from the file(s) pointed to by path
people <- read.json(path)

# The inferred schema can be visualized using the printSchema() method.
printSchema(people)
## root
##  |-- age: long (nullable = true)
##  |-- name: string (nullable = true)

# Register this DataFrame as a table.
createOrReplaceTempView(people, "people")

# SQL statements can be run by using the sql methods.
teenagers <- sql("SELECT name FROM people WHERE age >= 13 AND age <= 19")
head(teenagers)
##     name
## 1 Justin
```

Find full example code at "examples/src/main/r/RSparkSQLExample.R" in the Spark repo.

* SQL

```sql
CREATE TEMPORARY VIEW jsonTable
USING org.apache.spark.sql.json
OPTIONS (
  path "examples/src/main/resources/people.json"
)

SELECT * FROM jsonTable
```

### Data Source Option

Data source options of JSON can be set via:

* the `.option`/`.options` methods of
  * `DataFrameReader`
  * `DataFrameWriter`
  * `DataStreamReader`
  * `DataStreamWriter`
* the built-in functions below
  * `from_json`
  * `to_json`
  * `schema_of_json`
* `OPTIONS` clause at CREATE TABLE USING DATA\_SOURCE

| Property Name                        | Default                                                                                                       | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Scope      |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| `timeZone`                           | (value of `spark.sql.session.timeZone` configuration)                                                         | <p>Sets the string that indicates a time zone ID to be used to format timestamps in the JSON datasources or partition values. The following formats of <code>timeZone</code> are supported:<br></p><ul><li>Region-based zone ID: It should have the form 'area/city', such as 'America/Los_Angeles'.</li><li>Zone offset: It should be in the format '(+|-)HH:mm', for example '-08:00' or '+01:00'. Also 'UTC' and 'Z' are supported as aliases of '+00:00'.</li></ul><p>Other short names like 'CST' are not recommended to use because they can be ambiguous.</p>                                                                                                                                                                                                                                                                                        | read/write |
| `primitivesAsString`                 | `false`                                                                                                       | Infers all primitive values as a string type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | read       |
| `prefersDecimal`                     | `false`                                                                                                       | Infers all floating-point values as a decimal type. If the values do not fit in decimal, then it infers them as doubles.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | read       |
| `allowComments`                      | `false`                                                                                                       | Ignores Java/C++ style comment in JSON records.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | read       |
| `allowUnquotedFieldNames`            | `false`                                                                                                       | Allows unquoted JSON field names.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | read       |
| `allowSingleQuotes`                  | `true`                                                                                                        | Allows single quotes in addition to double quotes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | read       |
| `allowNumericLeadingZeros`           | `false`                                                                                                       | Allows leading zeros in numbers (e.g. 00012).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | read       |
| `allowBackslashEscapingAnyCharacter` | `false`                                                                                                       | Allows accepting quoting of all character using backslash quoting mechanism.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | read       |
| `mode`                               | `PERMISSIVE`                                                                                                  | <p>Allows a mode for dealing with corrupt records during parsing.<br></p><ul><li><code>PERMISSIVE</code>: when it meets a corrupted record, puts the malformed string into a field configured by <code>columnNameOfCorruptRecord</code>, and sets malformed fields to <code>null</code>. To keep corrupt records, an user can set a string type field named <code>columnNameOfCorruptRecord</code> in an user-defined schema. If a schema does not have the field, it drops corrupt records during parsing. When inferring a schema, it implicitly adds a <code>columnNameOfCorruptRecord</code> field in an output schema.</li><li><code>DROPMALFORMED</code>: ignores the whole corrupted records. This mode is unsupported in the JSON built-in functions.</li><li><code>FAILFAST</code>: throws an exception when it meets corrupted records.</li></ul> | read       |
| `columnNameOfCorruptRecord`          | (value of `spark.sql.columnNameOfCorruptRecord` configuration)                                                | Allows renaming the new field having malformed string created by `PERMISSIVE` mode. This overrides spark.sql.columnNameOfCorruptRecord.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | read       |
| `dateFormat`                         | `yyyy-MM-dd`                                                                                                  | Sets the string that indicates a date format. Custom date formats follow the formats at [datetime pattern](https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html). This applies to date type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | read/write |
| `timestampFormat`                    | `yyyy-MM-dd'T'HH:mm:ss[.SSS][XXX]`                                                                            | Sets the string that indicates a timestamp format. Custom date formats follow the formats at [datetime pattern](https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html). This applies to timestamp type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | read/write |
| `timestampNTZFormat`                 | yyyy-MM-dd'T'HH:mm:ss\[.SSS]                                                                                  | Sets the string that indicates a timestamp without timezone format. Custom date formats follow the formats at [Datetime Patterns](https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html). This applies to timestamp without timezone type, note that zone-offset and time-zone components are not supported when writing or reading this data type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | read/write |
| `enableDateTimeParsingFallback`      | Enabled if the time parser policy has legacy settings or if no custom date or timestamp pattern was provided. | Allows falling back to the backward compatible (Spark 1.x and 2.0) behavior of parsing dates and timestamps if values do not match the set patterns.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | read       |
| `multiLine`                          | `false`                                                                                                       | Parse one record, which may span multiple lines, per file. JSON built-in functions ignore this option.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | read       |
| `allowUnquotedControlChars`          | `false`                                                                                                       | Allows JSON Strings to contain unquoted control characters (ASCII characters with value less than 32, including tab and line feed characters) or not.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | read       |
| `encoding`                           | Detected automatically when `multiLine` is set to `true` (for reading), `UTF-8` (for writing)                 | For reading, allows to forcibly set one of standard basic or extended encoding for the JSON files. For example UTF-16BE, UTF-32LE. For writing, Specifies encoding (charset) of saved json files. JSON built-in functions ignore this option.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | read/write |
| `lineSep`                            | , `\r`,  (for reading),  (for writing)                                                                        | Defines the line separator that should be used for parsing. JSON built-in functions ignore this option.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | read/write |
| `samplingRatio`                      | `1.0`                                                                                                         | Defines fraction of input JSON objects used for schema inferring.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | read       |
| `dropFieldIfAllNull`                 | `false`                                                                                                       | Whether to ignore column of all null values or empty array during schema inference.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | read       |
| `locale`                             | `en-US`                                                                                                       | Sets a locale as language tag in IETF BCP 47 format. For instance, `locale` is used while parsing dates and timestamps.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | read       |
| `allowNonNumericNumbers`             | `true`                                                                                                        | <p>Allows JSON parser to recognize set of “Not-a-Number” (NaN) tokens as legal floating number values.<br></p><ul><li><code>+INF</code>: for positive infinity, as well as alias of <code>+Infinity</code> and <code>Infinity</code>.</li><li><code>-INF</code>: for negative infinity, alias <code>-Infinity</code>.</li><li><code>NaN</code>: for other not-a-numbers, like result of division by zero.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                         | read       |
| `compression`                        | (none)                                                                                                        | Compression codec to use when saving to file. This can be one of the known case-insensitive shorten names (none, bzip2, gzip, lz4, snappy and deflate). JSON built-in functions ignore this option.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | write      |
| `ignoreNullFields`                   | (value of `spark.sql.jsonGenerator.ignoreNullFields` configuration)                                           | Whether to ignore null fields when generating JSON objects.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | write      |

Other generic options can be found in Generic File Source Options.
