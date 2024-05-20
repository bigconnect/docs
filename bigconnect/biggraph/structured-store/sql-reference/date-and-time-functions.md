# Date and Time Functions

### CURRENT\_DATE

```
{CURRENT_DATE [()] | CURDATE() | SYSDATE | TODAY}
```

Returns the current date. When called multiple times within a transaction, this function returns the same value.

Example:

```
CURRENT_DATE()
```

### CURRENT\_TIME

```
{CURRENT_TIME [ () ] | CURTIME()}
```

Returns the current time. When called multiple times within a transaction, this function returns the same value.

Example:

```
CURRENT_TIME()
```

### CURRENT\_TIMESTAMP

```
{CURRENT_TIMESTAMP [([int])] | NOW([int])}
```

Returns the current timestamp. The precision parameter for nanoseconds precision is optional. This method always returns the same value within a transaction.

Example:

```
CURRENT_TIMESTAMP()
```

### DATEADD

```
{DATEADD| TIMESTAMPADD} (unitString, addIntLong, timestamp)
```

Adds units to a timestamp. The string indicates the unit. Use negative values to subtract units. `addIntLong` may be a long value when manipulating milliseconds, otherwise its range is restricted to `int`. The same units as in the EXTRACT function are supported. The DATEADD method returns a timestamp. The TIMESTAMPADD method returns a long.

Example:

```
DATEADD('MONTH', 1, DATE '2001-01-31')
```

### DATEDIFF

```
{DATEDIFF | TIMESTAMPDIFF} (unitString, aTimestamp, bTimestamp)
```

Returns the number of crossed unit boundaries between two timestamps. This method returns a `long`. The string indicates the unit. The same units as in the EXTRACT function are supported.

Example:

```
DATEDIFF('YEAR', T1.CREATED, T2.CREATED)
```

### DAYNAME

```
DAYNAME(date)
```

Returns the name of the day (in English).

Example:

```
DAYNAME(CREATED)
```

### DAY\_OF\_MONTH

```
DAY_OF_MONTH(date)
```

Returns the day of the month (1-31).

Example:

```
DAY_OF_MONTH(CREATED)
```

### DAY\_OF\_WEEK

```
DAY_OF_WEEK(date)
```

Returns the day of the week (1 means Sunday).

Example:

```
DAY_OF_WEEK(CREATED)
```

### DAY\_OF\_YEAR

```
DAY_OF_YEAR(date)
```

Returns the day of the year (1-366).

Example:

```
DAY_OF_YEAR(CREATED)
```

### EXTRACT

```
EXTRACT ({EPOCH | YEAR | YY | QUARTER | MONTH | MM | WEEK | ISO_WEEK
| DAY | DD | DAY_OF_YEAR | DOY | DAY_OF_WEEK | DOW | ISO_DAY_OF_WEEK
| HOUR | HH | MINUTE | MI | SECOND | SS | MILLISECOND | MS
| MICROSECOND | MCS | NANOSECOND | NS}
FROM timestamp)
```

Returns a specific value from a timestamps. This method returns an `int`.

Example:

```
EXTRACT(SECOND FROM CURRENT_TIMESTAMP)
```

### FORMATDATETIME

```
FORMATDATETIME (timestamp, formatString [,localeString [,timeZoneString]])
```

Formats a date, time, or timestamp as a string. The most important format characters are: `y` year, `M` month, `d` day, `H` hour, `m` minute, `s` second. For details about the format, see `java.text.SimpleDateFormat`. This method returns a `string`.

Example:

```
FORMATDATETIME(TIMESTAMP '2001-02-03 04:05:06', 'EEE, d MMM yyyy HH:mm:ss z', 'en', 'GMT')
```

### HOUR

```
HOUR(timestamp)
```

Returns the hour (0-23) from a timestamp.

Example:

```
HOUR(CREATED)
```

### MINUTE

```
MINUTE(timestamp)
```

Returns the minute (0-59) from a timestamp.

Example:

```
MINUTE(CREATED)
```

### MONTH

```
MONTH(timestamp)
```

Returns the month (1-12) from a timestamp.

Example:

```
MONTH(CREATED)
```

### MONTHNAME

```
MONTHNAME(date)
```

Returns the name of the month (in English).

Example:

```
MONTHNAME(CREATED)
```

### PARSEDATETIME

```
PARSEDATETIME(string, formatString [, localeString [, timeZoneString]])
```

Parses a string and returns a `timestamp`. The most important format characters are: `y` year, `M` month, `d` day, `H` hour, `m` minute, `s` second. For details about the format, see `java.text.SimpleDateFormat`.

Example:

```
PARSEDATETIME('Sat, 3 Feb 2001 03:05:06 GMT', 'EEE, d MMM yyyy HH:mm:ss z', 'en', 'GMT')
```

### QUARTER

```
QUARTER(timestamp)
```

Returns the quarter (1-4) from a timestamp.

Example:

```
QUARTER(CREATED)
```

### SECOND

```
SECOND(timestamp)
```

Returns the second (0-59) from a timestamp.

Example:

```
SECOND(CREATED)
```

### WEEK

```
WEEK(timestamp)
```

Returns the week (1-53) from a timestamp. This method uses the current system locale.

Example:

```
WEEK(CREATED)
```

### YEAR

```
YEAR(timestamp)
```

Returns the year from a timestamp.

Example:

```
YEAR(CREATED)
```

