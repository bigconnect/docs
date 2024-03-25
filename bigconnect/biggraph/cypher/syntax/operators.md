# Operators

### Operators at a glance <a href="#query-operators-summary" id="query-operators-summary"></a>

| Aggregation operators                | `DISTINCT`                                                                                                                                |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Property operators                   | `.` for static property access, `[]` for dynamic property access, `=` for replacing all properties, `+=` for mutating specific properties |
| Mathematical operators               | `+`, `-`, `*`, `/`, `%`, `^`                                                                                                              |
| Comparison operators                 | `=`, `<>`, `<`, `>`, `<=`, `>=`, `IS NULL`, `IS NOT NULL`                                                                                 |
| String-specific comparison operators | `STARTS WITH`, `ENDS WITH`, `CONTAINS`                                                                                                    |
| Boolean operators                    | `AND`, `OR`, `XOR`, `NOT`                                                                                                                 |
| String operators                     | `+` for concatenation, `=~` for regex matching                                                                                            |
| Temporal operators                   | `+` and `-` for operations between durations and temporal instants/durations, `*` and `/` for operations between durations and numbers    |
| Map operators                        | `.` for static value access by key, `[]` for dynamic value access by key                                                                  |
| List operators                       | `+` for concatenation, `IN` to check existence of an element in a list, `[]` for accessing element(s) dynamically                         |

### Aggregation operators

The aggregation operators comprise:

* remove duplicates values: `DISTINCT`

#### Using the `DISTINCT` operator

Retrieve the unique eye colors from `Person` nodes.

Query

```cypher
CREATE (a:Person { name: 'Anne', eyeColor: 'blue' }),(b:Person { name: 'Bill', eyeColor: 'brown' }),(c:Person { name: 'Carol', eyeColor: 'blue' })
WITH [a, b, c] AS ps
UNWIND ps AS p
RETURN DISTINCT p.eyeColor
```

Even though both **'Anne'** and **'Carol'** have blue eyes, **'blue'** is only returned once.

| p.eyeColor                                                                |
| ------------------------------------------------------------------------- |
| `"blue"`                                                                  |
| `"brown"`                                                                 |
| <p>2 rows<br>Nodes created: 3<br>Properties set: 6<br>Labels added: 3</p> |

`DISTINCT` is commonly used in conjunction with aggregating functions.

### Property operators

The property operators pertain to a node or a relationship, and comprise:

* statically access the property of a node or relationship using the dot operator: `.`
* dynamically access the property of a node or relationship using the subscript operator: `[]`
* property replacement `=` for replacing all properties of a node or relationship
* property mutation operator `+=` for setting specific properties of a node or relationship

#### Statically accessing a property of a node or relationship using the `.` operator

Query

```cypher
CREATE (a:Person { name: 'Jane', livesIn: 'London' }),(b:Person { name: 'Tom', livesIn: 'Copenhagen' })
WITH a, b
MATCH (p:Person)
RETURN p.name
```

| p.name                                                                    |
| ------------------------------------------------------------------------- |
| `"Jane"`                                                                  |
| `"Tom"`                                                                   |
| <p>2 rows<br>Nodes created: 2<br>Properties set: 4<br>Labels added: 2</p> |

#### Filtering on a dynamically-computed property key using the `[]` operator

Query

```cypher
CREATE (a:Restaurant { name: 'Hungry Jo', rating_hygiene: 10, rating_food: 7 }),(b:Restaurant { name: 'Buttercup Tea Rooms', rating_hygiene: 5, rating_food: 6 }),(c1:Category { name: 'hygiene' }),(c2:Category { name: 'food' })
WITH a, b, c1, c2
MATCH (restaurant:Restaurant),(category:Category)
WHERE restaurant["rating_" + category.name]> 6
RETURN DISTINCT restaurant.name
```

| restaurant.name                                                          |
| ------------------------------------------------------------------------ |
| `"Hungry Jo"`                                                            |
| <p>1 row<br>Nodes created: 4<br>Properties set: 8<br>Labels added: 4</p> |

