# Parameters

Cypher® supports querying with parameters. This means developers don’t have to resort to string building to create a query. Additionally, parameters make caching of execution plans much easier for Cypher, thus leading to faster query execution times.

Parameters can be used for:

* literals and expressions
* node and relationship ids
* for explicit indexes only: index values and queries

Parameters cannot be used for the following constructs, as these form part of the query structure that is compiled into a query plan:

* property keys; so, `MATCH (n) WHERE n.$param = 'something'` is invalid
* relationship types
* labels

Parameters may consist of letters and numbers, and any combination of these, but cannot start with a number or a currency symbol.

We provide below a comprehensive list of examples of parameter usage. In these examples, parameters are given in JSON; the exact manner in which they are to be submitted depends upon the driver being used.

| It is recommended that the new parameter syntax `$param` is used, as the old syntax `{param}` is deprecated and will be removed altogether in a later release. |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### String literal

Parameters

```javascript
{
  "name" : "Johan"
}
```

Query

```cypher
MATCH (n:Person)
WHERE n.name = $name
RETURN n
```

You can use parameters in this syntax as well:

Parameters

```javascript
{
  "name" : "Johan"
}
```

Query

```cypher
MATCH (n:Person { name: $name })
RETURN n
```

### Regular expression

Parameters

```javascript
{
  "regex" : ".*h.*"
}
```

Query

```cypher
MATCH (n:Person)
WHERE n.name =~ $regex
RETURN n.name
```

### Case-sensitive string pattern matching

Parameters

```javascript
{
  "name" : "Michael"
}
```

Query

```cypher
MATCH (n:Person)
WHERE n.name STARTS WITH $name
RETURN n.name
```

### Create node with properties

Parameters

```javascript
{
  "props" : {
    "name" : "Andy",
    "position" : "Developer"
  }
}
```

Query

```cypher
CREATE ($props)
```

### Create multiple nodes with properties

Parameters

```javascript
{
  "props" : [ {
    "awesome" : true,
    "name" : "Andy",
    "position" : "Developer"
  }, {
    "children" : 3,
    "name" : "Michael",
    "position" : "Developer"
  } ]
}
```

Query

```cypher
UNWIND $props AS properties
CREATE (n:Person)
SET n = properties
RETURN n
```

### Setting all properties on a node

Note that this will replace all the current properties.

Parameters

```javascript
{
  "props" : {
    "name" : "Andy",
    "position" : "Developer"
  }
}
```

Query

```cypher
MATCH (n:Person)
WHERE n.name='Michaela'
SET n = $props
```

### `SKIP` and `LIMIT`

Parameters

```javascript
{
  "s" : 1,
  "l" : 1
}
```

Query

```cypher
MATCH (n:Person)
RETURN n.name
SKIP $s
LIMIT $l
```

### Node id

Parameters

```javascript
{
  "id" : 0
}
```

Query

```cypher
MATCH (n)
WHERE id(n)= $id
RETURN n.name
```

### Multiple node ids

Parameters

```javascript
{
  "ids" : [ 0, 1, 2 ]
}
```

Query

```cypher
MATCH (n)
WHERE id(n) IN $ids
RETURN n.name
```

### Calling procedures

Parameters

```javascript
{
  "indexname" : ":Person(name)"
}
```

Query

```cypher
CALL db.resampleIndex($indexname)
```

### Index value (explicit indexes)

Parameters

```javascript
{
  "value" : "Michaela"
}
```

Query

```cypher
START n=node:people(name = $value)
RETURN n
```

### Index query (explicit indexes)

Parameters

```javascript
{
  "query" : "name:Bob"
}
```

Query

```cypher
START n=node:people($query)
RETURN n
```
