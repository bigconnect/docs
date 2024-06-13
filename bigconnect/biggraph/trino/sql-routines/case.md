# CASE

### Synopsis <a href="#synopsis" id="synopsis"></a>

Simple case:

```
CASE
  WHEN condition THEN statements
  [ ... ]
  [ ELSE statements ]
END CASE
```



Searched case:

```
CASE expression
  WHEN expression THEN statements
  [ ... ]
  [ ELSE statements ]
END
```



### Description <a href="#description" id="description"></a>

The `CASE` statement is an optional construct to allow conditional processing in SQL routines.

The `WHEN` clauses are evaluated sequentially, stopping after the first match, and therefore the order of the statements is significant. The statements of the `ELSE` clause are executed if none of the `WHEN` clauses match.

Unlike other languages like C or Java, SQL does not support case fall through, so processing stops at the end of the first matched case.

One or more `WHEN` clauses can be used.

### Examples <a href="#examples" id="examples"></a>

The following example shows a simple `CASE` statement usage:

```
FUNCTION simple_case(a bigint)
  RETURNS varchar
  BEGIN
    CASE a
      WHEN 0 THEN RETURN 'zero';
      WHEN 1 THEN RETURN 'one';
      ELSE RETURN 'more than one or negative';
    END CASE;
    RETURN NULL;
  END
```



Further examples of varying complexity that cover usage of the `CASE` statement in combination with other statements are available in the SQL routines examples documentation.

### See also <a href="#see-also" id="see-also"></a>

* Introduction to SQL routines
* Conditional expressions using `CASE`