#### Replacing all properties of a node or relationship using the `=` operator

Query

```cypher
CREATE (a:Person { name: 'Jane', age: 20 })
WITH a
MATCH (p:Person { name: 'Jane' })
SET p = { name: 'Ellen', livesIn: 'London' }
RETURN p.name, p.age, p.livesIn
```

All the existing properties on the node are replaced by those provided in the map; i.e. the `name` property is updated from `Jane` to `Ellen`, the `age` property is deleted, and the `livesIn` property is added.

| p.name                                                                   | p.age    | p.livesIn  |
| ------------------------------------------------------------------------ | -------- | ---------- |
| `"Ellen"`                                                                | `<null>` | `"London"` |
| <p>1 row<br>Nodes created: 1<br>Properties set: 5<br>Labels added: 1</p> |          |            |

#### Mutating specific properties of a node or relationship using the `+=` operator

Query

```cypher
CREATE (a:Person { name: 'Jane', age: 20 })
WITH a
MATCH (p:Person { name: 'Jane' })
SET p += { name: 'Ellen', livesIn: 'London' }
RETURN p.name, p.age, p.livesIn
```

The properties on the node are updated as follows by those provided in the map: the `name` property is updated from `Jane` to `Ellen`, the `age` property is left untouched, and the `livesIn` property is added.

| p.name                                                                   | p.age | p.livesIn  |
| ------------------------------------------------------------------------ | ----- | ---------- |
| `"Ellen"`                                                                | `20`  | `"London"` |
| <p>1 row<br>Nodes created: 1<br>Properties set: 4<br>Labels added: 1</p> |       |            |

### Mathematical operators

The mathematical operators comprise:

* addition: `+`
* subtraction or unary minus: `-`
* multiplication: `*`
* division: `/`
* modulo division: `%`
* exponentiation: `^`

#### Using the exponentiation operator `^`

Query

```cypher
WITH 2 AS number, 3 AS exponent
RETURN number ^ exponent AS result
```

| result |
| ------ |
| `8.0`  |
| 1 row  |

#### Using the unary minus operator `-`

Query

```cypher
WITH -3 AS a, 4 AS b
RETURN b - a AS result
```

| result |
| ------ |
| `7`    |
| 1 row  |

### Comparison operators

The comparison operators comprise:

* equality: `=`
* inequality: `<>`
* less than: `<`
* greater than: `>`
* less than or equal to: `<=`
* greater than or equal to: `>=`
* `IS NULL`
* `IS NOT NULL`

#### String-specific comparison operators comprise:

* `STARTS WITH`: perform case-sensitive prefix searching on strings
* `ENDS WITH`: perform case-sensitive suffix searching on strings
* `CONTAINS`: perform case-sensitive inclusion searching in strings

#### Comparing two numbers

Query

```cypher
WITH 4 AS one, 3 AS two
RETURN one > two AS result
```

| result |
| ------ |
| `true` |
| 1 row  |

#### Using `STARTS WITH` to filter names

Query

```cypher
WITH ['John', 'Mark', 'Jonathan', 'Bill'] AS somenames
UNWIND somenames AS names
WITH names AS candidate
WHERE candidate STARTS WITH 'Jo'
RETURN candidate
```

| candidate    |
| ------------ |
| `"John"`     |
| `"Jonathan"` |
| 2 rows       |

String matching contains more information regarding the string-specific comparison operators as well as additional examples illustrating the usage thereof.

### Equality and comparison of values

#### Equality

Cypher supports comparing values by equality using the `=` and `<>` operators.

Values of the same type are only equal if they are the same identical value (e.g. `3 = 3` and `"x" <> "xy"`).

Maps are only equal if they map exactly the same keys to equal values and lists are only equal if they contain the same sequence of equal values (e.g. `[3, 4] = [1+2, 8/2]`).

Values of different types are considered as equal according to the following rules:

