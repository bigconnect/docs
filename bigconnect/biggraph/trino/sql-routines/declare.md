# DECLARE

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DECLARE identifier [, ...] type [ DEFAULT expression ]
```



### Description <a href="#description" id="description"></a>

Use the `DECLARE` statement directly after the BEGIN keyword in SQL routines to define one or more variables with an `identifier` as name. Each statement must specify the data type of the variable with `type`. It can optionally include a default, initial value defined by an `expression`. The default value is `NULL` if not specified.

### Examples <a href="#examples" id="examples"></a>

A simple declaration of the variable `x` with the `tinyint` data type and the implicit default value of `null`:

```
DECLARE x tinyint;
```



A declaration of multiple string variables with length restricted to 25 characters:

```
DECLARE first_name, last_name, middle_name varchar(25);
```



A declaration of an exact decimal number with a default value:

```
DECLARE uptime_requirement decimal DEFAULT 99.999;
```



A declaration with a default value from an expression:

```
DECLARE start_time timestamp(3) with time zone DEFAULT now();
```



Further examples of varying complexity that cover usage of the `DECLARE` statement in combination with other statements are available in the SQL routines examples documentation.

### See also <a href="#see-also" id="see-also"></a>

* Introduction to SQL routines
* Data types
