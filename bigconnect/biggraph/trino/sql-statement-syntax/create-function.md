# CREATE FUNCTION

```
CREATE [OR REPLACE] FUNCTION
  routine_definition
```

### Description <a href="#description" id="description"></a>

Create or replace a Catalog routines. The `routine_definition` is composed of the usage of FUNCTION and nested statements. The name of the routine must be fully qualified with catalog and schema location, unless the default SQL routine storage catalog and schema are configured. The connector used in the catalog must support routine storage.

The optional `OR REPLACE` clause causes the routine to be replaced if it already exists rather than raising an error.

### Examples <a href="#examples" id="examples"></a>

The following example creates the `meaning_of_life` routine in the `default` schema of the `example` catalog:

```
CREATE FUNCTION example.default.meaning_of_life()
  RETURNS bigint
  BEGIN
    RETURN 42;
  END;
```

If the default catalog and schema for routine storage is configured, you can use the following more compact syntax:

```
CREATE FUNCTION meaning_of_life() RETURNS bigint RETURN 42;
```

Further examples of varying complexity that cover usage of the `FUNCTION` statement in combination with other statements are available in the SQL routines examples documentation.

### See also <a href="#see-also" id="see-also"></a>

* DROP FUNCTION
* SHOW FUNCTIONS
* Introduction to SQL routines
* SQL environment properties
