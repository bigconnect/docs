# INVALID\_PARAMETER\_VALUE error class

SQLSTATE: 22023

The value of parameter(s) `<parameter>` in `<functionName>` is invalid:

This error class has the following derived error classes:

### AES\_CRYPTO\_ERROR

detail message: `<detailMessage>`

### AES\_IV\_LENGTH

supports 16-byte CBC IVs and 12-byte GCM IVs, but got `<actualLength>` bytes for `<mode>`.

### AES\_KEY\_LENGTH

expects a binary value with 16, 24 or 32 bytes, but got `<actualLength>` bytes.

### DATETIME\_UNIT

expects one of the units without quotes YEAR, QUARTER, MONTH, WEEK, DAY, DAYOFYEAR, HOUR, MINUTE, SECOND, MILLISECOND, MICROSECOND, but got the string literal `<invalidValue>`.

### PATTERN

`<value>`.

### REGEX\_GROUP\_INDEX

Expects group index between 0 and `<groupCount>`, but got `<groupIndex>`.

### ZERO\_INDEX

expects `%1$`, `%2$` and so on, but got `%0$`.
