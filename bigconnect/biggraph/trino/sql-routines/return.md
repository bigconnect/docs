# RETURN

### Synopsis[#](https://trino.io/docs/current/routines/return.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
RETURN expression
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/routines/return.html#description) <a href="#description" id="description"></a>

Provide the value from a [SQL routines](https://trino.io/docs/current/routines/introduction.html) to the caller. The value is the result of evaluating the expression. It can be a static value, a declared variable or a more complex expression.

### Examples[#](https://trino.io/docs/current/routines/return.html#examples) <a href="#examples" id="examples"></a>

The following examples return a static value, the result of an expression, and the value of the variable x:

```
RETURN 42;
RETURN 6 * 7;
RETURN x;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further examples of varying complexity that cover usage of the `RETURN` statement in combination with other statements are available in the [SQL routines examples documentation](https://trino.io/docs/current/routines/examples.html).

All routines must contain a `RETURN` statement at the end of the top-level block in the `FUNCTION` declaration, even if it’s unreachable.

### See also[#](https://trino.io/docs/current/routines/return.html#see-also) <a href="#see-also" id="see-also"></a>

* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [FUNCTION](https://trino.io/docs/current/routines/function.html)
