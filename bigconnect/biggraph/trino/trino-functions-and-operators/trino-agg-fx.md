# Aggregate

Aggregate functions operate on a set of values to compute a single result.

Except for `count()`, `count_if()`, `max_by()`, `min_by()` and `approx_distinct()`, all of these aggregate functions ignore null values and return null for no input rows or when all values are null. For example, `sum()` returns null rather than zero and `avg()` does not include null values in the count. The `coalesce` function can be used to convert null into zero.

### Ordering During Aggregation <a href="#ordering-during-aggregation" id="ordering-during-aggregation"></a>

Some aggregate functions such as `array_agg()` produce different results depending on the order of input values. This ordering can be specified by writing an ORDER BY Clause within the aggregate function:

```sql
array_agg(x ORDER BY y DESC)
array_agg(x ORDER BY x, y, z)
```

### Filtering During Aggregation <a href="#filtering-during-aggregation" id="filtering-during-aggregation"></a>

The `FILTER` keyword can be used to remove rows from aggregation processing with a condition expressed using a `WHERE` clause. This is evaluated for each row before it is used in the aggregation and is supported for all aggregate functions.

```sql
aggregate_function(...) FILTER (WHERE <condition>)
```

A common and very useful example is to use `FILTER` to remove nulls from consideration when using `array_agg`:

```sql
SELECT array_agg(name) FILTER (WHERE name IS NOT NULL)
FROM region;
```

As another example, imagine you want to add a condition on the count for Iris flowers, modifying the following query:

```sql
SELECT species,
       count(*) AS count
FROM iris
GROUP BY species;
```

```
species    | count
-----------+-------
setosa     |   50
virginica  |   50
versicolor |   50
```

If you just use a normal `WHERE` statement you loose information:

```sql
SELECT species,
    count(*) AS count
FROM iris
WHERE petal_length_cm > 4
GROUP BY species;
```

```
species    | count
-----------+-------
virginica  |   50
versicolor |   34
```

Using a filter you retain all information:

```sql
SELECT species,
       count(*) FILTER (where petal_length_cm > 4) AS count
FROM iris
GROUP BY species;
```

```
species    | count
-----------+-------
virginica  |   50
setosa     |    0
versicolor |   34
```

### General Aggregate Functions <a href="#general-aggregate-functions" id="general-aggregate-functions"></a>

* `arbitrary`(_x_) → \[same as input]

Returns an arbitrary non-null value of `x`, if one exists.

* `array_agg`(_x_) → array<\[same as input]>

Returns an array created from the input `x` elements.

* `avg`(_x_) → double

Returns the average (arithmetic mean) of all input values.

* `avg`(_time interval type_) → time interval type

Returns the average interval length of all input values.

* `bool_and`(_boolean_) → boolean

Returns `TRUE` if every input value is `TRUE`, otherwise `FALSE`.

* `bool_or`(_boolean_) → boolean

Returns `TRUE` if any input value is `TRUE`, otherwise `FALSE`.

* `checksum`(_x_) → varbinary

Returns an order-insensitive checksum of the given values.

* `count`(_\*_ ) → bigint

Returns the number of input rows.

* `count`(_x_) → bigint

Returns the number of non-null input values.

* `count_if`(_x_) → bigint

Returns the number of `TRUE` input values. This function is equivalent to `count(CASE WHEN x THEN 1 END)`.

* `every`(_boolean_) → boolean

This is an alias for `bool_and()`.

* `geometric_mean`(_x_) → double

Returns the geometric mean of all input values.

* `max`(_x_) → \[same as input]

Returns the maximum value of all input values.

* `max`(_x_, _n_) → array<\[same as x]>

Returns `n` largest values of all input values of `x`.

* `max_by`(_x_, _y_) → \[same as x]

Returns the value of `x` associated with the maximum value of `y` over all input values.

* `max_by`(_x_, _y_, _n_) → array<\[same as x]>

Returns `n` values of `x` associated with the `n` largest of all input values of `y` in descending order of `y`.

* `min`(_x_) → \[same as input]

Returns the minimum value of all input values.

* `min`(_x_, _n_) → array<\[same as x]>

Returns `n` smallest values of all input values of `x`.

