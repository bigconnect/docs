# Aggregate functions

Aggregate functions operate on a set of values to compute a single result.

Except for [`count()`](https://trino.io/docs/current/functions/aggregate.html#count), [`count_if()`](https://trino.io/docs/current/functions/aggregate.html#count\_if), [`max_by()`](https://trino.io/docs/current/functions/aggregate.html#max\_by), [`min_by()`](https://trino.io/docs/current/functions/aggregate.html#min\_by) and [`approx_distinct()`](https://trino.io/docs/current/functions/aggregate.html#approx\_distinct), all of these aggregate functions ignore null values and return null for no input rows or when all values are null. For example, [`sum()`](https://trino.io/docs/current/functions/aggregate.html#sum) returns null rather than zero and [`avg()`](https://trino.io/docs/current/functions/aggregate.html#avg) does not include null values in the count. The `coalesce` function can be used to convert null into zero.

### Ordering during aggregation[#](https://trino.io/docs/current/functions/aggregate.html#ordering-during-aggregation) <a href="#ordering-during-aggregation" id="ordering-during-aggregation"></a>

Some aggregate functions such as [`array_agg()`](https://trino.io/docs/current/functions/aggregate.html#array\_agg) produce different results depending on the order of input values. This ordering can be specified by writing an [ORDER BY clause](https://trino.io/docs/current/sql/select.html#order-by-clause) within the aggregate function:

```
array_agg(x ORDER BY y DESC)
array_agg(x ORDER BY x, y, z)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Filtering during aggregation[#](https://trino.io/docs/current/functions/aggregate.html#filtering-during-aggregation) <a href="#filtering-during-aggregation" id="filtering-during-aggregation"></a>

The `FILTER` keyword can be used to remove rows from aggregation processing with a condition expressed using a `WHERE` clause. This is evaluated for each row before it is used in the aggregation and is supported for all aggregate functions.

```
aggregate_function(...) FILTER (WHERE <condition>)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

A common and very useful example is to use `FILTER` to remove nulls from consideration when using `array_agg`:

```
SELECT array_agg(name) FILTER (WHERE name IS NOT NULL)
FROM region;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

As another example, imagine you want to add a condition on the count for Iris flowers, modifying the following query:

```
SELECT species,
       count(*) AS count
FROM iris
GROUP BY species;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
species    | count
-----------+-------
setosa     |   50
virginica  |   50
versicolor |   50
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If you just use a normal `WHERE` statement you lose information:

```
SELECT species,
    count(*) AS count
FROM iris
WHERE petal_length_cm > 4
GROUP BY species;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
species    | count
-----------+-------
virginica  |   50
versicolor |   34
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Using a filter you retain all information:

```
SELECT species,
       count(*) FILTER (where petal_length_cm > 4) AS count
FROM iris
GROUP BY species;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
species    | count
-----------+-------
virginica  |   50
setosa     |    0
versicolor |   34
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### General aggregate functions[#](https://trino.io/docs/current/functions/aggregate.html#general-aggregate-functions) <a href="#general-aggregate-functions" id="general-aggregate-functions"></a>

any\_value(_x_) → \[same as input][#](https://trino.io/docs/current/functions/aggregate.html#any\_value)

Returns an arbitrary non-null value `x`, if one exists. `x` can be any valid expression. This allows you to return values from columns that are not directly part of the aggregation, inluding expressions using these columns, in a query.

For example, the following query returns the customer name from the `name` column, and returns the sum of all total prices as customer spend. The aggregation however uses the rows grouped by the customer identifier `custkey` a required, since only that column is guaranteed to be unique:

```
SELECT sum(o.totalprice) as spend,
    any_value(c.name)
FROM tpch.tiny.orders o
JOIN tpch.tiny.customer c
ON o.custkey  = c.custkey
GROUP BY c.custkey;
ORDER BY spend;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)arbitrary(_x_) → \[same as input][#](https://trino.io/docs/current/functions/aggregate.html#arbitrary)

Returns an arbitrary non-null value of `x`, if one exists. Identical to [`any_value()`](https://trino.io/docs/current/functions/aggregate.html#any\_value).

array\_agg(_x_) → array<\[same as input]>[#](https://trino.io/docs/current/functions/aggregate.html#array\_agg)

Returns an array created from the input `x` elements.

avg(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#avg)

Returns the average (arithmetic mean) of all input values.

avg(_time interval type_) → time interval type

Returns the average interval length of all input values.

bool\_and(_boolean_) → boolean[#](https://trino.io/docs/current/functions/aggregate.html#bool\_and)

Returns `TRUE` if every input value is `TRUE`, otherwise `FALSE`.

bool\_or(_boolean_) → boolean[#](https://trino.io/docs/current/functions/aggregate.html#bool\_or)

Returns `TRUE` if any input value is `TRUE`, otherwise `FALSE`.

checksum(_x_) → varbinary[#](https://trino.io/docs/current/functions/aggregate.html#checksum)

Returns an order-insensitive checksum of the given values.

count(_\*_) → bigint[#](https://trino.io/docs/current/functions/aggregate.html#count)

Returns the number of input rows.

count(_x_) → bigint

Returns the number of non-null input values.

count\_if(_x_) → bigint[#](https://trino.io/docs/current/functions/aggregate.html#count\_if)

Returns the number of `TRUE` input values. This function is equivalent to `count(CASE WHEN x THEN 1 END)`.

every(_boolean_) → boolean[#](https://trino.io/docs/current/functions/aggregate.html#every)

This is an alias for [`bool_and()`](https://trino.io/docs/current/functions/aggregate.html#bool\_and).

geometric\_mean(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#geometric\_mean)

Returns the geometric mean of all input values.

listagg(_x_, _separator_) → varchar[#](https://trino.io/docs/current/functions/aggregate.html#listagg)

Returns the concatenated input values, separated by the `separator` string.

Synopsis:

```
LISTAGG( expression [, separator] [ON OVERFLOW overflow_behaviour])
    WITHIN GROUP (ORDER BY sort_item, [...]) [FILTER (WHERE condition)]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If `separator` is not specified, the empty string will be used as `separator`.

In its simplest form the function looks like:

```
SELECT listagg(value, ',') WITHIN GROUP (ORDER BY value) csv_value
FROM (VALUES 'a', 'c', 'b') t(value);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

and results in:

```
csv_value
-----------
'a,b,c'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The overflow behaviour is by default to throw an error in case that the length of the output of the function exceeds `1048576` bytes:

```
SELECT listagg(value, ',' ON OVERFLOW ERROR) WITHIN GROUP (ORDER BY value) csv_value
FROM (VALUES 'a', 'b', 'c') t(value);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

There exists also the possibility to truncate the output `WITH COUNT` or `WITHOUT COUNT` of omitted non-null values in case that the length of the output of the function exceeds `1048576` bytes:

```
SELECT listagg(value, ',' ON OVERFLOW TRUNCATE '.....' WITH COUNT) WITHIN GROUP (ORDER BY value)
FROM (VALUES 'a', 'b', 'c') t(value);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

If not specified, the truncation filler string is by default `'...'`.

This aggregation function can be also used in a scenario involving grouping:

```
SELECT id, listagg(value, ',') WITHIN GROUP (ORDER BY o) csv_value
FROM (VALUES
    (100, 1, 'a'),
    (200, 3, 'c'),
    (200, 2, 'b')
) t(id, o, value)
GROUP BY id
ORDER BY id;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

results in:

```
 id  | csv_value
-----+-----------
 100 | a
 200 | b,c
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

This aggregation function supports [filtering during aggregation](https://trino.io/docs/current/functions/aggregate.html#aggregate-function-filtering-during-aggregation) for scenarios where the aggregation for the data not matching the filter condition still needs to show up in the output:

```
SELECT 
    country,
    listagg(city, ',')
        WITHIN GROUP (ORDER BY population DESC)
        FILTER (WHERE population >= 10_000_000) megacities
FROM (VALUES 
    ('India', 'Bangalore', 13_700_000),
    ('India', 'Chennai', 12_200_000),
    ('India', 'Ranchi', 1_547_000),
    ('Austria', 'Vienna', 1_897_000),
    ('Poland', 'Warsaw', 1_765_000)
) t(country, city, population)
GROUP BY country
ORDER BY country;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

results in:

```
 country |    megacities     
---------+-------------------
 Austria | NULL              
 India   | Bangalore,Chennai 
 Poland  | NULL
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The current implementation of `listagg` function does not support window frames.

max(_x_) → \[same as input][#](https://trino.io/docs/current/functions/aggregate.html#max)

Returns the maximum value of all input values.

max(_x_, _n_) → array<\[same as x]>

Returns `n` largest values of all input values of `x`.

max\_by(_x_, _y_) → \[same as x][#](https://trino.io/docs/current/functions/aggregate.html#max\_by)

Returns the value of `x` associated with the maximum value of `y` over all input values.

max\_by(_x_, _y_, _n_) → array<\[same as x]>

Returns `n` values of `x` associated with the `n` largest of all input values of `y` in descending order of `y`.

min(_x_) → \[same as input][#](https://trino.io/docs/current/functions/aggregate.html#min)

Returns the minimum value of all input values.

min(_x_, _n_) → array<\[same as x]>

Returns `n` smallest values of all input values of `x`.

min\_by(_x_, _y_) → \[same as x][#](https://trino.io/docs/current/functions/aggregate.html#min\_by)

Returns the value of `x` associated with the minimum value of `y` over all input values.

min\_by(_x_, _y_, _n_) → array<\[same as x]>

Returns `n` values of `x` associated with the `n` smallest of all input values of `y` in ascending order of `y`.

sum(_x_) → \[same as input][#](https://trino.io/docs/current/functions/aggregate.html#sum)

Returns the sum of all input values.

### Bitwise aggregate functions[#](https://trino.io/docs/current/functions/aggregate.html#bitwise-aggregate-functions) <a href="#bitwise-aggregate-functions" id="bitwise-aggregate-functions"></a>

bitwise\_and\_agg(_x_) → bigint[#](https://trino.io/docs/current/functions/aggregate.html#bitwise\_and\_agg)

Returns the bitwise AND of all input values in 2’s complement representation.

bitwise\_or\_agg(_x_) → bigint[#](https://trino.io/docs/current/functions/aggregate.html#bitwise\_or\_agg)

Returns the bitwise OR of all input values in 2’s complement representation.

### Map aggregate functions[#](https://trino.io/docs/current/functions/aggregate.html#map-aggregate-functions) <a href="#map-aggregate-functions" id="map-aggregate-functions"></a>

histogram(_x_) → map\<K,bigint>[#](https://trino.io/docs/current/functions/aggregate.html#histogram)

Returns a map containing the count of the number of times each input value occurs.

map\_agg(_key_, _value_) → map\<K,V>[#](https://trino.io/docs/current/functions/aggregate.html#map\_agg)

Returns a map created from the input `key` / `value` pairs.

map\_union(_x(K_, _V)_) → map\<K,V>[#](https://trino.io/docs/current/functions/aggregate.html#map\_union)

Returns the union of all the input maps. If a key is found in multiple input maps, that key’s value in the resulting map comes from an arbitrary input map.

For example, take the following histogram function that creates multiple maps from the Iris dataset:

```
SELECT histogram(floor(petal_length_cm)) petal_data
FROM memory.default.iris
GROUP BY species;

        petal_data
-- {4.0=6, 5.0=33, 6.0=11}
-- {4.0=37, 5.0=2, 3.0=11}
-- {1.0=50}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can combine these maps using `map_union`:

```
SELECT map_union(petal_data) petal_data_union
FROM (
       SELECT histogram(floor(petal_length_cm)) petal_data
       FROM memory.default.iris
       GROUP BY species
       );

             petal_data_union
--{4.0=6, 5.0=2, 6.0=11, 1.0=50, 3.0=11}
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)multimap\_agg(_key_, _value_) → map\<K,array(V)>[#](https://trino.io/docs/current/functions/aggregate.html#multimap\_agg)

Returns a multimap created from the input `key` / `value` pairs. Each key can be associated with multiple values.

### Approximate aggregate functions[#](https://trino.io/docs/current/functions/aggregate.html#approximate-aggregate-functions) <a href="#approximate-aggregate-functions" id="approximate-aggregate-functions"></a>

approx\_distinct(_x_) → bigint[#](https://trino.io/docs/current/functions/aggregate.html#approx\_distinct)

Returns the approximate number of distinct input values. This function provides an approximation of `count(DISTINCT x)`. Zero is returned if all input values are null.

This function should produce a standard error of 2.3%, which is the standard deviation of the (approximately normal) error distribution over all possible sets. It does not guarantee an upper bound on the error for any specific input set.

approx\_distinct(_x_, _e_) → bigint

Returns the approximate number of distinct input values. This function provides an approximation of `count(DISTINCT x)`. Zero is returned if all input values are null.

This function should produce a standard error of no more than `e`, which is the standard deviation of the (approximately normal) error distribution over all possible sets. It does not guarantee an upper bound on the error for any specific input set. The current implementation of this function requires that `e` be in the range of `[0.0040625, 0.26000]`.

approx\_most\_frequent(_buckets_, _value_, _capacity_) → map<\[same as value], bigint>[#](https://trino.io/docs/current/functions/aggregate.html#approx\_most\_frequent)

Computes the top frequent values up to `buckets` elements approximately. Approximate estimation of the function enables us to pick up the frequent values with less memory. Larger `capacity` improves the accuracy of underlying algorithm with sacrificing the memory capacity. The returned value is a map containing the top elements with corresponding estimated frequency.

The error of the function depends on the permutation of the values and its cardinality. We can set the capacity same as the cardinality of the underlying data to achieve the least error.

`buckets` and `capacity` must be `bigint`. `value` can be numeric or string type.

The function uses the stream summary data structure proposed in the paper [Efficient Computation of Frequent and Top-k Elements in Data Streams](https://www.cse.ust.hk/\~raywong/comp5331/References/EfficientComputationOfFrequentAndTop-kElementsInDataStreams.pdf) by A. Metwalley, D. Agrawl and A. Abbadi.

approx\_percentile(_x_, _percentage_) → \[same as x][#](https://trino.io/docs/current/functions/aggregate.html#approx\_percentile)

Returns the approximate percentile for all input values of `x` at the given `percentage`. The value of `percentage` must be between zero and one and must be constant for all input rows.

approx\_percentile(_x_, _percentages_) → array<\[same as x]>

Returns the approximate percentile for all input values of `x` at each of the specified percentages. Each element of the `percentages` array must be between zero and one, and the array must be constant for all input rows.

approx\_percentile(_x_, _w_, _percentage_) → \[same as x]

Returns the approximate weighed percentile for all input values of `x` using the per-item weight `w` at the percentage `percentage`. Weights must be greater or equal to 1. Integer-value weights can be thought of as a replication count for the value `x` in the percentile set. The value of `percentage` must be between zero and one and must be constant for all input rows.

approx\_percentile(_x_, _w_, _percentages_) → array<\[same as x]>

Returns the approximate weighed percentile for all input values of `x` using the per-item weight `w` at each of the given percentages specified in the array. Weights must be greater or equal to 1. Integer-value weights can be thought of as a replication count for the value `x` in the percentile set. Each element of the `percentages` array must be between zero and one, and the array must be constant for all input rows.

approx\_set(_x_) → HyperLogLog

See [HyperLogLog functions](https://trino.io/docs/current/functions/hyperloglog.html).

merge(_x_) → HyperLogLog

See [HyperLogLog functions](https://trino.io/docs/current/functions/hyperloglog.html).

merge(_qdigest(T)) -> qdigest(T_)

See [Quantile digest functions](https://trino.io/docs/current/functions/qdigest.html).

merge(_tdigest_) → tdigest

See [T-Digest functions](https://trino.io/docs/current/functions/tdigest.html).

numeric\_histogram(_buckets_, _value_) → map\<double, double>

Computes an approximate histogram with up to `buckets` number of buckets for all `value`s. This function is equivalent to the variant of [`numeric_histogram()`](https://trino.io/docs/current/functions/aggregate.html#numeric\_histogram) that takes a `weight`, with a per-item weight of `1`.

numeric\_histogram(_buckets_, _value_, _weight_) → map\<double, double>[#](https://trino.io/docs/current/functions/aggregate.html#numeric\_histogram)

Computes an approximate histogram with up to `buckets` number of buckets for all `value`s with a per-item weight of `weight`. The algorithm is based loosely on:

```
Yael Ben-Haim and Elad Tom-Tov, "A streaming parallel decision tree algorithm",
J. Machine Learning Research 11 (2010), pp. 849--872.
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

`buckets` must be a `bigint`. `value` and `weight` must be numeric.

qdigest\_agg(_x) -> qdigest(\[same as x]_)

See [Quantile digest functions](https://trino.io/docs/current/functions/qdigest.html).

qdigest\_agg(_x, w) -> qdigest(\[same as x]_)

See [Quantile digest functions](https://trino.io/docs/current/functions/qdigest.html).

qdigest\_agg(_x, w, accuracy) -> qdigest(\[same as x]_)

See [Quantile digest functions](https://trino.io/docs/current/functions/qdigest.html).

tdigest\_agg(_x_) → tdigest

See [T-Digest functions](https://trino.io/docs/current/functions/tdigest.html).

tdigest\_agg(_x_, _w_) → tdigest

See [T-Digest functions](https://trino.io/docs/current/functions/tdigest.html).

### Statistical aggregate functions[#](https://trino.io/docs/current/functions/aggregate.html#statistical-aggregate-functions) <a href="#statistical-aggregate-functions" id="statistical-aggregate-functions"></a>

corr(_y_, _x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#corr)

Returns correlation coefficient of input values.

covar\_pop(_y_, _x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#covar\_pop)

Returns the population covariance of input values.

covar\_samp(_y_, _x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#covar\_samp)

Returns the sample covariance of input values.

kurtosis(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#kurtosis)

Returns the excess kurtosis of all input values. Unbiased estimate using the following expression:

```
kurtosis(x) = n(n+1)/((n-1)(n-2)(n-3))sum[(x_i-mean)^4]/stddev(x)^4-3(n-1)^2/((n-2)(n-3))
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)regr\_intercept(_y_, _x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#regr\_intercept)

Returns linear regression intercept of input values. `y` is the dependent value. `x` is the independent value.

regr\_slope(_y_, _x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#regr\_slope)

Returns linear regression slope of input values. `y` is the dependent value. `x` is the independent value.

skewness(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#skewness)

Returns the Fisher’s moment coefficient of [skewness](https://wikipedia.org/wiki/Skewness) of all input values.

stddev(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#stddev)

This is an alias for [`stddev_samp()`](https://trino.io/docs/current/functions/aggregate.html#stddev\_samp).

stddev\_pop(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#stddev\_pop)

Returns the population standard deviation of all input values.

stddev\_samp(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#stddev\_samp)

Returns the sample standard deviation of all input values.

variance(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#variance)

This is an alias for [`var_samp()`](https://trino.io/docs/current/functions/aggregate.html#var\_samp).

var\_pop(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#var\_pop)

Returns the population variance of all input values.

var\_samp(_x_) → double[#](https://trino.io/docs/current/functions/aggregate.html#var\_samp)

Returns the sample variance of all input values.

### Lambda aggregate functions[#](https://trino.io/docs/current/functions/aggregate.html#lambda-aggregate-functions) <a href="#lambda-aggregate-functions" id="lambda-aggregate-functions"></a>

reduce\_agg(_inputValue T_, _initialState S_, _inputFunction(S_, _T_, _S)_, _combineFunction(S_, _S_, _S)_) → S[#](https://trino.io/docs/current/functions/aggregate.html#reduce\_agg)

Reduces all input values into a single value. `inputFunction` will be invoked for each non-null input value. In addition to taking the input value, `inputFunction` takes the current state, initially `initialState`, and returns the new state. `combineFunction` will be invoked to combine two states into a new state. The final state is returned:

```
SELECT id, reduce_agg(value, 0, (a, b) -> a + b, (a, b) -> a + b)
FROM (
    VALUES
        (1, 3),
        (1, 4),
        (1, 5),
        (2, 6),
        (2, 7)
) AS t(id, value)
GROUP BY id;
-- (1, 12)
-- (2, 13)

SELECT id, reduce_agg(value, 1, (a, b) -> a * b, (a, b) -> a * b)
FROM (
    VALUES
        (1, 3),
        (1, 4),
        (1, 5),
        (2, 6),
        (2, 7)
) AS t(id, value)
GROUP BY id;
-- (1, 60)
-- (2, 42)
```

The state type must be a boolean, integer, floating-point, char, varchar or date/time/interval.
