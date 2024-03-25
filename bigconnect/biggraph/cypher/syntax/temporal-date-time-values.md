# Temporal (Date/Time) values

The following table depicts the temporal value types and supported components:

| Type          | Date support | Time support | Time zone support |
| ------------- | ------------ | ------------ | ----------------- |
| Date          | X            |              |                   |
| Time          |              | X            | X                 |
| LocalTime     |              | X            |                   |
| DateTime      | X            | X            | X                 |
| LocalDateTime | X            | X            |                   |
| Duration      | -            | -            | -                 |

_Date_, _Time_, _LocalTime_, _DateTime_ and _LocalDateTime_ are _temporal instant_ types. A temporal instant value expresses a point in time with varying degrees of precision.

By contrast, _Duration_ is not a temporal instant type. A _Duration_ represents a temporal amount, capturing the difference in time between two instants, and can be negative. Duration only captures the amount of time between two instants, and thus does not encapsulate a start time and end time.

### Time zones

Time zones are represented either as an offset from UTC, or as a logical identifier of a _named time zone_ (these are based on the [IANA time zone database](https://www.iana.org/time-zones)). In either case the time is stored as UTC internally, and the time zone offset is only applied when the time is presented. This means that temporal instants can be ordered without taking time zone into account. If, however, two times are identical in UTC, then they are ordered by timezone.

When creating a time using a named time zone, the offset from UTC is computed from the rules in the time zone database to create a time instant in UTC, and to ensure the named time zone is a valid one.

It is possible for time zone rules to change in the IANA time zone database. For example, there could be alterations to the rules for daylight savings time in a certain area. If this occurs after the creation of a temporal instant, the presented time could differ from the originally-entered time, insofar as the local timezone is concerned. However, the absolute time in UTC would remain the same.

There are three ways of specifying a time zone in Cypher®:

* Specifying the offset from UTC in hours and minutes ([ISO 8601](https://en.wikipedia.org/wiki/ISO\_8601))
* Specifying a named time zone
* Specifying both the offset and the time zone name (with the requirement that these match)

The named time zone form uses the rules of the IANA time zone database to manage _daylight savings time_ (DST).

The default time zone of the database can be configured using the configuration option [`db.temporal.timezone`](https://neo4j.com/docs/operations-manual/3.5/reference/configuration-settings#config\_db.temporal.timezone). This configuration option influences the creation of temporal types for the following functions:

* Getting the current date and time without specifying a time zone.
* Creating a temporal type from its components without specifying a time zone.
* Creating a temporal type by parsing a string without specifying a time zone.
* Creating a temporal type by combining or selecting values that do not have a time zone component, and without specifying a time zone.
* Truncating a temporal value that does not have a time zone component, and without specifying a time zone.

### Temporal instants

#### Specifying temporal instants

A temporal instant consists of three parts; the `date`, the `time`, and the `timezone`. These parts may then be combined to produce the various temporal value types. Literal characters are denoted in **`bold`**.

| Temporal instant type | Composition of parts                            |
| --------------------- | ----------------------------------------------- |
| _Date_                | `<date>`                                        |
| _Time_                | `<time><timezone>` or **`T`**`<time><timezone>` |
| _LocalTime_           | `<time>` or **`T`**`<time>`                     |
| _DateTime_\*          | `<date>`**`T`**`<time><timezone>`               |
| _LocalDateTime_\*     | `<date>`**`T`**`<time>`                         |

\*When `date` and `time` are combined, `date` must be complete; i.e. fully identify a particular day.

**Specifying dates**

| Component               | Format | Description                                                                                                                                                  |
| ----------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Year                    | `YYYY` | Specified with at least four digits ([special rules apply in certain cases](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#cypher-temporal-year)) |
| Month                   | `MM`   | Specified with a double digit number from `01` to `12`                                                                                                       |
| Week                    | `ww`   | Always prefixed with **`W`** and specified with a double digit number from `01` to `53`                                                                      |
| Quarter                 | `q`    | Always prefixed with **`Q`** and specified with a single digit number from `1` to `4`                                                                        |
| Day of the month        | `DD`   | Specified with a double digit number from `01` to `31`                                                                                                       |
| Day of the week         | `D`    | Specified with a single digit number from `1` to `7`                                                                                                         |
| Day of the quarter      | `DD`   | Specified with a double digit number from `01` to `92`                                                                                                       |
| Ordinal day of the year | `DDD`  | Specified with a triple digit number from `001` to `366`                                                                                                     |

If the year is before `0000` or after `9999`, the following additional rules apply:

* **`-`** must prefix any year before `0000`
* **`+`** must prefix any year after `9999`
* The year must be separated from the next component with the following characters:
  * **`-`** if the next component is month or day of the year
  * Either **`-`** or **`W`** if the next component is week of the year
  * **`Q`** if the next component is quarter of the year

If the year component is prefixed with either `-` or `+`, and is separated from the next component, `Year` is allowed to contain up to nine digits. Thus, the allowed range of years is between -999,999,999 and +999,999,999. For all other cases, i.e. the year is between `0000` and `9999` (inclusive), `Year` must have exactly four digits (the year component is interpreted as a year of the Common Era (CE)).

The following formats are supported for specifying dates:

| Format               | Description                      | Example      | Interpretation of example |
| -------------------- | -------------------------------- | ------------ | ------------------------- |
| `YYYY-MM-DD`         | Calendar date: `Year-Month-Day`  | `2015-07-21` | `2015-07-21`              |
| `YYYYMMDD`           | Calendar date: `Year-Month-Day`  | `20150721`   | `2015-07-21`              |
| `YYYY-MM`            | Calendar date: `Year-Month`      | `2015-07`    | `2015-07-01`              |
| `YYYYMM`             | Calendar date: `Year-Month`      | `201507`     | `2015-07-01`              |
| `YYYY-`**`W`**`ww-D` | Week date: `Year-Week-Day`       | `2015-W30-2` | `2015-07-21`              |
| `YYYY`**`W`**`wwD`   | Week date: `Year-Week-Day`       | `2015W302`   | `2015-07-21`              |
| `YYYY-`**`W`**`ww`   | Week date: `Year-Week`           | `2015-W30`   | `2015-07-20`              |
| `YYYY`**`W`**`ww`    | Week date: `Year-Week`           | `2015W30`    | `2015-07-20`              |
| `YYYY-`**`Q`**`q-DD` | Quarter date: `Year-Quarter-Day` | `2015-Q2-60` | `2015-05-30`              |
| `YYYY`**`Q`**`qDD`   | Quarter date: `Year-Quarter-Day` | `2015Q260`   | `2015-05-30`              |
| `YYYY-`**`Q`**`q`    | Quarter date: `Year-Quarter`     | `2015-Q2`    | `2015-04-01`              |
| `YYYY`**`Q`**`q`     | Quarter date: `Year-Quarter`     | `2015Q2`     | `2015-04-01`              |
| `YYYY-DDD`           | Ordinal date: `Year-Day`         | `2015-202`   | `2015-07-21`              |
| `YYYYDDD`            | Ordinal date: `Year-Day`         | `2015202`    | `2015-07-21`              |
| `YYYY`               | Year                             | `2015`       | `2015-01-01`              |

The least significant components can be omitted. Cypher will assume omitted components to have their lowest possible value. For example, `2013-06` will be interpreted as being the same date as `2013-06-01`.

**Specifying times**

| Component  | Format      | Description                                                                                                                                                                                                                                                                                                   |
| ---------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Hour`     | `HH`        | Specified with a double digit number from `00` to `23`                                                                                                                                                                                                                                                        |
| `Minute`   | `MM`        | Specified with a double digit number from `00` to `59`                                                                                                                                                                                                                                                        |
| `Second`   | `SS`        | Specified with a double digit number from `00` to `59`                                                                                                                                                                                                                                                        |
| `fraction` | `sssssssss` | Specified with a number from `0` to `999999999`. It is not required to specify trailing zeros. `fraction` is an optional, sub-second component of `Second`. This can be separated from `Second` using either a full stop (`.`) or a comma (`,`). The `fraction` is in addition to the two digits of `Second`. |

Cypher does not support leap seconds; [UTC-SLS](https://www.cl.cam.ac.uk/\~mgk25/time/utc-sls/) (_UTC with Smoothed Leap Seconds_) is used to manage the difference in time between UTC and TAI (_International Atomic Time_).

The following formats are supported for specifying times:

| Format               | Description                   | Example        | Interpretation of example |
| -------------------- | ----------------------------- | -------------- | ------------------------- |
| `HH:MM:SS.sssssssss` | `Hour:Minute:Second.fraction` | `21:40:32.142` | `21:40:32.142`            |
| `HHMMSS.sssssssss`   | `Hour:Minute:Second.fraction` | `214032.142`   | `21:40:32.142`            |
| `HH:MM:SS`           | `Hour:Minute:Second`          | `21:40:32`     | `21:40:32.000`            |
| `HHMMSS`             | `Hour:Minute:Second`          | `214032`       | `21:40:32.000`            |
| `HH:MM`              | `Hour:Minute`                 | `21:40`        | `21:40:00.000`            |
| `HHMM`               | `Hour:Minute`                 | `2140`         | `21:40:00.000`            |
| `HH`                 | `Hour`                        | `21`           | `21:00:00.000`            |

The least significant components can be omitted. For example, a time may be specified with `Hour` and `Minute`, leaving out `Second` and `fraction`. On the other hand, specifying a time with `Hour` and `Second`, while leaving out `Minute`, is not possible.

**Specifying time zones**

The time zone is specified in one of the following ways:

* As an offset from UTC
* Using the **`Z`** shorthand for the UTC (`±00:00`) time zone

When specifying a time zone as an offset from UTC, the rules below apply:

* The time zone always starts with either a plus (`+`) or minus (`-`) sign.
  * Positive offsets, i.e. time zones beginning with `+`, denote time zones east of UTC.
  * Negative offsets, i.e. time zones beginning with `-`, denote time zones west of UTC.
* A double-digit hour offset follows the `+`/`-` sign.
* An optional double-digit minute offset follows the hour offset, optionally separated by a colon (`:`).
* The time zone of the International Date Line is denoted either by `+12:00` or `-12:00`, depending on country.

When creating values of the _DateTime_ temporal instant type, the time zone may also be specified using a named time zone, using the names from the IANA time zone database. This may be provided either in addition to, or in place of the offset. The named time zone is given last and is enclosed in square brackets (`[]`). Should both the offset and the named time zone be provided, the offset must match the named time zone.

The following formats are supported for specifying time zones:

| Format             | Description             | Example                      | Supported for `DateTime` | Supported for `Time` |
| ------------------ | ----------------------- | ---------------------------- | ------------------------ | -------------------- |
| **`Z`**            | UTC                     | `Z`                          | X                        | X                    |
| `±HH:MM`           | `Hour:Minute`           | `+09:30`                     | X                        | X                    |
| `±HH:MM[ZoneName]` | `Hour:Minute[ZoneName]` | `+08:45[Australia/Eucla]`    | X                        |                      |
| `±HHMM`            | `Hour:Minute`           | `+0100`                      | X                        | X                    |
| `±HHMM[ZoneName]`  | `Hour:Minute[ZoneName]` | `+0200[Africa/Johannesburg]` | X                        |                      |
| `±HH`              | `Hour`                  | `-08`                        | X                        | X                    |
| `±HH[ZoneName]`    | `Hour[ZoneName]`        | `+08[Asia/Singapore]`        | X                        |                      |
| `[ZoneName]`       | `[ZoneName]`            | `[America/Regina]`           | X                        |                      |

**Examples**

We show below examples of parsing temporal instant values using various formats. For more details, refer to [An overview of temporal instant type creation](https://neo4j.com/docs/cypher-manual/3.5/functions/temporal/#functions-temporal-create-overview).

Parsing a _DateTime_ using the _calendar date_ format:

Query

```cypher
RETURN datetime('2015-06-24T12:50:35.556+0100') AS theDateTime
```

| theDateTime                     |
| ------------------------------- |
| `2015-06-24T12:50:35.556+01:00` |
| 1 row                           |

Parsing a _LocalDateTime_ using the _ordinal date_ format:

Query

```cypher
RETURN localdatetime('2015185T19:32:24') AS theLocalDateTime
```

| theLocalDateTime      |
| --------------------- |
| `2015-07-04T19:32:24` |
| 1 row                 |

Parsing a _Date_ using the _week date_ format:

Query

```cypher
RETURN date('+2015-W13-4') AS theDate
```

| theDate      |
| ------------ |
| `2015-03-26` |
| 1 row        |

Parsing a _Time_:

Query

```cypher
RETURN time('125035.556+0100') AS theTime
```

| theTime              |
| -------------------- |
| `12:50:35.556+01:00` |
| 1 row                |

Parsing a _LocalTime_:

Query

```cypher
RETURN localtime('12:50:35.556') AS theLocalTime
```

| theLocalTime   |
| -------------- |
| `12:50:35.556` |
| 1 row          |

#### Accessing components of temporal instants

Components of temporal instant values can be accessed as properties.

| Component                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Description                                                                                                                                                                                                             | Type    | Range/Format                                                                                                                                                                                                             | Date | DateTime | LocalDateTime | Time | LocalTime |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---- | -------- | ------------- | ---- | --------- |
| `instant.year`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | The `year` component represents the [astronomical year number](https://en.wikipedia.org/wiki/Astronomical\_year\_numbering) of the instant [\[1\]](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnotedef1) | Integer | At least 4 digits. For more information, see the [rules for using the `Year` component](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#cypher-temporal-year)                                                  | X    | X        | X             |      |           |
| `instant.quarter`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | The _quarter-of-the-year_ component                                                                                                                                                                                     | Integer | `1` to `4`                                                                                                                                                                                                               | X    | X        | X             |      |           |
| `instant.month`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | The _month-of-the-year_ component                                                                                                                                                                                       | Integer | `1` to `12`                                                                                                                                                                                                              | X    | X        | X             |      |           |
| `instant.week`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | The _week-of-the-year_ component [\[2\]](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnotedef2)                                                                                                           | Integer | `1` to `53`                                                                                                                                                                                                              | X    | X        | X             |      |           |
| `instant.weekYear`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | The _year_ that the _week-of-year_ component belongs to [\[3\]](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnotedef3)                                                                                    | Integer | At least 4 digits. For more information, see the [rules for using the `Year` component](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#cypher-temporal-year)                                                  | X    | X        | X             |      |           |
| `instant.dayOfQuarter`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | The _day-of-the-quarter_ component                                                                                                                                                                                      | Integer | `1` to `92`                                                                                                                                                                                                              | X    | X        | X             |      |           |
| `instant.quarterDay`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | The _day-of-the-quarter_ component (alias for `instant.dayOfQuarter`)                                                                                                                                                   | Integer | `1` to `92`                                                                                                                                                                                                              | X    | X        | X             |      |           |
| `instant.day`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | The _day-of-the-month_ component                                                                                                                                                                                        | Integer | `1` to `31`                                                                                                                                                                                                              | X    | X        | X             |      |           |
| `instant.ordinalDay`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | The _day-of-the-year_ component                                                                                                                                                                                         | Integer | `1` to `366`                                                                                                                                                                                                             | X    | X        | X             |      |           |
| `instant.dayOfWeek`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | The _day-of-the-week_ component (the first day of the week is _Monday_)                                                                                                                                                 | Integer | `1` to `7`                                                                                                                                                                                                               | X    | X        | X             |      |           |
| `instant.weekDay`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | The _day-of-the-week_ component (alias for `instant.dayOfWeek`)                                                                                                                                                         | Integer | `1` to `7`                                                                                                                                                                                                               | X    | X        | X             |      |           |
| `instant.hour`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | The _hour_ component                                                                                                                                                                                                    | Integer | `0` to `23`                                                                                                                                                                                                              |      | X        | X             | X    | X         |
| `instant.minute`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | The _minute_ component                                                                                                                                                                                                  | Integer | `0` to `59`                                                                                                                                                                                                              |      | X        | X             | X    | X         |
| `instant.second`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | The _second_ component                                                                                                                                                                                                  | Integer | `0` to `59`                                                                                                                                                                                                              |      | X        | X             | X    | X         |
| `instant.millisecond`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | The _millisecond_ component                                                                                                                                                                                             | Integer | `0` to `999`                                                                                                                                                                                                             |      | X        | X             | X    | X         |
| `instant.microsecond`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | The _microsecond_ component                                                                                                                                                                                             | Integer | `0` to `999999`                                                                                                                                                                                                          |      | X        | X             | X    | X         |
| `instant.nanosecond`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | The _nanosecond_ component                                                                                                                                                                                              | Integer | `0` to `999999999`                                                                                                                                                                                                       |      | X        | X             | X    | X         |
| `instant.timezone`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | The _timezone_ component                                                                                                                                                                                                | String  | Depending on how the [time zone was specified](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#cypher-temporal-specify-time-zone), this is either a time zone name or an offset from UTC in the format `±HHMM` |      | X        |               | X    |           |
| `instant.offset`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | The _timezone_ offset                                                                                                                                                                                                   | String  | `±HHMM`                                                                                                                                                                                                                  |      | X        |               | X    |           |
| `instant.offsetMinutes`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | The _timezone_ offset in minutes                                                                                                                                                                                        | Integer | `-1080` to `+1080`                                                                                                                                                                                                       |      | X        |               | X    |           |
| `instant.offsetSeconds`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | The _timezone_ offset in seconds                                                                                                                                                                                        | Integer | `-64800` to `+64800`                                                                                                                                                                                                     |      | X        |               | X    |           |
| `instant.epochMillis`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | The number of milliseconds between `1970-01-01T00:00:00+0000` and the instant [\[4\]](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnotedef4)                                                              | Integer | Positive for instants after and negative for instants before `1970-01-01T00:00:00+0000`                                                                                                                                  |      | X        |               |      |           |
| `instant.epochSeconds`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | The number of seconds between `1970-01-01T00:00:00+0000` and the instant [\[5\]](https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnotedef5)                                                                   | Integer | Positive for instants after and negative for instants before `1970-01-01T00:00:00+0000`                                                                                                                                  |      | X        |               |      |           |
| <p><a href="https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnoteref1">1</a>. This is in accordance with the <a href="https://en.wikipedia.org/wiki/Gregorian_calendar">Gregorian calendar</a>; i.e. years AD/CE start at year 1, and the year before that (year 1 BC/BCE) is 0, while year 2 BCE is -1 etc.</p><p><a href="https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnoteref2">2</a>. The <a href="https://en.wikipedia.org/wiki/ISO_week_date#First_week">first week of any year</a> is the week that contains the first Thursday of the year, and thus always contains January 4.</p><p><a href="https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnoteref3">3</a>. For dates from December 29, this could be the next year, and for dates until January 3 this could be the previous year, depending on how week 1 begins.</p><p><a href="https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnoteref4">4</a>. <code>datetime().epochMillis</code> returns the equivalent value of the <a href="https://neo4j.com/docs/cypher-manual/3.5/functions/scalar/#functions-timestamp"><code>timestamp()</code></a> function.</p><p><a href="https://neo4j.com/docs/cypher-manual/3.5/syntax/temporal/#tnoteref5">5</a>. For the <em>nanosecond</em> part of the <em>epoch</em> offset, the regular <em>nanosecond</em> component (<code>instant.nanosecond</code>) can be used.</p> |                                                                                                                                                                                                                         |         |                                                                                                                                                                                                                          |      |          |               |      |           |

The following query shows how to extract the components of a _Date_ value:

Query

```cypher
WITH date({ year:1984, month:10, day:11 }) AS d
RETURN d.year, d.quarter, d.month, d.week, d.weekYear, d.day, d.ordinalDay, d.dayOfWeek, d.dayOfQuarter
```

| d.year | d.quarter | d.month | d.week | d.weekYear | d.day | d.ordinalDay | d.dayOfWeek | d.dayOfQuarter |
| ------ | --------- | ------- | ------ | ---------- | ----- | ------------ | ----------- | -------------- |
| `1984` | `4`       | `10`    | `41`   | `1984`     | `11`  | `285`        | `4`         | `11`           |
| 1 row  |           |         |        |            |       |              |             |                |

The following query shows how to extract the date related components of a _DateTime_ value:

Query

```cypher
WITH datetime({ year:1984, month:11, day:11, hour:12, minute:31, second:14, nanosecond: 645876123, timezone:'Europe/Stockholm' }) AS d
RETURN d.year, d.quarter, d.month, d.week, d.weekYear, d.day, d.ordinalDay, d.dayOfWeek, d.dayOfQuarter
```

| d.year | d.quarter | d.month | d.week | d.weekYear | d.day | d.ordinalDay | d.dayOfWeek | d.dayOfQuarter |
| ------ | --------- | ------- | ------ | ---------- | ----- | ------------ | ----------- | -------------- |
| `1984` | `4`       | `11`    | `45`   | `1984`     | `11`  | `316`        | `7`         | `42`           |
| 1 row  |           |         |        |            |       |              |             |                |

The following query shows how to extract the time related components of a _DateTime_ value:

Query

```cypher
WITH datetime({ year:1984, month:11, day:11, hour:12, minute:31, second:14, nanosecond: 645876123, timezone:'Europe/Stockholm' }) AS d
RETURN d.hour, d.minute, d.second, d.millisecond, d.microsecond, d.nanosecond
```

| d.hour | d.minute | d.second | d.millisecond | d.microsecond | d.nanosecond |
| ------ | -------- | -------- | ------------- | ------------- | ------------ |
| `12`   | `31`     | `14`     | `645`         | `645876`      | `645876123`  |
| 1 row  |          |          |               |               |              |

The following query shows how to extract the epoch time and timezone related components of a _DateTime_ value:

Query

```cypher
WITH datetime({ year:1984, month:11, day:11, hour:12, minute:31, second:14, nanosecond: 645876123, timezone:'Europe/Stockholm' }) AS d
RETURN d.timezone, d.offset, d.offsetMinutes, d.epochSeconds, d.epochMillis
```

| d.timezone           | d.offset   | d.offsetMinutes | d.epochSeconds | d.epochMillis  |
| -------------------- | ---------- | --------------- | -------------- | -------------- |
| `"Europe/Stockholm"` | `"+01:00"` | `60`            | `469020674`    | `469020674645` |
| 1 row                |            |                 |                |                |

### Durations

#### Specifying durations

A _Duration_ represents a temporal amount, capturing the difference in time between two instants, and can be negative.

The specification of a _Duration_ is prefixed with a **`P`**, and can use either a _unit-based form_ or a _date-and-time-based form_:

* Unit-based form: **`P`**`[n`**`Y`**`][n`**`M`**`][n`**`W`**`][n`**`D`**`][`**`T`**`[n`**`H`**`][n`**`M`**`][n`**`S`**`]]`
  * The square brackets (`[]`) denote an optional component (components with a zero value may be omitted).
  * The `n` denotes a numeric value which can be arbitrarily large.
  * The value of the last — and least significant — component may contain a decimal fraction.
  * Each component must be suffixed by a component identifier denoting the unit.
  * The unit-based form uses **`M`** as a suffix for both months and minutes. Therefore, time parts must always be preceded with **`T`**, even when no components of the date part are given.
* Date-and-time-based form: **`P`**`<date>`**`T`**`<time>`
  * Unlike the unit-based form, this form requires each component to be within the bounds of a valid _LocalDateTime_.

The following table lists the component identifiers for the unit-based form:

| Component identifier | Description | Comments                         |
| -------------------- | ----------- | -------------------------------- |
| **`Y`**              | Years       |                                  |
| **`M`**              | Months      | Must be specified before **`T`** |
| **`W`**              | Weeks       |                                  |
| **`D`**              | Days        |                                  |
| **`H`**              | Hours       |                                  |
| **`M`**              | Minutes     | Must be specified after **`T`**  |
| **`S`**              | Seconds     |                                  |

**Examples**

The following examples demonstrate various methods of parsing _Duration_ values. For more details, refer to [Creating a _Duration_ from a string](https://neo4j.com/docs/cypher-manual/3.5/functions/temporal/duration/#functions-duration-create-string).

Return a _Duration_ of `14` _days_, `16` _hours_ and `12` _minutes_:

Query

```cypher
RETURN duration('P14DT16H12M') AS theDuration
```

| theDuration   |
| ------------- |
| `P14DT16H12M` |
| 1 row         |

Return a _Duration_ of `5` _months_, `1` _day_ and `12` _hours_:

Query

```cypher
RETURN duration('P5M1.5D') AS theDuration
```

| theDuration |
| ----------- |
| `P5M1DT12H` |
| 1 row       |

Return a _Duration_ of `45` seconds:

Query

```cypher
RETURN duration('PT0.75M') AS theDuration
```

| theDuration |
| ----------- |
| `PT45S`     |
| 1 row       |

Return a _Duration_ of `2` _weeks_, `3` _days_ and `12` _hours_:

Query

```cypher
RETURN duration('P2.5W') AS theDuration
```

| theDuration |
| ----------- |
| `P17DT12H`  |
| 1 row       |

#### Accessing components of durations

A _Duration_ can have several components, each categorized into _Months_, _Days_, and _Seconds_ groups.

Components of _Duration_ values are truncated within their component groups as follows:

| Component Group         | Component                          | Description                 | Type                                                                                             | Details                                                                                                     |
| ----------------------- | ---------------------------------- | --------------------------- | ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| _Months_                | `duration.years`                   | The total number of _years_ | Integer                                                                                          | Each set of `4` _quarters_ is counted as `1` _year_; each set of `12` _months_ is counted as `1` _year_.    |
| `duration.quarters`     | The total number of _quarters_     | Integer                     | Each _year_ is counted as `4` _quarters_; each set of `3` _months_ is counted as `1` _quarter_.  |                                                                                                             |
| `duration.months`       | The total number of _months_       | Integer                     | Each _year_ is counted as `12` _months_; each\_quarter\_ is counted as `3` _months_.             |                                                                                                             |
| _Days_                  | `duration.weeks`                   | The total number of _weeks_ | Integer                                                                                          | Each set of `7` _days_ is counted as `1` _week_.                                                            |
| `duration.days`         | The total number of _days_         | Integer                     | Each _week_ is counted as `7` _days_.                                                            |                                                                                                             |
| _Seconds_               | `duration.hours`                   | The total number of _hours_ | Integer                                                                                          | Each set of `60` _minutes_ is counted as `1` _hour_; each set of `3600` _seconds_ is counted as `1` _hour_. |
| `duration.minutes`      | The total number of _minutes_      | Integer                     | Each _hour_ is counted as `60` _minutes_; each set of `60` _seconds_ is counted as `1` _minute_. |                                                                                                             |
| `duration.seconds`      | The total number of _seconds_      | Integer                     | Each _hour_ is counted as `3600` _seconds_; each _minute_ is counted as `60` _seconds_.          |                                                                                                             |
| `duration.milliseconds` | The total number of _milliseconds_ | Integer                     | Each set of `1000` _milliseconds_ is counted as `1` _second_.                                    |                                                                                                             |
| `duration.microseconds` | The total number of _microseconds_ | Integer                     | Each _millisecond_ is counted as `1000` _microseconds_.                                          |                                                                                                             |
| `duration.nanoseconds`  | The total number of _nanoseconds_  | Integer                     | Each _microsecond_ is counted as `1000` _nanoseconds_.                                           |                                                                                                             |

| <p>Please note that:</p><ul><li>Cypher uses <a href="https://www.cl.cam.ac.uk/~mgk25/time/utc-sls/">UTC-SLS</a> when handling leap seconds.</li><li>There are not always <code>24</code> <em>hours</em> in <code>1</code> <em>day</em>; when switching to/from daylight savings time, a <em>day</em> can have <code>23</code> or <code>25</code> <em>hours</em>.</li><li>There are not always the same number of <em>days</em> in a <em>month</em>.</li><li>Due to leap years, there are not always the same number of <em>days</em> in a <em>year</em>.</li></ul> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

It is also possible to access the smaller (less significant) components of a component group bounded by the largest (most significant) component of the group:

| Component                       | Component Group | Description                                                                 | Type    |
| ------------------------------- | --------------- | --------------------------------------------------------------------------- | ------- |
| `duration.quartersOfYear`       | Months          | The number of _quarters_ in the group that do not make a whole _year_       | Integer |
| `duration.monthsOfYear`         | Months          | The number of _months_ in the group that do not make a whole _year_         | Integer |
| `duration.monthsOfQuarter`      | Months          | The number of _months_ in the group that do not make a whole _quarter_      | Integer |
| `duration.daysOfWeek`           | Days            | The number of _days_ in the group that do not make a whole _week_           | Integer |
| `duration.minutesOfHour`        | Seconds         | The number of _minutes_ in the group that do not make a whole _hour_        | Integer |
| `duration.secondsOfMinute`      | Seconds         | The number of _seconds_ in the group that do not make a whole _minute_      | Integer |
| `duration.millisecondsOfSecond` | Seconds         | The number of _milliseconds_ in the group that do not make a whole _second_ | Integer |
| `duration.microsecondsOfSecond` | Seconds         | The number of _microseconds_ in the group that do not make a whole _second_ | Integer |
| `duration.nanosecondsOfSecond`  | Seconds         | The number of _nanoseconds_ in the group that do not make a whole _second_  | Integer |

The following query shows how to extract the month based components of a _Duration_ value:

Query

```cypher
WITH duration({ years: 1, months:5, days: 111, minutes: 42 }) AS d
RETURN d.years, d.quarters, d.quartersOfYear, d.months, d.monthsOfYear, d.monthsOfQuarter
```

| d.years | d.quarters | d.quartersOfYear | d.months | d.monthsOfYear | d.monthsOfQuarter |
| ------- | ---------- | ---------------- | -------- | -------------- | ----------------- |
| `1`     | `5`        | `1`              | `17`     | `5`            | `2`               |
| 1 row   |            |                  |          |                |                   |

The following query shows how to extract the day based components of a _Duration_ value:

Query

```cypher
WITH duration({ months:5, days: 25, hours: 1 }) AS d
RETURN d.weeks, d.days, d.daysOfWeek
```

| d.weeks | d.days | d.daysOfWeek |
| ------- | ------ | ------------ |
| `3`     | `25`   | `4`          |
| 1 row   |        |              |

The following query shows how to extract the most significant second based components of a _Duration_ value:

Query

```cypher
WITH duration({ years: 1, months:1, days:1, hours: 1, minutes: 1, seconds: 1, nanoseconds: 111111111 }) AS d
RETURN d.hours, d.minutes, d.seconds, d.milliseconds, d.microseconds, d.nanoseconds
```

| d.hours | d.minutes | d.seconds | d.milliseconds | d.microseconds | d.nanoseconds   |
| ------- | --------- | --------- | -------------- | -------------- | --------------- |
| `1`     | `61`      | `3661`    | `3661111`      | `3661111111`   | `3661111111111` |
| 1 row   |           |           |                |                |                 |

The following query shows how to extract the less significant second based components of a _Duration_ value:

Query

```cypher
WITH duration({ years: 1, months:1, days:1, hours: 1, minutes: 1, seconds: 1, nanoseconds: 111111111 }) AS d
RETURN d.minutesOfHour, d.secondsOfMinute, d.millisecondsOfSecond, d.microsecondsOfSecond, d.nanosecondsOfSecond
```

| d.minutesOfHour | d.secondsOfMinute | d.millisecondsOfSecond | d.microsecondsOfSecond | d.nanosecondsOfSecond |
| --------------- | ----------------- | ---------------------- | ---------------------- | --------------------- |
| `1`             | `1`               | `111`                  | `111111`               | `111111111`           |
| 1 row           |                   |                        |                        |                       |

### Examples

The following examples illustrate the use of some of the temporal functions and operators. Refer to [Temporal functions - instant types](https://neo4j.com/docs/cypher-manual/3.5/functions/temporal/) and [Temporal operators](https://neo4j.com/docs/cypher-manual/3.5/syntax/operators/#query-operators-temporal) for more details.

Create a _Duration_ representing 1.5 _days_:

Query

```cypher
RETURN duration({ days: 1, hours: 12 }) AS theDuration
```

| theDuration |
| ----------- |
| `P1DT12H`   |
| 1 row       |

Compute the _Duration_ between two temporal instants:

Query

```cypher
RETURN duration.between(date('1984-10-11'), date('2015-06-24')) AS theDuration
```

| theDuration |
| ----------- |
| `P30Y8M13D` |
| 1 row       |

Compute the number of days between two _Date_ values:

Query

```cypher
RETURN duration.inDays(date('2014-10-11'), date('2015-08-06')) AS theDuration
```

| theDuration |
| ----------- |
| `P299D`     |
| 1 row       |

Get the _Date_ of Thursday in the current week:

Query

```cypher
RETURN date.truncate('week', date(), { dayOfWeek: 4 }) AS thursday
```

| thursday     |
| ------------ |
| `2021-09-30` |
| 1 row        |

Get the _Date_ of the last day of the next month:

Query

```cypher
RETURN date.truncate('month', date()+ duration('P2M'))- duration('P1D') AS lastDay
```

| lastDay      |
| ------------ |
| `2021-10-31` |
| 1 row        |

Add a _Duration_ to a _Date_:

Query

```cypher
RETURN time('13:42:19')+ duration({ days: 1, hours: 12 }) AS theTime
```

| theTime     |
| ----------- |
| `01:42:19Z` |
| 1 row       |

Add two _Duration_ values:

Query

```cypher
RETURN duration({ days: 2, hours: 7 })+ duration({ months: 1, hours: 18 }) AS theDuration
```

| theDuration |
| ----------- |
| `P1M2DT25H` |
| 1 row       |

Multiply a _Duration_ by a number:

Query

```cypher
RETURN duration({ hours: 5, minutes: 21 })* 14 AS theDuration
```

| theDuration |
| ----------- |
| `PT74H54M`  |
| 1 row       |

Divide a _Duration_ by a number:

Query

```cypher
RETURN duration({ hours: 3, minutes: 16 })/ 2 AS theDuration
```

| theDuration |
| ----------- |
| `PT1H38M`   |
| 1 row       |

Examine whether two instants are less than one day apart:

Query

```cypher
WITH datetime('2015-07-21T21:40:32.142+0100') AS date1, datetime('2015-07-21T17:12:56.333+0100') AS date2
RETURN
CASE
WHEN date1 < date2
THEN date1 + duration("P1D")> date2
ELSE date2 + duration("P1D")> date1 END AS lessThanOneDayApart
```

| lessThanOneDayApart |
| ------------------- |
| `true`              |
| 1 row               |

Return the abbreviated name of the current month:

Query

```cypher
RETURN ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"][date().month-1] AS month
```

| month   |
| ------- |
| `"Sep"` |
| 1 row   |

### Temporal indexing

All temporal types can be indexed, and thereby support exact lookups for equality predicates. Indexes for temporal instant types additionally support range lookups.
