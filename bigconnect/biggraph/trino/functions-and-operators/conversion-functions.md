# Conversion functions

Trino will implicitly convert numeric and character values to the correct type if such a conversion is possible. Trino will not convert between character and numeric types. For example, a query that expects a varchar will not automatically convert a bigint value to an equivalent varchar.

When necessary, values can be explicitly cast to a particular type.

### Conversion functions[#](https://trino.io/docs/current/functions/conversion.html#id1) <a href="#id1" id="id1"></a>

cast(_value AS type_) → type[#](https://trino.io/docs/current/functions/conversion.html#cast)

Explicitly cast a value as a type. This can be used to cast a varchar to a numeric value type and vice versa.

try\_cast(_value AS type_) → type[#](https://trino.io/docs/current/functions/conversion.html#try\_cast)

Like `cast()`, but returns null if the cast fails.

### Formatting[#](https://trino.io/docs/current/functions/conversion.html#formatting) <a href="#formatting" id="formatting"></a>

format(_format_, _args..._) → varchar[#](https://trino.io/docs/current/functions/conversion.html#format)

Returns a formatted string using the specified [format string](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Formatter.html#syntax) and arguments:

```
SELECT format('%s%%', 123);
-- '123%'

SELECT format('%.5f', pi());
-- '3.14159'

SELECT format('%03d', 8);
-- '008'

SELECT format('%,.2f', 1234567.89);
-- '1,234,567.89'

SELECT format('%-7s,%7s', 'hello', 'world');
-- 'hello  ,  world'

SELECT format('%2$s %3$s %1$s', 'a', 'b', 'c');
-- 'b c a'

SELECT format('%1$tA, %1$tB %1$te, %1$tY', date '2006-07-04');
-- 'Tuesday, July 4, 2006'
```

format\_number(_number_) → varchar[#](https://trino.io/docs/current/functions/conversion.html#format\_number)

Returns a formatted string using a unit symbol:

```
SELECT format_number(123456); -- '123K'
SELECT format_number(1000000); -- '1M'
```



### Data size[#](https://trino.io/docs/current/functions/conversion.html#data-size) <a href="#data-size" id="data-size"></a>

The `parse_data_size` function supports the following units:

| Unit | Description | Value |
| ---- | ----------- | ----- |
| `B`  | Bytes       | 1     |
| `kB` | Kilobytes   | 1024  |
| `MB` | Megabytes   | 10242 |
| `GB` | Gigabytes   | 10243 |
| `TB` | Terabytes   | 10244 |
| `PB` | Petabytes   | 10245 |
| `EB` | Exabytes    | 10246 |
| `ZB` | Zettabytes  | 10247 |
| `YB` | Yottabytes  | 10248 |

parse\_data\_size(_string_)[#](https://trino.io/docs/current/functions/conversion.html#parse\_data\_size)

Parses `string` of format `value unit` into a number, where `value` is the fractional number of `unit` values:

```
SELECT parse_data_size('1B'); -- 1
SELECT parse_data_size('1kB'); -- 1024
SELECT parse_data_size('1MB'); -- 1048576
SELECT parse_data_size('2.3MB'); -- 2411724
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Miscellaneous[#](https://trino.io/docs/current/functions/conversion.html#miscellaneous) <a href="#miscellaneous" id="miscellaneous"></a>

typeof(_expr_) → varchar[#](https://trino.io/docs/current/functions/conversion.html#typeof)

Returns the name of the type of the provided expression:

```
SELECT typeof(123); -- integer
SELECT typeof('cat'); -- varchar(3)
SELECT typeof(cos(2) + 1.5); -- double
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)\
