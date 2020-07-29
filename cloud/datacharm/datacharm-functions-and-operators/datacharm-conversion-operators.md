# Conversion

## Conversion Functions <a id="functions-conversion--page-root"></a>

DataCharm will implicitly convert numeric and character values to the correct type if such a conversion is possible. DataCharm will not convert between character and numeric types. For example, a query that expects a varchar will not automatically convert a bigint value to an equivalent varchar.

When necessary, values can be explicitly cast to a particular type.

### Conversion Functions <a id="id1"></a>

`cast`\(_value AS type_\) → type

Explicitly cast a value as a type. This can be used to cast a varchar to a numeric value type and vice versa.`try_cast`\(_value AS type_\) → type

Like `cast()`, but returns null if the cast fails.

### Formatting <a id="formatting"></a>

`format`\(_format_, _args..._\) → varchar

Returns a formatted string using the specified [format string](https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html#syntax) and arguments:

```sql
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

### Data Size <a id="data-size"></a>

The `parse_presto_data_size` function supports the following units:

| Unit | Description | Value |
| :--- | :--- | :--- |
| `B` | Bytes | 1 |
| `kB` | Kilobytes | 1024 |
| `MB` | Megabytes | 10242 |
| `GB` | Gigabytes | 10243 |
| `TB` | Terabytes | 10244 |
| `PB` | Petabytes | 10245 |
| `EB` | Exabytes | 10246 |
| `ZB` | Zettabytes | 10247 |
| `YB` | Yottabytes | 10248 |

`parse_presto_data_size`\(_string\) -&gt; decimal\(38_\)

Parses `string` of format `value unit` into a number, where `value` is the fractional number of `unit` values:

```sql
SELECT parse_presto_data_size('1B'); -- 1
SELECT parse_presto_data_size('1kB'); -- 1024
SELECT parse_presto_data_size('1MB'); -- 1048576
SELECT parse_presto_data_size('2.3MB'); -- 2411724
```

### Miscellaneous <a id="miscellaneous"></a>

`typeof`\(_expr_\) → varchar

Returns the name of the type of the provided expression:

```sql
SELECT typeof(123); -- integer
SELECT typeof('cat'); -- varchar(3)
SELECT typeof(cos(2) + 1.5); -- double
```

