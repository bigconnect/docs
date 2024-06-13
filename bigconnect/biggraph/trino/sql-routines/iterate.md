# ITERATE

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
ITERATE label
```



### Description <a href="#description" id="description"></a>

The `ITERATE` statement allows processing of blocks in SQL routines to move processing back to the start of a context block. Contexts are defined by a `label`. If no label is found, the functions fails with an error message.

### Examples <a href="#examples" id="examples"></a>

```
FUNCTION count()
RETURNS bigint
BEGIN
  DECLARE a int DEFAULT 0;
  DECLARE b int DEFAULT 0;
  top: REPEAT
    SET a = a + 1;
    IF a <= 3 THEN
        ITERATE top;
    END IF;
    SET b = b + 1;
  RETURN b;
END
```



Further examples of varying complexity that cover usage of the `ITERATE` statement in combination with other statements are available in the SQL routines examples documentation.

### See also <a href="#see-also" id="see-also"></a>

* Introduction to SQL routines
* LEAVE
