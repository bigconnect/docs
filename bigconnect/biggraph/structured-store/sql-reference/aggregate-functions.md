# Aggregate Functions

### AVG

```
AVG ([DISTINCT] expression)
```

The average (mean) value. If no rows are selected, the result is `NULL`. Aggregates are only allowed in select statements. The returned value is of the same data type as the parameter.

#### Parameters

* `DISTINCT` - optional keyword. If presents, will average the unique values.

#### Examples

Calculating average players' age:

```
SELECT AVG(age) "AverageAge" FROM Players;
```

### BIT\_AND

```
BIT_AND (expression)
```

The bitwise AND of all non-null values. If no rows are selected, the result is NULL. Aggregates are only allowed in select statements.

A logical AND operation is performed on each pair of corresponding bits of two binary expressions of equal length.

In each pair, it returns 1 if the first bit is 1 AND the second bit is 1. Else, it returns 0.

### BIT\_OR

```
BIT_OR (expression)
```

The bitwise OR of all non-null values. If no rows are selected, the result is NULL. Aggregates are only allowed in select statements.

A logical OR operation is performed on each pair of corresponding bits of two binary expressions of equal length.

In each pair, the result is 1 if the first bit is 1 OR the second bit is 1 OR both bits are 1, and otherwise the result is 0.

### COUNT

```
COUNT (* | [DISTINCT] expression)
```

The count of all entries or of the non-null values. This method returns a long. If no entries are selected, the result is 0. Aggregates are only allowed in select statements.

#### Example

Calculate the number of players in every city:

```
SELECT city_id, COUNT(*) FROM Players GROUP BY city_id;
```

### GROUP\_CONCAT

```
GROUP_CONCAT([DISTINCT] expression || [expression || [expression ...]]
  [ORDER BY expression [ASC|DESC], [[ORDER BY expression [ASC|DESC]]]
  [SEPARATOR expression])
```

Concatenates strings with a separator. The default separator is a ',' (without whitespace). This method returns a string. If no entries are selected, the result is NULL. Aggregates are only allowed in select statements.

The `expression` can be a concatenation of columns and strings using the `||` operator, for example: `column1 || "=" || column2`.

#### Parameters

* `DISTINCT` - filters the result set for unique sets of expressions.
* `expression` - specifies an expression that may be a column name, a result of another function, or a math operation.
* `ORDER BY` - orders rows by expression.
* `SEPARATOR` - overrides a string separator. By default, the separator character is the comma ','.

#### Example

Group all players' names in one row:

```
SELECT GROUP_CONCAT(name ORDER BY id SEPARATOR ', ') FROM Players;
```

### MAX

```
MAX (expression)
```

Returns the highest value. If no entries are selected, the result is NULL. Aggregates are only allowed in select statements. The returned value is of the same data type as the parameter.

#### Parameters

* `expression` - may be a column name, a result of another function, or a math operation.

#### Example

Return the height of the ​tallest player:

```
SELECT MAX(height) FROM Players;
```

### MIN

```
MIN (expression)
```

Returns the lowest value. If no entries are selected, the result is NULL. Aggregates are only allowed in select statements. The returned value is of the same data type as the parameter.

#### Parameters

* `expression` - may be a column name, the result of another function, or a math operation.

#### Example

Return the age of the youngest player:

```
SELECT MIN(age) FROM Players;
```

### SUM

```
SUM ([DISTINCT] expression)
```

Returns the sum of all values. If no entries are selected, the result is NULL. Aggregates are only allowed in select statements. The data type of the returned value depends on the parameter data.

#### Parameters

* `DISTINCT` - accumulate unique values only.
* `expression` - may be a column name, the result of another function, or a math operation.

#### Example

Get the total number of goals scored by all players:

```
SELECT SUM(goal) FROM Players;
```
