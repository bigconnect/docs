# DATATYPE\_MISMATCH error class

SQLSTATE: 42K09

Cannot resolve `<sqlExpr>` due to data type mismatch:

This error class has the following derived error classes:

### ARRAY\_FUNCTION\_DIFF\_TYPES

Input to `<functionName>` should have been `<dataType>` followed by a value with same element type, but it’s \[`<leftType>`, `<rightType>`].

### BINARY\_ARRAY\_DIFF\_TYPES

Input to function `<functionName>` should have been two `<arrayType>` with same element type, but it’s \[`<leftType>`, `<rightType>`].

### BINARY\_OP\_DIFF\_TYPES

the left and right operands of the binary operator have incompatible types (`<left>` and `<right>`).

### BINARY\_OP\_WRONG\_TYPE

the binary operator requires the input type `<inputType>`, not `<actualDataType>`.

### BLOOM\_FILTER\_BINARY\_OP\_WRONG\_TYPE

The Bloom filter binary input to `<functionName>` should be either a constant value or a scalar subquery expression, but it’s `<actual>`.

### BLOOM\_FILTER\_WRONG\_TYPE

Input to function `<functionName>` should have been `<expectedLeft>` followed by value with `<expectedRight>`, but it’s \[`<actual>`].

### CANNOT\_CONVERT\_TO\_JSON

Unable to convert column `<name>` of type `<type>` to JSON.

### CANNOT\_DROP\_ALL\_FIELDS

Cannot drop all fields in struct.

### CAST\_WITHOUT\_SUGGESTION

cannot cast `<srcType>` to `<targetType>`.

### CAST\_WITH\_CONF\_SUGGESTION

cannot cast `<srcType>` to `<targetType>` with ANSI mode on. If you have to cast `<srcType>` to `<targetType>`, you can set `<config>` as `<configVal>`.

### CAST\_WITH\_FUNC\_SUGGESTION

cannot cast `<srcType>` to `<targetType>`. To convert values from `<srcType>` to `<targetType>`, you can use the functions `<functionNames>` instead.

### CREATE\_MAP\_KEY\_DIFF\_TYPES

The given keys of function `<functionName>` should all be the same type, but they are `<dataType>`.

### CREATE\_MAP\_VALUE\_DIFF\_TYPES

The given values of function `<functionName>` should all be the same type, but they are `<dataType>`.

### CREATE\_NAMED\_STRUCT\_WITHOUT\_FOLDABLE\_STRING

Only foldable `STRING` expressions are allowed to appear at odd position, but they are `<inputExprs>`.

### DATA\_DIFF\_TYPES

Input to `<functionName>` should all be the same type, but it’s `<dataType>`.

### FILTER\_NOT\_BOOLEAN

Filter expression `<filter>` of type `<type>` is not a boolean.

### HASH\_MAP\_TYPE

Input to the function `<functionName>` cannot contain elements of the “MAP” type. In Spark, same maps may have different hashcode, thus hash expressions are prohibited on “MAP” elements. To restore previous behavior set “spark.sql.legacy.allowHashOnMapType” to “true”.

### INPUT\_SIZE\_NOT\_ONE

Length of `<exprName>` should be 1.

### INVALID\_ARG\_VALUE

The `<inputName>` value must to be a `<requireType>` literal of `<validValues>`, but got `<inputValue>`.

### INVALID\_JSON\_MAP\_KEY\_TYPE

Input schema `<schema>` can only contain STRING as a key type for a MAP.

### INVALID\_JSON\_SCHEMA

Input schema `<schema>` must be a struct, an array or a map.

### INVALID\_MAP\_KEY\_TYPE

The key of map cannot be/contain `<keyType>`.

### INVALID\_ORDERING\_TYPE

The `<functionName>` does not support ordering on type `<dataType>`.

### INVALID\_ROW\_LEVEL\_OPERATION\_ASSIGNMENTS

`<errors>`

### IN\_SUBQUERY\_DATA\_TYPE\_MISMATCH

The data type of one or more elements in the left hand side of an IN subquery is not compatible with the data type of the output of the subquery. Mismatched columns: \[`<mismatchedColumns>`], left side: \[`<leftType>`], right side: \[`<rightType>`].

### IN\_SUBQUERY\_LENGTH\_MISMATCH

