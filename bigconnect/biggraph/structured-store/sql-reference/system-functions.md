# System Functions

### COALESCE

Returns the first value that is not null.

```
{COALESCE | NVL } (aValue, bValue [,...])
```

Examples:

```
COALESCE(A, B, C)
```

### DECODE

Returns the first matching value. NULL is considered to match NULL. If no match was found, then NULL or the last parameter (if the parameter count is even) is returned.

```
DECODE(value, whenValue, thenValue [,...])
```

Examples:

```
DECODE(RAND()>0.5, 0, 'Red', 1, 'Black')
```

### GREATEST

Returns the largest value that is not NULL, or NULL if all values are NULL.

```
GREATEST(aValue, bValue [,...])
```

Examples:

```
GREATEST(1, 2, 3)
```

### IFNULL

Returns the value of 'a' if it is not null, otherwise 'b'.

```
IFNULL(aValue, bValue)
```

Examples:

```
IFNULL(NULL, '')
```

### LEAST

Returns the smallest value that is not NULL, or NULL if all values are NULL.

```
LEAST(aValue, bValue [,...])
```

Examples:

```
LEAST(1, 2, 3)
```

### NULLIF

Returns NULL if 'a' is equals to 'b', otherwise 'a'.

```
NULLIF(aValue, bValue)
```

Examples:

```
NULLIF(A, B)
```

### NVL2

If the test value is null, then 'b' is returned. Otherwise, 'a' is returned. The data type of the returned value is the data type of 'a' if this is a text type.

```
NVL2(testValue, aValue, bValue)
```

Examples:

```
NVL2(X, 'not null', 'null')
```

### CASEWHEN

Returns 'aValue' if the boolean expression is true, otherwise 'bValue'.

```
CASEWHEN (boolean , aValue , bValue)
```

Examples:

```
CASEWHEN(ID=1, 'A', 'B')
```

### CAST

Converts a value to another data type. The following conversion rules are used:

* When converting a number to a boolean, 0 is considered as false and every other value is true.
* When converting a boolean to a number, false is 0 and true is 1.
* When converting a number to a number of another type, the value is checked for overflow.
* When converting a number to binary, the number of bytes will match the precision.
* When converting a string to binary, it is hex encoded.
* A hex string can be converted into the binary form and then to a number. If a direct conversion is not possible, the value is first converted to a string.

```
CAST (value AS dataType)
```

Examples:

```
CAST(NAME AS INT);
CAST(65535 AS BINARY);
CAST(CAST('FFFF' AS BINARY) AS INT);
```

### CONVERT

Converts a value to another data type.

```
CONVERT (value , dataType)
```

Examples:

```
CONVERT(NAME, INT)
```

### TABLE

Returns the result set. TABLE\_DISTINCT removes duplicate rows.

```
TABLE	| TABLE_DISTINCT	(name dataType = expression)
```

Examples:

```
SELECT * FROM TABLE(ID INT=(1, 2), NAME VARCHAR=('Hello', 'World'))
```

