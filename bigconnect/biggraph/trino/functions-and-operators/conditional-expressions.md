# Conditional expressions

### CASE[#](https://trino.io/docs/current/functions/conditional.html#case) <a href="#case" id="case"></a>

The standard SQL `CASE` expression has two forms. The “simple” form searches each `value` expression from left to right until it finds one that equals `expression`:

```
CASE expression
    WHEN value THEN result
    [ WHEN ... ]
    [ ELSE result ]
END
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `result` for the matching `value` is returned. If no match is found, the `result` from the `ELSE` clause is returned if it exists, otherwise null is returned. Example:

```
SELECT a,
       CASE a
           WHEN 1 THEN 'one'
           WHEN 2 THEN 'two'
           ELSE 'many'
       END
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The “searched” form evaluates each boolean `condition` from left to right until one is true and returns the matching `result`:

```
CASE
    WHEN condition THEN result
    [ WHEN ... ]
    [ ELSE result ]
END
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If no conditions are true, the `result` from the `ELSE` clause is returned if it exists, otherwise null is returned. Example:

```
SELECT a, b,
       CASE
           WHEN a = 1 THEN 'aaa'
           WHEN b = 2 THEN 'bbb'
           ELSE 'ccc'
       END
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

SQL routines can use [`CASE` statements](https://trino.io/docs/current/routines/case.html) that use a slightly different syntax from the CASE expressions. Specifically note the requirements for terminating each clause with a semicolon `;` and the usage of `END CASE`.

### IF[#](https://trino.io/docs/current/functions/conditional.html#if) <a href="#if" id="if"></a>

The `IF` expression has two forms, one supplying only a `true_value` and the other supplying both a `true_value` and a `false_value`:

if(_condition_, _true\_value_)[#](https://trino.io/docs/current/functions/conditional.html#id0)

Evaluates and returns `true_value` if `condition` is true, otherwise null is returned and `true_value` is not evaluated.

if(_condition_, _true\_value_, _false\_value_)

Evaluates and returns `true_value` if `condition` is true, otherwise evaluates and returns `false_value`.

The following `IF` and `CASE` expressions are equivalent:

```
SELECT
  orderkey,
  totalprice,
  IF(totalprice >= 150000, 'High Value', 'Low Value')
FROM tpch.sf1.orders;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
SELECT
  orderkey,
  totalprice,
  CASE
    WHEN totalprice >= 150000 THEN 'High Value'
    ELSE 'Low Value'
  END
FROM tpch.sf1.orders;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

SQL routines can use [`IF` statements](https://trino.io/docs/current/routines/if.html) that use a slightly different syntax from `IF` expressions. Specifically note the requirement for terminating each clause with a semicolon `;` and the usage of `END IF`.

### COALESCE[#](https://trino.io/docs/current/functions/conditional.html#coalesce) <a href="#coalesce" id="coalesce"></a>

coalesce(_value1_, _value2_\[, _..._])[#](https://trino.io/docs/current/functions/conditional.html#id1)

Returns the first non-null `value` in the argument list. Like a `CASE` expression, arguments are only evaluated if necessary.

### NULLIF[#](https://trino.io/docs/current/functions/conditional.html#nullif) <a href="#nullif" id="nullif"></a>

nullif(_value1_, _value2_)[#](https://trino.io/docs/current/functions/conditional.html#id2)

Returns null if `value1` equals `value2`, otherwise returns `value1`.

### TRY[#](https://trino.io/docs/current/functions/conditional.html#try) <a href="#try" id="try"></a>

try(_expression_)[#](https://trino.io/docs/current/functions/conditional.html#id3)

Evaluate an expression and handle certain types of errors by returning `NULL`.

In cases where it is preferable that queries produce `NULL` or default values instead of failing when corrupt or invalid data is encountered, the `TRY` function may be useful. To specify default values, the `TRY` function can be used in conjunction with the `COALESCE` function.

The following errors are handled by `TRY`:

* Division by zero
* Invalid cast or function argument
* Numeric value out of range

#### Examples[#](https://trino.io/docs/current/functions/conditional.html#examples) <a href="#examples" id="examples"></a>

Source table with some invalid data:

```
SELECT * FROM shipping;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 origin_state | origin_zip | packages | total_cost
--------------+------------+----------+------------
 California   |      94131 |       25 |        100
 California   |      P332a |        5 |         72
 California   |      94025 |        0 |        155
 New Jersey   |      08544 |      225 |        490
(4 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Query failure without `TRY`:

```
SELECT CAST(origin_zip AS BIGINT) FROM shipping;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
Query failed: Cannot cast 'P332a' to BIGINT
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

`NULL` values with `TRY`:

```
SELECT TRY(CAST(origin_zip AS BIGINT)) FROM shipping;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 origin_zip
------------
      94131
 NULL
      94025
      08544
(4 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Query failure without `TRY`:

```
SELECT total_cost / packages AS per_package FROM shipping;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
Query failed: Division by zero
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Default values with `TRY` and `COALESCE`:

```
SELECT COALESCE(TRY(total_cost / packages), 0) AS per_package FROM shipping;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 per_package
-------------
          4
         14
          0
         19
(4 rows)
```
