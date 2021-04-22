# Math

### Mathematical Operators <a id="mathematical-operators"></a>

| Operator | Description |
| :--- | :--- |
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division \(integer division performs truncation\) |
| `%` | Modulus \(remainder\) |

### Mathematical Functions <a id="mathematical-functions"></a>

* `abs`\(_x_\) → \[same as input\]

 Returns the absolute value of `x`.

* `cbrt`\(_x_\) → double

Returns the cube root of `x`.

* `ceil`\(_x_\) → \[same as input\]

This is an alias for `ceiling()`.

* `ceiling`\(_x_\) → \[same as input\]

Returns `x` rounded up to the nearest integer.

* `degrees`\(_x_\) → double

Converts angle `x` in radians to degrees.

* `e`\(\) → double

Returns the constant Euler’s number.

* `exp`\(_x_\) → double

Returns Euler’s number raised to the power of `x`.

* `floor`\(_x_\) → \[same as input\]

Returns `x` rounded down to the nearest integer.

* `ln`\(_x_\) → double

Returns the natural logarithm of `x`.

* `log`\(_b_, _x_\) → double

Returns the base `b` logarithm of `x`.

* `log2`\(_x_\) → double

Returns the base 2 logarithm of `x`.

* `log10`\(_x_\) → double

Returns the base 10 logarithm of `x`.

* `mod`\(_n_, _m_\) → \[same as input\]

Returns the modulus \(remainder\) of `n` divided by `m`.

* `pi`\(\) → double

Returns the constant Pi.

* `pow`\(_x_, _p_\) → double

This is an alias for `power()`.

* `power`\(_x_, _p_\) → double

Returns `x` raised to the power of `p`.

* `radians`\(_x_\) → double

Converts angle `x` in degrees to radians.`round`\(_x_\) → \[same as input\]

Returns `x` rounded to the nearest integer.`round`\(_x_, _d_\) → \[same as input\]

Returns `x` rounded to `d` decimal places.`sign`\(_x_\) → \[same as input\]

Returns the signum function of `x`, that is:

* 0 if the argument is 0,
* 1 if the argument is greater than 0,
* -1 if the argument is less than 0.

For double arguments, the function additionally returns:

* NaN if the argument is NaN,
* 1 if the argument is +Infinity,
* -1 if the argument is -Infinity.



* `sqrt`\(_x_\) → double

Returns the square root of `x`.

* `truncate`\(_x_\) → double

Returns `x` rounded to integer by dropping digits after decimal point.

* `width_bucket`\(_x_, _bound1_, _bound2_, _n_\) → bigint

Returns the bin number of `x` in an equi-width histogram with the specified `bound1` and `bound2` bounds and `n` number of buckets.

* `width_bucket`\(_x_, _bins_\) → bigint

Returns the bin number of `x` according to the bins specified by the array `bins`. The `bins` parameter must be an array of doubles and is assumed to be in sorted ascending order.

### Random Functions <a id="random-functions"></a>

* `rand`\(\) → double

This is an alias for `random()`.

* `random`\(\) → double

Returns a pseudo-random value in the range 0.0 &lt;= x &lt; 1.0.

* `random`\(_n_\) → \[same as input\]

Returns a pseudo-random number between 0 and n \(exclusive\).

* `random`\(_m_, _n_\) → \[same as input\]

Returns a pseudo-random number between m and n \(exclusive\).

### Trigonometric Functions <a id="trigonometric-functions"></a>

All trigonometric function arguments are expressed in radians. See unit conversion functions `degrees()` and `radians()`.

* `acos`\(_x_\) → double

Returns the arc cosine of `x`.

* `asin`\(_x_\) → double

Returns the arc sine of `x`.

* `atan`\(_x_\) → double

Returns the arc tangent of `x`.

* `atan2`\(_y_, _x_\) → double

Returns the arc tangent of `y / x`.

* `cos`\(_x_\) → double

Returns the cosine of `x`.

* `cosh`\(_x_\) → double

Returns the hyperbolic cosine of `x`.

* `sin`\(_x_\) → double

Returns the sine of `x`.

* `tan`\(_x_\) → double

Returns the tangent of `x`.

* `tanh`\(_x_\) → double

Returns the hyperbolic tangent of `x`.

### Floating Point Functions <a id="floating-point-functions"></a>

* `infinity`\(\) → double

Returns the constant representing positive infinity.

* `is_finite`\(_x_\) → boolean

Determine if `x` is finite.

* `is_infinite`\(_x_\) → boolean

Determine if `x` is infinite.

* `is_nan`\(_x_\) → boolean

Determine if `x` is not-a-number.

* `nan`\(\) → double

Returns the constant representing not-a-number.

### Base Conversion Functions <a id="base-conversion-functions"></a>

* `from_base`\(_string_, _radix_\) → bigint

Returns the value of `string` interpreted as a base-`radix` number.

* `to_base`\(_x_, _radix_\) → varchar

Returns the base-`radix` representation of `x`.

### Statistical Functions <a id="statistical-functions"></a>

* `cosine_similarity`\(_x_, _y_\) → double

Returns the cosine similarity between the sparse vectors `x` and `y`:

```text
SELECT cosine_similarity(MAP(ARRAY['a'], ARRAY[1.0]), MAP(ARRAY['a'], ARRAY[2.0])); -- 1.0
```

* `wilson_interval_lower`\(_successes_, _trials_, _z_\) → double

Returns the lower bound of the Wilson score interval of a Bernoulli trial process at a confidence specified by the z-score `z`.

* `wilson_interval_upper`\(_successes_, _trials_, _z_\) → double

Returns the upper bound of the Wilson score interval of a Bernoulli trial process at a confidence specified by the z-score `z`.

### Cumulative Distribution Functions <a id="cumulative-distribution-functions"></a>

* `beta_cdf`\(_a_, _b_, _v_\) → double

Compute the Beta cdf with given a, b parameters: P\(N &lt; v; a, b\). The a, b parameters must be positive real numbers and value v must be a real value. The value v must lie on the interval \[0, 1\].

* `inverse_beta_cdf`\(_a_, _b_, _p_\) → double

Compute the inverse of the Beta cdf with given a, b parameters for the cumulative probability \(p\): P\(N &lt; n\). The a, b parameters must be positive real values. The probability p must lie on the interval \[0, 1\].

* `inverse_normal_cdf`\(_mean_, _sd_, _p_\) → double

Compute the inverse of the Normal cdf with given mean and standard deviation \(sd\) for the cumulative probability \(p\): P\(N &lt; n\). The mean must be a real value and the standard deviation must be a real and positive value. The probability p must lie on the interval \(0, 1\).

* `normal_cdf`\(_mean_, _sd_, _v_\) → double

Compute the Normal cdf with given mean and standard deviation \(sd\): P\(N &lt; v; mean, sd\). The mean and value v must be real values and the standard deviation must be a real and positive value.

