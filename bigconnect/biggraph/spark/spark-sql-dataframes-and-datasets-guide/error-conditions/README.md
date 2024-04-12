# Error Conditions

This is a list of common, named error conditions returned by Spark SQL.

Also see SQLSTATE Codes.

#### AGGREGATE\_FUNCTION\_WITH\_NONDETERMINISTIC\_EXPRESSION

SQLSTATE: none assigned

Non-deterministic expression `<sqlExpr>` should not appear in the arguments of an aggregate function.

#### ALL\_PARTITION\_COLUMNS\_NOT\_ALLOWED

SQLSTATE: none assigned

Cannot use all columns for partition columns.

#### ALTER\_TABLE\_COLUMN\_DESCRIPTOR\_DUPLICATE

SQLSTATE: 42710

ALTER TABLE `<type>` column `<columnName>` specifies descriptor “`<optionName>`” more than once, which is invalid.

#### AMBIGUOUS\_ALIAS\_IN\_NESTED\_CTE

SQLSTATE: none assigned

Name `<name>` is ambiguous in nested CTE. Please set `<config>` to “CORRECTED” so that name defined in inner CTE takes precedence. If set it to “LEGACY”, outer CTE definitions will take precedence. See ‘`<docroot>`/sql-migration-guide.html#query-engine’.

#### AMBIGUOUS\_COLUMN\_OR\_FIELD

SQLSTATE: 42702

Column or field `<name>` is ambiguous and has `<n>` matches.

#### AMBIGUOUS\_COLUMN\_REFERENCE

SQLSTATE: 42702

Column `<name>` is ambiguous. It’s because you joined several DataFrame together, and some of these DataFrames are the same. This column points to one of the DataFrame but Spark is unable to figure out which one. Please alias the DataFrames with different names via `DataFrame.alias` before joining them, and specify the column using qualified name, e.g. `df.alias("a").join(df.alias("b"), col("a.id") > col("b.id"))`.

#### AMBIGUOUS\_LATERAL\_COLUMN\_ALIAS

SQLSTATE: 42702

Lateral column alias `<name>` is ambiguous and has `<n>` matches.

#### AMBIGUOUS\_REFERENCE

SQLSTATE: 42704

Reference `<name>` is ambiguous, could be: `<referenceNames>`.

#### AMBIGUOUS\_REFERENCE\_TO\_FIELDS

SQLSTATE: 42000

Ambiguous reference to the field `<field>`. It appears `<count>` times in the schema.

#### ARITHMETIC\_OVERFLOW

SQLSTATE: 22003

`<message>`.`<alternative>` If necessary set `<config>` to “false” to bypass this error.

#### AS\_OF\_JOIN

SQLSTATE: none assigned

Invalid as-of join.

For more details see AS\_OF\_JOIN

#### AVRO\_INCOMPATIBLE\_READ\_TYPE

SQLSTATE: none assigned

Cannot convert Avro `<avroPath>` to SQL `<sqlPath>` because the original encoded data type is `<avroType>`, however you’re trying to read the field as `<sqlType>`, which would lead to an incorrect answer. To allow reading this field, enable the SQL configuration: “spark.sql.legacy.avro.allowIncompatibleSchema”.

#### BATCH\_METADATA\_NOT\_FOUND

SQLSTATE: 42K03

Unable to find batch `<batchMetadataFile>`.

#### BINARY\_ARITHMETIC\_OVERFLOW

SQLSTATE: 22003

`<value1>` `<symbol>` `<value2>` caused overflow.

#### CALL\_ON\_STREAMING\_DATASET\_UNSUPPORTED

SQLSTATE: none assigned

The method `<methodName>` can not be called on streaming Dataset/DataFrame.

#### CANNOT\_CAST\_DATATYPE

SQLSTATE: 42846

Cannot cast `<sourceType>` to `<targetType>`.

#### CANNOT\_CONVERT\_PROTOBUF\_FIELD\_TYPE\_TO\_SQL\_TYPE

SQLSTATE: none assigned

Cannot convert Protobuf `<protobufColumn>` to SQL `<sqlColumn>` because schema is incompatible (protobufType = `<protobufType>`, sqlType = `<sqlType>`).

#### CANNOT\_CONVERT\_PROTOBUF\_MESSAGE\_TYPE\_TO\_SQL\_TYPE

SQLSTATE: none assigned

Unable to convert `<protobufType>` of Protobuf to SQL type `<toType>`.

#### CANNOT\_CONVERT\_SQL\_TYPE\_TO\_PROTOBUF\_FIELD\_TYPE

SQLSTATE: none assigned

Cannot convert SQL `<sqlColumn>` to Protobuf `<protobufColumn>` because schema is incompatible (protobufType = `<protobufType>`, sqlType = `<sqlType>`).

#### CANNOT\_CONVERT\_SQL\_VALUE\_TO\_PROTOBUF\_ENUM\_TYPE

SQLSTATE: none assigned

Cannot convert SQL `<sqlColumn>` to Protobuf `<protobufColumn>` because `<data>` is not in defined values for enum: `<enumString>`.

#### CANNOT\_DECODE\_URL

SQLSTATE: 22546

The provided URL cannot be decoded: `<url>`. Please ensure that the URL is properly formatted and try again.

#### CANNOT\_INVOKE\_IN\_TRANSFORMATIONS

SQLSTATE: none assigned

Dataset transformations and actions can only be invoked by the driver, not inside of other Dataset transformations; for example, dataset1.map(x => dataset2.values.count() \* x) is invalid because the values transformation and count action cannot be performed inside of the dataset1.map transformation. For more information, see SPARK-28702.

#### CANNOT\_LOAD\_FUNCTION\_CLASS

SQLSTATE: none assigned

Cannot load class `<className>` when registering the function `<functionName>`, please make sure it is on the classpath.

#### CANNOT\_LOAD\_PROTOBUF\_CLASS

SQLSTATE: none assigned

Could not load Protobuf class with name `<protobufClassName>`. `<explanation>`.

#### CANNOT\_MERGE\_INCOMPATIBLE\_DATA\_TYPE

SQLSTATE: 42825

Failed to merge incompatible data types `<left>` and `<right>`. Please check the data types of the columns being merged and ensure that they are compatible. If necessary, consider casting the columns to compatible data types before attempting the merge.

#### CANNOT\_MERGE\_SCHEMAS

SQLSTATE: 42KD9

Failed merging schemas: Initial schema: `<left>` Schema that cannot be merged with the initial schema: `<right>`.

#### CANNOT\_MODIFY\_CONFIG

SQLSTATE: 46110

Cannot modify the value of the Spark config: `<key>`. See also ‘`<docroot>`/sql-migration-guide.html#ddl-statements’.

#### CANNOT\_PARSE\_DECIMAL

SQLSTATE: 22018

Cannot parse decimal. Please ensure that the input is a valid number with optional decimal point or comma separators.

#### CANNOT\_PARSE\_INTERVAL

SQLSTATE: none assigned

Unable to parse `<intervalString>`. Please ensure that the value provided is in a valid format for defining an interval. You can reference the documentation for the correct format. If the issue persists, please double check that the input value is not null or empty and try again.

#### CANNOT\_PARSE\_JSON\_FIELD

SQLSTATE: 2203G

Cannot parse the field name `<fieldName>` and the value `<fieldValue>` of the JSON token type `<jsonType>` to target Spark data type `<dataType>`.

#### CANNOT\_PARSE\_PROTOBUF\_DESCRIPTOR

SQLSTATE: none assigned

Error parsing descriptor bytes into Protobuf FileDescriptorSet.

#### CANNOT\_PARSE\_TIMESTAMP

SQLSTATE: 22007

`<message>`. If necessary set `<ansiConfig>` to “false” to bypass this error.

#### CANNOT\_READ\_FILE\_FOOTER

SQLSTATE: none assigned

Could not read footer for file: `<file>`. Please ensure that the file is in either ORC or Parquet format. If not, please convert it to a valid format. If the file is in the valid format, please check if it is corrupt. If it is, you can choose to either ignore it or fix the corruption.

#### CANNOT\_RECOGNIZE\_HIVE\_TYPE

SQLSTATE: 429BB

Cannot recognize hive type string: `<fieldType>`, column: `<fieldName>`. The specified data type for the field cannot be recognized by Spark SQL. Please check the data type of the specified field and ensure that it is a valid Spark SQL data type. Refer to the Spark SQL documentation for a list of valid data types and their format. If the data type is correct, please ensure that you are using a supported version of Spark SQL.

