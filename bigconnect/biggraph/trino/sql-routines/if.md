# IF

### Synopsis[#](https://trino.io/docs/current/routines/if.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
IF condition
  THEN statements
  [ ELSEIF condition THEN statements ]
  [ ... ]
  [ ELSE statements ]
END IF
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/routines/if.html#description) <a href="#description" id="description"></a>

The `IF THEN` statement is an optional construct to allow conditional processing in [SQL routines](https://trino.io/docs/current/routines/introduction.html). Each `condition` following an `IF` or `ELSEIF` must evaluate to a boolean. The result of processing the expression must result in a boolean `true` value to process the `statements` in the `THEN` block. A result of `false` results in skipping the `THEN` block and moving to evaluate the next `ELSEIF` and `ELSE` blocks in order.

The `ELSEIF` and `ELSE` segments are optional.

### Examples[#](https://trino.io/docs/current/routines/if.html#examples) <a href="#examples" id="examples"></a>

```
FUNCTION simple_if(a bigint)
  RETURNS varchar
  BEGIN
    IF a = 0 THEN
      RETURN 'zero';
    ELSEIF a = 1 THEN
      RETURN 'one';
    ELSE
      RETURN 'more than one or negative';
    END IF;
  END
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further examples of varying complexity that cover usage of the `IF` statement in combination with other statements are available in the [SQL routines examples documentation](https://trino.io/docs/current/routines/examples.html).

### See also[#](https://trino.io/docs/current/routines/if.html#see-also) <a href="#see-also" id="see-also"></a>

* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [Conditional expressions using `IF`](https://trino.io/docs/current/functions/conditional.html#if-expression)
