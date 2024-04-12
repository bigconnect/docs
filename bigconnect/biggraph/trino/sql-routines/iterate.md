# ITERATE

### Synopsis[#](https://trino.io/docs/current/routines/iterate.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
ITERATE label
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/routines/iterate.html#description) <a href="#description" id="description"></a>

The `ITERATE` statement allows processing of blocks in [SQL routines](https://trino.io/docs/current/routines/introduction.html) to move processing back to the start of a context block. Contexts are defined by a [`label`](https://trino.io/docs/current/routines/introduction.html#routine-label). If no label is found, the functions fails with an error message.

### Examples[#](https://trino.io/docs/current/routines/iterate.html#examples) <a href="#examples" id="examples"></a>

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

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further examples of varying complexity that cover usage of the `ITERATE` statement in combination with other statements are available in the [SQL routines examples documentation](https://trino.io/docs/current/routines/examples.html).

### See also[#](https://trino.io/docs/current/routines/iterate.html#see-also) <a href="#see-also" id="see-also"></a>

* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [LEAVE](https://trino.io/docs/current/routines/leave.html)
