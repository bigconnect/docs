# WHILE

### Synopsis[#](https://trino.io/docs/current/routines/while.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
[label :] WHILE condition DO
  statements
END WHILE
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/routines/while.html#description) <a href="#description" id="description"></a>

The `WHILE` statement is an optional construct in [SQL routines](https://trino.io/docs/current/routines/introduction.html) to allow processing of a block of statements as long as a condition is met. The condition is validated as a first step of each iteration.

The expression that defines the `condition` is evaluated at least once. If the result is `true`, processing moves to `DO`, through following `statements` and back to `WHILE` and the `condition`. If the result is `false`, processing moves to `END WHILE` and continues with the next statement in the function.

The optional `label` before the `WHILE` keyword can be used to [name the block](https://trino.io/docs/current/routines/introduction.html#routine-label).

Note that a `WHILE` statement is very similar, with the difference that for `REPEAT` the statements are processed at least once, and for `WHILE` blocks the statements might not be processed at all.

### Examples[#](https://trino.io/docs/current/routines/while.html#examples) <a href="#examples" id="examples"></a>

```
WHILE p > 1 DO
  SET r = r * n;
  SET p = p - 1;
END WHILE;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further examples of varying complexity that cover usage of the `WHILE` statement in combination with other statements are available in the [SQL routines examples documentation](https://trino.io/docs/current/routines/examples.html).

### See also[#](https://trino.io/docs/current/routines/while.html#see-also) <a href="#see-also" id="see-also"></a>

* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [LOOP](https://trino.io/docs/current/routines/loop.html)
* [REPEAT](https://trino.io/docs/current/routines/repeat.html)

\