#### CANNOT\_RENAME\_ACROSS\_SCHEMA

SQLSTATE: 0AKD0

Renaming a `<type>` across schemas is not allowed.

#### CANNOT\_RESOLVE\_STAR\_EXPAND

SQLSTATE: none assigned

Cannot resolve `<targetString>`.\* given input columns `<columns>`. Please check that the specified table or struct exists and is accessible in the input columns.

#### CANNOT\_RESTORE\_PERMISSIONS\_FOR\_PATH

SQLSTATE: none assigned

Failed to set permissions on created path `<path>` back to `<permission>`.

#### CANNOT\_UPDATE\_FIELD

SQLSTATE: none assigned

Cannot update `<table>` field `<fieldName>` type:

For more details see CANNOT\_UPDATE\_FIELD

#### CANNOT\_UP\_CAST\_DATATYPE

SQLSTATE: none assigned

Cannot up cast `<expression>` from `<sourceType>` to `<targetType>`. `<details>`

#### CAST\_INVALID\_INPUT

SQLSTATE: 22018

The value `<expression>` of the type `<sourceType>` cannot be cast to `<targetType>` because it is malformed. Correct the value as per the syntax, or change its target type. Use `try_cast` to tolerate malformed input and return NULL instead. If necessary set `<ansiConfig>` to “false” to bypass this error.

#### CAST\_OVERFLOW

SQLSTATE: 22003

The value `<value>` of the type `<sourceType>` cannot be cast to `<targetType>` due to an overflow. Use `try_cast` to tolerate overflow and return NULL instead. If necessary set `<ansiConfig>` to “false” to bypass this error.

#### CAST\_OVERFLOW\_IN\_TABLE\_INSERT

SQLSTATE: 22003

Fail to insert a value of `<sourceType>` type into the `<targetType>` type column `<columnName>` due to an overflow. Use `try_cast` on the input value to tolerate overflow and return NULL instead.

#### CODEC\_NOT\_AVAILABLE

SQLSTATE: none assigned

The codec `<codecName>` is not available. Consider to set the config `<configKey>` to `<configVal>`.

#### CODEC\_SHORT\_NAME\_NOT\_FOUND

SQLSTATE: none assigned

Cannot find a short name for the codec `<codecName>`.

#### COLUMN\_ALIASES\_IS\_NOT\_ALLOWED

SQLSTATE: none assigned

Columns aliases are not allowed in `<op>`.

#### COLUMN\_ALREADY\_EXISTS

SQLSTATE: 42711

The column `<columnName>` already exists. Consider to choose another name or rename the existing column.

#### COLUMN\_NOT\_DEFINED\_IN\_TABLE

SQLSTATE: none assigned

`<colType>` column `<colName>` is not defined in table `<tableName>`, defined table columns are: `<tableCols>`.

#### COLUMN\_NOT\_FOUND

SQLSTATE: 42703

The column `<colName>` cannot be found. Verify the spelling and correctness of the column name according to the SQL config `<caseSensitiveConfig>`.

#### COMPARATOR\_RETURNS\_NULL

SQLSTATE: none assigned

The comparator has returned a NULL for a comparison between `<firstValue>` and `<secondValue>`. It should return a positive integer for “greater than”, 0 for “equal” and a negative integer for “less than”. To revert to deprecated behavior where NULL is treated as 0 (equal), you must set “spark.sql.legacy.allowNullComparisonResultInArraySort” to “true”.

#### CONCURRENT\_QUERY

SQLSTATE: none assigned

Another instance of this query was just started by a concurrent session.

#### CONCURRENT\_STREAM\_LOG\_UPDATE

SQLSTATE: 40000

Concurrent update to the log. Multiple streaming jobs detected for `<batchId>`. Please make sure only one streaming job runs on a specific checkpoint location at a time.

#### CONNECT

SQLSTATE: none assigned

Generic Spark Connect error.

For more details see CONNECT

#### CONVERSION\_INVALID\_INPUT

SQLSTATE: 22018

The value `<str>` (`<fmt>`) cannot be converted to `<targetType>` because it is malformed. Correct the value as per the syntax, or change its format. Use `<suggestion>` to tolerate malformed input and return NULL instead.

#### CREATE\_PERMANENT\_VIEW\_WITHOUT\_ALIAS

SQLSTATE: none assigned

Not allowed to create the permanent view `<name>` without explicitly assigning an alias for the expression `<attr>`.

#### CREATE\_TABLE\_COLUMN\_DESCRIPTOR\_DUPLICATE

SQLSTATE: 42710

CREATE TABLE column `<columnName>` specifies descriptor “`<optionName>`” more than once, which is invalid.

#### CREATE\_VIEW\_COLUMN\_ARITY\_MISMATCH

SQLSTATE: 21S01

Cannot create view `<viewName>`, the reason is

For more details see CREATE\_VIEW\_COLUMN\_ARITY\_MISMATCH

#### DATATYPE\_MISMATCH

SQLSTATE: 42K09

Cannot resolve `<sqlExpr>` due to data type mismatch:

For more details see DATATYPE\_MISMATCH

#### DATATYPE\_MISSING\_SIZE

SQLSTATE: 42K01

DataType `<type>` requires a length parameter, for example `<type>`(10). Please specify the length.

#### DATA\_SOURCE\_NOT\_FOUND

SQLSTATE: 42K02

Failed to find the data source: `<provider>`. Please find packages at `https://spark.apache.org/third-party-projects.html`.

#### DATETIME\_OVERFLOW

SQLSTATE: 22008

Datetime operation overflow: `<operation>`.

#### DECIMAL\_PRECISION\_EXCEEDS\_MAX\_PRECISION

SQLSTATE: 22003

Decimal precision `<precision>` exceeds max precision `<maxPrecision>`.

#### DEFAULT\_DATABASE\_NOT\_EXISTS

SQLSTATE: 42704

Default database `<defaultDatabase>` does not exist, please create it first or change default database to `<defaultDatabase>`.

#### DISTINCT\_WINDOW\_FUNCTION\_UNSUPPORTED

SQLSTATE: none assigned

Distinct window functions are not supported: `<windowExpr>`.

#### DIVIDE\_BY\_ZERO

SQLSTATE: 22012

Division by zero. Use `try_divide` to tolerate divisor being 0 and return NULL instead. If necessary set `<config>` to “false” to bypass this error.

#### DUPLICATED\_FIELD\_NAME\_IN\_ARROW\_STRUCT

SQLSTATE: none assigned

Duplicated field names in Arrow Struct are not allowed, got `<fieldNames>`.

#### DUPLICATED\_MAP\_KEY

SQLSTATE: 23505

Duplicate map key `<key>` was found, please check the input data. If you want to remove the duplicated keys, you can set `<mapKeyDedupPolicy>` to “LAST\_WIN” so that the key inserted at last takes precedence.

#### DUPLICATED\_METRICS\_NAME

SQLSTATE: none assigned

The metric name is not unique: `<metricName>`. The same name cannot be used for metrics with different results. However multiple instances of metrics with with same result and name are allowed (e.g. self-joins).

#### DUPLICATE\_CLAUSES

SQLSTATE: none assigned

Found duplicate clauses: `<clauseName>`. Please, remove one of them.

#### DUPLICATE\_KEY

SQLSTATE: 23505

Found duplicate keys `<keyColumn>`.

#### DUPLICATE\_ROUTINE\_PARAMETER\_ASSIGNMENT

SQLSTATE: 4274K

Call to function `<functionName>` is invalid because it includes multiple argument assignments to the same parameter name `<parameterName>`.

For more details see DUPLICATE\_ROUTINE\_PARAMETER\_ASSIGNMENT

#### EMPTY\_JSON\_FIELD\_VALUE

SQLSTATE: 42604

Failed to parse an empty string for data type `<dataType>`.

#### ENCODER\_NOT\_FOUND

SQLSTATE: none assigned

Not found an encoder of the type `<typeName>` to Spark SQL internal representation. Consider to change the input type to one of supported at ‘`<docroot>`/sql-ref-datatypes.html’.

#### EVENT\_TIME\_IS\_NOT\_ON\_TIMESTAMP\_TYPE

SQLSTATE: none assigned

The event time `<eventName>` has the invalid type `<eventType>`, but expected “TIMESTAMP”.

#### EXCEED\_LIMIT\_LENGTH

SQLSTATE: none assigned

