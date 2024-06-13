# LOOP

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
[label :] LOOP
    statements
END LOOP
```

### Description <a href="#description" id="description"></a>

The `LOOP` statement is an optional construct in SQL routines to allow processing of a block of statements repeatedly.

The block of `statements` is processed until an explicit use of `LEAVE` causes processing to exit the loop. If processing reaches `END LOOP`, another iteration of processing from the beginning starts. `LEAVE` statements are typically wrapped in an `IF` statement that declares a condition to stop the loop.

The optional `label` before the `LOOP` keyword can be used to name the block.

### Examples <a href="#examples" id="examples"></a>

The following function counts up to `100` in a loop starting from the input value `i` and returns the number of incremental steps in the loop to get to `100`.

```
FUNCTION to_one_hundred(i int)
  RETURNS int
  BEGIN
    DECLARE count int DEFAULT 0;
    abc: LOOP
      IF i >= 100 THEN
        LEAVE abc;
      END IF
      SET count = count + 1;
      SET i = i + 1;
    END LOOP;
    RETURN;
  END
```



Further examples of varying complexity that cover usage of the `LOOP` statement in combination with other statements are available in the SQL routines examples documentation.

### See also <a href="#see-also" id="see-also"></a>

* Introduction to SQL routines
* LEAVE

\
