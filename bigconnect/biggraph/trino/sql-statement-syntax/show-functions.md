# SHOW FUNCTIONS

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SHOW FUNCTIONS [ FROM schema ] [ LIKE pattern ]
```

### Description <a href="#description" id="description"></a>

List functions in `schema` or all functions in the current session path. This can include built-in functions, functions from a custom plugin, and SQL routines.

For each function returned, the following information is displayed:

* Function name
* Return type
* Argument types
* Function type
* Deterministic
* Description

Use the optional `FROM` keyword to only list functions in a specific catalog and schema. The location in `schema` must be specified as `cataglog_name.schema_name`.

Specify a pattern in the optional `LIKE` clause to filter the results to the desired subset.

### Examples <a href="#examples" id="examples"></a>

List all SQL routines and plugin functions in the `default` schema of the `example` catalog:

```
SHOW FUNCTIONS FROM example.default;
```

List all functions with a name beginning with `array`:

```
SHOW FUNCTIONS LIKE 'array%';
```

List all functions with a name beginning with `cf`:

```
SHOW FUNCTIONS LIKE 'cf%';
```

Example output:

```
     Function      | Return Type | Argument Types | Function Type | Deterministic |               Description
 ------------------+-------------+----------------+---------------+---------------+-----------------------------------------
 cf_getgroups      | varchar     |                | scalar        | true          | Returns the current session's groups
 cf_getprincipal   | varchar     |                | scalar        | true          | Returns the current session's principal
 cf_getuser        | varchar     |                | scalar        | true          | Returns the current session's user
```

### See also <a href="#see-also" id="see-also"></a>

* Functions and operators
* SQL routines
* Functions
* CREATE FUNCTION
* DROP FUNCTION

\
