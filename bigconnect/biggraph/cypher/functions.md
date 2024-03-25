# Functions

Please note

* Functions in Cypher return `null` if an input parameter is `null`.
* Functions taking a string as input all operate on _Unicode characters_ rather than on a standard `char[]`. For example, the `size()` function applied to any _Unicode character_ will return **1**, even if the character does not fit in the 16 bits of one `char`.

## **Predicate functions**

These functions return either true or false for the given arguments.

| Function | Description                                                                                                                        |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| all()    | Tests whether the predicate holds for all elements in a list.                                                                      |
| any()    | Tests whether the predicate holds for at least one element in a list.                                                              |
| exists() | Returns true if a match for the pattern exists in the graph, or if the specified property exists in the node, relationship or map. |
| none()   | Returns true if the predicate holds for no element in a list.                                                                      |
| single() | Returns true if the predicate holds for exactly one of the elements in a list.                                                     |

## **Scalar functions**

These functions return a single value.

| Function                             | Description                                                                                                   |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| coalesce()                           | Returns the first non-`null` value in a list of expressions.                                                  |
| endNode()                            | Returns the end node of a relationship.                                                                       |
| head()                               | Returns the first element in a list.                                                                          |
| id()                                 | Returns the id of a relationship or node.                                                                     |
| last()                               | Returns the last element in a list.                                                                           |
| length()                             | Returns the length of a path.                                                                                 |
| properties()                         | Returns a map containing all the properties of a node or relationship.                                        |
| randomUUID()                         | Returns a string value corresponding to a randomly-generated UUID.                                            |
| size()                               | Returns the number of items in a list.                                                                        |
| size() applied to pattern expression | Returns the number of paths matching the pattern expression.                                                  |
| size() applied to string             | Returns the number of Unicode characters in a string.                                                         |
| startNode()                          | Returns the start node of a relationship.                                                                     |
| timestamp()                          | Returns the difference, measured in milliseconds, between the current time and midnight, January 1, 1970 UTC. |
| toBoolean()                          | Converts a string value to a boolean value.                                                                   |
| toFloat()                            | Converts an integer or string value to a floating point number.                                               |
| toInteger()                          | Converts a floating point or string value to an integer value.                                                |
| type()                               | Returns the string representation of the relationship type.                                                   |

## **Aggregating functions**

These functions take multiple values as arguments, and calculate and return an aggregated value from them.

| Function               | Description                                                                                   |
| ---------------------- | --------------------------------------------------------------------------------------------- |
| avg() - Numeric values | Returns the average of a set of numeric values.                                               |
| avg() - Durations      | Returns the average of a set of Durations.                                                    |
| collect()              | Returns a list containing the values returned by an expression.                               |
| count()                | Returns the number of values or rows.                                                         |
| max()                  | Returns the maximum value in a set of values.                                                 |
| min()                  | Returns the minimum value in a set of values.                                                 |
| percentileCont()       | Returns the percentile of a value over a group using linear interpolation.                    |
| percentileDisc()       | Returns the nearest value to the given percentile over a group using a rounding method.       |
| stDev()                | Returns the standard deviation for the given value over a group for a sample of a population. |
| stDevP()               | Returns the standard deviation for the given value over a group for an entire population.     |
| sum() - Numeric values | Returns the sum of a set of numeric values.                                                   |
| sum() - Durations      | Returns the sum of a set of Durations.                                                        |

## **List functions**

These functions return lists of other values.&#x20;

| Function        | Description                                                                                                                          |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| extract()       | Returns a list `lresult` containing the values resulting from an expression which has been applied to each element in a list `list`. |
| filter()        | Returns a list `lresult` containing all the elements from a list `list` that comply with a predicate.                                |
| keys()          | Returns a list containing the string representations for all the property names of a node, relationship, or map.                     |
| labels()        | Returns a list containing the string representations for all the labels of a node.                                                   |
| nodes()         | Returns a list containing all the nodes in a path.                                                                                   |
| range()         | Returns a list comprising all integer values within a specified range.                                                               |
| reduce()        | Runs an expression against individual elements of a list, storing the result of the expression in an accumulator.                    |
| relationships() | Returns a list containing all the relationships in a path.                                                                           |
| reverse()       | Returns a list in which the order of all elements in the original list have been reversed.                                           |
| tail()          | Returns all but the first element in a list.                                                                                         |

## **Mathematical functions - numeric**

These functions all operate on numerical expressions only, and will return an error if used on any other values.

| Function | Description                                                                                                               |
| -------- | ------------------------------------------------------------------------------------------------------------------------- |
| abs()    | Returns the absolute value of a number.                                                                                   |
| ceil()   | Returns the smallest floating point number that is greater than or equal to a number and equal to a mathematical integer. |
| floor()  | Returns the largest floating point number that is less than or equal to a number and equal to a mathematical integer.     |
| rand()   | Returns a random floating point number in the range from 0 (inclusive) to 1 (exclusive); i.e. `[0,1)`.                    |
| round()  | Returns the value of a number rounded to the nearest integer.                                                             |
| sign()   | Returns the signum of a number: `0` if the number is `0`, `-1` for any negative number, and `1` for any positive number.  |