* `min_by`(_x_, _y_) → \[same as x]

Returns the value of `x` associated with the minimum value of `y` over all input values.`min_by`(_x_, _y_, _n_) → array<\[same as x]>

Returns `n` values of `x` associated with the `n` smallest of all input values of `y` in ascending order of `y`.

* `sum`(_x_) → \[same as input]

Returns the sum of all input values.

### Bitwise Aggregate Functions <a href="#bitwise-aggregate-functions" id="bitwise-aggregate-functions"></a>

* `bitwise_and_agg`(_x_) → bigint

Returns the bitwise AND of all input values in 2’s complement representation.

* `bitwise_or_agg`(_x_) → bigint

Returns the bitwise OR of all input values in 2’s complement representation.

### Map Aggregate Functions <a href="#map-aggregate-functions" id="map-aggregate-functions"></a>

* `histogram`(_x) -> map(K_, _bigint_)

Returns a map containing the count of the number of times each input value occurs.

* `map_agg`(_key_, _value) -> map(K_, _V_)

Returns a map created from the input `key` / `value` pairs.

* `map_union`(_x(K_, _V)) -> map(K_, _V_)

Returns the union of all the input maps. If a key is found in multiple input maps, that key’s value in the resulting map comes from an arbitrary input map.

* `multimap_agg`(_key_, _value) -> map(K_, _array(V)_)

Returns a multimap created from the input `key` / `value` pairs. Each key can be associated with multiple values.

### Approximate Aggregate Functions <a href="#approximate-aggregate-functions" id="approximate-aggregate-functions"></a>

* `approx_distinct`(_x_) → bigint

Returns the approximate number of distinct input values. This function provides an approximation of `count(DISTINCT x)`. Zero is returned if all input values are null.

This function should produce a standard error of 2.3%, which is the standard deviation of the (approximately normal) error distribution over all possible sets. It does not guarantee an upper bound on the error for any specific input set.`approx_distinct`(_x_, _e_) → bigint

Returns the approximate number of distinct input values. This function provides an approximation of `count(DISTINCT x)`. Zero is returned if all input values are null.

This function should produce a standard error of no more than `e`, which is the standard deviation of the (approximately normal) error distribution over all possible sets. It does not guarantee an upper bound on the error for any specific input set. The current implementation of this function requires that `e` be in the range of `[0.0040625, 0.26000]`.

* `approx_most_frequent`(_buckets_, _value_, _capacity_) → map<\[same as value], bigint>

Computes the top frequent values up to `buckets` elements approximately. Approximate estimation of the function enables us to pick up the frequent values with less memory. Larger `capacity` improves the accuracy of underlying algorithm with sacrificing the memory capacity. The returned value is a map containing the top elements with corresponding estimated frequency.

The error of the function depends on the permutation of the values and its cardinality. We can set the capacity same as the cardinality of the underlying data to achieve the least error.

`buckets` and `capacity` must be `bigint`. `value` can be numeric or string type.

