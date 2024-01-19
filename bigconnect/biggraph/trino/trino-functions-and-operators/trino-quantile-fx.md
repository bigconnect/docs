# Quantile Digest Functions

DataCharm implements the `approx_percentile` function with the quantile digest data structure. The underlying data structure, `qdigest`, is exposed as a data type in DataCharm, and can be created, queried and stored separately from `approx_percentile`.

### Data Structures <a id="data-structures"></a>

A quantile digest is a data sketch which stores approximate percentile information. The type for this data structure is called `qdigest`, and it takes a parameter which must be one of `bigint`, `double` or `real` which represent the set of numbers that may be ingested by the `qdigest`. They may be merged without losing precision, and for storage and retrieval they may be cast to/from `VARBINARY`.

### Functions <a id="functions"></a>

* `merge`\(_qdigest_\) → qdigest

Merges all input `qdigest`s into a single `qdigest`.

* `value_at_quantile`\(_qdigest\(T\)_, _quantile_\) → T

Returns the approximate percentile values from the quantile digest given the number `quantile` between 0 and 1.

* `values_at_quantiles`\(_qdigest\(T\)_, _quantiles_\) → T

Returns the approximate percentile values as an array given the input quantile digest and array of values between 0 and 1 which represent the quantiles to return.

* `qdigest_agg`\(_x_\) → qdigest&lt;\[same as x\]&gt;

Returns the `qdigest` which is composed of all input values of `x`.

* `qdigest_agg`\(_x_, _w_\) → qdigest&lt;\[same as x\]&gt;

Returns the `qdigest` which is composed of all input values of `x` using the per-item weight `w`.

* `qdigest_agg`\(_x_, _w_, _accuracy_\) → qdigest&lt;\[same as x\]&gt;

Returns the `qdigest` which is composed of all input values of `x` using the per-item weight `w` and maximum error of `accuracy`. `accuracy` must be a value greater than zero and less than one, and it must be constant for all input rows.