## **Mathematical functions - logarithmic**

These functions all operate on numerical expressions only, and will return an error if used on any other values.

| Function | Description                                                                                                     |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| e()      | Returns the base of the natural logarithm, `e`.                                                                 |
| exp()    | Returns `e^n`, where `e` is the base of the natural logarithm, and `n` is the value of the argument expression. |
| log()    | Returns the natural logarithm of a number.                                                                      |
| log10()  | Returns the common logarithm (base 10) of a number.                                                             |
| sqrt()   | Returns the square root of a number.                                                                            |

## **Mathematical functions - trigonometric**

These functions all operate on numerical expressions only, and will return an error if used on any other values.

All trigonometric functions operate on radians, unless otherwise specified.

| Function   | Description                                                 |
| ---------- | ----------------------------------------------------------- |
| acos()     | Returns the arccosine of a number in radians.               |
| asin()     | Returns the arcsine of a number in radians.                 |
| atan()     | Returns the arctangent of a number in radians.              |
| atan2()    | Returns the arctangent2 of a set of coordinates in radians. |
| cos()      | Returns the cosine of a number.                             |
| cot()      | Returns the cotangent of a number.                          |
| degrees()  | Converts radians to degrees.                                |
| haversin() | Returns half the versine of a number.                       |
| pi()       | Returns the mathematical constant _pi_.                     |
| radians()  | Converts degrees to radians.                                |
| sin()      | Returns the sine of a number.                               |
| tan()      | Returns the tangent of a number.                            |

## **String functions**

These functions are used to manipulate strings or to create a string representation of another value.

| Function    | Description                                                                                                                               |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| left()      | Returns a string containing the specified number of leftmost characters of the original string.                                           |
| lTrim()     | Returns the original string with leading whitespace removed.                                                                              |
| replace()   | Returns a string in which all occurrences of a specified string in the original string have been replaced by another (specified) string.  |
| reverse()   | Returns a string in which the order of all characters in the original string have been reversed.                                          |
| right()     | Returns a string containing the specified number of rightmost characters of the original string.                                          |
| rTrim()     | Returns the original string with trailing whitespace removed.                                                                             |
| split()     | Returns a list of strings resulting from the splitting of the original string around matches of the given delimiter.                      |
| substring() | Returns a substring of the original string, beginning with a 0-based index start and length.                                              |
| toLower()   | Returns the original string in lowercase.                                                                                                 |
| toString()  | Converts an integer, float, boolean or temporal type (i.e. Date, Time, LocalTime, DateTime, LocalDateTime or Duration) value to a string. |
| toUpper()   | Returns the original string in uppercase.                                                                                                 |
| trim()      | Returns the original string with leading and trailing whitespace removed.                                                                 |

## **Temporal functions - instant types**

Values of the temporal types — _Date_, _Time_, _LocalTime_, _DateTime_, and _LocalDateTime_ — can be created manipulated using the following functions:

