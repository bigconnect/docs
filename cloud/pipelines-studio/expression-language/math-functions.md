# Math Functions

Use math functions to perform math on numeric values.

You can replace any argument with a literal or an expression that evaluates to the argument. String literals must be enclosed in single or double quotation marks.

You can use the following data types with math functions:

* Double
* Float
* Integer
* Long
* StringNote: Data Pipelines parses a string as a Double value if the numeric value in a String field contains a dot ( . ). Otherwise, it parses the string as a Long value.

The expression language provides the following math functions:

### math:abs()

Returns the absolute value, or positive version, of the argument. If the argument is already positive, returns the original number.

Return type: Double, Float, Int, or Long, based on the data type of the argument.

### math:ceil()&#x20;

Returns the smallest integer greater than or equal to the argument. For example, ${math:ceil(8.0+3.6)} returns 12.0.&#x20;

Return type: Double.&#x20;

### math:floor ()&#x20;

Returns the largest integer greater than or equal to the argument.&#x20;

Return type: Double.&#x20;

### math:max()&#x20;

Returns the greater of two arguments.&#x20;

For example, if the value of the tip\_amount field is 2.5 and the value of the fare\_amount field is 12.5, then ${math:max(record:value("/tip\_amount"), record:value("/fare\_amount"))} returns 12.5.&#x20;

Return type: Double, Float, Int, or Long, based on the data type of the argument.&#x20;

### math:min()&#x20;

Returns the lesser of two arguments.&#x20;

Return type: Double, Float, Int, or Long, based on the data type of the argument.&#x20;

### math:round()&#x20;

Returns the closest number to the argument, rounding up for ties.&#x20;

Return type: Double or Long.
