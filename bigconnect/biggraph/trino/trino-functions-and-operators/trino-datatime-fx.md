# Date and Time

These functions and operators operate on date and time data types.

### Date and Time Operators <a href="#date-and-time-operators" id="date-and-time-operators"></a>

| Operator | Example                                             | Result                    |
| -------- | --------------------------------------------------- | ------------------------- |
| `+`      | `date '2012-08-08' + interval '2' day`              | `2012-08-10`              |
| `+`      | `time '01:00' + interval '3' hour`                  | `04:00:00.000`            |
| `+`      | `timestamp '2012-08-08 01:00' + interval '29' hour` | `2012-08-09 06:00:00.000` |
| `+`      | `timestamp '2012-10-31 01:00' + interval '1' month` | `2012-11-30 01:00:00.000` |
| `+`      | `interval '2' day + interval '3' hour`              | `2 03:00:00.000`          |
| `+`      | `interval '3' year + interval '5' month`            | `3-5`                     |
| `-`      | `date '2012-08-08' - interval '2' day`              | `2012-08-06`              |
| `-`      | `time '01:00' - interval '3' hour`                  | `22:00:00.000`            |
| `-`      | `timestamp '2012-08-08 01:00' - interval '29' hour` | `2012-08-06 20:00:00.000` |
| `-`      | `timestamp '2012-10-31 01:00' - interval '1' month` | `2012-09-30 01:00:00.000` |
| `-`      | `interval '2' day - interval '3' hour`              | `1 21:00:00.000`          |
| `-`      | `interval '3' year - interval '5' month`            | `2-7`                     |

### Time Zone Conversion <a href="#time-zone-conversion" id="time-zone-conversion"></a>

The `AT TIME ZONE` operator sets the time zone of a timestamp:

```sql
SELECT timestamp '2012-10-31 01:00 UTC';
-- 2012-10-31 01:00:00.000 UTC

SELECT timestamp '2012-10-31 01:00 UTC' AT TIME ZONE 'America/Los_Angeles';
-- 2012-10-30 18:00:00.000 America/Los_Angeles
```

### Date and Time Functions <a href="#date-and-time-functions" id="date-and-time-functions"></a>

* `current_date`

Returns the current date as of the start of the query.

* `current_time`

Returns the current time with time zone as of the start of the query.

* `current_timestamp`

Returns the current timestamp with time zone as of the start of the query, with `3` digits of subsecond precision.

`current_timestamp`(_p_)

Returns the current timestamp with time zone as of the start of the query, with `p` digits of subsecond precision:

```sql
SELECT current_timestamp(6);
-- 2020-06-24 08:25:31.759993 America/Los_Angeles
```

* `current_timezone`() → varchar

Returns the current time zone in the format defined by IANA (e.g., `America/Los_Angeles`) or as fixed offset from UTC (e.g., `+08:35`)

* `date`(_x_) → date

This is an alias for `CAST(x AS date)`.

* `last_day_of_month`(_x_) → date

Returns the last day of the month.

* `from_iso8601_timestamp`(_string_) → timestamp(3) with time zone

Parses the ISO 8601 formatted date `string`, optionally with time and time zone, into a `timestamp(3) with time zone`. The time defaults to `00:00:00.000`, and the time zone defaults to the session time zone:

```sql
SELECT from_iso8601_timestamp('2020-05-11');
-- 2020-05-11 00:00:00.000 America/Vancouver

SELECT from_iso8601_timestamp('2020-05-11T11:15:05');
-- 2020-05-11 11:15:05.000 America/Vancouver

SELECT from_iso8601_timestamp('2020-05-11T11:15:05.055+01:00');
-- 2020-05-11 11:15:05.055 +01:00
```

* `from_iso8601_date`(_string_) → date

Parses the ISO 8601 formatted date `string` into a `date`. The date can be a calendar date, a week date using ISO week numbering, or year and day of year combined:

```sql
SELECT from_iso8601_date('2020-05-11');
-- 2020-05-11

SELECT from_iso8601_date('2020-W10');
-- 2020-03-02

SELECT from_iso8601_date('2020-123');
-- 2020-05-02
```

* `at_timezone`(_timestamp_, _zone_) → timestamp(p) with time zone

Change the time zone component of `timestamp` with precision `p` to `zone` while preserving the instant in time.

