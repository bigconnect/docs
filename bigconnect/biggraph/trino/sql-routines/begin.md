# BEGIN

### Synopsis[#](https://trino.io/docs/current/routines/begin.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
BEGIN
  [ DECLARE ... ]
  statements
END
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/routines/begin.html#description) <a href="#description" id="description"></a>

Marks the start and end of a block in a [SQL routine](https://trino.io/docs/current/routines/introduction.html). `BEGIN` can be used wherever a statement can be used to group multiple statements together and to declare variables local to the block. A typical use case is as first statement within a [FUNCTION](https://trino.io/docs/current/routines/function.html). Blocks can also be nested.

After the `BEGIN` keyword, you can add variable declarations using [DECLARE](https://trino.io/docs/current/routines/declare.html) statements, followed by one or more statements that define the main body of the routine, separated by `;`. The following statements can be used:

* [CASE](https://trino.io/docs/current/routines/case.html)
* [IF](https://trino.io/docs/current/routines/if.html)
* [ITERATE](https://trino.io/docs/current/routines/iterate.html)
* [LEAVE](https://trino.io/docs/current/routines/leave.html)
* [LOOP](https://trino.io/docs/current/routines/loop.html)
* [REPEAT](https://trino.io/docs/current/routines/repeat.html)
* [RETURN](https://trino.io/docs/current/routines/return.html)
* [SET](https://trino.io/docs/current/routines/set.html)
* [WHILE](https://trino.io/docs/current/routines/while.html)
* Nested [BEGIN](https://trino.io/docs/current/routines/begin.html) blocks

### Examples[#](https://trino.io/docs/current/routines/begin.html#examples) <a href="#examples" id="examples"></a>

The following example computes the value `42`:

```
FUNCTION meaning_of_life()
  RETURNS tinyint
  BEGIN
    DECLARE a tinyint DEFAULT 6;
    DECLARE b tinyint DEFAULT 7;
    RETURN a * b;
  END
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further examples of varying complexity that cover usage of the `BEGIN` statement in combination with other statements are available in the [SQL routines examples documentation](https://trino.io/docs/current/routines/examples.html).

### See also[#](https://trino.io/docs/current/routines/begin.html#see-also) <a href="#see-also" id="see-also"></a>

* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [FUNCTION](https://trino.io/docs/current/routines/function.html)
