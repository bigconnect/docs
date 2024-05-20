# Mathematical functions and operators

### Mathematical operators[#](https://trino.io/docs/current/functions/math.html#mathematical-operators) <a href="#mathematical-operators" id="mathematical-operators"></a>

| Operator | Description                                     |
| -------- | ----------------------------------------------- |
| `+`      | Addition                                        |
| `-`      | Subtraction                                     |
| `*`      | Multiplication                                  |
| `/`      | Division (integer division performs truncation) |
| `%`      | Modulus (remainder)                             |

### Mathematical functions[#](https://trino.io/docs/current/functions/math.html#mathematical-functions) <a href="#mathematical-functions" id="mathematical-functions"></a>

abs(_x_) → \[same as input][#](https://trino.io/docs/current/functions/math.html#abs)

Returns the absolute value of `x`.

cbrt(_x_) → double[#](https://trino.io/docs/current/functions/math.html#cbrt)

Returns the cube root of `x`.

ceil(_x_) → \[same as input][#](https://trino.io/docs/current/functions/math.html#ceil)

This is an alias for `ceiling()`.

ceiling(_x_) → \[same as input][#](https://trino.io/docs/current/functions/math.html#ceiling)

Returns `x` rounded up to the nearest integer.

degrees(_x_) → double[#](https://trino.io/docs/current/functions/math.html#degrees)

Converts angle `x` in radians to degrees.

e() → double[#](https://trino.io/docs/current/functions/math.html#e)

Returns the constant Euler’s number.

exp(_x_) → double[#](https://trino.io/docs/current/functions/math.html#exp)

Returns Euler’s number raised to the power of `x`.

floor(_x_) → \[same as input][#](https://trino.io/docs/current/functions/math.html#floor)

Returns `x` rounded down to the nearest integer.

ln(_x_) → double[#](https://trino.io/docs/current/functions/math.html#ln)

Returns the natural logarithm of `x`.

log(_b_, _x_) → double[#](https://trino.io/docs/current/functions/math.html#log)

Returns the base `b` logarithm of `x`.

log2(_x_) → double[#](https://trino.io/docs/current/functions/math.html#log2)

Returns the base 2 logarithm of `x`.

log10(_x_) → double[#](https://trino.io/docs/current/functions/math.html#log10)

Returns the base 10 logarithm of `x`.

mod(_n_, _m_) → \[same as input][#](https://trino.io/docs/current/functions/math.html#mod)

Returns the modulus (remainder) of `n` divided by `m`.

pi() → double[#](https://trino.io/docs/current/functions/math.html#pi)

Returns the constant Pi.

pow(_x_, _p_) → double[#](https://trino.io/docs/current/functions/math.html#pow)

This is an alias for `power()`.

power(_x_, _p_) → double[#](https://trino.io/docs/current/functions/math.html#power)

Returns `x` raised to the power of `p`.

radians(_x_) → double[#](https://trino.io/docs/current/functions/math.html#radians)

Converts angle `x` in degrees to radians.

round(_x_) → \[same as input][#](https://trino.io/docs/current/functions/math.html#round)

Returns `x` rounded to the nearest integer.

round(_x_, _d_) → \[same as input]

Returns `x` rounded to `d` decimal places.

sign(_x_) → \[same as input][#](https://trino.io/docs/current/functions/math.html#sign)

Returns the signum function of `x`, that is:

* 0 if the argument is 0,
* 1 if the argument is greater than 0,
* \-1 if the argument is less than 0.

For double arguments, the function additionally returns:

* NaN if the argument is NaN,
* 1 if the argument is +Infinity,
* \-1 if the argument is -Infinity.

sqrt(_x_) → double[#](https://trino.io/docs/current/functions/math.html#sqrt)

Returns the square root of `x`.

truncate(_x_) → double[#](https://trino.io/docs/current/functions/math.html#truncate)

Returns `x` rounded to integer by dropping digits after decimal point.

width\_bucket(_x_, _bound1_, _bound2_, _n_) → bigint[#](https://trino.io/docs/current/functions/math.html#width\_bucket)

Returns the bin number of `x` in an equi-width histogram with the specified `bound1` and `bound2` bounds and `n` number of buckets.

width\_bucket(_x_, _bins_) → bigint

Returns the bin number of `x` according to the bins specified by the array `bins`. The `bins` parameter must be an array of doubles and is assumed to be in sorted ascending order.

### Random functions[#](https://trino.io/docs/current/functions/math.html#random-functions) <a href="#random-functions" id="random-functions"></a>

rand() → double[#](https://trino.io/docs/current/functions/math.html#rand)

This is an alias for `random()`.

random() → double[#](https://trino.io/docs/current/functions/math.html#random)

Returns a pseudo-random value in the range 0.0 <= x < 1.0.

random(_n_) → \[same as input]

Returns a pseudo-random number between 0 and n (exclusive).

random(_m_, _n_) → \[same as input]

Returns a pseudo-random number between m and n (exclusive).

### Trigonometric functions[#](https://trino.io/docs/current/functions/math.html#trigonometric-functions) <a href="#trigonometric-functions" id="trigonometric-functions"></a>

All trigonometric function arguments are expressed in radians. See unit conversion functions `degrees()` and `radians()`.

acos(_x_) → double#

Returns the arc cosine of `x`.

asin(_x_) → double[#](https://trino.io/docs/current/functions/math.html#asin)

Returns the arc sine of `x`.

atan(_x_) → double[#](https://trino.io/docs/current/functions/math.html#atan)

Returns the arc tangent of `x`.

atan2(_y_, _x_) → double[#](https://trino.io/docs/current/functions/math.html#atan2)

Returns the arc tangent of `y / x`.

cos(_x_) → double[#](https://trino.io/docs/current/functions/math.html#cos)

Returns the cosine of `x`.

cosh(_x_) → double[#](https://trino.io/docs/current/functions/math.html#cosh)

Returns the hyperbolic cosine of `x`.

sin(_x_) → double[#](https://trino.io/docs/current/functions/math.html#sin)

Returns the sine of `x`.

sinh(_x_) → double[#](https://trino.io/docs/current/functions/math.html#sinh)

Returns the hyperbolic sine of `x`.

tan(_x_) → double[#](https://trino.io/docs/current/functions/math.html#tan)

Returns the tangent of `x`.

tanh(_x_) → double[#](https://trino.io/docs/current/functions/math.html#tanh)

Returns the hyperbolic tangent of `x`.

### Floating point functions[#](https://trino.io/docs/current/functions/math.html#floating-point-functions) <a href="#floating-point-functions" id="floating-point-functions"></a>

infinity() → double[#](https://trino.io/docs/current/functions/math.html#infinity)

Returns the constant representing positive infinity.

is\_finite(_x_) → boolean[#](https://trino.io/docs/current/functions/math.html#is\_finite)

Determine if `x` is finite.

is\_infinite(_x_) → boolean[#](https://trino.io/docs/current/functions/math.html#is\_infinite)

Determine if `x` is infinite.

is\_nan(_x_) → boolean[#](https://trino.io/docs/current/functions/math.html#is\_nan)

Determine if `x` is not-a-number.

nan() → double[#](https://trino.io/docs/current/functions/math.html#nan)

Returns the constant representing not-a-number.

### Base conversion functions[#](https://trino.io/docs/current/functions/math.html#base-conversion-functions) <a href="#base-conversion-functions" id="base-conversion-functions"></a>

from\_base(_string_, _radix_) → bigint[#](https://trino.io/docs/current/functions/math.html#from\_base)

Returns the value of `string` interpreted as a base-`radix` number.

to\_base(_x_, _radix_) → varchar[#](https://trino.io/docs/current/functions/math.html#to\_base)

Returns the base-`radix` representation of `x`.

### Statistical functions[#](https://trino.io/docs/current/functions/math.html#statistical-functions) <a href="#statistical-functions" id="statistical-functions"></a>

cosine\_similarity(_x_, _y_) → double[#](https://trino.io/docs/current/functions/math.html#cosine\_similarity)

Returns the cosine similarity between the sparse vectors `x` and `y`:

```
SELECT cosine_similarity(MAP(ARRAY['a'], ARRAY[1.0]), MAP(ARRAY['a'], ARRAY[2.0])); -- 1.0
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)wilson\_interval\_lower(_successes_, _trials_, _z_) → double[#](https://trino.io/docs/current/functions/math.html#wilson\_interval\_lower)

Returns the lower bound of the Wilson score interval of a Bernoulli trial process at a confidence specified by the z-score `z`.

wilson\_interval\_upper(_successes_, _trials_, _z_) → double[#](https://trino.io/docs/current/functions/math.html#wilson\_interval\_upper)

Returns the upper bound of the Wilson score interval of a Bernoulli trial process at a confidence specified by the z-score `z`.

### Cumulative distribution functions[#](https://trino.io/docs/current/functions/math.html#cumulative-distribution-functions) <a href="#cumulative-distribution-functions" id="cumulative-distribution-functions"></a>

beta\_cdf(_a_, _b_, _v_) → double[#](https://trino.io/docs/current/functions/math.html#beta\_cdf)

Compute the Beta cdf with given a, b parameters: P(N < v; a, b). The a, b parameters must be positive real numbers and value v must be a real value. The value v must lie on the interval \[0, 1].

inverse\_beta\_cdf(_a_, _b_, _p_) → double[#](https://trino.io/docs/current/functions/math.html#inverse\_beta\_cdf)

Compute the inverse of the Beta cdf with given a, b parameters for the cumulative probability (p): P(N < n). The a, b parameters must be positive real values. The probability p must lie on the interval \[0, 1].

inverse\_normal\_cdf(_mean_, _sd_, _p_) → double[#](https://trino.io/docs/current/functions/math.html#inverse\_normal\_cdf)

Compute the inverse of the Normal cdf with given mean and standard deviation (sd) for the cumulative probability (p): P(N < n). The mean must be a real value and the standard deviation must be a real and positive value. The probability p must lie on the interval (0, 1).

normal\_cdf(_mean_, _sd_, _v_) → double[#](https://trino.io/docs/current/functions/math.html#normal\_cdf)

Compute the Normal cdf with given mean and standard deviation (sd): P(N < v; mean, sd). The mean and value v must be real values and the standard deviation must be a real and positive value.
