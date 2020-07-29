# Bitwise

## Bitwise Functions <a id="functions-bitwise--page-root"></a>

* `bit_count`\(_x_, _bits_\) → bigint

Count the number of bits set in `x` \(treated as `bits`-bit signed integer\) in 2’s complement representation:

```sql
SELECT bit_count(9, 64); -- 2
SELECT bit_count(9, 8); -- 2
SELECT bit_count(-7, 64); -- 62
SELECT bit_count(-7, 8); -- 6
```

* `bitwise_and`\(_x_, _y_\) → bigint

Returns the bitwise AND of `x` and `y` in 2’s complement representation.

* `bitwise_not`\(_x_\) → bigint

Returns the bitwise NOT of `x` in 2’s complement representation.

* `bitwise_or`\(_x_, _y_\) → bigint

Returns the bitwise OR of `x` and `y` in 2’s complement representation.

* `bitwise_xor`\(_x_, _y_\) → bigint

Returns the bitwise XOR of `x` and `y` in 2’s complement representation.

* `bitwise_and_agg`\(_x_\) → bigint

Returns the bitwise AND of all input values in 2’s complement representation.

* `bitwise_or_agg`\(_x_\) → bigint

Returns the bitwise OR of all input values in 2’s complement representation.

