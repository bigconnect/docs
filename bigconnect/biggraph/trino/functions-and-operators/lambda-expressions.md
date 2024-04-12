# Lambda expressions

Lambda expressions are anonymous functions which are passed as arguments to higher-order SQL functions.

Lambda expressions are written with `->`:

```
x -> x + 1
(x, y) -> x + y
x -> regexp_like(x, 'a+')
x -> x[1] / x[2]
x -> IF(x > 0, x, -x)
x -> COALESCE(x, 0)
x -> CAST(x AS JSON)
x -> x + TRY(1 / 0)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/functions/lambda.html#limitations) <a href="#limitations" id="limitations"></a>

Most SQL expressions can be used in a lambda body, with a few exceptions:

* Subqueries are not supported: `x -> 2 + (SELECT 3)`
* Aggregations are not supported: `x -> max(y)`

### Examples[#](https://trino.io/docs/current/functions/lambda.html#examples) <a href="#examples" id="examples"></a>

Obtain the squared elements of an array column with [`transform()`](https://trino.io/docs/current/functions/array.html#transform):

```
SELECT numbers,
       transform(numbers, n -> n * n) as squared_numbers
FROM (
    VALUES
        (ARRAY[1, 2]),
        (ARRAY[3, 4]),
        (ARRAY[5, 6, 7])
) AS t(numbers);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
  numbers  | squared_numbers
-----------+-----------------
 [1, 2]    | [1, 4]
 [3, 4]    | [9, 16]
 [5, 6, 7] | [25, 36, 49]
(3 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The function [`transform()`](https://trino.io/docs/current/functions/array.html#transform) can be also employed to safely cast the elements of an array to strings:

```
SELECT transform(prices, n -> TRY_CAST(n AS VARCHAR) || '$') as price_tags
FROM (
    VALUES
        (ARRAY[100, 200]),
        (ARRAY[30, 4])
) AS t(prices);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
  price_tags
--------------
 [100$, 200$]
 [30$, 4$]
(2 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Besides the array column being manipulated, other columns can be captured as well within the lambda expression. The following statement provides a showcase of this feature for calculating the value of the linear function `f(x) = ax + b` with [`transform()`](https://trino.io/docs/current/functions/array.html#transform):

```
SELECT xvalues,
       a,
       b,
       transform(xvalues, x -> a * x + b) as linear_function_values
FROM (
    VALUES
        (ARRAY[1, 2], 10, 5),
        (ARRAY[3, 4], 4, 2)
) AS t(xvalues, a, b);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 xvalues | a  | b | linear_function_values
---------+----+---+------------------------
 [1, 2]  | 10 | 5 | [15, 25]
 [3, 4]  |  4 | 2 | [14, 18]
(2 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Find the array elements containing at least one value greater than `100` with [`any_match()`](https://trino.io/docs/current/functions/array.html#any\_match):

```
SELECT numbers
FROM (
    VALUES
        (ARRAY[1,NULL,3]),
        (ARRAY[10,20,30]),
        (ARRAY[100,200,300])
) AS t(numbers)
WHERE any_match(numbers, n ->  COALESCE(n, 0) > 100);
-- [100, 200, 300]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Capitalize the first word in a string via [`regexp_replace()`](https://trino.io/docs/current/functions/regexp.html#regexp\_replace):

```
SELECT regexp_replace('once upon a time ...', '^(\w)(\w*)(\s+.*)$',x -> upper(x[1]) || x[2] || x[3]);
-- Once upon a time ...
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Lambda expressions can be also applied in aggregation functions. Following statement is a sample the overly complex calculation of the sum of all elements of a column by making use of [`reduce_agg()`](https://trino.io/docs/current/functions/aggregate.html#reduce\_agg):

```
SELECT reduce_agg(value, 0, (a, b) -> a + b, (a, b) -> a + b) sum_values
FROM (
    VALUES (1), (2), (3), (4), (5)
) AS t(value);
-- 15
```
