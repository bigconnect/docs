# Date and time functions and operators

These functions and operators operate on [date and time data types](https://trino.io/docs/current/language/types.html#date-time-data-types).

### Date and time operators[#](https://trino.io/docs/current/functions/datetime.html#date-and-time-operators) <a href="#date-and-time-operators" id="date-and-time-operators"></a>

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

### Time zone conversion[#](https://trino.io/docs/current/functions/datetime.html#time-zone-conversion) <a href="#time-zone-conversion" id="time-zone-conversion"></a>

The `AT TIME ZONE` operator sets the time zone of a timestamp:

```
SELECT timestamp '2012-10-31 01:00 UTC';
-- 2012-10-31 01:00:00.000 UTC

SELECT timestamp '2012-10-31 01:00 UTC' AT TIME ZONE 'America/Los_Angeles';
-- 2012-10-30 18:00:00.000 America/Los_Angeles
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Date and time functions[#](https://trino.io/docs/current/functions/datetime.html#date-and-time-functions) <a href="#date-and-time-functions" id="date-and-time-functions"></a>

current\_date[#](https://trino.io/docs/current/functions/datetime.html#current\_date)

Returns the current date as of the start of the query.

current\_time[#](https://trino.io/docs/current/functions/datetime.html#current\_time)

Returns the current time with time zone as of the start of the query.

current\_timestamp[#](https://trino.io/docs/current/functions/datetime.html#current\_timestamp)

Returns the current timestamp with time zone as of the start of the query, with `3` digits of subsecond precision,

current\_timestamp(_p_)

Returns the current [timestamp with time zone](https://trino.io/docs/current/language/types.html#timestamp-with-time-zone-data-type) as of the start of the query, with `p` digits of subsecond precision:

```
SELECT current_timestamp(6);
-- 2020-06-24 08:25:31.759993 America/Los_Angeles
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)current\_timezone() → varchar[#](https://trino.io/docs/current/functions/datetime.html#current\_timezone)

Returns the current time zone in the format defined by IANA (e.g., `America/Los_Angeles`) or as fixed offset from UTC (e.g., `+08:35`)

date(_x_) → date[#](https://trino.io/docs/current/functions/datetime.html#date)

This is an alias for `CAST(x AS date)`.

last\_day\_of\_month(_x_) → date[#](https://trino.io/docs/current/functions/datetime.html#last\_day\_of\_month)

Returns the last day of the month.

from\_iso8601\_timestamp(_string_) → timestamp(3) with time zone[#](https://trino.io/docs/current/functions/datetime.html#from\_iso8601\_timestamp)

Parses the ISO 8601 formatted date `string`, optionally with time and time zone, into a `timestamp(3) with time zone`. The time defaults to `00:00:00.000`, and the time zone defaults to the session time zone:

```
SELECT from_iso8601_timestamp('2020-05-11');
-- 2020-05-11 00:00:00.000 America/Vancouver

SELECT from_iso8601_timestamp('2020-05-11T11:15:05');
-- 2020-05-11 11:15:05.000 America/Vancouver

SELECT from_iso8601_timestamp('2020-05-11T11:15:05.055+01:00');
-- 2020-05-11 11:15:05.055 +01:00
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)from\_iso8601\_timestamp\_nanos(_string_) → timestamp(9) with time zone[#](https://trino.io/docs/current/functions/datetime.html#from\_iso8601\_timestamp\_nanos)

Parses the ISO 8601 formatted date and time `string`. The time zone defaults to the session time zone:

```
SELECT from_iso8601_timestamp_nanos('2020-05-11T11:15:05');
-- 2020-05-11 11:15:05.000000000 America/Vancouver

SELECT from_iso8601_timestamp_nanos('2020-05-11T11:15:05.123456789+01:00');
-- 2020-05-11 11:15:05.123456789 +01:00
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)from\_iso8601\_date(_string_) → date[#](https://trino.io/docs/current/functions/datetime.html#from\_iso8601\_date)

Parses the ISO 8601 formatted date `string` into a `date`. The date can be a calendar date, a week date using ISO week numbering, or year and day of year combined:

```
SELECT from_iso8601_date('2020-05-11');
-- 2020-05-11

SELECT from_iso8601_date('2020-W10');
-- 2020-03-02

SELECT from_iso8601_date('2020-123');
-- 2020-05-02
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)at\_timezone(_timestamp(p)_, _zone_) → timestamp(p) with time zone[#](https://trino.io/docs/current/functions/datetime.html#at\_timezone)

Returns the timestamp specified in `timestamp` with the time zone converted from the session time zone to the time zone specified in `zone` with precision `p`. In the following example, the session time zone is set to `America/New_York`, which is three hours ahead of `America/Los_Angeles`:

```
SELECT current_timezone()
-- America/New_York

SELECT at_timezone(TIMESTAMP '2022-11-01 09:08:07.321', 'America/Los_Angeles')
-- 2022-11-01 06:08:07.321 America/Los_Angeles
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)with\_timezone(_timestamp(p)_, _zone_) → timestamp(p) with time zone[#](https://trino.io/docs/current/functions/datetime.html#with\_timezone)

Returns the timestamp specified in `timestamp` with the time zone specified in `zone` with precision `p`:

```
SELECT current_timezone()
-- America/New_York

SELECT with_timezone(TIMESTAMP '2022-11-01 09:08:07.321', 'America/Los_Angeles')
-- 2022-11-01 09:08:07.321 America/Los_Angeles
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)from\_unixtime(_unixtime_) → timestamp(3) with time zone[#](https://trino.io/docs/current/functions/datetime.html#from\_unixtime)

Returns the UNIX timestamp `unixtime` as a timestamp with time zone. `unixtime` is the number of seconds since `1970-01-01 00:00:00 UTC`.

from\_unixtime(_unixtime_, _zone_) → timestamp(3) with time zone

Returns the UNIX timestamp `unixtime` as a timestamp with time zone using `zone` for the time zone. `unixtime` is the number of seconds since `1970-01-01 00:00:00 UTC`.

from\_unixtime(_unixtime_, _hours_, _minutes_) → timestamp(3) with time zone

Returns the UNIX timestamp `unixtime` as a timestamp with time zone using `hours` and `minutes` for the time zone offset. `unixtime` is the number of seconds since `1970-01-01 00:00:00` in `double` data type.

from\_unixtime\_nanos(_unixtime_) → timestamp(9) with time zone[#](https://trino.io/docs/current/functions/datetime.html#from\_unixtime\_nanos)

Returns the UNIX timestamp `unixtime` as a timestamp with time zone. `unixtime` is the number of nanoseconds since `1970-01-01 00:00:00.000000000 UTC`:

```
SELECT from_unixtime_nanos(100);
-- 1970-01-01 00:00:00.000000100 UTC

SELECT from_unixtime_nanos(DECIMAL '1234');
-- 1970-01-01 00:00:00.000001234 UTC

SELECT from_unixtime_nanos(DECIMAL '1234.499');
-- 1970-01-01 00:00:00.000001234 UTC

SELECT from_unixtime_nanos(DECIMAL '-1234');
-- 1969-12-31 23:59:59.999998766 UTC
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)localtime[#](https://trino.io/docs/current/functions/datetime.html#localtime)

Returns the current time as of the start of the query.

localtimestamp[#](https://trino.io/docs/current/functions/datetime.html#localtimestamp)

Returns the current timestamp as of the start of the query, with `3` digits of subsecond precision.

localtimestamp(_p_)

Returns the current [timestamp](https://trino.io/docs/current/language/types.html#timestamp-data-type) as of the start of the query, with `p` digits of subsecond precision:

```
SELECT localtimestamp(6);
-- 2020-06-10 15:55:23.383628
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)now() → timestamp(3) with time zone[#](https://trino.io/docs/current/functions/datetime.html#now)

This is an alias for `current_timestamp`.

to\_iso8601(_x_) → varchar[#](https://trino.io/docs/current/functions/datetime.html#to\_iso8601)

Formats `x` as an ISO 8601 string. `x` can be date, timestamp, or timestamp with time zone.

to\_milliseconds(_interval_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#to\_milliseconds)

Returns the day-to-second `interval` as milliseconds.

to\_unixtime(_timestamp_) → double[#](https://trino.io/docs/current/functions/datetime.html#to\_unixtime)

Returns `timestamp` as a UNIX timestamp.

Note

The following SQL-standard functions do not use parenthesis:

* `current_date`
* `current_time`
* `current_timestamp`
* `localtime`
* `localtimestamp`

### Truncation function[#](https://trino.io/docs/current/functions/datetime.html#truncation-function) <a href="#truncation-function" id="truncation-function"></a>

The `date_trunc` function supports the following units:

| Unit          | Example Truncated Value   |
| ------------- | ------------------------- |
| `millisecond` | `2001-08-22 03:04:05.321` |
| `second`      | `2001-08-22 03:04:05.000` |
| `minute`      | `2001-08-22 03:04:00.000` |
| `hour`        | `2001-08-22 03:00:00.000` |
| `day`         | `2001-08-22 00:00:00.000` |
| `week`        | `2001-08-20 00:00:00.000` |
| `month`       | `2001-08-01 00:00:00.000` |
| `quarter`     | `2001-07-01 00:00:00.000` |
| `year`        | `2001-01-01 00:00:00.000` |

The above examples use the timestamp `2001-08-22 03:04:05.321` as the input.

date\_trunc(_unit_, _x_) → \[same as input][#](https://trino.io/docs/current/functions/datetime.html#date\_trunc)

Returns `x` truncated to `unit`:

```
SELECT date_trunc('day' , TIMESTAMP '2022-10-20 05:10:00');
-- 2022-10-20 00:00:00.000

SELECT date_trunc('month' , TIMESTAMP '2022-10-20 05:10:00');
-- 2022-10-01 00:00:00.000

SELECT date_trunc('year', TIMESTAMP '2022-10-20 05:10:00');
-- 2022-01-01 00:00:00.000
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Interval functions[#](https://trino.io/docs/current/functions/datetime.html#interval-functions) <a href="#interval-functions" id="interval-functions"></a>

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

date\_add(_unit_, _value_, _timestamp_) → \[same as input][#](https://trino.io/docs/current/functions/datetime.html#date\_add)

Adds an interval `value` of type `unit` to `timestamp`. Subtraction can be performed by using a negative value:

```
SELECT date_add('second', 86, TIMESTAMP '2020-03-01 00:00:00');
-- 2020-03-01 00:01:26.000

SELECT date_add('hour', 9, TIMESTAMP '2020-03-01 00:00:00');
-- 2020-03-01 09:00:00.000

SELECT date_add('day', -1, TIMESTAMP '2020-03-01 00:00:00 UTC');
-- 2020-02-29 00:00:00.000 UTC
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)date\_diff(_unit_, _timestamp1_, _timestamp2_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#date\_diff)

Returns `timestamp2 - timestamp1` expressed in terms of `unit`:

```
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

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Duration function[#](https://trino.io/docs/current/functions/datetime.html#duration-function) <a href="#duration-function" id="duration-function"></a>

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

parse\_duration(_string_) → interval[#](https://trino.io/docs/current/functions/datetime.html#parse\_duration)

Parses `string` of format `value unit` into an interval, where `value` is fractional number of `unit` values:

```
SELECT parse_duration('42.8ms');
-- 0 00:00:00.043

SELECT parse_duration('3.81 d');
-- 3 19:26:24.000

SELECT parse_duration('5m');
-- 0 00:05:00.000
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)human\_readable\_seconds(_double_) → varchar[#](https://trino.io/docs/current/functions/datetime.html#human\_readable\_seconds)

Formats the double value of `seconds` into a human readable string containing `weeks`, `days`, `hours`, `minutes`, and `seconds`:

```
SELECT human_readable_seconds(96);
-- 1 minute, 36 seconds

SELECT human_readable_seconds(3762);
-- 1 hour, 2 minutes, 42 seconds

SELECT human_readable_seconds(56363463);
-- 93 weeks, 1 day, 8 hours, 31 minutes, 3 seconds
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### MySQL date functions[#](https://trino.io/docs/current/functions/datetime.html#mysql-date-functions) <a href="#mysql-date-functions" id="mysql-date-functions"></a>

The functions in this section use a format string that is compatible with the MySQL `date_parse` and `str_to_date` functions. The following table, based on the MySQL manual, describes the format specifiers:

| Specifier | Description                                                                                                                                                                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `%a`      | Abbreviated weekday name (`Sun` .. `Sat`)                                                                                                                                                                                                        |
| `%b`      | Abbreviated month name (`Jan` .. `Dec`)                                                                                                                                                                                                          |
| `%c`      | Month, numeric (`1` .. `12`), this specifier does not support `0` as a month.                                                                                                                                                                    |
| `%D`      | Day of the month with English suffix (`0th`, `1st`, `2nd`, `3rd`, …)                                                                                                                                                                             |
| `%d`      | Day of the month, numeric (`01` .. `31`), this specifier does not support `0` as a month or day.                                                                                                                                                 |
| `%e`      | Day of the month, numeric (`1` .. `31`), this specifier does not support `0` as a day.                                                                                                                                                           |
| `%f`      | Fraction of second (6 digits for printing: `000000` .. `999000`; 1 - 9 digits for parsing: `0` .. `999999999`), timestamp is truncated to milliseconds.                                                                                          |
| `%H`      | Hour (`00` .. `23`)                                                                                                                                                                                                                              |
| `%h`      | Hour (`01` .. `12`)                                                                                                                                                                                                                              |
| `%I`      | Hour (`01` .. `12`)                                                                                                                                                                                                                              |
| `%i`      | Minutes, numeric (`00` .. `59`)                                                                                                                                                                                                                  |
| `%j`      | Day of year (`001` .. `366`)                                                                                                                                                                                                                     |
| `%k`      | Hour (`0` .. `23`)                                                                                                                                                                                                                               |
| `%l`      | Hour (`1` .. `12`)                                                                                                                                                                                                                               |
| `%M`      | Month name (`January` .. `December`)                                                                                                                                                                                                             |
| `%m`      | Month, numeric (`01` .. `12`), this specifier does not support `0` as a month.                                                                                                                                                                   |
| `%p`      | `AM` or `PM`                                                                                                                                                                                                                                     |
| `%r`      | Time of day, 12-hour (equivalent to `%h:%i:%s %p`)                                                                                                                                                                                               |
| `%S`      | Seconds (`00` .. `59`)                                                                                                                                                                                                                           |
| `%s`      | Seconds (`00` .. `59`)                                                                                                                                                                                                                           |
| `%T`      | Time of day, 24-hour (equivalent to `%H:%i:%s`)                                                                                                                                                                                                  |
| `%U`      | Week (`00` .. `53`), where Sunday is the first day of the week                                                                                                                                                                                   |
| `%u`      | Week (`00` .. `53`), where Monday is the first day of the week                                                                                                                                                                                   |
| `%V`      | Week (`01` .. `53`), where Sunday is the first day of the week; used with `%X`                                                                                                                                                                   |
| `%v`      | Week (`01` .. `53`), where Monday is the first day of the week; used with `%x`                                                                                                                                                                   |
| `%W`      | Weekday name (`Sunday` .. `Saturday`)                                                                                                                                                                                                            |
| `%w`      | Day of the week (`0` .. `6`), where Sunday is the first day of the week, this specifier is not supported,consider using [`day_of_week()`](https://trino.io/docs/current/functions/datetime.html#day\_of\_week) (it uses `1-7` instead of `0-6`). |
| `%X`      | Year for the week where Sunday is the first day of the week, numeric, four digits; used with `%V`                                                                                                                                                |
| `%x`      | Year for the week, where Monday is the first day of the week, numeric, four digits; used with `%v`                                                                                                                                               |
| `%Y`      | Year, numeric, four digits                                                                                                                                                                                                                       |
| `%y`      | Year, numeric (two digits), when parsing, two-digit year format assumes range `1970` .. `2069`, so “70” will result in year `1970` but “69” will produce `2069`.                                                                                 |
| `%%`      | A literal `%` character                                                                                                                                                                                                                          |
| `%x`      | `x`, for any `x` not listed above                                                                                                                                                                                                                |

Warning

The following specifiers are not currently supported: `%D %U %u %V %w %X`

date\_format(_timestamp_, _format_) → varchar[#](https://trino.io/docs/current/functions/datetime.html#date\_format)

Formats `timestamp` as a string using `format`:

```
SELECT date_format(TIMESTAMP '2022-10-20 05:10:00', '%m-%d-%Y %H');
-- 10-20-2022 05
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)date\_parse(_string_, _format) → timestamp(3_)[#](https://trino.io/docs/current/functions/datetime.html#date\_parse)

Parses `string` into a timestamp using `format`:

```
SELECT date_parse('2022/10/20/05', '%Y/%m/%d/%H');
-- 2022-10-20 05:00:00.000
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Java date functions[#](https://trino.io/docs/current/functions/datetime.html#java-date-functions) <a href="#java-date-functions" id="java-date-functions"></a>

The functions in this section use a format string that is compatible with JodaTime’s [DateTimeFormat](http://joda-time.sourceforge.net/apidocs/org/joda/time/format/DateTimeFormat.html) pattern format.

format\_datetime(_timestamp_, _format_) → varchar[#](https://trino.io/docs/current/functions/datetime.html#format\_datetime)

Formats `timestamp` as a string using `format`.

parse\_datetime(_string_, _format_) → timestamp with time zone[#](https://trino.io/docs/current/functions/datetime.html#parse\_datetime)

Parses `string` into a timestamp with time zone using `format`.

### Extraction function[#](https://trino.io/docs/current/functions/datetime.html#extraction-function) <a href="#extraction-function" id="extraction-function"></a>

The `extract` function supports the following fields:

| Field             | Description                                                                                   |
| ----------------- | --------------------------------------------------------------------------------------------- |
| `YEAR`            | [`year()`](https://trino.io/docs/current/functions/datetime.html#year)                        |
| `QUARTER`         | [`quarter()`](https://trino.io/docs/current/functions/datetime.html#quarter)                  |
| `MONTH`           | [`month()`](https://trino.io/docs/current/functions/datetime.html#month)                      |
| `WEEK`            | [`week()`](https://trino.io/docs/current/functions/datetime.html#week)                        |
| `DAY`             | [`day()`](https://trino.io/docs/current/functions/datetime.html#day)                          |
| `DAY_OF_MONTH`    | [`day()`](https://trino.io/docs/current/functions/datetime.html#day)                          |
| `DAY_OF_WEEK`     | [`day_of_week()`](https://trino.io/docs/current/functions/datetime.html#day\_of\_week)        |
| `DOW`             | [`day_of_week()`](https://trino.io/docs/current/functions/datetime.html#day\_of\_week)        |
| `DAY_OF_YEAR`     | [`day_of_year()`](https://trino.io/docs/current/functions/datetime.html#day\_of\_year)        |
| `DOY`             | [`day_of_year()`](https://trino.io/docs/current/functions/datetime.html#day\_of\_year)        |
| `YEAR_OF_WEEK`    | [`year_of_week()`](https://trino.io/docs/current/functions/datetime.html#year\_of\_week)      |
| `YOW`             | [`year_of_week()`](https://trino.io/docs/current/functions/datetime.html#year\_of\_week)      |
| `HOUR`            | [`hour()`](https://trino.io/docs/current/functions/datetime.html#hour)                        |
| `MINUTE`          | [`minute()`](https://trino.io/docs/current/functions/datetime.html#minute)                    |
| `SECOND`          | [`second()`](https://trino.io/docs/current/functions/datetime.html#second)                    |
| `TIMEZONE_HOUR`   | [`timezone_hour()`](https://trino.io/docs/current/functions/datetime.html#timezone\_hour)     |
| `TIMEZONE_MINUTE` | [`timezone_minute()`](https://trino.io/docs/current/functions/datetime.html#timezone\_minute) |

The types supported by the `extract` function vary depending on the field to be extracted. Most fields support all date and time types.

extract(_field FROM x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#extract)

Returns `field` from `x`:

```
SELECT extract(YEAR FROM TIMESTAMP '2022-10-20 05:10:00');
-- 2022
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Note

This SQL-standard function uses special syntax for specifying the arguments.

### Convenience extraction functions[#](https://trino.io/docs/current/functions/datetime.html#convenience-extraction-functions) <a href="#convenience-extraction-functions" id="convenience-extraction-functions"></a>

day(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#day)

Returns the day of the month from `x`.

day\_of\_month(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#day\_of\_month)

This is an alias for [`day()`](https://trino.io/docs/current/functions/datetime.html#day).

day\_of\_week(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#day\_of\_week)

Returns the ISO day of the week from `x`. The value ranges from `1` (Monday) to `7` (Sunday).

day\_of\_year(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#day\_of\_year)

Returns the day of the year from `x`. The value ranges from `1` to `366`.

dow(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#dow)

This is an alias for [`day_of_week()`](https://trino.io/docs/current/functions/datetime.html#day\_of\_week).

doy(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#doy)

This is an alias for [`day_of_year()`](https://trino.io/docs/current/functions/datetime.html#day\_of\_year).

hour(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#hour)

Returns the hour of the day from `x`. The value ranges from `0` to `23`.

millisecond(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#millisecond)

Returns the millisecond of the second from `x`.

minute(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#minute)

Returns the minute of the hour from `x`.

month(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#month)

Returns the month of the year from `x`.

quarter(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#quarter)

Returns the quarter of the year from `x`. The value ranges from `1` to `4`.

second(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#second)

Returns the second of the minute from `x`.

timezone\_hour(_timestamp_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#timezone\_hour)

Returns the hour of the time zone offset from `timestamp`.

timezone\_minute(_timestamp_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#timezone\_minute)

Returns the minute of the time zone offset from `timestamp`.

week(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#week)

Returns the [ISO week](https://wikipedia.org/wiki/ISO\_week\_date) of the year from `x`. The value ranges from `1` to `53`.

week\_of\_year(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#week\_of\_year)

This is an alias for [`week()`](https://trino.io/docs/current/functions/datetime.html#week).

year(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#year)

Returns the year from `x`.

year\_of\_week(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#year\_of\_week)

Returns the year of the [ISO week](https://wikipedia.org/wiki/ISO\_week\_date) from `x`.

yow(_x_) → bigint[#](https://trino.io/docs/current/functions/datetime.html#yow)

This is an alias for [`year_of_week()`](https://trino.io/docs/current/functions/datetime.html#year\_of\_week).