The function uses the stream summary data structure proposed in the paper [Efficient Computation of Frequent and Top-k Elements in Data Streams](https://www.cse.ust.hk/\~raywong/comp5331/References/EfficientComputationOfFrequentAndTop-kElementsInDataStreams.pdf) by A. Metwalley, D. Agrawl and A. Abbadi.

* `approx_percentile`(_x_, _percentage_) → \[same as x]

Returns the approximate percentile for all input values of `x` at the given `percentage`. The value of `percentage` must be between zero and one and must be constant for all input rows.

* `approx_percentile`(_x_, _percentages_) → array<\[same as x]>

Returns the approximate percentile for all input values of `x` at each of the specified percentages. Each element of the `percentages` array must be between zero and one, and the array must be constant for all input rows.

* `approx_percentile`(_x_, _w_, _percentage_) → \[same as x]

Returns the approximate weighed percentile for all input values of `x` using the per-item weight `w` at the percentage `p`. Weights must be strictly positive. Integer-value weights can be thought of as a replication count for the value `x` in the percentile set. The value of `p` must be between zero and one and must be constant for all input rows.

* `approx_percentile`(_x_, _w_, _percentage_, _accuracy_) → \[same as x]

Returns the approximate weighed percentile for all input values of `x` using the per-item weight `w` at the percentage `p`, with a maximum rank error of `accuracy`. Weights must be strictly positive. Integer-value weights can be thought of as a replication count for the value `x` in the percentile set. The value of `p` must be between zero and one and must be constant for all input rows. `accuracy` must be a value greater than zero and less than one, and it must be constant for all input rows.

* `approx_percentile`(_x_, _w_, _percentages_) → array<\[same as x]>

Returns the approximate weighed percentile for all input values of `x` using the per-item weight `w` at each of the given percentages specified in the array. Weights must be strictly positive. Integer-value weights can be thought of as a replication count for the value `x` in the percentile set. Each element of the array must be between zero and one, and the array must be constant for all input rows.

* `approx_set`(_x_) → HyperLogLog

See HyperLogLog Functions.

* `merge`(_x_) → HyperLogLog

See HyperLogLog Functions.

* `merge`(_qdigest(T)) -> qdigest(T_)

See Quantile Digest Functions.

* `numeric_histogram`(_buckets_, _value_) → map\<double, double>

Computes an approximate histogram with up to `buckets` number of buckets for all `value`s. This function is equivalent to the variant of `numeric_histogram()` that takes a `weight`, with a per-item weight of `1`.

* `numeric_histogram`(_buckets_, _value_, _weight_) → map\<double, double>

Computes an approximate histogram with up to `buckets` number of buckets for all `value`s with a per-item weight of `weight`. The algorithm is based loosely on: _Yael Ben-Haim and Elad Tom-Tov, "A streaming parallel decision tree algorithm", J. Machine Learning Research 11 (2010), pp. 849--872._

`buckets` must be a `bigint`. `value` and `weight` must be numeric.

* `qdigest_agg`(_x_) → qdigest<\[same as x]>

See Quantile Digest Functions.

* `qdigest_agg`(_x_, _w_) → qdigest<\[same as x]>

See Quantile Digest Functions.

* `qdigest_agg`(_x_, _w_, _accuracy_) → qdigest<\[same as x]>

See Quantile Digest Functions.

### Statistical Aggregate Functions <a href="#statistical-aggregate-functions" id="statistical-aggregate-functions"></a>

* `corr`(_y_, _x_) → double

Returns correlation coefficient of input values.

* `covar_pop`(_y_, _x_) → double

Returns the population covariance of input values.

* `covar_samp`(_y_, _x_) → double

Returns the sample covariance of input values.

* `kurtosis`(_x_) → double

Returns the excess kurtosis of all input values. Unbiased estimate using the following expression:

```
kurtosis(x) = n(n+1)/((n-1)(n-2)(n-3))sum[(x_i-mean)^4]/stddev(x)^4-3(n-1)^2/((n-2)(n-3))
```

* `regr_intercept`(_y_, _x_) → double

Returns linear regression intercept of input values. `y` is the dependent value. `x` is the independent value.

* `regr_slope`(_y_, _x_) → double

Returns linear regression slope of input values. `y` is the dependent value. `x` is the independent value.

* `skewness`(_x_) → double

Returns the skewness of all input values.

* `stddev`(_x_) → double

This is an alias for `stddev_samp()`.

* `stddev_pop`(_x_) → double

Returns the population standard deviation of all input values.

* `stddev_samp`(_x_) → double

Returns the sample standard deviation of all input values.

`pvariance`(_x_) → double

This is an alias for `var_samp()`.

`var_pop`(_x_) → double

Returns the population variance of all input values.`var_samp`(_x_) → double

Returns the sample variance of all input values.

### Lambda Aggregate Functions <a href="#lambda-aggregate-functions" id="lambda-aggregate-functions"></a>

`reduce_agg`(_inputValue T_, _initialState S_, _inputFunction(S_, _T_, _S)_, _combineFunction(S_, _S_, _S)_) → S

Reduces all input values into a single value. `inputFunction` will be invoked for each non-null input value. In addition to taking the input value, `inputFunction` takes the current state, initially `initialState`, and returns the new state. `combineFunction` will be invoked to combine two states into a new state. The final state is returned:

```sql
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

The state type must be a boolean, integer, floating-point, or date/time/interval.