| Function                                             | Description                                                                                |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| date()                                               | Returns the current _Date_.                                                                |
| date.transaction()                                   | Returns the current _Date_ using the `transaction` clock.                                  |
| date.statement()                                     | Returns the current _Date_ using the `statement` clock.                                    |
| date.realtime()                                      | Returns the current _Date_ using the `realtime` clock.                                     |
| date({year \[, month, day]})                         | Returns a calendar (Year-Month-Day) _Date_.                                                |
| date({year \[, week, dayOfWeek]})                    | Returns a week (Year-Week-Day) _Date_.                                                     |
| date({year \[, quarter, dayOfQuarter]})              | Returns a quarter (Year-Quarter-Day) _Date_.                                               |
| date({year \[, ordinalDay]})                         | Returns an ordinal (Year-Day) _Date_.                                                      |
| date(string)                                         | Returns a _Date_ by parsing a string.                                                      |
| date({map})                                          | Returns a _Date_ from a map of another temporal value’s components.                        |
| date.truncate()                                      | Returns a _Date_ obtained by truncating a value at a specific component boundary.          |
| datetime()                                           | Returns the current _DateTime_.                                                            |
| datetime.transaction()                               | Returns the current _DateTime_ using the `transaction` clock.                              |
| datetime.statement()                                 | Returns the current _DateTime_ using the `statement` clock.                                |
| datetime.realtime()                                  | Returns the current _DateTime_ using the `realtime` clock.                                 |
| datetime({year \[, month, day, …​]})                 | Returns a calendar (Year-Month-Day) _DateTime_.                                            |
| datetime({year \[, week, dayOfWeek, …​]})            | Returns a week (Year-Week-Day) _DateTime_.                                                 |
| datetime({year \[, quarter, dayOfQuarter, …​]})      | Returns a quarter (Year-Quarter-Day) _DateTime_.                                           |
| datetime({year \[, ordinalDay, …​]})                 | Returns an ordinal (Year-Day) _DateTime_.                                                  |
| datetime(string)                                     | Returns a _DateTime_ by parsing a string.                                                  |
| datetime({map})                                      | Returns a _DateTime_ from a map of another temporal value’s components.                    |
| datetime({epochSeconds})                             | Returns a _DateTime_ from a timestamp.                                                     |
| datetime.truncate()                                  | Returns a _DateTime_ obtained by truncating a value at a specific component boundary.      |
| localdatetime()                                      | Returns the current _LocalDateTime_.                                                       |
| localdatetime.transaction()                          | Returns the current _LocalDateTime_ using the `transaction` clock.                         |
| localdatetime.statement()                            | Returns the current _LocalDateTime_ using the `statement` clock.                           |
| localdatetime.realtime()                             | Returns the current _LocalDateTime_ using the `realtime` clock.                            |
| localdatetime({year \[, month, day, …​]})            | Returns a calendar (Year-Month-Day) _LocalDateTime_.                                       |
| localdatetime({year \[, week, dayOfWeek, …​]})       | Returns a week (Year-Week-Day) _LocalDateTime_.                                            |
| localdatetime({year \[, quarter, dayOfQuarter, …​]}) | Returns a quarter (Year-Quarter-Day) _DateTime_.                                           |
| localdatetime({year \[, ordinalDay, …​]})            | Returns an ordinal (Year-Day) _LocalDateTime_.                                             |
| localdatetime(string)                                | Returns a _LocalDateTime_ by parsing a string.                                             |
| localdatetime({map})                                 | Returns a _LocalDateTime_ from a map of another temporal value’s components.               |
| localdatetime.truncate()                             | Returns a _LocalDateTime_ obtained by truncating a value at a specific component boundary. |
| localtime()                                          | Returns the current _LocalTime_.                                                           |
| localtime.transaction()                              | Returns the current _LocalTime_ using the `transaction` clock.                             |
| localtime.statement()                                | Returns the current _LocalTime_ using the `statement` clock.                               |
| localtime.realtime()                                 | Returns the current _LocalTime_ using the `realtime` clock.                                |
| localtime({hour \[, minute, second, …​]})            | Returns a _LocalTime_ with the specified component values.                                 |
| localtime(string)                                    | Returns a _LocalTime_ by parsing a string.                                                 |
| localtime({time \[, hour, …​]})                      | Returns a _LocalTime_ from a map of another temporal value’s components.                   |
| localtime.truncate()                                 | Returns a _LocalTime_ obtained by truncating a value at a specific component boundary.     |
| time()                                               | Returns the current _Time_.                                                                |
| time.transaction()                                   | Returns the current _Time_ using the `transaction` clock.                                  |
| time.statement()                                     | Returns the current _Time_ using the `statement` clock.                                    |
| time.realtime()                                      | Returns the current _Time_ using the `realtime` clock.                                     |
| time({hour \[, minute, …​]})                         | Returns a _Time_ with the specified component values.                                      |
| time(string)                                         | Returns a _Time_ by parsing a string.                                                      |
| time({time \[, hour, …​, timezone]})                 | Returns a _Time_ from a map of another temporal value’s components.                        |
| time.truncate()                                      | Returns a _Time_ obtained by truncating a value at a specific component boundary.          |

**Temporal functions - duration**

Duration values of the temporal types can be created manipulated using the following functions:

| Function             | Description                                                                                                                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| duration({map})      | Returns a _Duration_ from a map of its components.                                                                                 |
| duration(string)     | Returns a _Duration_ by parsing a string.                                                                                          |
| duration.between()   | Returns a _Duration_ equal to the difference between two given instants.                                                           |
| duration.inMonths()  | Returns a _Duration_ equal to the difference in whole months, quarters or years between two given instants.                        |
| duration.inDays()    | Returns a _Duration_ equal to the difference in whole days or weeks between two given instants.                                    |
| duration.inSeconds() | Returns a _Duration_ equal to the difference in seconds and fractions of seconds, or minutes or hours, between two given instants. |

## **Spatial functions**

These functions are used to specify 2D or 3D points in a geographic or cartesian Coordinate Reference System and to calculate the geodesic distance between two points.

| Function               | Description                                                                                                |
| ---------------------- | ---------------------------------------------------------------------------------------------------------- |
| distance()             | Returns a floating point number representing the geodesic distance between any two points in the same CRS. |
| point() - Cartesian 2D | Returns a 2D point object, given two coordinate values in the Cartesian coordinate system.                 |
| point() - Cartesian 3D | Returns a 3D point object, given three coordinate values in the Cartesian coordinate system.               |
| point() - WGS 84 2D    | Returns a 2D point object, given two coordinate values in the WGS 84 geographic coordinate system.         |
| point() - WGS 84 3D    | Returns a 3D point object, given three coordinate values in the WGS 84 geographic coordinate system.       |

## **User-defined functions**

User-defined functions are written in Java, deployed into the database and are called in the same way as any other Cypher function. There are two main types of functions that can be developed and used:

| Type        | Description                                                     |
| ----------- | --------------------------------------------------------------- |
| Scalar      | For each row the function takes parameters and returns a result |
| Aggregating | Consumes many rows and produces an aggregated result            |
