# SET

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SET identifier = expression
```

### Description <a href="#description" id="description"></a>

Use the `SET` statement in SQL routines to assign a value to a variable, referenced by comma-separated `identifier`s. The value is determined by evaluating the `expression` after the `=` sign.

Before the assignment the variable must be defined with a `DECLARE` statement. The data type of the variable must be identical to the data type of evaluating the `expression`.

### Examples <a href="#examples" id="examples"></a>

The following functions returns the value `1` after setting the counter variable multiple times to different values:

```
FUNCTION one()
  RETURNS bigint
  BEGIN
    DECLARE counter tinyint DEFAULT 1;
    SET counter = 0;
    SET counter = counter + 2;
    SET counter = counter / counter;
    RETURN counter;
  END
```



Further examples of varying complexity that cover usage of the `SET` statement in combination with other statements are available in the SQL routines examples documentation.

### See also <a href="#see-also" id="see-also"></a>

* Introduction to SQL routines
* DECLARE