* `with_timezone`(_timestamp_, _zone_) → timestamp(p) with time zone

Returns a timestamp with time zone from `timestamp` with precision `p` and `zone`.

* `from_unixtime`(_unixtime) -> timestamp(3_)

Returns the UNIX timestamp `unixtime` as a timestamp. `unixtime` is the number of seconds since `1970-01-01 00:00:00 UTC`.

* `from_unixtime`(_unixtime_, _zone_) → timestamp(3) with time zone

Returns the UNIX timestamp `unixtime` as a timestamp with time zone using `zone` for the time zone. `unixtime` is the number of seconds since `1970-01-01 00:00:00 UTC`.

* `from_unixtime`(_unixtime_, _hours_, _minutes_) → timestamp(3) with time zone

Returns the UNIX timestamp `unixtime` as a timestamp with time zone using `hours` and `minutes` for the time zone offset. `unixtime` is the number of seconds since `1970-01-01 00:00:00` in `double` data type.

* `localtime`

Returns the current time as of the start of the query.

* `localtimestamp`

Returns the current timestamp as of the start of the query, with `3` digits of subsecond precision.

* `localtimestamp`(_p_)

Returns the current timestamp as of the start of the query, with `p` digits of subsecond precision:

```sql
SELECT localtimestamp(6);
-- 2020-06-10 15:55:23.383628
```

* `now`() → timestamp(3) with time zone

This is an alias for `current_timestamp`.

* `to_iso8601`(_x_) → varchar

Formats `x` as an ISO 8601 string. `x` can be date, timestamp, or timestamp with time zone.

* `to_milliseconds`(_interval_) → bigint

Returns the day-to-second `interval` as milliseconds.

* `to_unixtime`(_timestamp_) → double

Returns `timestamp` as a UNIX timestamp.

{% hint style="info" %}
The following SQL-standard functions do not use parenthesis:

* `current_date`
* `current_time`
* `current_timestamp`
* `localtime`
* `localtimestamp`
{% endhint %}

### Truncation Function <a href="#truncation-function" id="truncation-function"></a>

The `date_trunc` function supports the following units:

| Unit      | Example Truncated Value   |
| --------- | ------------------------- |
| `second`  | `2001-08-22 03:04:05.000` |
| `minute`  | `2001-08-22 03:04:00.000` |
| `hour`    | `2001-08-22 03:00:00.000` |
| `day`     | `2001-08-22 00:00:00.000` |
| `week`    | `2001-08-20 00:00:00.000` |
| `month`   | `2001-08-01 00:00:00.000` |
| `quarter` | `2001-07-01 00:00:00.000` |
| `year`    | `2001-01-01 00:00:00.000` |

The above examples use the timestamp `2001-08-22 03:04:05.321` as the input.

* `date_trunc`(_unit_, _x_) → \[same as input]

Returns `x` truncated to `unit`.

### Interval Functions <a href="#interval-functions" id="interval-functions"></a>

The functions in this section support the following interval units:

| Unit          | Description        |
| ------------- | ------------------ |
| `millisecond` | Milliseconds       |
| `second`      | Seconds            |
| `minute`      | Minutes            |
| `hour`        | Hours              |
| `day`         | Days               |
| `week`        | Weeks              |
| `month`       | Months             |
| `quarter`     | Quarters of a year |
| `year`        | Years              |

* `date_add`(_unit_, _value_, _timestamp_) → \[same as input]

Adds an interval `value` of type `unit` to `timestamp`. Subtraction can be performed by using a negative value:

```sql
SELECT date_add('second', 86, TIMESTAMP '2020-03-01 00:00:00');
-- 2020-03-01 00:01:26.000

SELECT date_add('hour', 9, TIMESTAMP '2020-03-01 00:00:00');
-- 2020-03-01 09:00:00.000

SELECT date_add('day', -1, TIMESTAMP '2020-03-01 00:00:00 UTC');
-- 2020-02-29 00:00:00.000 UTC
```

* `date_diff`(_unit_, _timestamp1_, _timestamp2_) → bigint

Returns `timestamp2 - timestamp1` expressed in terms of `unit`:

```sql
SELECT date_diff('second', TIMESTAMP '2020-03-01 00:00:00', TIMESTAMP '2020-03-02 00:00:00');
-- 86400

SELECT date_diff('hour', TIMESTAMP '2020-03-01 00:00:00 UTC', TIMESTAMP '2020-03-02 00:00:00 UTC');
-- 24

SELECT date_diff('day', DATE '2020-03-01', DATE '2020-03-02');
-- 1

SELECT date_diff('second', TIMESTAMP '2020-06-01 12:30:45.000000000', TIMESTAMP '2020-06-02 12:30:45.123456789');
-- 86400

SELECT date_diff('millisecond', TIMESTAMP '2020-06-01 12:30:45.000000000', TIMESTAMP '2020-06-02 12:30:45.123456789');
-- 86400123
```

### Duration Function <a href="#duration-function" id="duration-function"></a>

The `parse_duration` function supports the following units:

| Unit | Description  |
| ---- | ------------ |
| `ns` | Nanoseconds  |
| `us` | Microseconds |
| `ms` | Milliseconds |
| `s`  | Seconds      |
| `m`  | Minutes      |
| `h`  | Hours        |
| `d`  | Days         |

* `parse_duration`(_string_) → interval

Parses `string` of format `value unit` into an interval, where `value` is fractional number of `unit` values:

```sql
SELECT parse_duration('42.8ms');
-- 0 00:00:00.043

SELECT parse_duration('3.81 d');
-- 3 19:26:24.000

SELECT parse_duration('5m');
-- 0 00:05:00.000
```

### MySQL Date Functions <a href="#mysql-date-functions" id="mysql-date-functions"></a>

The functions in this section use a format string that is compatible with the MySQL `date_parse` and `str_to_date` functions. The following table, based on the MySQL manual, describes the format specifiers:

| Specifier | Description                                                                                                    |
| --------- | -------------------------------------------------------------------------------------------------------------- |
| `%a`      | Abbreviated weekday name (`Sun` .. `Sat`)                                                                      |
| `%b`      | Abbreviated month name (`Jan` .. `Dec`)                                                                        |
| `%c`      | Month, numeric (`1` .. `12`)                                                                                   |
| `%D`      | Day of the month with English suffix (`0th`, `1st`, `2nd`, `3rd`, …)                                           |
| `%d`      | Day of the month, numeric (`01` .. `31`)                                                                       |
| `%e`      | Day of the month, numeric (`1` .. `31`)                                                                        |
| `%f`      | Fraction of second (6 digits for printing: `000000` .. `999000`; 1 - 9 digits for parsing: `0` .. `999999999`) |
| `%H`      | Hour (`00` .. `23`)                                                                                            |
| `%h`      | Hour (`01` .. `12`)                                                                                            |
| `%I`      | Hour (`01` .. `12`)                                                                                            |
| `%i`      | Minutes, numeric (`00` .. `59`)                                                                                |
| `%j`      | Day of year (`001` .. `366`)                                                                                   |
| `%k`      | Hour (`0` .. `23`)                                                                                             |
| `%l`      | Hour (`1` .. `12`)                                                                                             |
| `%M`      | Month name (`January` .. `December`)                                                                           |
| `%m`      | Month, numeric (`01` .. `12`)                                                                                  |
| `%p`      | `AM` or `PM`                                                                                                   |
| `%r`      | Time, 12-hour (`hh:mm:ss` followed by `AM` or `PM`)                                                            |
| `%S`      | Seconds (`00` .. `59`)                                                                                         |
| `%s`      | Seconds (`00` .. `59`)                                                                                         |
| `%T`      | Time, 24-hour (`hh:mm:ss`)                                                                                     |
| `%U`      | Week (`00` .. `53`), where Sunday is the first day of the week                                                 |
| `%u`      | Week (`00` .. `53`), where Monday is the first day of the week                                                 |
| `%V`      | Week (`01` .. `53`), where Sunday is the first day of the week; used with `%X`                                 |
| `%v`      | Week (`01` .. `53`), where Monday is the first day of the week; used with `%x`                                 |
| `%W`      | Weekday name (`Sunday` .. `Saturday`)                                                                          |
| `%w`      | Day of the week (`0` .. `6`), where Sunday is the first day of the week                                        |
| `%X`      | Year for the week where Sunday is the first day of the week, numeric, four digits; used with `%V`              |
| `%x`      | Year for the week, where Monday is the first day of the week, numeric, four digits; used with `%v`             |
| `%Y`      | Year, numeric, four digits                                                                                     |
| `%y`      | Year, numeric (two digits)                                                                                     |
| `%%`      | A literal `%` character                                                                                        |
| `%x`      | `x`, for any `x` not listed above                                                                              |