Exceeds char/varchar type length limitation: `<limit>`.

#### EXPRESSION\_TYPE\_IS\_NOT\_ORDERABLE

SQLSTATE: none assigned

Column expression `<expr>` cannot be sorted because its type `<exprType>` is not orderable.

#### FAILED\_EXECUTE\_UDF

SQLSTATE: 39000

Failed to execute user defined function (`<functionName>`: (`<signature>`) => `<result>`).

#### FAILED\_FUNCTION\_CALL

SQLSTATE: 38000

Failed preparing of the function `<funcName>` for call. Please, double check function’s arguments.

#### FAILED\_PARSE\_STRUCT\_TYPE

SQLSTATE: 22018

Failed parsing struct: `<raw>`.

#### FAILED\_RENAME\_PATH

SQLSTATE: 42K04

Failed to rename `<sourcePath>` to `<targetPath>` as destination already exists.

#### FAILED\_RENAME\_TEMP\_FILE

SQLSTATE: none assigned

Failed to rename temp file `<srcPath>` to `<dstPath>` as FileSystem.rename returned false.

#### FIELDS\_ALREADY\_EXISTS

SQLSTATE: none assigned

Cannot `<op>` column, because `<fieldNames>` already exists in `<struct>`.

#### FIELD\_NOT\_FOUND

SQLSTATE: 42704

No such struct field `<fieldName>` in `<fields>`.

#### FORBIDDEN\_OPERATION

SQLSTATE: 42809

The operation `<statement>` is not allowed on the `<objectType>`: `<objectName>`.

#### GENERATED\_COLUMN\_WITH\_DEFAULT\_VALUE

SQLSTATE: none assigned

A column cannot have both a default value and a generation expression but column `<colName>` has default value: (`<defaultValue>`) and generation expression: (`<genExpr>`).

#### GRAPHITE\_SINK\_INVALID\_PROTOCOL

SQLSTATE: none assigned

Invalid Graphite protocol: `<protocol>`.

#### GRAPHITE\_SINK\_PROPERTY\_MISSING

SQLSTATE: none assigned

Graphite sink requires ‘`<property>`’ property.

#### GROUPING\_COLUMN\_MISMATCH

SQLSTATE: 42803

Column of grouping (`<grouping>`) can’t be found in grouping columns `<groupingColumns>`.

#### GROUPING\_ID\_COLUMN\_MISMATCH

SQLSTATE: 42803

Columns of grouping\_id (`<groupingIdColumn>`) does not match grouping columns (`<groupByColumns>`).

#### GROUPING\_SIZE\_LIMIT\_EXCEEDED

SQLSTATE: 54000

Grouping sets size cannot be greater than `<maxSize>`.

#### GROUP\_BY\_AGGREGATE

SQLSTATE: 42903

Aggregate functions are not allowed in GROUP BY, but found `<sqlExpr>`.

#### GROUP\_BY\_POS\_AGGREGATE

SQLSTATE: 42903

GROUP BY `<index>` refers to an expression `<aggExpr>` that contains an aggregate function. Aggregate functions are not allowed in GROUP BY.

#### GROUP\_BY\_POS\_OUT\_OF\_RANGE

SQLSTATE: 42805

GROUP BY position `<index>` is not in select list (valid range is \[1, `<size>`]).

#### GROUP\_EXPRESSION\_TYPE\_IS\_NOT\_ORDERABLE

SQLSTATE: none assigned

The expression `<sqlExpr>` cannot be used as a grouping expression because its data type `<dataType>` is not an orderable data type.

#### HLL\_INVALID\_INPUT\_SKETCH\_BUFFER

SQLSTATE: none assigned

Invalid call to `<function>`; only valid HLL sketch buffers are supported as inputs (such as those produced by the `hll_sketch_agg` function).

#### HLL\_INVALID\_LG\_K

SQLSTATE: none assigned

Invalid call to `<function>`; the `lgConfigK` value must be between `<min>` and `<max>`, inclusive: `<value>`.

#### HLL\_UNION\_DIFFERENT\_LG\_K

SQLSTATE: none assigned

Sketches have different `lgConfigK` values: `<left>` and `<right>`. Set the `allowDifferentLgConfigK` parameter to true to call `<function>` with different `lgConfigK` values.

#### IDENTIFIER\_TOO\_MANY\_NAME\_PARTS

SQLSTATE: 42601

`<identifier>` is not a valid identifier as it has more than 2 name parts.

#### INCOMPARABLE\_PIVOT\_COLUMN

SQLSTATE: 42818

Invalid pivot column `<columnName>`. Pivot columns must be comparable.

#### INCOMPATIBLE\_COLUMN\_TYPE

SQLSTATE: 42825

`<operator>` can only be performed on tables with compatible column types. The `<columnOrdinalNumber>` column of the `<tableOrdinalNumber>` table is `<dataType1>` type which is not compatible with `<dataType2>` at the same column of the first table.`<hint>`.

#### INCOMPATIBLE\_DATASOURCE\_REGISTER

SQLSTATE: none assigned

Detected an incompatible DataSourceRegister. Please remove the incompatible library from classpath or upgrade it. Error: `<message>`

#### INCOMPATIBLE\_DATA\_FOR\_TABLE

SQLSTATE: KD000

Cannot write incompatible data for the table `<tableName>`:

For more details see INCOMPATIBLE\_DATA\_FOR\_TABLE

#### INCOMPATIBLE\_JOIN\_TYPES

SQLSTATE: 42613

The join types `<joinType1>` and `<joinType2>` are incompatible.

#### INCOMPATIBLE\_VIEW\_SCHEMA\_CHANGE

SQLSTATE: none assigned

The SQL query of view `<viewName>` has an incompatible schema change and column `<colName>` cannot be resolved. Expected `<expectedNum>` columns named `<colName>` but got `<actualCols>`. Please try to re-create the view by running: `<suggestion>`.

#### INCOMPLETE\_TYPE\_DEFINITION

SQLSTATE: 42K01

Incomplete complex type:

For more details see INCOMPLETE\_TYPE\_DEFINITION

#### INCONSISTENT\_BEHAVIOR\_CROSS\_VERSION

SQLSTATE: 42K0B

You may get a different result due to the upgrading to

For more details see INCONSISTENT\_BEHAVIOR\_CROSS\_VERSION

#### INCORRECT\_END\_OFFSET

SQLSTATE: 22003

Max offset with `<rowsPerSecond>` rowsPerSecond is `<maxSeconds>`, but it’s `<endSeconds>` now.

#### INCORRECT\_RAMP\_UP\_RATE

SQLSTATE: 22003

Max offset with `<rowsPerSecond>` rowsPerSecond is `<maxSeconds>`, but ‘rampUpTimeSeconds’ is `<rampUpTimeSeconds>`.

#### INDEX\_ALREADY\_EXISTS

SQLSTATE: 42710

Cannot create the index `<indexName>` on table `<tableName>` because it already exists.

#### INDEX\_NOT\_FOUND

SQLSTATE: 42704

Cannot find the index `<indexName>` on table `<tableName>`.

#### INSERT\_COLUMN\_ARITY\_MISMATCH

SQLSTATE: 21S01

Cannot write to `<tableName>`, the reason is

For more details see INSERT\_COLUMN\_ARITY\_MISMATCH

#### INSERT\_PARTITION\_COLUMN\_ARITY\_MISMATCH

SQLSTATE: 21S01

Cannot write to ‘`<tableName>`’, `<reason>`: Table columns: `<tableColumns>`. Partition columns with static values: `<staticPartCols>`. Data columns: `<dataColumns>`.

#### INSUFFICIENT\_TABLE\_PROPERTY

SQLSTATE: none assigned

Can’t find table property:

For more details see INSUFFICIENT\_TABLE\_PROPERTY

#### INTERNAL\_ERROR

SQLSTATE: XX000

`<message>`

#### INTERNAL\_ERROR\_BROADCAST

SQLSTATE: XX000

`<message>`

#### INTERNAL\_ERROR\_EXECUTOR

SQLSTATE: XX000

`<message>`

#### INTERNAL\_ERROR\_MEMORY

SQLSTATE: XX000

`<message>`

#### INTERNAL\_ERROR\_NETWORK

SQLSTATE: XX000

`<message>`

#### INTERNAL\_ERROR\_SHUFFLE

SQLSTATE: XX000

`<message>`

#### INTERNAL\_ERROR\_STORAGE

