# Window

Window functions perform calculations across rows of the query result. They run after the `HAVING` clause but before the `ORDER BY` clause. Invoking a window function requires special syntax using the `OVER` clause to specify the window. A window has three components:

* The partition specification, which separates the input rows into different partitions. This is analogous to how the `GROUP BY` clause separates rows into different groups for aggregate functions.
* The ordering specification, which determines the order in which input rows will be processed by the window function.
* The window frame, which specifies a sliding window of rows to be processed by the function for a given row. If the frame is not specified, it defaults to `RANGE UNBOUNDED PRECEDING`, which is the same as `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`. This frame contains all rows from the start of the partition up to the last peer of the current row. In the absence of `ORDER BY`, all rows are considered peers, so `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` is equivalent to `BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`

For example, the following query ranks orders for each clerk by price:

```sql
SELECT orderkey, clerk, totalprice,
       rank() OVER (PARTITION BY clerk
                    ORDER BY totalprice DESC) AS rnk
FROM orders
ORDER BY clerk, rnk
```

### Aggregate Functions <a id="aggregate-functions"></a>

All [Aggregate Functions](datacharm-agg-fx.md) can be used as window functions by adding the `OVER` clause. The aggregate function is computed for each row over the rows within the current row’s window frame.

For example, the following query produces a rolling sum of order prices by day for each clerk:

```sql
SELECT clerk, orderdate, orderkey, totalprice,
       sum(totalprice) OVER (PARTITION BY clerk
                             ORDER BY orderdate) AS rolling_sum
FROM orders
ORDER BY clerk, orderdate, orderkey
```

### Ranking Functions <a id="ranking-functions"></a>

* `cume_dist`\(\) → bigint

Returns the cumulative distribution of a value in a group of values. The result is the number of rows preceding or peer with the row in the window ordering of the window partition divided by the total number of rows in the window partition. Thus, any tie values in the ordering will evaluate to the same distribution value.

* `dense_rank`\(\) → bigint

Returns the rank of a value in a group of values. This is similar to `rank()`, except that tie values do not produce gaps in the sequence.

* `ntile`\(_n_\) → bigint

Divides the rows for each window partition into `n` buckets ranging from `1` to at most `n`. Bucket values will differ by at most `1`. If the number of rows in the partition does not divide evenly into the number of buckets, then the remainder values are distributed one per bucket, starting with the first bucket.

For example, with `6` rows and `4` buckets, the bucket values would be as follows: `1` `1` `2` `2` `3` `4`

* `percent_rank`\(\) → double

Returns the percentage ranking of a value in group of values. The result is `(r - 1) / (n - 1)` where `r` is the `rank()` of the row and `n` is the total number of rows in the window partition.

* `rank`\(\) → bigint

Returns the rank of a value in a group of values. The rank is one plus the number of rows preceding the row that are not peer with the row. Thus, tie values in the ordering will produce gaps in the sequence. The ranking is performed for each window partition.

* `row_number`\(\) → bigint

Returns a unique, sequential number for each row, starting with one, according to the ordering of rows within the window partition.

### Value Functions <a id="value-functions"></a>

By default, null values are respected. If `IGNORE NULLS` is specified, all rows where `x` is null are excluded from the calculation. If `IGNORE NULLS` is specified and `x` is null for all rows, the `default_value` is returned, or if it is not specified, `null` is returned.

* `first_value`\(_x_\) → \[same as input\]

Returns the first value of the window.

* `last_value`\(_x_\) → \[same as input\]

Returns the last value of the window.

* `nth_value`\(_x_, _offset_\) → \[same as input\]

Returns the value at the specified offset from the beginning of the window. Offsets start at `1`. The offset can be any scalar expression. If the offset is null or greater than the number of values in the window, `null` is returned. It is an error for the offset to be zero or negative.

* `lead`\(_x_\[, _offset_\[, _default\_value_\]\]\) → \[same as input\]

Returns the value at `offset` rows after the current row in the window partition. Offsets start at `0`, which is the current row. The offset can be any scalar expression. The default `offset` is `1`. If the offset is null, `null` is returned. If the offset refers to a row that is not within the partition, the `default_value` is returned, or if it is not specified `null` is returned. The `lead()` function requires that the window ordering be specified. Window frame must not be specified.

* `lag`\(_x_\[, _offset_\[, _default\_value_\]\]\) → \[same as input\]

Returns the value at `offset` rows before the current row in the window partition. Offsets start at `0`, which is the current row. The offset can be any scalar expression. The default `offset` is `1`. If the offset is null, `null` is returned. If the offset refers to a row that is not within the partition, the `default_value` is returned, or if it is not specified `null` is returned. The `lag()` function requires that the window ordering be specified. Window frame must not be specified.

