# CREATE FUNCTION

```
CREATE [OR REPLACE] FUNCTION
  routine_definition
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/create-function.html#description) <a href="#description" id="description"></a>

Create or replace a [Catalog routines](https://trino.io/docs/current/routines/introduction.html#routine-catalog). The `routine_definition` is composed of the usage of [FUNCTION](https://trino.io/docs/current/routines/function.html) and nested statements. The name of the routine must be fully qualified with catalog and schema location, unless the [default SQL routine storage catalog and schema](https://trino.io/docs/current/admin/properties-sql-environment.html) are configured. The connector used in the catalog must support routine storage.

The optional `OR REPLACE` clause causes the routine to be replaced if it already exists rather than raising an error.

### Examples[#](https://trino.io/docs/current/sql/create-function.html#examples) <a href="#examples" id="examples"></a>

The following example creates the `meaning_of_life` routine in the `default` schema of the `example` catalog:

```
CREATE FUNCTION example.default.meaning_of_life()
  RETURNS bigint
  BEGIN
    RETURN 42;
  END;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If the [default catalog and schema for routine storage](https://trino.io/docs/current/admin/properties-sql-environment.html) is configured, you can use the following more compact syntax:

```
CREATE FUNCTION meaning_of_life() RETURNS bigint RETURN 42;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further examples of varying complexity that cover usage of the `FUNCTION` statement in combination with other statements are available in the [SQL routines examples documentation](https://trino.io/docs/current/routines/examples.html).

### See also[#](https://trino.io/docs/current/sql/create-function.html#see-also) <a href="#see-also" id="see-also"></a>

* [DROP FUNCTION](https://trino.io/docs/current/sql/drop-function.html)
* [SHOW FUNCTIONS](https://trino.io/docs/current/sql/show-functions.html)
* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [SQL environment properties](https://trino.io/docs/current/admin/properties-sql-environment.html)
