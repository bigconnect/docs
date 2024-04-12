# SHOW FUNCTIONS

### Synopsis[#](https://trino.io/docs/current/sql/show-functions.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW FUNCTIONS [ FROM schema ] [ LIKE pattern ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/show-functions.html#description) <a href="#description" id="description"></a>

List functions in `schema` or all functions in the current session path. This can include built-in functions, [functions from a custom plugin](https://trino.io/docs/current/develop/functions.html), and [SQL routines](https://trino.io/docs/current/routines.html).

For each function returned, the following information is displayed:

* Function name
* Return type
* Argument types
* Function type
* Deterministic
* Description

Use the optional `FROM` keyword to only list functions in a specific catalog and schema. The location in `schema` must be specified as `cataglog_name.schema_name`.

[Specify a pattern](https://trino.io/docs/current/functions/comparison.html#like-operator) in the optional `LIKE` clause to filter the results to the desired subset.

### Examples[#](https://trino.io/docs/current/sql/show-functions.html#examples) <a href="#examples" id="examples"></a>

List all SQL routines and plugin functions in the `default` schema of the `example` catalog:

```
SHOW FUNCTIONS FROM example.default;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

List all functions with a name beginning with `array`:

```
SHOW FUNCTIONS LIKE 'array%';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

List all functions with a name beginning with `cf`:

```
SHOW FUNCTIONS LIKE 'cf%';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Example output:

```
     Function      | Return Type | Argument Types | Function Type | Deterministic |               Description
 ------------------+-------------+----------------+---------------+---------------+-----------------------------------------
 cf_getgroups      | varchar     |                | scalar        | true          | Returns the current session's groups
 cf_getprincipal   | varchar     |                | scalar        | true          | Returns the current session's principal
 cf_getuser        | varchar     |                | scalar        | true          | Returns the current session's user
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/show-functions.html#see-also) <a href="#see-also" id="see-also"></a>

* [Functions and operators](https://trino.io/docs/current/functions.html)
* [SQL routines](https://trino.io/docs/current/routines.html)
* [Functions](https://trino.io/docs/current/develop/functions.html)
* [CREATE FUNCTION](https://trino.io/docs/current/sql/create-function.html)
* [DROP FUNCTION](https://trino.io/docs/current/sql/drop-function.html)

\