The number of columns in the left hand side of an IN subquery does not match the number of columns in the output of subquery. Left hand side columns(length: `<leftLength>`): \[`<leftColumns>`], right hand side columns(length: `<rightLength>`): \[`<rightColumns>`].

### MAP\_CONCAT\_DIFF\_TYPES

The `<functionName>` should all be of type map, but it’s `<dataType>`.

### MAP\_FUNCTION\_DIFF\_TYPES

Input to `<functionName>` should have been `<dataType>` followed by a value with same key type, but it’s \[`<leftType>`, `<rightType>`].

### MAP\_ZIP\_WITH\_DIFF\_TYPES

Input to the `<functionName>` should have been two maps with compatible key types, but it’s \[`<leftType>`, `<rightType>`].

### NON\_FOLDABLE\_INPUT

the input `<inputName>` should be a foldable `<inputType>` expression; however, got `<inputExpr>`.

### NON\_STRING\_TYPE

all arguments must be strings.

### NULL\_TYPE

Null typed values cannot be used as arguments of `<functionName>`.

### PARAMETER\_CONSTRAINT\_VIOLATION

The `<leftExprName>`(`<leftExprValue>`) must be `<constraint>` the `<rightExprName>`(`<rightExprValue>`).

### RANGE\_FRAME\_INVALID\_TYPE

The data type `<orderSpecType>` used in the order specification does not match the data type `<valueBoundaryType>` which is used in the range frame.

### RANGE\_FRAME\_MULTI\_ORDER

A range window frame with value boundaries cannot be used in a window specification with multiple order by expressions: `<orderSpec>`.

### RANGE\_FRAME\_WITHOUT\_ORDER

A range window frame cannot be used in an unordered window specification.

### SEQUENCE\_WRONG\_INPUT\_TYPES

`<functionName>` uses the wrong parameter type. The parameter type must conform to:

1. The start and stop expressions must resolve to the same type.
2. If start and stop expressions resolve to the `<startType>` type, then the step expression must resolve to the `<stepType>` type.
3. Otherwise, if start and stop expressions resolve to the `<otherStartType>` type, then the step expression must resolve to the same type.

### SPECIFIED\_WINDOW\_FRAME\_DIFF\_TYPES

Window frame bounds `<lower>` and `<upper>` do not have the same type: `<lowerType>` <> `<upperType>`.

### SPECIFIED\_WINDOW\_FRAME\_INVALID\_BOUND

Window frame upper bound `<upper>` does not follow the lower bound `<lower>`.

### SPECIFIED\_WINDOW\_FRAME\_UNACCEPTED\_TYPE

The data type of the `<location>` bound `<exprType>` does not match the expected data type `<expectedType>`.

### SPECIFIED\_WINDOW\_FRAME\_WITHOUT\_FOLDABLE

Window frame `<location>` bound `<expression>` is not a literal.

### SPECIFIED\_WINDOW\_FRAME\_WRONG\_COMPARISON

The lower bound of a window frame must be `<comparison>` to the upper bound.

### STACK\_COLUMN\_DIFF\_TYPES

The data type of the column (`<columnIndex>`) do not have the same type: `<leftType>` (`<leftParamIndex>`) <> `<rightType>` (`<rightParamIndex>`).

### TYPE\_CHECK\_FAILURE\_WITH\_HINT

`<msg><hint>`.

### UNEXPECTED\_CLASS\_TYPE

class `<className>` not found.

### UNEXPECTED\_INPUT\_TYPE

Parameter `<paramIndex>` requires the `<requiredType>` type, however `<inputSql>` has the type `<inputType>`.

### UNEXPECTED\_NULL

The `<exprName>` must not be null.

### UNEXPECTED\_RETURN\_TYPE

The `<functionName>` requires return `<expectedType>` type, but the actual is `<actualType>` type.

### UNEXPECTED\_STATIC\_METHOD

cannot find a static method `<methodName>` that matches the argument types in `<className>`.

### UNSUPPORTED\_INPUT\_TYPE

The input of `<functionName>` can’t be `<dataType>` type data.

### VALUE\_OUT\_OF\_RANGE

The `<exprName>` must be between `<valueRange>` (current value = `<currentValue>`).

### WRONG\_NUM\_ARG\_TYPES

The expression requires `<expectedNum>` argument types but the actual number is `<actualNum>`.

### WRONG\_NUM\_ENDPOINTS

The number of endpoints must be >= 2 to construct intervals but the actual number is `<actualNumber>`.
