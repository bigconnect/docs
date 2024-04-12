# T-Digest functions

### Data structures[#](https://trino.io/docs/current/functions/tdigest.html#data-structures) <a href="#data-structures" id="data-structures"></a>

A T-digest is a data sketch which stores approximate percentile information. The Trino type for this data structure is called `tdigest`. T-digests can be merged, and for storage and retrieval they can be cast to and from `VARBINARY`.

### Functions[#](https://trino.io/docs/current/functions/tdigest.html#functions) <a href="#functions" id="functions"></a>

merge(_tdigest_) → tdigest

Aggregates all inputs into a single `tdigest`.

value\_at\_quantile(_tdigest_, _quantile_) → double

Returns the approximate percentile value from the T-digest, given the number `quantile` between 0 and 1.

values\_at\_quantiles(_tdigest_, _quantiles_)

Returns the approximate percentile values as an array, given the input T-digest and an array of values between 0 and 1, which represent the quantiles to return.

tdigest\_agg(_x_) → tdigest[#](https://trino.io/docs/current/functions/tdigest.html#tdigest\_agg)

Composes all input values of `x` into a `tdigest`. `x` can be of any numeric type.

tdigest\_agg(_x_, _w_) → tdigest

Composes all input values of `x` into a `tdigest` using the per-item weight `w`. `w` must be greater or equal than 1. `x` and `w` can be of any numeric type.