SQLSTATE: XX000

`<message>`

#### INTERVAL\_ARITHMETIC\_OVERFLOW

SQLSTATE: 22015

`<message>`.`<alternative>`

#### INTERVAL\_DIVIDED\_BY\_ZERO

SQLSTATE: 22012

Division by zero. Use `try_divide` to tolerate divisor being 0 and return NULL instead.

#### INVALID\_ARRAY\_INDEX

SQLSTATE: 22003

The index `<indexValue>` is out of bounds. The array has `<arraySize>` elements. Use the SQL function `get()` to tolerate accessing element at invalid index and return NULL instead. If necessary set `<ansiConfig>` to “false” to bypass this error.

#### INVALID\_ARRAY\_INDEX\_IN\_ELEMENT\_AT

SQLSTATE: 22003

The index `<indexValue>` is out of bounds. The array has `<arraySize>` elements. Use `try_element_at` to tolerate accessing element at invalid index and return NULL instead. If necessary set `<ansiConfig>` to “false” to bypass this error.

#### INVALID\_BITMAP\_POSITION

SQLSTATE: 22003

The 0-indexed bitmap position `<bitPosition>` is out of bounds. The bitmap has `<bitmapNumBits>` bits (`<bitmapNumBytes>` bytes).

#### INVALID\_BOUNDARY

SQLSTATE: none assigned

The boundary `<boundary>` is invalid: `<invalidValue>`.

For more details see INVALID\_BOUNDARY

#### INVALID\_BUCKET\_FILE

SQLSTATE: none assigned

Invalid bucket file: `<path>`.

#### INVALID\_BYTE\_STRING

SQLSTATE: none assigned

The expected format is ByteString, but was `<unsupported>` (`<class>`).

#### INVALID\_COLUMN\_NAME\_AS\_PATH

SQLSTATE: 46121

The datasource `<datasource>` cannot save the column `<columnName>` because its name contains some characters that are not allowed in file paths. Please, use an alias to rename it.

#### INVALID\_COLUMN\_OR\_FIELD\_DATA\_TYPE

SQLSTATE: 42000

Column or field `<name>` is of type `<type>` while it’s required to be `<expectedType>`.

#### INVALID\_CURSOR

SQLSTATE: HY109

The cursor is invalid.

For more details see INVALID\_CURSOR

#### INVALID\_DEFAULT\_VALUE

SQLSTATE: none assigned

Failed to execute `<statement>` command because the destination table column `<colName>` has a DEFAULT value `<defaultValue>`,

For more details see INVALID\_DEFAULT\_VALUE

#### INVALID\_DRIVER\_MEMORY

SQLSTATE: F0000

System memory `<systemMemory>` must be at least `<minSystemMemory>`. Please increase heap size using the –driver-memory option or “`<config>`” in Spark configuration.

#### INVALID\_EMPTY\_LOCATION

SQLSTATE: 42K05

The location name cannot be empty string, but `<location>` was given.

#### INVALID\_ESC

SQLSTATE: none assigned

Found an invalid escape string: `<invalidEscape>`. The escape string must contain only one character.

#### INVALID\_ESCAPE\_CHAR

SQLSTATE: none assigned

`EscapeChar` should be a string literal of length one, but got `<sqlExpr>`.

#### INVALID\_EXECUTOR\_MEMORY

SQLSTATE: F0000

Executor memory `<executorMemory>` must be at least `<minSystemMemory>`. Please increase executor memory using the –executor-memory option or “`<config>`” in Spark configuration.

#### INVALID\_EXTRACT\_BASE\_FIELD\_TYPE

SQLSTATE: 42000

Can’t extract a value from `<base>`. Need a complex type \[STRUCT, ARRAY, MAP] but got `<other>`.

#### INVALID\_EXTRACT\_FIELD

SQLSTATE: 42601

Cannot extract `<field>` from `<expr>`.

#### INVALID\_EXTRACT\_FIELD\_TYPE

SQLSTATE: 42000

Field name should be a non-null string literal, but it’s `<extraction>`.

#### INVALID\_FIELD\_NAME

SQLSTATE: 42000

Field name `<fieldName>` is invalid: `<path>` is not a struct.

#### INVALID\_FORMAT

SQLSTATE: 42601

The format is invalid: `<format>`.

For more details see INVALID\_FORMAT

#### INVALID\_FRACTION\_OF\_SECOND

SQLSTATE: 22023

The fraction of sec must be zero. Valid range is \[0, 60]. If necessary set `<ansiConfig>` to “false” to bypass this error.

#### INVALID\_HANDLE

SQLSTATE: HY000

The handle `<handle>` is invalid.

For more details see INVALID\_HANDLE

#### INVALID\_HIVE\_COLUMN\_NAME

SQLSTATE: none assigned

Cannot create the table `<tableName>` having the nested column `<columnName>` whose name contains invalid characters `<invalidChars>` in Hive metastore.

#### INVALID\_IDENTIFIER

SQLSTATE: 42602

The identifier `<ident>` is invalid. Please, consider quoting it with back-quotes as `<ident>`.

#### INVALID\_INDEX\_OF\_ZERO

SQLSTATE: 22003

The index 0 is invalid. An index shall be either `< 0 or >` 0 (the first element has index 1).

#### INVALID\_INLINE\_TABLE

SQLSTATE: none assigned

Invalid inline table.

For more details see INVALID\_INLINE\_TABLE

#### INVALID\_JSON\_ROOT\_FIELD

SQLSTATE: 22032

Cannot convert JSON root field to target Spark type.

#### INVALID\_JSON\_SCHEMA\_MAP\_TYPE

SQLSTATE: 22032

Input schema `<jsonSchema>` can only contain STRING as a key type for a MAP.

#### INVALID\_LAMBDA\_FUNCTION\_CALL

SQLSTATE: none assigned

Invalid lambda function call.

For more details see INVALID\_LAMBDA\_FUNCTION\_CALL

#### INVALID\_LATERAL\_JOIN\_TYPE

SQLSTATE: 42613

The `<joinType>` JOIN with LATERAL correlation is not allowed because an OUTER subquery cannot correlate to its join partner. Remove the LATERAL correlation or use an INNER JOIN, or LEFT OUTER JOIN instead.

#### INVALID\_LIMIT\_LIKE\_EXPRESSION

SQLSTATE: none assigned

The limit like expression `<expr>` is invalid.

For more details see INVALID\_LIMIT\_LIKE\_EXPRESSION

#### INVALID\_NON\_DETERMINISTIC\_EXPRESSIONS

SQLSTATE: none assigned

The operator expects a deterministic expression, but the actual expression is `<sqlExprs>`.

#### INVALID\_NUMERIC\_LITERAL\_RANGE

SQLSTATE: none assigned

Numeric literal `<rawStrippedQualifier>` is outside the valid range for `<typeName>` with minimum value of `<minValue>` and maximum value of `<maxValue>`. Please adjust the value accordingly.

#### INVALID\_OBSERVED\_METRICS

SQLSTATE: none assigned

Invalid observed metrics.

For more details see INVALID\_OBSERVED\_METRICS

#### INVALID\_OPTIONS

SQLSTATE: 42K06

Invalid options:

For more details see INVALID\_OPTIONS

#### INVALID\_PANDAS\_UDF\_PLACEMENT

SQLSTATE: 0A000

The group aggregate pandas UDF `<functionList>` cannot be invoked together with as other, non-pandas aggregate functions.

#### INVALID\_PARAMETER\_VALUE

SQLSTATE: 22023

The value of parameter(s) `<parameter>` in `<functionName>` is invalid:

For more details see INVALID\_PARAMETER\_VALUE

#### INVALID\_PARTITION\_OPERATION

SQLSTATE: none assigned

The partition command is invalid.

For more details see INVALID\_PARTITION\_OPERATION

#### INVALID\_PROPERTY\_KEY

SQLSTATE: 42602

`<key>` is an invalid property key, please use quotes, e.g. SET `<key>`=`<value>`.

#### INVALID\_PROPERTY\_VALUE

SQLSTATE: 42602

`<value>` is an invalid property value, please use quotes, e.g. SET `<key>`=`<value>`

#### INVALID\_SCHEMA

SQLSTATE: 42K07

The input schema `<inputSchema>` is not a valid schema string.

For more details see INVALID\_SCHEMA

#### INVALID\_SET\_SYNTAX

SQLSTATE: 42000

