# DROP FUNCTION

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DROP FUNCTION [ IF EXISTS ] routine_name ( [ [ parameter_name ] data_type [, ...] ] )
```

### Description <a href="#description" id="description"></a>

Removes a [Catalog routines](https://trino.io/docs/current/routines/introduction.html#routine-catalog). The value of `routine_name` must be fully qualified with catalog and schema location of the routine, unless the [default SQL routine storage catalog and schema](https://trino.io/docs/current/admin/properties-sql-environment.html) are configured.

The `data_type`s must be included for routines that use parameters to ensure the routine with the correct name and parameter signature is removed.

The optional `IF EXISTS` clause causes the error to be suppressed if the function does not exist.

### Examples <a href="#examples" id="examples"></a>

The following example removes the `meaning_of_life` routine in the `default` schema of the `example` catalog:

```
DROP FUNCTION example.default.meaning_of_life();
```

If the routine uses a input parameter, the type must be added:

```
DROP FUNCTION multiply_by_two(bigint);
```

If the [default catalog and schema for routine storage](https://trino.io/docs/current/admin/properties-sql-environment.html) is configured, you can use the following more compact syntax:

```
DROP FUNCTION meaning_of_life();
```

### See also <a href="#see-also" id="see-also"></a>

* [CREATE FUNCTION](https://trino.io/docs/current/sql/create-function.html)
* [SHOW FUNCTIONS](https://trino.io/docs/current/sql/show-functions.html)
* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [SQL environment properties](https://trino.io/docs/current/admin/properties-sql-environment.html)
