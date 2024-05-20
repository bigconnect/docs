# Numeric Functions

### ABS

```
ABS (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Returns the absolute value of an expression.

#### Example <a href="#example" id="example"></a>

Calculate an absolute value:

```
SELECT transfer_id, ABS (price) from Transfers;
```

### ACOS

```
ACOS (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the arc cosine. This method returns a `double`.

#### Example <a href="#example-2" id="example-2"></a>

Get arc cos value:

```
SELECT acos(angle) FROM Triangles;
```

### ASIN

```
ASIN (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the arc sine. This method returns a `double`.

#### Example <a href="#example-3" id="example-3"></a>

Calculate an arc sine:

```
SELECT asin(angle) FROM Triangles;
```

### ATAN

```
ATAN (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the arc tangent. This method returns a `double`.

#### Example <a href="#example-4" id="example-4"></a>

Get an arc tangent:

```
SELECT atan(angle) FROM Triangles;
```

### COS

```
COS (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the trigonometric cosine. This method returns a `double`.

#### Example <a href="#example-5" id="example-5"></a>

Get a cosine:

```
SELECT COS(angle) FROM Triangles;
```

### COSH

```
COSH (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the hyperbolic cosine. This method returns a `double`.

#### Example <a href="#example-6" id="example-6"></a>

Get an hyperbolic cosine:

```
SELECT HCOS(angle) FROM Triangles;
```

### COT

```
COT (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the trigonometric cotangent (1/TAN(ANGLE)). This method returns a `double`.

#### Example <a href="#example-7" id="example-7"></a>

Gets a​ trigonometric cotangent:

```
SELECT COT(angle) FROM Triangles;
```

### SIN

```
SIN (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the trigonometric sine. This method returns a `double`.

#### Example <a href="#example-8" id="example-8"></a>

Get a trigonometric sine:

```
SELECT SIN(angle) FROM Triangles;
```

### SINH

```
SINH (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the hyperbolic sine. This method returns a `double`.

#### Example <a href="#example-9" id="example-9"></a>

Get a hyperbolic sine:

```
SELECT SINH(angle) FROM Triangles;
```

### TAN

```
TAN (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the trigonometric tangent. This method returns a `double`.

#### Example <a href="#example-10" id="example-10"></a>

Get a trigonometric tangent:

```
SELECT TAN(angle) FROM Triangles;
```

### TANH

```
TANH (expression)
```

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Description

Calculates the hyperbolic tangent. This method returns a `double`.

#### Example <a href="#example-11" id="example-11"></a>

Get a hyperbolic tangent:

```
SELECT TANH(angle) FROM Triangles;
```

### ATAN2

```
ATAN2 (y, x)
```

#### Parameters

* `x and y` - the arguments.

#### Description

Calculates the angle when converting the rectangular coordinates to polar coordinates. This method returns a `double`.

#### Example <a href="#example-12" id="example-12"></a>

Get a hyperbolic tangent:

```
SELECT ATAN2(X, Y) FROM Triangles;
```

### BITAND

```
BITAND (y, x)
```

#### Parameters

* `x and y` - the arguments.

#### Description

The bitwise AND operation. This method returns a `long`.

#### Example <a href="#example-13" id="example-13"></a>

```
SELECT BITAND(X, Y) FROM Triangles;
```

### BITGET

```
BITGET (y, x)
```

#### Parameters

* `x and y` - the arguments.

#### Description

Returns true if and only if the first parameter has a bit set in the position specified by the second parameter. This method returns a `boolean`. The second parameter is zero-indexed; the least significant bit has position 0.

#### Example <a href="#example-14" id="example-14"></a>

Check that 3rd bit is 1:

```
SELECT BITGET(X, 3) from Triangles;
```

### BITOR

```
BITOR (y, x)
```

#### Parameters

* `x and y` - the arguments.

#### Description

The bitwise OR operation. This method returns a `long`.

#### Example <a href="#example-15" id="example-15"></a>

Calculate OR between two fields:

```
SELECT BITGET(X, Y) from Triangles;
```

### BITXOR

```
BITXOR (y, x)
```

#### Parameters

* `x and y` - the arguments.

#### Description

The bitwise XOR operation. This method returns a `long`.

#### Example <a href="#example-16" id="example-16"></a>

Calculate XOR between two fields:

```
SELECT BITXOR(X, Y) FROM Triangles;
```

### MOD

```
MOD (y, x)
```

#### Parameters

* `x and y` - the arguments.

#### Description

The modulo operation. This method returns a `long`.

#### Example <a href="#example-17" id="example-17"></a>

Calculate MOD between two fields:

```
SELECT BITXOR(X, Y) FROM Triangles;
```

### CEILING

```
CEIL (expression)
CEILING (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also Java Math.ceil. This method returns a `double`.

#### Example <a href="#example-18" id="example-18"></a>

Calculate a ceiling price for items:

```
SELECT item_id, CEILING(price) FROM Items;
```

### DEGREES

```
DEGREES (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also `Java Math.toDegrees`. This method returns a `double`.

#### Example <a href="#example-19" id="example-19"></a>

Converts the argument value to degrees:

```
SELECT DEGREES(X) FROM Triangles;
```

### EXP

```
EXP (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also `Java Math.exp`. This method returns a `double`.

#### Example <a href="#example-20" id="example-20"></a>

Calculates exp:

```
SELECT EXP(X) FROM Triangles;
```

### FLOOR

```
FLOOR (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also `Java Math.floor`. This method returns a `double`.

#### Example <a href="#example-21" id="example-21"></a>

Calculates floor price:

```
SELECT FLOOR(X) FROM Items;
```

### LOG

```
LOG (expression)
LN (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also `Java Math.log`. This method returns a `double`.

#### Example <a href="#example-22" id="example-22"></a>

Calculates LOG:

```
SELECT LOG(X) from Items;
```

### LOG10

```
LOG10 (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also `Java Math.log10` (in Java 5). This method returns a `double`.

#### Example <a href="#example-23" id="example-23"></a>

Calculate LOG10:

```
SELECT LOG(X) FROM Items;
```

### RADIANS

```
RADIANS (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also Java Math.toRadians. This method returns a double.

#### Example <a href="#example-24" id="example-24"></a>

Calculates RADIANS:

```
SELECT RADIANS(X) FROM Items;
```

### SQRT

```
SQRT (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also `Java Math.sqrt`. This method returns a `double`.

#### Example <a href="#example-25" id="example-25"></a>

Calculates SQRT:

```
SELECT SQRT(X) FROM Items;
```

### PI

```
PI (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also `Java Math.PI`. This method returns a `double`.

#### Example <a href="#example-26" id="example-26"></a>

Calculates PI:

```
SELECT PI(X) FROM Items;
```

### POWER

```
POWER (X, Y)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

See also `Java Math.pow`. This method returns a `double`.

#### Example <a href="#example-27" id="example-27"></a>

Calculate the ​power of 2:

```
SELECT pow(2, n) FROM Rows;
```

### RAND

```
{RAND | RANDOM} ([expression])
```

#### Parameters

* `expression` - any valid numeric expression seeds the session’s random number generator.

#### Description

Calling the function without a parameter returns the next a pseudo random number. Calling it with a parameter seeds the session’s random number generator. This method returns a `double` between 0 (including) and 1 (excluding).

#### Example <a href="#example-28" id="example-28"></a>

Gets a random number for every play:

```
SELECT random() FROM Play;
```

### RANDOM\_UUID

```
{RANDOM_UUID | UUID} ()
```

#### Description

Returns a new UUID with 122 pseudo random bits.

#### Example <a href="#example-29" id="example-29"></a>

Gets random number for every Player:

```
SELECT UUID(),name FROM Player;
```

### ROUND

```
ROUND ( expression [, precision] )
```

#### Parameters

* `expression` - any valid numeric expression.
* `precision` - the number of digits after the decimal to round to. Rounds to the nearest long if the number of digits if not set.

#### Description

Rounds to a number of digits, or to the nearest long if the number of digits if not set. This method returns a `numeric` (the same type as the input).

#### Example <a href="#example-30" id="example-30"></a>

Convert every Player’s age to an integer number:

```
SELECT name, ROUND(age) FROM Player;
```

### ROUNDMAGIC

```
ROUNDMAGIC (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

This function is good for rounding numbers, but it can be slow. It has special handling for numbers around 0. Only numbers smaller than or equal to `+/-1000000000000` are supported. The value is converted to a String internally, and then the last 4 characters are checked. '000x' becomes '0000' and '999x' becomes '999999', which is rounded automatically. This method returns a `double`.

#### Example <a href="#example-31" id="example-31"></a>

Round every Player’s age:

```
SELECT name, ROUNDMAGIC(AGE/3*3) FROM Player;
```

### SECURE\_RAND

```
SECURE_RAND (int)
```

#### Parameters

* `int` - specifies the number​ of digits.

#### Description

Generate a number of cryptographically secure random numbers. This method returns `bytes`.

#### Example <a href="#example-32" id="example-32"></a>

Get a truly random number:

```
SELECT name, SECURE_RAND(10) FROM Player;
```

### SIGN

```
SIGN (expression)
```

#### Parameters

* `expression` - any valid numeric expression.

#### Description

Return -1 if the value is smaller 0, 0 if zero, and otherwise 1.

#### Example <a href="#example-33" id="example-33"></a>

Get a sign for every value:

```
SELECT name, SIGN(VALUE) FROM Player;
```

### ENCRYPT

```
ENCRYPT (algorithmString , keyBytes , dataBytes)
```

#### Parameters

* `algorithmString` - sets a supported AES algorithm.
* `keyBytes` - sets a key.
* `dataBytes` - sets data.

#### Description

Encrypt data using a key. The supported algorithm is AES. The block size is 16 bytes. This method returns `bytes`.

#### Example <a href="#example-34" id="example-34"></a>

Encrypt players name:

```
SELECT ENCRYPT('AES', '00', STRINGTOUTF8(Name)) FROM Player;
```

### DECRYPT

```
DECRYPT (algorithmString , keyBytes , dataBytes)
```

#### Parameters

* `algorithmString` - sets a supported AES algorithm.
* `keyBytes` - sets a key.
* `dataBytes` - sets data.

#### Description

Decrypts data using a key. The supported algorithm is AES. The block size is 16 bytes. This method returns bytes.

#### Example <a href="#example-35" id="example-35"></a>

Decrypt Players' names:

```
SELECT DECRYPT('AES', '00', '3fabb4de8f1ee2e97d7793bab2db1116'))) FROM Player;
```

### TRUNCATE

```
{TRUNC | TRUNCATE} (\{\{numeric, digitsInt} | timestamp | date | timestampString})
```

#### Description

Truncates to a number of digits (to the next value closer to 0). This method returns a `double`. When used with a timestamp, truncates a timestamp to a date (day) value. When used with a date, truncates a date to a date (day) value less time part. When used with a timestamp as string, truncates a timestamp to a date (day) value.

#### Example <a href="#example-36" id="example-36"></a>

```
TRUNCATE(VALUE, 2);
```

### COMPRESS

```
COMPRESS(dataBytes [, algorithmString])
```

#### Parameters

* `dataBytes` - data to compress.
* `algorithmString` - an algorithm to use for compression.

#### Description

Compress the data using the specified compression algorithm. Supported algorithms are: LZF (faster but lower compression; default), and DEFLATE (higher compression). Compression does not always reduce size. Very small objects and objects with little redundancy may get larger. This method returns `bytes`.

#### Example <a href="#example-37" id="example-37"></a>

```
COMPRESS(STRINGTOUTF8('Test'))
```

### EXPAND

```
EXPAND(dataBytes)
```

#### Parameters

* `dataBytes` - data to expand.

#### Description

Expand data that was compressed using the COMPRESS function. This method returns `bytes`.

#### Example <a href="#example-38" id="example-38"></a>

```
UTF8TOSTRING(EXPAND(COMPRESS(STRINGTOUTF8('Test'))))
```

### ZERO

```
ZERO()
```

#### Description

Return the value 0. This function can be used even if numeric literals are disabled.

#### Example <a href="#example-39" id="example-39"></a>

```
ZERO()
```
