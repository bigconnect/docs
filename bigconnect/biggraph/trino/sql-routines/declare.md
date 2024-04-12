# DECLARE

### Synopsis[#](https://trino.io/docs/current/routines/declare.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DECLARE identifier [, ...] type [ DEFAULT expression ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/routines/declare.html#description) <a href="#description" id="description"></a>

Use the `DECLARE` statement directly after the [BEGIN](https://trino.io/docs/current/routines/begin.html) keyword in [SQL routines](https://trino.io/docs/current/routines.html) to define one or more variables with an `identifier` as name. Each statement must specify the [data type](https://trino.io/docs/current/language/types.html) of the variable with `type`. It can optionally include a default, initial value defined by an `expression`. The default value is `NULL` if not specified.

### Examples[#](https://trino.io/docs/current/routines/declare.html#examples) <a href="#examples" id="examples"></a>

A simple declaration of the variable `x` with the `tinyint` data type and the implicit default value of `null`:

```
DECLARE x tinyint;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

A declaration of multiple string variables with length restricted to 25 characters:

```
DECLARE first_name, last_name, middle_name varchar(25);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

A declaration of an exact decimal number with a default value:

```
DECLARE uptime_requirement decimal DEFAULT 99.999;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

A declaration with a default value from an expression:

```
DECLARE start_time timestamp(3) with time zone DEFAULT now();
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Further examples of varying complexity that cover usage of the `DECLARE` statement in combination with other statements are available in the [SQL routines examples documentation](https://trino.io/docs/current/routines/examples.html).

### See also[#](https://trino.io/docs/current/routines/declare.html#see-also) <a href="#see-also" id="see-also"></a>

* [Introduction to SQL routines](https://trino.io/docs/current/routines/introduction.html)
* [Data types](https://trino.io/docs/current/language/types.html)
