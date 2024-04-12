# UNSUPPORTED\_FEATURE error class

SQLSTATE: 0A000

The feature is not supported:

This error class has the following derived error classes:

### AES\_MODE

AES-`<mode>` with the padding `<padding>` by the `<functionName>` function.

### AES\_MODE\_AAD

`<functionName>` with AES-`<mode>` does not support additional authenticate data (AAD).

### AES\_MODE\_IV

`<functionName>` with AES-`<mode>` does not support initialization vectors (IVs).

### ANALYZE\_UNCACHED\_TEMP\_VIEW

The ANALYZE TABLE FOR COLUMNS command can operate on temporary views that have been cached already. Consider to cache the view `<viewName>`.

### ANALYZE\_UNSUPPORTED\_COLUMN\_TYPE

The ANALYZE TABLE FOR COLUMNS command does not support the type `<columnType>` of the column `<columnName>` in the table `<tableName>`.

### ANALYZE\_VIEW

The ANALYZE TABLE command does not support views.

### CATALOG\_OPERATION

Catalog `<catalogName>` does not support `<operation>`.

### COMBINATION\_QUERY\_RESULT\_CLAUSES

Combination of ORDER BY/SORT BY/DISTRIBUTE BY/CLUSTER BY.

### COMMENT\_NAMESPACE

Attach a comment to the namespace `<namespace>`.

### DESC\_TABLE\_COLUMN\_PARTITION

DESC TABLE COLUMN for a specific partition.

### DROP\_DATABASE

Drop the default database `<database>`.

### DROP\_NAMESPACE

Drop the namespace `<namespace>`.

### HIVE\_TABLE\_TYPE

The `<tableName>` is hive `<tableType>`.

### HIVE\_WITH\_ANSI\_INTERVALS

Hive table `<tableName>` with ANSI intervals.

### INSERT\_PARTITION\_SPEC\_IF\_NOT\_EXISTS

INSERT INTO `<tableName>` with IF NOT EXISTS in the PARTITION spec.

### LATERAL\_COLUMN\_ALIAS\_IN\_AGGREGATE\_FUNC

Referencing a lateral column alias `<lca>` in the aggregate function `<aggFunc>`.

### LATERAL\_COLUMN\_ALIAS\_IN\_AGGREGATE\_WITH\_WINDOW\_AND\_HAVING

Referencing lateral column alias `<lca>` in the aggregate query both with window expressions and with having clause. Please rewrite the aggregate query by removing the having clause or removing lateral alias reference in the SELECT list.

### LATERAL\_COLUMN\_ALIAS\_IN\_GROUP\_BY

Referencing a lateral column alias via GROUP BY alias/ALL is not supported yet.

### LATERAL\_COLUMN\_ALIAS\_IN\_WINDOW

Referencing a lateral column alias `<lca>` in window expression `<windowExpr>`.

### LATERAL\_JOIN\_USING

JOIN USING with LATERAL correlation.

### LITERAL\_TYPE

Literal for ‘`<value>`’ of `<type>`.

### MULTIPLE\_BUCKET\_TRANSFORMS

Multiple bucket TRANSFORMs.

### MULTI\_ACTION\_ALTER

The target JDBC server hosting table `<tableName>` does not support ALTER TABLE with multiple actions. Split the ALTER TABLE up into individual actions to avoid this error.

### ORC\_TYPE\_CAST

Unable to convert `<orcType>` of Orc to data type `<toType>`.

### PANDAS\_UDAF\_IN\_PIVOT

Pandas user defined aggregate function in the PIVOT clause.

### PARAMETER\_MARKER\_IN\_UNEXPECTED\_STATEMENT

Parameter markers are not allowed in `<statement>`.

### PARTITION\_WITH\_NESTED\_COLUMN\_IS\_UNSUPPORTED

Invalid partitioning: `<cols>` is missing or is in a map or array.

### PIVOT\_AFTER\_GROUP\_BY

PIVOT clause following a GROUP BY clause. Consider pushing the GROUP BY into a subquery.

### PIVOT\_TYPE

Pivoting by the value ‘`<value>`’ of the column data type `<type>`.

### PURGE\_PARTITION

Partition purge.

### PURGE\_TABLE

Purge table.

### PYTHON\_UDF\_IN\_ON\_CLAUSE

Python UDF in the ON clause of a `<joinType>` JOIN. In case of an INNNER JOIN consider rewriting to a CROSS JOIN with a WHERE clause.

### REMOVE\_NAMESPACE\_COMMENT

Remove a comment from the namespace `<namespace>`.

### REPLACE\_NESTED\_COLUMN

The replace function does not support nested column `<colName>`.

### SET\_NAMESPACE\_PROPERTY

`<property>` is a reserved namespace property, `<msg>`.

### SET\_OPERATION\_ON\_MAP\_TYPE

Cannot have MAP type columns in DataFrame which calls set operations (INTERSECT, EXCEPT, etc.), but the type of column `<colName>` is `<dataType>`.

### SET\_PROPERTIES\_AND\_DBPROPERTIES

set PROPERTIES and DBPROPERTIES at the same time.

### SET\_TABLE\_PROPERTY

`<property>` is a reserved table property, `<msg>`.

### TABLE\_OPERATION

Table `<tableName>` does not support `<operation>`. Please check the current catalog and namespace to make sure the qualified table name is expected, and also check the catalog implementation which is configured by “spark.sql.catalog”.

### TIME\_TRAVEL

Time travel on the relation: `<relationId>`.

### TOO\_MANY\_TYPE\_ARGUMENTS\_FOR\_UDF\_CLASS

UDF class with `<num>` type arguments.

### TRANSFORM\_DISTINCT\_ALL

TRANSFORM with the DISTINCT/ALL clause.

### TRANSFORM\_NON\_HIVE

TRANSFORM with SERDE is only supported in hive mode.