Expected format is ‘SET’, ‘SET key’, or ‘SET key=value’. If you want to include special characters in key, or include semicolon in value, please use backquotes, e.g., SET `key`=`value`.

#### INVALID\_SQL\_ARG

SQLSTATE: none assigned

The argument `<name>` of `sql()` is invalid. Consider to replace it by a SQL literal.

#### INVALID\_SQL\_SYNTAX

SQLSTATE: 42000

Invalid SQL syntax:

For more details see INVALID\_SQL\_SYNTAX

#### INVALID\_SUBQUERY\_EXPRESSION

SQLSTATE: 42823

Invalid subquery:

For more details see INVALID\_SUBQUERY\_EXPRESSION

#### INVALID\_TEMP\_OBJ\_REFERENCE

SQLSTATE: none assigned

Cannot create the persistent object `<objName>` of the type `<obj>` because it references to the temporary object `<tempObjName>` of the type `<tempObj>`. Please make the temporary object `<tempObjName>` persistent, or make the persistent object `<objName>` temporary.

#### INVALID\_TIME\_TRAVEL\_TIMESTAMP\_EXPR

SQLSTATE: none assigned

The time travel timestamp expression `<expr>` is invalid.

For more details see INVALID\_TIME\_TRAVEL\_TIMESTAMP\_EXPR

#### INVALID\_TYPED\_LITERAL

SQLSTATE: 42604

The value of the typed literal `<valueType>` is invalid: `<value>`.

#### INVALID\_UDF\_IMPLEMENTATION

SQLSTATE: none assigned

Function `<funcName>` does not implement ScalarFunction or AggregateFunction.

#### INVALID\_URL

SQLSTATE: none assigned

The url is invalid: `<url>`. If necessary set `<ansiConfig>` to “false” to bypass this error.

#### INVALID\_USAGE\_OF\_STAR\_OR\_REGEX

SQLSTATE: 42000

Invalid usage of `<elem>` in `<prettyName>`.

#### INVALID\_VIEW\_TEXT

SQLSTATE: none assigned

The view `<viewName>` cannot be displayed due to invalid view text: `<viewText>`. This may be caused by an unauthorized modification of the view or an incorrect query syntax. Please check your query syntax and verify that the view has not been tampered with.

#### INVALID\_WHERE\_CONDITION

SQLSTATE: 42903

The WHERE condition `<condition>` contains invalid expressions: `<expressionList>`. Rewrite the query to avoid window functions, aggregate functions, and generator functions in the WHERE clause.

#### INVALID\_WINDOW\_SPEC\_FOR\_AGGREGATION\_FUNC

SQLSTATE: none assigned

Cannot specify ORDER BY or a window frame for `<aggFunc>`.

#### INVALID\_WRITE\_DISTRIBUTION

SQLSTATE: none assigned

The requested write distribution is invalid.

For more details see INVALID\_WRITE\_DISTRIBUTION

#### JOIN\_CONDITION\_IS\_NOT\_BOOLEAN\_TYPE

SQLSTATE: none assigned

The join condition `<joinCondition>` has the invalid type `<conditionType>`, expected “BOOLEAN”.

#### LOAD\_DATA\_PATH\_NOT\_EXISTS

SQLSTATE: none assigned

LOAD DATA input path does not exist: `<path>`.

#### LOCAL\_MUST\_WITH\_SCHEMA\_FILE

SQLSTATE: none assigned

LOCAL must be used together with the schema of `file`, but got: `<actualSchema>`.

#### LOCATION\_ALREADY\_EXISTS

SQLSTATE: 42710

Cannot name the managed table as `<identifier>`, as its associated location `<location>` already exists. Please pick a different table name, or remove the existing location first.

#### MALFORMED\_CSV\_RECORD

SQLSTATE: none assigned

Malformed CSV record: `<badRecord>`

#### MALFORMED\_PROTOBUF\_MESSAGE

SQLSTATE: none assigned

Malformed Protobuf messages are detected in message deserialization. Parse Mode: `<failFastMode>`. To process malformed protobuf message as null result, try setting the option ‘mode’ as ‘PERMISSIVE’.

#### MALFORMED\_RECORD\_IN\_PARSING

SQLSTATE: 22023

Malformed records are detected in record parsing: `<badRecord>`. Parse Mode: `<failFastMode>`. To process malformed records as null result, try setting the option ‘mode’ as ‘PERMISSIVE’.

For more details see MALFORMED\_RECORD\_IN\_PARSING

#### MERGE\_CARDINALITY\_VIOLATION

SQLSTATE: 23K01

The ON search condition of the MERGE statement matched a single row from the target table with multiple rows of the source table. This could result in the target row being operated on more than once with an update or delete operation and is not allowed.

#### MISSING\_AGGREGATION

SQLSTATE: 42803

The non-aggregating expression `<expression>` is based on columns which are not participating in the GROUP BY clause. Add the columns or the expression to the GROUP BY, aggregate the expression, or use `<expressionAnyValue>` if you do not care which of the values within a group is returned.

#### MISSING\_ATTRIBUTES

SQLSTATE: none assigned

Resolved attribute(s) `<missingAttributes>` missing from `<input>` in operator `<operator>`.

For more details see MISSING\_ATTRIBUTES

#### MISSING\_GROUP\_BY

SQLSTATE: 42803

The query does not include a GROUP BY clause. Add GROUP BY or turn it into the window functions using OVER clauses.

#### MULTI\_SOURCES\_UNSUPPORTED\_FOR\_EXPRESSION

SQLSTATE: none assigned

The expression `<expr>` does not support more than one source.

#### MULTI\_UDF\_INTERFACE\_ERROR

SQLSTATE: none assigned

Not allowed to implement multiple UDF interfaces, UDF class `<className>`.

#### NAMED\_PARAMETERS\_NOT\_SUPPORTED

SQLSTATE: 4274K

Named parameters are not supported for function `<functionName>`; please retry the query with positional arguments to the function call instead.

#### NAMED\_PARAMETER\_SUPPORT\_DISABLED

SQLSTATE: none assigned

Cannot call function `<functionName>` because named argument references are not enabled here. In this case, the named argument reference was `<argument>`. Set “spark.sql.allowNamedFunctionArguments” to “true” to turn on feature.

#### NESTED\_AGGREGATE\_FUNCTION

SQLSTATE: 42607

It is not allowed to use an aggregate function in the argument of another aggregate function. Please use the inner aggregate function in a sub-query.

#### NON\_LAST\_MATCHED\_CLAUSE\_OMIT\_CONDITION

SQLSTATE: 42613

When there are more than one MATCHED clauses in a MERGE statement, only the last MATCHED clause can omit the condition.

#### NON\_LAST\_NOT\_MATCHED\_BY\_SOURCE\_CLAUSE\_OMIT\_CONDITION

SQLSTATE: 42613

When there are more than one NOT MATCHED BY SOURCE clauses in a MERGE statement, only the last NOT MATCHED BY SOURCE clause can omit the condition.

#### NON\_LAST\_NOT\_MATCHED\_BY\_TARGET\_CLAUSE\_OMIT\_CONDITION

SQLSTATE: 42613

When there are more than one NOT MATCHED \[BY TARGET] clauses in a MERGE statement, only the last NOT MATCHED \[BY TARGET] clause can omit the condition.

#### NON\_LITERAL\_PIVOT\_VALUES

SQLSTATE: 42K08

Literal expressions required for pivot values, found `<expression>`.

#### NON\_PARTITION\_COLUMN

SQLSTATE: 42000

PARTITION clause cannot contain the non-partition column: `<columnName>`.

#### NON\_TIME\_WINDOW\_NOT\_SUPPORTED\_IN\_STREAMING

SQLSTATE: none assigned

Window function is not supported in `<windowFunc>` (as column `<columnName>`) on streaming DataFrames/Datasets. Structured Streaming only supports time-window aggregation using the WINDOW function. (window specification: `<windowSpec>`)

#### NOT\_ALLOWED\_IN\_FROM

SQLSTATE: none assigned

Not allowed in the FROM clause:

For more details see NOT\_ALLOWED\_IN\_FROM

#### NOT\_A\_CONSTANT\_STRING

SQLSTATE: 42601

The expression `<expr>` used for the routine or clause `<name>` must be a constant STRING which is NOT NULL.

For more details see NOT\_A\_CONSTANT\_STRING

#### NOT\_A\_PARTITIONED\_TABLE

SQLSTATE: none assigned

