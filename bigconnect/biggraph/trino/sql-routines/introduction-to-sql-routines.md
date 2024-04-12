# Introduction to SQL routines

A SQL routine is a custom, user-defined function authored by a user of Trino and written in the SQL routine language. You can declare the routine body within a [FUNCTION](https://trino.io/docs/current/routines/function.html) block as [inline routines](https://trino.io/docs/current/routines/introduction.html#routine-inline) or [catalog routines](https://trino.io/docs/current/routines/introduction.html#routine-catalog).

### Inline routines[#](https://trino.io/docs/current/routines/introduction.html#inline-routines) <a href="#inline-routines" id="inline-routines"></a>

An inline routine declares and uses the routine within a query processing context. The routine is declared in a `WITH` block before the query:

```
WITH
  FUNCTION abc(x integer)
    RETURNS integer
    RETURN x * 2
SELECT abc(21);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Inline routine names must follow SQL identifier naming conventions, and cannot contain `.` characters.

The routine declaration is only valid within the context of the query. A separate later invocation of the routine is not possible. If this is desired, use a [catalog routine](https://trino.io/docs/current/routines/introduction.html#routine-catalog).

Multiple inline routine declarations are comma-separated, and can include routines calling each other, as long as a called routine is declared before the first invocation.

```
WITH
  FUNCTION abc(x integer)
    RETURNS integer
    RETURN x * 2,
  FUNCTION xyz(x integer)
    RETURNS integer
    RETURN abc(x) + 1
SELECT xyz(21);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note that inline routines can mask and override the meaning of a built-in function:

```
WITH
  FUNCTION abs(x integer)
    RETURNS integer
    RETURN x * 2
SELECT abs(-10); -- -20, not 10!
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Catalog routines[#](https://trino.io/docs/current/routines/introduction.html#catalog-routines) <a href="#catalog-routines" id="catalog-routines"></a>

You can store a routine in the context of a catalog, if the connector used in the catalog supports routine storage. The following connectors support catalog routine storage:

* [Hive connector](https://trino.io/docs/current/connector/hive.html)
* [Memory connector](https://trino.io/docs/current/connector/memory.html)

In this scenario, the following commands can be used:

* [CREATE FUNCTION](https://trino.io/docs/current/sql/create-function.html) to create and store a routine.
* [DROP FUNCTION](https://trino.io/docs/current/sql/drop-function.html) to remove a routine.
* [SHOW FUNCTIONS](https://trino.io/docs/current/sql/show-functions.html) to display a list of routines in a catalog.

Catalog routines must use a name that combines the catalog name and schema name with the routine name, such as `example.default.power` for the `power` routine in the `default` schema of the `example` catalog.

Invocation must use the fully qualified name, such as `example.default.power`.

### SQL environment configuration[#](https://trino.io/docs/current/routines/introduction.html#sql-environment-configuration) <a href="#sql-environment-configuration" id="sql-environment-configuration"></a>

Configuration of the `sql.default-function-catalog` and `sql.default-function-schema` [SQL environment properties](https://trino.io/docs/current/admin/properties-sql-environment.html) allows you to set the default storage for SQL routines. The catalog and schema must be added to the `sql.path` as well. This enables users to call SQL routines and perform all [SQL routine management](https://trino.io/docs/current/language/sql-support.html#sql-routine-management) without specifying the full path to the routine.

Note

Use the [Memory connector](https://trino.io/docs/current/connector/memory.html) in a catalog for simple storing and testing of your SQL routines.

### Routine declaration[#](https://trino.io/docs/current/routines/introduction.html#routine-declaration) <a href="#routine-declaration" id="routine-declaration"></a>

Refer to the documentation for the [FUNCTION](https://trino.io/docs/current/routines/function.html) keyword for more details about declaring the routine overall. The routine body is composed with statements from the following list:

* [BEGIN](https://trino.io/docs/current/routines/begin.html)
* [CASE](https://trino.io/docs/current/routines/case.html)
* [DECLARE](https://trino.io/docs/current/routines/declare.html)
* [IF](https://trino.io/docs/current/routines/if.html)
* [ITERATE](https://trino.io/docs/current/routines/iterate.html)
* [LEAVE](https://trino.io/docs/current/routines/leave.html)
* [LOOP](https://trino.io/docs/current/routines/loop.html)
* [REPEAT](https://trino.io/docs/current/routines/repeat.html)
* [RETURN](https://trino.io/docs/current/routines/return.html)
* [SET](https://trino.io/docs/current/routines/set.html)
* [WHILE](https://trino.io/docs/current/routines/while.html)

Statements can also use [built-in functions and operators](https://trino.io/docs/current/functions.html) as well as other routines, although recursion is not supported for routines.

Find simple examples in each statement documentation, and refer to the [example documentation](https://trino.io/docs/current/routines/examples.html) for more complex use cases that combine multiple statements.

Note

User-defined functions can alternatively be written in Java and deployed as a plugin. Details are available in the [developer guide](https://trino.io/docs/current/develop/functions.html).

### Labels[#](https://trino.io/docs/current/routines/introduction.html#labels) <a href="#labels" id="labels"></a>

Routines can contain labels as markers for a specific block in the declaration before the following keywords:

* `CASE`
* `IF`
* `LOOP`
* `REPEAT`
* `WHILE`

The label is used to name the block to continue processing with the `ITERATE` statement or exit the block with the `LEAVE` statement. This flow control is supported for nested blocks, allowing to continue or exit an outer block, not just the innermost block. For example, the following snippet uses the label `top` to name the complete block from `REPEAT` to `END REPEAT`:

```
top: REPEAT
  SET a = a + 1;
  IF a <= 3 THEN
    ITERATE top;
  END IF;
  SET b = b + 1;
  UNTIL a >= 10
END REPEAT;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Labels can be used with the `ITERATE` and `LEAVE` statements to continue processing the block or leave the block. This flow control is also supported for nested blocks and labels.

### Recommendations[#](https://trino.io/docs/current/routines/introduction.html#recommendations) <a href="#recommendations" id="recommendations"></a>

Processing routines can potentially be resource intensive on the cluster in terms of memory and processing. Take the following considerations into account when writing and running SQL routines:

* Some checks for the runtime behavior of routines are in place. For example, routines that take longer to process than a hardcoded threshold are automatically terminated.
* Avoid creation of arrays in a looping construct. Each iteration creates a separate new array with all items and copies the data for each modification, leaving the prior array in memory for automated clean up later. Use a [lambda expression](https://trino.io/docs/current/functions/lambda.html) instead of the loop.
* Avoid concatenating strings in a looping construct. Each iteration creates a separate new string and copying the old string for each modification, leaving the prior string in memory for automated clean up later. Use a [lambda expression](https://trino.io/docs/current/functions/lambda.html) instead of the loop.
* Most routines should declare the `RETURNS NULL ON NULL INPUT` characteristics unless the code has some special handling for null values. You must declare this explicitly since `CALLED ON NULL INPUT` is the default characteristic.

### Limitations[#](https://trino.io/docs/current/routines/introduction.html#limitations) <a href="#limitations" id="limitations"></a>

The following limitations apply to SQL routines.

* Routines must be declared before they are referenced.
* Recursion cannot be declared or processed.
* Mutual recursion can not be declared or processed.
* Queries cannot be processed in a routine.

Specifically this means that routines can not use `SELECT` queries to retrieve data or any other queries to process data within the routine. Instead queries can use routines to process data. Routines only work on data provided as input values and only provide output data from the `RETURN` statement.
