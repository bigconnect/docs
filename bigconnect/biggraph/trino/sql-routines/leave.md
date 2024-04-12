# LEAVE

### Synopsis[#](https://trino.io/docs/current/routines/leave.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
LEAVE label
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/routines/leave.html#description) <a href="#description" id="description"></a>

The `LEAVE` statement allows processing of blocks in [SQL routines](https://trino.io/docs/current/routines/introduction.html) to move out of a specified context. Contexts are defined by a [`label`](https://trino.io/docs/current/routines/introduction.html#routine-label). If no label is found, the functions fails with an error message.

### Examples[#](https://trino.io/docs/current/routines/leave.html#examples) <a href="#examples" id="examples"></a>

The following function includes a `LOOP` labelled `top`. The conditional `IF` statement inside the loop can cause the exit from processing the loop when the value for the parameter `p` is 1 or less. This can be the case if the value is passed in as 1 or less or after a number of iterations through the loop.

```
FUNCTION my_pow(n int, p int)
RETURNS int
BEGIN
  DECLARE r int DEFAULT n;
  top: LOOP
    IF p <= 1 THEN
      LEAVE top;
    END IF;
    SET r = r * n;
    SET p = p - 1;
  END LOOP;
  RETURN r;
END
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further examples of varying complexity that cover usage of the `LEAVE` statement in combination with other statements are available in the [SQL routines examples documentation](https://trino.io/docs/current/routines/examples.html).

### See also[#](https://trino.io/docs/current/routines/leave.html#see-also) <a href="#see-also" id="see-also"></a>

* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [ITERATE](https://trino.io/docs/current/routines/iterate.html)

\