Operation `<operation>` is not allowed for `<tableIdentWithDB>` because it is not a partitioned table.

#### NOT\_NULL\_CONSTRAINT\_VIOLATION

SQLSTATE: 42000

Assigning a NULL is not allowed here.

For more details see NOT\_NULL\_CONSTRAINT\_VIOLATION

#### NOT\_SUPPORTED\_CHANGE\_COLUMN

SQLSTATE: none assigned

ALTER TABLE ALTER/CHANGE COLUMN is not supported for changing `<table>`’s column `<originName>` with type `<originType>` to `<newName>` with type `<newType>`.

#### NOT\_SUPPORTED\_COMMAND\_FOR\_V2\_TABLE

SQLSTATE: 46110

`<cmd>` is not supported for v2 tables.

#### NOT\_SUPPORTED\_COMMAND\_WITHOUT\_HIVE\_SUPPORT

SQLSTATE: none assigned

`<cmd>` is not supported, if you want to enable it, please set “spark.sql.catalogImplementation” to “hive”.

#### NOT\_SUPPORTED\_IN\_JDBC\_CATALOG

SQLSTATE: 46110

Not supported command in JDBC catalog:

For more details see NOT\_SUPPORTED\_IN\_JDBC\_CATALOG

#### NO\_DEFAULT\_COLUMN\_VALUE\_AVAILABLE

SQLSTATE: 42608

Can’t determine the default value for `<colName>` since it is not nullable and it has no default value.

#### NO\_HANDLER\_FOR\_UDAF

SQLSTATE: none assigned

No handler for UDAF ‘`<functionName>`’. Use sparkSession.udf.register(…) instead.

#### NO\_SQL\_TYPE\_IN\_PROTOBUF\_SCHEMA

SQLSTATE: none assigned

Cannot find `<catalystFieldPath>` in Protobuf schema.

#### NO\_UDF\_INTERFACE

SQLSTATE: none assigned

UDF class `<className>` doesn’t implement any UDF interface.

#### NULLABLE\_COLUMN\_OR\_FIELD

SQLSTATE: 42000

Column or field `<name>` is nullable while it’s required to be non-nullable.

#### NULLABLE\_ROW\_ID\_ATTRIBUTES

SQLSTATE: 42000

Row ID attributes cannot be nullable: `<nullableRowIdAttrs>`.

#### NULL\_MAP\_KEY

SQLSTATE: 2200E

Cannot use null as map key.

#### NUMERIC\_OUT\_OF\_SUPPORTED\_RANGE

SQLSTATE: 22003

The value `<value>` cannot be interpreted as a numeric since it has more than 38 digits.

#### NUMERIC\_VALUE\_OUT\_OF\_RANGE

SQLSTATE: 22003

`<value>` cannot be represented as Decimal(`<precision>`, `<scale>`). If necessary set `<config>` to “false” to bypass this error, and return NULL instead.

#### NUM\_COLUMNS\_MISMATCH

SQLSTATE: 42826

`<operator>` can only be performed on inputs with the same number of columns, but the first input has `<firstNumColumns>` columns and the `<invalidOrdinalNum>` input has `<invalidNumColumns>` columns.

#### NUM\_TABLE\_VALUE\_ALIASES\_MISMATCH

SQLSTATE: none assigned

Number of given aliases does not match number of output columns. Function name: `<funcName>`; number of aliases: `<aliasesNum>`; number of output columns: `<outColsNum>`.

#### OPERATION\_CANCELED

SQLSTATE: HY008

Operation has been canceled.

#### ORDER\_BY\_POS\_OUT\_OF\_RANGE

SQLSTATE: 42805

ORDER BY position `<index>` is not in select list (valid range is \[1, `<size>`]).

#### PARSE\_EMPTY\_STATEMENT

SQLSTATE: 42617

Syntax error, unexpected empty statement.

#### PARSE\_SYNTAX\_ERROR

SQLSTATE: 42601

Syntax error at or near `<error><hint>`.

#### PARTITIONS\_ALREADY\_EXIST

SQLSTATE: 428FT

Cannot ADD or RENAME TO partition(s) `<partitionList>` in table `<tableName>` because they already exist. Choose a different name, drop the existing partition, or add the IF NOT EXISTS clause to tolerate a pre-existing partition.

#### PARTITIONS\_NOT\_FOUND

SQLSTATE: 428FT

The partition(s) `<partitionList>` cannot be found in table `<tableName>`. Verify the partition specification and table name. To tolerate the error on drop use ALTER TABLE … DROP IF EXISTS PARTITION.

#### PATH\_ALREADY\_EXISTS

SQLSTATE: 42K04

Path `<outputPath>` already exists. Set mode as “overwrite” to overwrite the existing path.

#### PATH\_NOT\_FOUND

SQLSTATE: 42K03

Path does not exist: `<path>`.

#### PIVOT\_VALUE\_DATA\_TYPE\_MISMATCH

SQLSTATE: 42K09

Invalid pivot value ‘`<value>`’: value data type `<valueType>` does not match pivot column data type `<pivotType>`.

#### PLAN\_VALIDATION\_FAILED\_RULE\_EXECUTOR

SQLSTATE: none assigned

The input plan of `<ruleExecutor>` is invalid: `<reason>`

#### PLAN\_VALIDATION\_FAILED\_RULE\_IN\_BATCH

SQLSTATE: none assigned

Rule `<rule>` in batch `<batch>` generated an invalid plan: `<reason>`

#### PROTOBUF\_DEPENDENCY\_NOT\_FOUND

SQLSTATE: none assigned

Could not find dependency: `<dependencyName>`.

#### PROTOBUF\_DESCRIPTOR\_FILE\_NOT\_FOUND

SQLSTATE: none assigned

Error reading Protobuf descriptor file at path: `<filePath>`.

#### PROTOBUF\_FIELD\_MISSING

SQLSTATE: none assigned

Searching for `<field>` in Protobuf schema at `<protobufSchema>` gave `<matchSize>` matches. Candidates: `<matches>`.

#### PROTOBUF\_FIELD\_MISSING\_IN\_SQL\_SCHEMA

SQLSTATE: none assigned

Found `<field>` in Protobuf schema but there is no match in the SQL schema.

#### PROTOBUF\_FIELD\_TYPE\_MISMATCH

SQLSTATE: none assigned

Type mismatch encountered for field: `<field>`.

#### PROTOBUF\_MESSAGE\_NOT\_FOUND

SQLSTATE: none assigned

Unable to locate Message `<messageName>` in Descriptor.

#### PROTOBUF\_TYPE\_NOT\_SUPPORT

SQLSTATE: none assigned

Protobuf type not yet supported: `<protobufType>`.

#### RECURSIVE\_PROTOBUF\_SCHEMA

SQLSTATE: none assigned

Found recursive reference in Protobuf schema, which can not be processed by Spark by default: `<fieldDescriptor>`. try setting the option `recursive.fields.max.depth` 0 to 10. Going beyond 10 levels of recursion is not allowed.

#### RECURSIVE\_VIEW

SQLSTATE: none assigned

Recursive view `<viewIdent>` detected (cycle: `<newPath>`).

#### REF\_DEFAULT\_VALUE\_IS\_NOT\_ALLOWED\_IN\_PARTITION

SQLSTATE: none assigned

References to DEFAULT column values are not allowed within the PARTITION clause.

#### RENAME\_SRC\_PATH\_NOT\_FOUND

SQLSTATE: 42K03

Failed to rename as `<sourcePath>` was not found.

#### REPEATED\_CLAUSE

SQLSTATE: 42614

The `<clause>` clause may be used at most once per `<operation>` operation.

#### REQUIRED\_PARAMETER\_NOT\_FOUND

SQLSTATE: 4274K

Cannot invoke function `<functionName>` because the parameter named `<parameterName>` is required, but the function call did not supply a value. Please update the function call to supply an argument value (either positionally at index `<index>` or by name) and retry the query again.

#### REQUIRES\_SINGLE\_PART\_NAMESPACE

SQLSTATE: 42K05

`<sessionCatalog>` requires a single-part namespace, but got `<namespace>`.

#### ROUTINE\_ALREADY\_EXISTS

SQLSTATE: 42723

Cannot create the function `<routineName>` because it already exists. Choose a different name, drop or replace the existing function, or add the IF NOT EXISTS clause to tolerate a pre-existing function.

#### ROUTINE\_NOT\_FOUND

SQLSTATE: 42883

