# Comments

To add comments to your queries, use double slash. Examples:

```cypher
MATCH (n) RETURN n //This is an end of line comment
```

```cypher
MATCH (n)
//This is a whole line comment
RETURN n
```

```cypher
MATCH (n) WHERE n.property = '//This is NOT a comment' RETURN n
```