* Paths are treated as lists of alternating nodes and relationships and are equal to all lists that contain that very same sequence of nodes and relationships.
* Testing any value against `null` with both the `=` and the `<>` operators always is `null`. This includes `null = null` and `null <> null`. The only way to reliably test if a value `v` is `null` is by using the special `v IS NULL`, or `v IS NOT NULL` equality operators.

All other combinations of types of values cannot be compared with each other. Especially, nodes, relationships, and literal maps are incomparable with each other.

It is an error to compare values that cannot be compared.

### Ordering and comparison of values

The comparison operators `<=`, `<` (for ascending) and `>=`, `>` (for descending) are used to compare values for ordering. The following points give some details on how the comparison is performed.

* Numerical values are compared for ordering using numerical order (e.g. `3 < 4` is true).
* The special value `java.lang.Double.NaN` is regarded as being larger than all other numbers.
* String values are compared for ordering using lexicographic order (e.g. `"x" < "xy"`).
* Boolean values are compared for ordering such that `false < true`.
* **Comparison** of spatial values:
  * Point values can only be compared within the same Coordinate Reference System (CRS) — otherwise, the result will be `null`.
  * For two points `a` and `b` within the same CRS, `a` is considered to be greater than `b` if `a.x > b.x` and `a.y > b.y` (and `a.z > b.z` for 3D points).
  * `a` is considered less than `b` if `a.x < b.x` and `a.y < b.y` (and `a.z < b.z` for 3D points).
  * If none if the above is true, the points are considered incomparable and any comparison operator between them will return `null`.
* **Ordering** of spatial values:
  * `ORDER BY` requires all values to be orderable.
  * Points are ordered after arrays and before temporal types.
  * Points of different CRS are ordered by the CRS code (the value of SRID field). For the currently supported set of Coordinate Reference Systems this means the order: 4326, 4979, 7302, 9157
  * Points of the same CRS are ordered by each coordinate value in turn, `x` first, then `y` and finally `z`.
  * Note that this order is different to the order returned by the spatial index, which will be the order of the space filling curve.
* **Comparison** of temporal values:
  * Temporal instant values are comparable within the same type. An instant is considered less than another instant if it occurs before that instant in time, and it is considered greater than if it occurs after.
  * Instant values that occur at the same point in time — but that have a different time zone — are not considered equal, and must therefore be ordered in some predictable way. Cypher prescribes that, after the primary order of point in time, instant values be ordered by effective time zone offset, from west (negative offset from UTC) to east (positive offset from UTC). This has the effect that times that represent the same point in time will be ordered with the time with the earliest local time first. If two instant values represent the same point in time, and have the same time zone offset, but a different named time zone (this is possible for _DateTime_ only, since _Time_ only has an offset), these values are not considered equal, and ordered by the time zone identifier, alphabetically, as its third ordering component.
  * _Duration_ values cannot be compared, since the length of a _day_, _month_ or _year_ is not known without knowing which _day_, _month_ or _year_ it is. Since _Duration_ values are not comparable, the result of applying a comparison operator between two _Duration_ values is `null`. If the type, point in time, offset, and time zone name are all equal, then the values are equal, and any difference in order is impossible to observe.