The function `<routineName>` cannot be found. Verify the spelling and correctness of the schema and catalog. If you did not qualify the name with a schema and catalog, verify the current\_schema() output, or qualify the name with the correct schema and catalog. To tolerate the error on drop use DROP FUNCTION IF EXISTS.

#### RULE\_ID\_NOT\_FOUND

SQLSTATE: 22023

Not found an id for the rule name “`<ruleName>`”. Please modify RuleIdCollection.scala if you are adding a new rule.

#### SCALAR\_SUBQUERY\_IS\_IN\_GROUP\_BY\_OR\_AGGREGATE\_FUNCTION

SQLSTATE: none assigned

The correlated scalar subquery ‘`<sqlExpr>`’ is neither present in GROUP BY, nor in an aggregate function. Add it to GROUP BY using ordinal position or wrap it in `first()` (or `first_value`) if you don’t care which value you get.

#### SCALAR\_SUBQUERY\_TOO\_MANY\_ROWS

SQLSTATE: 21000

More than one row returned by a subquery used as an expression.

#### SCHEMA\_ALREADY\_EXISTS

SQLSTATE: 42P06

Cannot create schema `<schemaName>` because it already exists. Choose a different name, drop the existing schema, or add the IF NOT EXISTS clause to tolerate pre-existing schema.

#### SCHEMA\_NOT\_EMPTY

SQLSTATE: 2BP01

Cannot drop a schema `<schemaName>` because it contains objects. Use DROP SCHEMA … CASCADE to drop the schema and all its objects.

#### SCHEMA\_NOT\_FOUND

SQLSTATE: 42704

The schema `<schemaName>` cannot be found. Verify the spelling and correctness of the schema and catalog. If you did not qualify the name with a catalog, verify the current\_schema() output, or qualify the name with the correct catalog. To tolerate the error on drop use DROP SCHEMA IF EXISTS.

#### SECOND\_FUNCTION\_ARGUMENT\_NOT\_INTEGER

SQLSTATE: 22023

The second argument of `<functionName>` function needs to be an integer.

#### SEED\_EXPRESSION\_IS\_UNFOLDABLE

SQLSTATE: none assigned

The seed expression `<seedExpr>` of the expression `<exprWithSeed>` must be foldable.

#### SORT\_BY\_WITHOUT\_BUCKETING

SQLSTATE: none assigned

sortBy must be used together with bucketBy.

#### SPECIFY\_BUCKETING\_IS\_NOT\_ALLOWED

SQLSTATE: none assigned

Cannot specify bucketing information if the table schema is not specified when creating and will be inferred at runtime.

#### SPECIFY\_PARTITION\_IS\_NOT\_ALLOWED

SQLSTATE: none assigned

It is not allowed to specify partition columns when the table schema is not defined. When the table schema is not provided, schema and partition columns will be inferred.

#### SQL\_CONF\_NOT\_FOUND

SQLSTATE: none assigned

The SQL config `<sqlConf>` cannot be found. Please verify that the config exists.

#### STAR\_GROUP\_BY\_POS

SQLSTATE: 0A000

Star (\*) is not allowed in a select list when GROUP BY an ordinal position is used.

#### STATIC\_PARTITION\_COLUMN\_IN\_INSERT\_COLUMN\_LIST

SQLSTATE: none assigned

Static partition column `<staticName>` is also specified in the column list.

#### STREAM\_FAILED

SQLSTATE: none assigned

Query \[id = `<id>`, runId = `<runId>`] terminated with exception: `<message>`

#### SUM\_OF\_LIMIT\_AND\_OFFSET\_EXCEEDS\_MAX\_INT

SQLSTATE: none assigned

The sum of the LIMIT clause and the OFFSET clause must not be greater than the maximum 32-bit integer value (2,147,483,647) but found limit = `<limit>`, offset = `<offset>`.

#### TABLE\_OR\_VIEW\_ALREADY\_EXISTS

SQLSTATE: 42P07

Cannot create table or view `<relationName>` because it already exists. Choose a different name, drop or replace the existing object, or add the IF NOT EXISTS clause to tolerate pre-existing objects.

#### TABLE\_OR\_VIEW\_NOT\_FOUND

SQLSTATE: 42P01

The table or view `<relationName>` cannot be found. Verify the spelling and correctness of the schema and catalog. If you did not qualify the name with a schema, verify the current\_schema() output, or qualify the name with the correct schema and catalog. To tolerate the error on drop use DROP VIEW IF EXISTS or DROP TABLE IF EXISTS.

#### TABLE\_VALUED\_FUNCTION\_TOO\_MANY\_TABLE\_ARGUMENTS

SQLSTATE: none assigned

There are too many table arguments for table-valued function. It allows one table argument, but got: `<num>`. If you want to allow it, please set “spark.sql.allowMultipleTableArguments.enabled” to “true”

#### TASK\_WRITE\_FAILED

SQLSTATE: none assigned

Task failed while writing rows to `<path>`.

#### TEMP\_TABLE\_OR\_VIEW\_ALREADY\_EXISTS

SQLSTATE: 42P07

Cannot create the temporary view `<relationName>` because it already exists. Choose a different name, drop or replace the existing view, or add the IF NOT EXISTS clause to tolerate pre-existing views.

#### TEMP\_VIEW\_NAME\_TOO\_MANY\_NAME\_PARTS

SQLSTATE: 428EK

CREATE TEMPORARY VIEW or the corresponding Dataset APIs only accept single-part view names, but got: `<actualName>`.

#### TOO\_MANY\_ARRAY\_ELEMENTS

SQLSTATE: 54000

Cannot initialize array with `<numElements>` elements of size `<size>`.

#### UDTF\_ALIAS\_NUMBER\_MISMATCH

SQLSTATE: none assigned

The number of aliases supplied in the AS clause does not match the number of columns output by the UDTF. Expected `<aliasesSize>` aliases, but got `<aliasesNames>`. Please ensure that the number of aliases provided matches the number of columns output by the UDTF.

#### UNABLE\_TO\_ACQUIRE\_MEMORY

SQLSTATE: 53200

Unable to acquire `<requestedBytes>` bytes of memory, got `<receivedBytes>`.

#### UNABLE\_TO\_CONVERT\_TO\_PROTOBUF\_MESSAGE\_TYPE

SQLSTATE: none assigned

Unable to convert SQL type `<toType>` to Protobuf type `<protobufType>`.

#### UNABLE\_TO\_INFER\_SCHEMA

SQLSTATE: 42KD9

Unable to infer schema for `<format>`. It must be specified manually.

#### UNBOUND\_SQL\_PARAMETER

SQLSTATE: 42P02

Found the unbound parameter: `<name>`. Please, fix `args` and provide a mapping of the parameter to a SQL literal.

#### UNCLOSED\_BRACKETED\_COMMENT

SQLSTATE: 42601

Found an unclosed bracketed comment. Please, append \*/ at the end of the comment.

#### UNEXPECTED\_INPUT\_TYPE

SQLSTATE: 42K09

Parameter `<paramIndex>` of function `<functionName>` requires the `<requiredType>` type, however `<inputSql>` has the type `<inputType>`.

#### UNEXPECTED\_POSITIONAL\_ARGUMENT

SQLSTATE: 4274K

Cannot invoke function `<functionName>` because it contains positional argument(s) following the named argument assigned to `<parameterName>`; please rearrange them so the positional arguments come first and then retry the query again.

#### UNKNOWN\_PROTOBUF\_MESSAGE\_TYPE

SQLSTATE: none assigned

Attempting to treat `<descriptorName>` as a Message, but it was `<containingType>`.

#### UNPIVOT\_REQUIRES\_ATTRIBUTES

SQLSTATE: 42K0A

UNPIVOT requires all given `<given>` expressions to be columns when no `<empty>` expressions are given. These are not columns: \[`<expressions>`].

#### UNPIVOT\_REQUIRES\_VALUE\_COLUMNS

SQLSTATE: 42K0A

At least one value column needs to be specified for UNPIVOT, all columns specified as ids.

#### UNPIVOT\_VALUE\_DATA\_TYPE\_MISMATCH

SQLSTATE: 42K09

Unpivot value columns must share a least common type, some types do not: \[`<types>`].

#### UNPIVOT\_VALUE\_SIZE\_MISMATCH

SQLSTATE: 428C4

All unpivot value columns must have the same size as there are value column names (`<names>`).

#### UNRECOGNIZED\_PARAMETER\_NAME

