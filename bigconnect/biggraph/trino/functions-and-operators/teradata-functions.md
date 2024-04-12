# Teradata functions

These functions provide compatibility with Teradata SQL.

### String functions[#](https://trino.io/docs/current/functions/teradata.html#string-functions) <a href="#string-functions" id="string-functions"></a>

char2hexint(_string_) → varchar[#](https://trino.io/docs/current/functions/teradata.html#char2hexint)

Returns the hexadecimal representation of the UTF-16BE encoding of the string.

index(_string_, _substring_) → bigint[#](https://trino.io/docs/current/functions/teradata.html#index)

Alias for [`strpos()`](https://trino.io/docs/current/functions/string.html#strpos) function.

### Date functions[#](https://trino.io/docs/current/functions/teradata.html#date-functions) <a href="#date-functions" id="date-functions"></a>

The functions in this section use a format string that is compatible with the Teradata datetime functions. The following table, based on the Teradata reference manual, describes the supported format specifiers:

| Specifier     | Description                        |
| ------------- | ---------------------------------- |
| `- / , . ; :` | Punctuation characters are ignored |
| `dd`          | Day of month (1-31)                |
| `hh`          | Hour of day (1-12)                 |
| `hh24`        | Hour of the day (0-23)             |
| `mi`          | Minute (0-59)                      |
| `mm`          | Month (01-12)                      |
| `ss`          | Second (0-59)                      |
| `yyyy`        | 4-digit year                       |
| `yy`          | 2-digit year                       |

Warning

Case insensitivity is not currently supported. All specifiers must be lowercase.

to\_char(_timestamp_, _format_) → varchar[#](https://trino.io/docs/current/functions/teradata.html#to\_char)

Formats `timestamp` as a string using `format`.

to\_timestamp(_string_, _format_) → timestamp[#](https://trino.io/docs/current/functions/teradata.html#to\_timestamp)

Parses `string` into a `TIMESTAMP` using `format`.

to\_date(_string_, _format_) → date[#](https://trino.io/docs/current/functions/teradata.html#to\_date)

Parses `string` into a `DATE` using `format`.