* **Ordering** of temporal values:
  * `ORDER BY` requires all values to be orderable.
  * Temporal instances are ordered after spatial instances and before strings.
  * Comparable values should be ordered in the same order as implied by their comparison order.
  * Temporal instant values are first ordered by type, and then by comparison order within the type.
  * Since no complete comparison order can be defined for _Duration_ values, we define an order for `ORDER BY` specifically for _Duration_:
    * _Duration_ values are ordered by normalising all components as if all years were `365.2425` days long (`PT8765H49M12S`), all months were `30.436875` (`1/12` year) days long (`PT730H29M06S`), and all days were `24` hours long \[[1](https://neo4j.com/docs/cypher-manual/3.5/syntax/operators/#\_footnotedef\_1)].
* Comparing for ordering when one argument is `null` (e.g. `null < 3` is `null`).

### Chaining comparison operations

Comparisons can be chained arbitrarily, e.g., `x < y <= z` is equivalent to `x < y AND y <= z`.

Formally, if `a, b, c, ..., y, z` are expressions and `op1, op2, ..., opN` are comparison operators, then `a op1 b op2 c ... y opN z` is equivalent to `a op1 b and b op2 c and ... y opN z`.

Note that `a op1 b op2 c` does not imply any kind of comparison between `a` and `c`, so that, e.g., `x < y > z` is perfectly legal (although perhaps not elegant).

The example:

```cypher
MATCH (n) WHERE 21 < n.age <= 30 RETURN n
```

is equivalent to

```cypher
MATCH (n) WHERE 21 < n.age AND n.age <= 30 RETURN n
```

Thus it will match all nodes where the age is between 21 and 30.

This syntax extends to all equality and inequality comparisons, as well as extending to chains longer than three.

For example:

```cypher
a < b = c <= d <> e
```

Is equivalent to:

```cypher
a < b AND b = c AND c <= d AND d <> e
```

### Boolean operators

The boolean operators — also known as logical operators — comprise:

* conjunction: `AND`
* disjunction: `OR`,
* exclusive disjunction: `XOR`
* negation: `NOT`

Here is the truth table for `AND`, `OR`, `XOR` and `NOT`.

| a       | b       | a `AND` b | a `OR` b | a `XOR` b | `NOT` a |
| ------- | ------- | --------- | -------- | --------- | ------- |
| `false` | `false` | `false`   | `false`  | `false`   | `true`  |
| `false` | `null`  | `false`   | `null`   | `null`    | `true`  |
| `false` | `true`  | `false`   | `true`   | `true`    | `true`  |
| `true`  | `false` | `false`   | `true`   | `true`    | `false` |
| `true`  | `null`  | `null`    | `true`   | `null`    | `false` |
| `true`  | `true`  | `true`    | `true`   | `false`   | `false` |
| `null`  | `false` | `false`   | `null`   | `null`    | `null`  |
| `null`  | `null`  | `null`    | `null`   | `null`    | `null`  |
| `null`  | `true`  | `null`    | `true`   | `null`    | `null`  |

#### Using boolean operators to filter numbers

Query

```cypher
WITH [2, 4, 7, 9, 12] AS numberlist
UNWIND numberlist AS number
WITH number
WHERE number = 4 OR (number > 6 AND number < 10)
RETURN number
```

| number |
| ------ |
| `4`    |
| `7`    |
| `9`    |
| 3 rows |

### String operators

The string operators comprise:

* concatenating strings: `+`
* matching a regular expression: `=~`

#### Using a regular expression with `=~` to filter words

Query

```cypher
WITH ['mouse', 'chair', 'door', 'house'] AS wordlist
UNWIND wordlist AS word
WITH word
WHERE word =~ '.*ous.*'
RETURN word
```

| word      |
| --------- |
| `"mouse"` |
| `"house"` |
| 2 rows    |

### Temporal operators

Temporal operators comprise:

* adding a _Duration_ to either a temporal instant or another _Duration_: `+`
* subtracting a _Duration_ from either a temporal instant or another _Duration_: `-`
* multiplying a _Duration_ with a number: `*`
* dividing a _Duration_ by a number: `/`

The following table shows — for each combination of operation and operand type — the type of the value returned from the application of each temporal operator:

| Operator | Left-hand operand | Right-hand operand | Type of result                   |
| -------- | ----------------- | ------------------ | -------------------------------- |
| `+`      | Temporal instant  | _Duration_         | The type of the temporal instant |
| `+`      | _Duration_        | Temporal instant   | The type of the temporal instant |
| `-`      | Temporal instant  | _Duration_         | The type of the temporal instant |
| `+`      | _Duration_        | _Duration_         | _Duration_                       |
| `-`      | _Duration_        | _Duration_         | _Duration_                       |
| `*`      | _Duration_        | Number             | _Duration_                       |
| `*`      | Number            | _Duration_         | _Duration_                       |
| `/`      | _Duration_        | Number             | _Duration_                       |

#### Adding and subtracting a _Duration_ to or from a temporal instant

Query

```cypher
WITH localdatetime({ year:1984, month:10, day:11, hour:12, minute:31, second:14 }) AS aDateTime, duration({ years: 12, nanoseconds: 2 }) AS aDuration
RETURN aDateTime + aDuration, aDateTime - aDuration
```

| aDateTime + aDuration           | aDateTime - aDuration           |
| ------------------------------- | ------------------------------- |
| `1996-10-11T12:31:14.000000002` | `1972-10-11T12:31:13.999999998` |
| 1 row                           |                                 |

Components of a _Duration_ that do not apply to the temporal instant are ignored. For example, when adding a _Duration_ to a _Date_, the _hours_, _minutes_, _seconds_ and _nanoseconds_ of the _Duration_ are ignored (_Time_ behaves in an analogous manner):

Query

```cypher
WITH date({ year:1984, month:10, day:11 }) AS aDate, duration({ years: 12, nanoseconds: 2 }) AS aDuration
RETURN aDate + aDuration, aDate - aDuration
```

| aDate + aDuration | aDate - aDuration |
| ----------------- | ----------------- |
| `1996-10-11`      | `1972-10-11`      |
| 1 row             |                   |

Adding two durations to a temporal instant is not an associative operation. This is because non-existing dates are truncated to the nearest existing date:

Query

```cypher
RETURN (date("2011-01-31")+ duration("P1M"))+ duration("P12M") AS date1, date("2011-01-31")+(duration("P1M")+ duration("P12M")) AS date2
```

| date1        | date2        |
| ------------ | ------------ |
| `2012-02-28` | `2012-02-29` |
| 1 row        |              |

#### Adding and subtracting a _Duration_ to or from another _Duration_

Query

```cypher
WITH duration({ years: 12, months: 5, days: 14, hours: 16, minutes: 12, seconds: 70, nanoseconds: 1 }) AS duration1, duration({ months:1, days: -14, hours: 16, minutes: -12, seconds: 70 }) AS duration2
RETURN duration1, duration2, duration1 + duration2, duration1 - duration2
```

| duration1                       | duration2           | duration1 + duration2       | duration1 - duration2       |
| ------------------------------- | ------------------- | --------------------------- | --------------------------- |
| `P12Y5M14DT16H13M10.000000001S` | `P1M-14DT15H49M10S` | `P12Y6MT32H2M20.000000001S` | `P12Y4M28DT24M0.000000001S` |
| 1 row                           |                     |                             |                             |

#### Multiplying and dividing a _Duration_ with or by a number

These operations are interpreted simply as component-wise operations with overflow to smaller units based on an average length of units in the case of division (and multiplication with fractions).

Query

```cypher
WITH duration({ days: 14, minutes: 12, seconds: 70, nanoseconds: 1 }) AS aDuration
RETURN aDuration, aDuration * 2, aDuration / 3
```

| aDuration               | aDuration \* 2          | aDuration / 3            |
| ----------------------- | ----------------------- | ------------------------ |
| `P14DT13M10.000000001S` | `P28DT26M20.000000002S` | `P4DT16H4M23.333333333S` |
| 1 row                   |                         |                          |

### Map operators

The map operators comprise:

* statically access the value of a map by key using the dot operator: `.`
* dynamically access the value of a map by key using the subscript operator: `[]`

#### Statically accessing the value of a nested map by key using the `.` operator

Query

```cypher
WITH { person: { name: 'Anne', age: 25 }} AS p
RETURN p.person.name
```

| p.person.name |
| ------------- |
| `"Anne"`      |
| 1 row         |

#### Dynamically accessing the value of a map by key using the `[]` operator and a parameter

A parameter may be used to specify the key of the value to access:

Parameters

```javascript
{
  "myKey" : "name"
}
```

Query

```cypher
WITH { name: 'Anne', age: 25 } AS a
RETURN a[$myKey] AS result
```

| result   |
| -------- |
| `"Anne"` |
| 1 row    |

### List operators

The list operators comprise:

* concatenating lists `l1` and `l2`: `[l1] + [l2]`
* checking if an element `e` exists in a list `l`: `e IN [l]`
* dynamically accessing an element(s) in a list using the subscript operator: `[]`

#### Concatenating two lists using `+`

Query

```cypher
RETURN [1,2,3,4,5]+[6,7] AS myList
```

| myList            |
| ----------------- |
| `[1,2,3,4,5,6,7]` |
| 1 row             |

#### Using `IN` to check if a number is in a list

Query

```cypher
WITH [2, 3, 4, 5] AS numberlist
UNWIND numberlist AS number
WITH number
WHERE number IN [2, 3, 8]
RETURN number
```

| number |
| ------ |
| `2`    |
| `3`    |
| 2 rows |

#### Using `IN` for more complex list membership operations

The general rule is that the `IN` operator will evaluate to `true` if the list given as the right-hand operand contains an element which has the same _type and contents (or value)_ as the left-hand operand. Lists are only comparable to other lists, and elements of a list `innerList` are compared pairwise in ascending order from the first element in `innerList` to the last element in `innerList`.

The following query checks whether or not the list `[2, 1]` is an element of the list `[1, [2, 1], 3]`:

Query

```cypher
RETURN [2, 1] IN [1,[2, 1], 3] AS inList
```

The query evaluates to `true` as the right-hand list contains, as an element, the list `[1, 2]` which is of the same type (a list) and contains the same contents (the numbers `2` and `1` in the given order) as the left-hand operand. If the left-hand operator had been `[1, 2]` instead of `[2, 1]`, the query would have returned `false`.

| inList |
| ------ |
| `true` |
| 1 row  |

At first glance, the contents of the left-hand operand and the right-hand operand _appear_ to be the same in the following query:

Query

```cypher
RETURN [1, 2] IN [1, 2] AS inList
```

However, `IN` evaluates to `false` as the right-hand operand does not contain an element that is of the same _type_ — i.e. a _list_ — as the left-hand-operand.

| inList  |
| ------- |
| `false` |
| 1 row   |

The following query can be used to ascertain whether or not a list — obtained from, say, the [labels()](https://neo4j.com/docs/cypher-manual/3.5/functions/list/#functions-labels) function — contains at least one element that is also present in another list:

```cypher
MATCH (n)
WHERE size([label IN labels(n) WHERE label IN ['Person', 'Employee'] | 1]) > 0
RETURN count(n)
```

As long as `labels(n)` returns either `Person` or `Employee` (or both), the query will return a value greater than zero.

#### Accessing elements in a list using the `[]` operator

Query

```cypher
WITH ['Anne', 'John', 'Bill', 'Diane', 'Eve'] AS names
RETURN names[1..3] AS result
```

The square brackets will extract the elements from the start index `1`, and up to (but excluding) the end index `3`.

| result            |
| ----------------- |
| `["John","Bill"]` |
| 1 row             |

#### Dynamically accessing an element in a list using the `[]` operator and a parameter

A parameter may be used to specify the index of the element to access:

Parameters

```javascript
{
  "myIndex" : 1
}
```

Query

```cypher
WITH ['Anne', 'John', 'Bill', 'Diane', 'Eve'] AS names
RETURN names[$myIndex] AS result
```

| result   |
| -------- |
| `"John"` |
| 1 row    |

#### Using `IN` with `[]` on a nested list

`IN` can be used in conjunction with `[]` to test whether an element exists in a nested list:

Parameters

```javascript
{
  "myIndex" : 1
}
```

Query

```cypher
WITH [[1, 2, 3]] AS l
RETURN 3 IN l[0] AS result
```

| result |
| ------ |
| `true` |
| 1 row  |

[1](https://neo4j.com/docs/cypher-manual/3.5/syntax/operators/#\_footnoteref\_1). The `365.2425` days per year comes from the frequency of leap years. A leap year occurs on a year with an ordinal number divisible by `4`, that is not divisible by `100`, unless it divisible by `400`. This means that over `400` years there are `((365 * 4 + 1) * 25 - 1) * 4 + 1 = 146097` days, which means an average of `365.2425` days per year.