SQLSTATE: 4274K

Cannot invoke function `<functionName>` because the function call included a named argument reference for the argument named `<argumentName>`, but this function does not include any signature containing an argument with this name. Did you mean one of the following? \[`<proposal>`].

#### UNRECOGNIZED\_SQL\_TYPE

SQLSTATE: 42704

Unrecognized SQL type - name: `<typeName>`, id: `<jdbcType>`.

#### UNRESOLVABLE\_TABLE\_VALUED\_FUNCTION

SQLSTATE: none assigned

Could not resolve `<name>` to a table-valued function. Please make sure that `<name>` is defined as a table-valued function and that all required parameters are provided correctly. If `<name>` is not defined, please create the table-valued function before using it. For more information about defining table-valued functions, please refer to the Apache Spark documentation.

#### UNRESOLVED\_ALL\_IN\_GROUP\_BY

SQLSTATE: 42803

Cannot infer grouping columns for GROUP BY ALL based on the select clause. Please explicitly specify the grouping columns.

#### UNRESOLVED\_COLUMN

SQLSTATE: 42703

A column or function parameter with name `<objectName>` cannot be resolved.

For more details see UNRESOLVED\_COLUMN

#### UNRESOLVED\_FIELD

SQLSTATE: 42703

A field with name `<fieldName>` cannot be resolved with the struct-type column `<columnPath>`.

For more details see UNRESOLVED\_FIELD

#### UNRESOLVED\_MAP\_KEY

SQLSTATE: 42703

Cannot resolve column `<objectName>` as a map key. If the key is a string literal, add the single quotes ‘’ around it.

For more details see UNRESOLVED\_MAP\_KEY

#### UNRESOLVED\_ROUTINE

SQLSTATE: 42883

Cannot resolve function `<routineName>` on search path `<searchPath>`.

#### UNRESOLVED\_USING\_COLUMN\_FOR\_JOIN

SQLSTATE: 42703

USING column `<colName>` cannot be resolved on the `<side>` side of the join. The `<side>`-side columns: \[`<suggestion>`].

#### UNSET\_NONEXISTENT\_PROPERTIES

SQLSTATE: none assigned

Attempted to unset non-existent properties \[`<properties>`] in table `<table>`.

#### UNSUPPORTED\_ADD\_FILE

SQLSTATE: none assigned

Don’t support add file.

For more details see UNSUPPORTED\_ADD\_FILE

#### UNSUPPORTED\_ARROWTYPE

SQLSTATE: 0A000

Unsupported arrow type `<typeName>`.

#### UNSUPPORTED\_CHAR\_OR\_VARCHAR\_AS\_STRING

SQLSTATE: none assigned

The char/varchar type can’t be used in the table schema. If you want Spark treat them as string type as same as Spark 3.0 and earlier, please set “spark.sql.legacy.charVarcharAsString” to “true”.

#### UNSUPPORTED\_DATASOURCE\_FOR\_DIRECT\_QUERY

SQLSTATE: none assigned

Unsupported data source type for direct query on files: `<dataSourceType>`

#### UNSUPPORTED\_DATATYPE

SQLSTATE: 0A000

Unsupported data type `<typeName>`.

#### UNSUPPORTED\_DATA\_TYPE\_FOR\_DATASOURCE

SQLSTATE: none assigned

The `<format>` datasource doesn’t support the column `<columnName>` of the type `<columnType>`.

#### UNSUPPORTED\_DEFAULT\_VALUE

SQLSTATE: none assigned

DEFAULT column values is not supported.

For more details see UNSUPPORTED\_DEFAULT\_VALUE

#### UNSUPPORTED\_DESERIALIZER

SQLSTATE: 0A000

The deserializer is not supported:

For more details see UNSUPPORTED\_DESERIALIZER

#### UNSUPPORTED\_EXPRESSION\_GENERATED\_COLUMN

SQLSTATE: none assigned

Cannot create generated column `<fieldName>` with generation expression `<expressionStr>` because `<reason>`.

#### UNSUPPORTED\_EXPR\_FOR\_OPERATOR

SQLSTATE: none assigned

A query operator contains one or more unsupported expressions. Consider to rewrite it to avoid window functions, aggregate functions, and generator functions in the WHERE clause. Invalid expressions: \[`<invalidExprSqls>`]

#### UNSUPPORTED\_EXPR\_FOR\_WINDOW

SQLSTATE: 42P20

Expression `<sqlExpr>` not supported within a window function.

#### UNSUPPORTED\_FEATURE

SQLSTATE: 0A000

The feature is not supported:

For more details see UNSUPPORTED\_FEATURE

#### UNSUPPORTED\_GENERATOR

SQLSTATE: 0A000

The generator is not supported:

For more details see UNSUPPORTED\_GENERATOR

#### UNSUPPORTED\_GROUPING\_EXPRESSION

SQLSTATE: none assigned

grouping()/grouping\_id() can only be used with GroupingSets/Cube/Rollup.

#### UNSUPPORTED\_INSERT

SQLSTATE: none assigned

Can’t insert into the target.

For more details see UNSUPPORTED\_INSERT

#### UNSUPPORTED\_MERGE\_CONDITION

SQLSTATE: none assigned

MERGE operation contains unsupported `<condName>` condition.

For more details see UNSUPPORTED\_MERGE\_CONDITION

#### UNSUPPORTED\_OVERWRITE

SQLSTATE: none assigned

Can’t overwrite the target that is also being read from.

For more details see UNSUPPORTED\_OVERWRITE

#### UNSUPPORTED\_SAVE\_MODE

SQLSTATE: none assigned

The save mode `<saveMode>` is not supported for:

For more details see UNSUPPORTED\_SAVE\_MODE

#### UNSUPPORTED\_SUBQUERY\_EXPRESSION\_CATEGORY

SQLSTATE: 0A000

Unsupported subquery expression:

For more details see UNSUPPORTED\_SUBQUERY\_EXPRESSION\_CATEGORY

#### UNSUPPORTED\_TYPED\_LITERAL

SQLSTATE: 0A000

Literals of the type `<unsupportedType>` are not supported. Supported types are `<supportedTypes>`.

#### UNTYPED\_SCALA\_UDF

SQLSTATE: none assigned

You’re using untyped Scala UDF, which does not have the input type information. Spark may blindly pass null to the Scala closure with primitive-type argument, and the closure will see the default value of the Java type for the null argument, e.g. `udf((x: Int) => x, IntegerType)`, the result is 0 for null input. To get rid of this error, you could:

1. use typed Scala UDF APIs(without return type parameter), e.g. `udf((x: Int) => x)`.
2. use Java UDF APIs, e.g. `udf(new UDF1[String, Integer] { override def call(s: String): Integer = s.length() }, IntegerType)`, if input types are all non primitive.
3. set “spark.sql.legacy.allowUntypedScalaUDF” to “true” and use this API with caution.

#### VIEW\_ALREADY\_EXISTS

SQLSTATE: 42P07

Cannot create view `<relationName>` because it already exists. Choose a different name, drop or replace the existing object, or add the IF NOT EXISTS clause to tolerate pre-existing objects.

#### VIEW\_NOT\_FOUND

SQLSTATE: 42P01

The view `<relationName>` cannot be found. Verify the spelling and correctness of the schema and catalog. If you did not qualify the name with a schema, verify the current\_schema() output, or qualify the name with the correct schema and catalog. To tolerate the error on drop use DROP VIEW IF EXISTS.

#### WINDOW\_FUNCTION\_AND\_FRAME\_MISMATCH

SQLSTATE: none assigned

`<funcName>` function can only be evaluated in an ordered row-based window frame with a single offset: `<windowExpr>`.

#### WINDOW\_FUNCTION\_WITHOUT\_OVER\_CLAUSE

SQLSTATE: none assigned

Window function `<funcName>` requires an OVER clause.

#### WRITE\_STREAM\_NOT\_ALLOWED

SQLSTATE: none assigned

`writeStream` can be called only on streaming Dataset/DataFrame.

#### WRONG\_COMMAND\_FOR\_OBJECT\_TYPE

SQLSTATE: none assigned

The operation `<operation>` requires a `<requiredType>`. But `<objectName>` is a `<foundType>`. Use `<alternative>` instead.

#### WRONG\_NUM\_ARGS

SQLSTATE: 42605

The `<functionName>` requires `<expectedNum>` parameters but the actual number is `<actualNum>`.

For more details see WRONG\_NUM\_ARGS