{% hint style="warning" %}
The following specifiers are not currently supported: `%D %U %u %V %w %X`
{% endhint %}

* `date_format`(_timestamp_, _format_) → varchar

Formats `timestamp` as a string using `format`.

* `date_parse`(_string_, _format) -> timestamp(3_)

Parses `string` into a timestamp using `format`.

### Java Date Functions <a href="#java-date-functions" id="java-date-functions"></a>

The functions in this section use a format string that is compatible with JodaTime’s [DateTimeFormat](http://joda-time.sourceforge.net/apidocs/org/joda/time/format/DateTimeFormat.html) pattern format.

* `format_datetime`(_timestamp_, _format_) → varchar

Formats `timestamp` as a string using `format`.

* `parse_datetime`(_string_, _format_) → timestamp with time zone

Parses `string` into a timestamp with time zone using `format`.

### Extraction Function <a href="#extraction-function" id="extraction-function"></a>

The `extract` function supports the following fields:

| Field             | Description         |
| ----------------- | ------------------- |
| `YEAR`            | `year()`            |
| `QUARTER`         | `quarter()`         |
| `MONTH`           | `month()`           |
| `WEEK`            | `week()`            |
| `DAY`             | `day()`             |
| `DAY_OF_MONTH`    | `day()`             |
| `DAY_OF_WEEK`     | `day_of_week()`     |
| `DOW`             | `day_of_week()`     |
| `DAY_OF_YEAR`     | `day_of_year()`     |
| `DOY`             | `day_of_year()`     |
| `YEAR_OF_WEEK`    | `year_of_week()`    |
| `YOW`             | `year_of_week()`    |
| `HOUR`            | `hour()`            |
| `MINUTE`          | `minute()`          |
| `SECOND`          | `second()`          |
| `TIMEZONE_HOUR`   | `timezone_hour()`   |
| `TIMEZONE_MINUTE` | `timezone_minute()` |

The types supported by the `extract` function vary depending on the field to be extracted. Most fields support all date and time types.

* `extract`(_field FROM x_) → bigint

Returns `field` from `x`.

{% hint style="info" %}
This SQL-standard function uses special syntax for specifying the arguments.
{% endhint %}

### Convenience Extraction Functions <a href="#convenience-extraction-functions" id="convenience-extraction-functions"></a>

* `day`(_x_) → bigint

Returns the day of the month from `x`.

* `day_of_month`(_x_) → bigint

This is an alias for `day()`.

* `day_of_week`(_x_) → bigint

Returns the ISO day of the week from `x`. The value ranges from `1` (Monday) to `7` (Sunday).

* `day_of_year`(_x_) → bigint

Returns the day of the year from `x`. The value ranges from `1` to `366`.

* `dow`(_x_) → bigint

This is an alias for `day_of_week()`.

* `doy`(_x_) → bigint

This is an alias for `day_of_year()`.

* `hour`(_x_) → bigint

Returns the hour of the day from `x`. The value ranges from `0` to `23`.

* `millisecond`(_x_) → bigint

Returns the millisecond of the second from `x`.

* `minute`(_x_) → bigint

Returns the minute of the hour from `x`.

* `month`(_x_) → bigint

Returns the month of the year from `x`.

* `quarter`(_x_) → bigint

Returns the quarter of the year from `x`. The value ranges from `1` to `4`.

* `second`(_x_) → bigint

Returns the second of the minute from `x`.

* `timezone_hour`(_timestamp_) → bigint

Returns the hour of the time zone offset from `timestamp`.

* `timezone_minute`(_timestamp_) → bigint

Returns the minute of the time zone offset from `timestamp`.

* `week`(_x_) → bigint

Returns the [ISO week](https://en.wikipedia.org/wiki/ISO\_week\_date) of the year from `x`. The value ranges from `1` to `53`.

* `week_of_year`(_x_) → bigint

This is an alias for `week()`.

* `year`(_x_) → bigint

Returns the year from `x`.

* `year_of_week`(_x_) → bigint

Returns the year of the [ISO week](https://en.wikipedia.org/wiki/ISO\_week\_date) from `x`.

* `yow`(_x_) → bigint

This is an alias for `year_of_week()`.
