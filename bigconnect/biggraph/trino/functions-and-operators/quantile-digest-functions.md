# Quantile digest functions

### Data structures[#](https://trino.io/docs/current/functions/qdigest.html#data-structures) <a href="#data-structures" id="data-structures"></a>

A quantile digest is a data sketch which stores approximate percentile information. The Trino type for this data structure is called `qdigest`, and it takes a parameter which must be one of `bigint`, `double` or `real` which represent the set of numbers that may be ingested by the `qdigest`. They may be merged without losing precision, and for storage and retrieval they may be cast to/from `VARBINARY`.

### Functions[#](https://trino.io/docs/current/functions/qdigest.html#functions) <a href="#functions" id="functions"></a>

merge(_qdigest_) → qdigest

Merges all input `qdigest`s into a single `qdigest`.

value\_at\_quantile(_qdigest(T)_, _quantile_) → T[#](https://trino.io/docs/current/functions/qdigest.html#value\_at\_quantile)

Returns the approximate percentile value from the quantile digest given the number `quantile` between 0 and 1.

quantile\_at\_value(_qdigest(T)_, _T_) → quantile[#](https://trino.io/docs/current/functions/qdigest.html#quantile\_at\_value)

Returns the approximate `quantile` number between 0 and 1 from the quantile digest given an input value. Null is returned if the quantile digest is empty or the input value is outside of the range of the quantile digest.

values\_at\_quantiles(_qdigest(T)_, _quantiles) -> array(T_)[#](https://trino.io/docs/current/functions/qdigest.html#values\_at\_quantiles)

Returns the approximate percentile values as an array given the input quantile digest and array of values between 0 and 1 which represent the quantiles to return.

qdigest\_agg(_x) -> qdigest(\[same as x]_)[#](https://trino.io/docs/current/functions/qdigest.html#qdigest\_agg)

Returns the `qdigest` which is composed of all input values of `x`.

qdigest\_agg(_x, w) -> qdigest(\[same as x]_)

Returns the `qdigest` which is composed of all input values of `x` using the per-item weight `w`.

qdigest\_agg(_x, w, accuracy) -> qdigest(\[same as x]_)

Returns the `qdigest` which is composed of all input values of `x` using the per-item weight `w` and maximum error of `accuracy`. `accuracy` must be a value greater than zero and less than one, and it must be constant for all input rows.
