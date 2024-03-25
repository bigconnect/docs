# Lists

### Lists in general <a href="#cypher-lists-general" id="cypher-lists-general"></a>

A literal list is created by using brackets and separating the elements in the list with commas.

Query

```cypher
RETURN [0, 1, 2, 3, 4, 5, 6, 7, 8, 9] AS list
```

| list                    |
| ----------------------- |
| `[0,1,2,3,4,5,6,7,8,9]` |
| 1 row                   |

In our examples, we’ll use the `range` function. It gives you a list containing all numbers between given start and end numbers. Range is inclusive in both ends.

To access individual elements in the list, we use the square brackets again. This will extract from the start index and up to but not including the end index.

Query

```cypher
RETURN range(0, 10)[3]
```

| range(0, 10)\[3] |
| ---------------- |
| `3`              |
| 1 row            |

You can also use negative numbers, to start from the end of the list instead.

Query

```cypher
RETURN range(0, 10)[-3]
```

| range(0, 10)\[-3] |
| ----------------- |
| `8`               |
| 1 row             |

Finally, you can use ranges inside the brackets to return ranges of the list.

Query

```cypher
RETURN range(0, 10)[0..3]
```

| range(0, 10)\[0..3] |
| ------------------- |
| `[0,1,2]`           |
| 1 row               |

Query

```cypher
RETURN range(0, 10)[0..-5]
```

| range(0, 10)\[0..-5] |
| -------------------- |
| `[0,1,2,3,4,5]`      |
| 1 row                |

Query

```cypher
RETURN range(0, 10)[-5..]
```

| range(0, 10)\[-5..] |
| ------------------- |
| `[6,7,8,9,10]`      |
| 1 row               |

Query

```cypher
RETURN range(0, 10)[..4]
```

| range(0, 10)\[..4] |
| ------------------ |
| `[0,1,2,3]`        |
| 1 row              |

| Out-of-bound slices are simply truncated, but out-of-bound single elements return `null`. |
| ----------------------------------------------------------------------------------------- |

Query

```cypher
RETURN range(0, 10)[15]
```

| range(0, 10)\[15] |
| ----------------- |
| `<null>`          |
| 1 row             |

Query

```cypher
RETURN range(0, 10)[5..15]
```

| range(0, 10)\[5..15] |
| -------------------- |
| `[5,6,7,8,9,10]`     |
| 1 row                |

You can get the `size` of a list as follows:

Query

```cypher
RETURN size(range(0, 10)[0..3])
```

| size(range(0, 10)\[0..3]) |
| ------------------------- |
| `3`                       |
| 1 row                     |

### List comprehension

List comprehension is a syntactic construct available in Cypher® for creating a list based on existing lists. It follows the form of the mathematical set-builder notation (set comprehension) instead of the use of map and filter functions.

Query

```cypher
RETURN [x IN range(0,10) WHERE x % 2 = 0 | x^3] AS result
```

| result                              |
| ----------------------------------- |
| `[0.0,8.0,64.0,216.0,512.0,1000.0]` |
| 1 row                               |

Either the `WHERE` part, or the expression, can be omitted, if you only want to filter or map respectively.

Query

```cypher
RETURN [x IN range(0,10) WHERE x % 2 = 0] AS result
```

| result           |
| ---------------- |
| `[0,2,4,6,8,10]` |
| 1 row            |

Query

```cypher
RETURN [x IN range(0,10)| x^3] AS result
```

| result                                                         |
| -------------------------------------------------------------- |
| `[0.0,1.0,8.0,27.0,64.0,125.0,216.0,343.0,512.0,729.0,1000.0]` |
| 1 row                                                          |

### Pattern comprehension

Pattern comprehension is a syntactic construct available in Cypher for creating a list based on matchings of a pattern. A pattern comprehension will match the specified pattern just like a normal `MATCH` clause, with predicates just like a normal `WHERE` clause, but will yield a custom projection as specified.

The following graph is used for the example below:

Graph

```
  N0 [
    label = "{Person|name = \'Keanu Reeves\'\l}"
  ]
  N0 -> N7 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "ACTED_IN\n"
  ]
  N0 -> N6 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "ACTED_IN\n"
  ]
  N0 -> N5 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "ACTED_IN\n"
  ]
  N0 -> N4 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "ACTED_IN\n"
  ]
  N0 -> N3 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "ACTED_IN\n"
  ]
  N0 -> N2 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "ACTED_IN\n"
  ]
  N0 -> N1 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "ACTED_IN\n"
  ]
  N1 [
    label = "{Movie|title = \'Johnny Mnemonic\'\lreleased = 1995\l}"
  ]
  N2 [
    label = "{Movie|title = \'Somethings Gotta Give\'\lreleased = 2003\l}"
  ]
  N3 [
    label = "{Movie|title = \'The Matrix Revolutions\'\lreleased = 2003\l}"
  ]
  N4 [
    label = "{Movie|title = \'The Matrix Reloaded\'\lreleased = 2003\l}"
  ]
  N5 [
    label = "{Movie|title = \'The Replacements\'\lreleased = 2000\l}"
  ]
  N6 [
    label = "{Movie|title = \'The Matrix\'\lreleased = 1999\l}"
  ]
  N7 [
    label = "{Movie|title = \'The Devils Advocate\'\lreleased = 1997\l}"
  ]
```

Query

```cypher
MATCH (a:Person { name: 'Keanu Reeves' })
RETURN [(a)-->(b) WHERE b:Movie | b.released] AS years
```

| years                                  |
| -------------------------------------- |
| `[1997,1999,2000,2003,2003,2003,1995]` |
| 1 row                                  |

The whole predicate, including the `WHERE` keyword, is optional and may be omitted.
