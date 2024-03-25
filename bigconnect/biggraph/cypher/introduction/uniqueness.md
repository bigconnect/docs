# Uniqueness

While pattern matching, BigConnect makes sure to not include matches where the same graph relationship is found multiple times in a single pattern. In most use cases, this is a sensible thing to do.

Example: looking for a user’s friends of friends should not return said user.

Let’s create a few nodes and relationships:

```cypher
CREATE (adam:User { name: 'Adam' }),(pernilla:User { name: 'Pernilla' }),(david:User { name: 'David'
  }),
  (adam)-[:FRIEND]->(pernilla),(pernilla)-[:FRIEND]->(david)
```

Which gives us the following graph:

```
  N0 [
    label = "{User|name = \'Adam\'\l}"
  ]
  N0 -> N1 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "FRIEND\n"
  ]
  N1 [
    label = "{User|name = \'Pernilla\'\l}"
  ]
  N1 -> N2 [
    color = "#2e3436"
    fontcolor = "#2e3436"
    label = "FRIEND\n"
  ]
  N2 [
    label = "{User|name = \'David\'\l}"
  ]
```

Now let’s look for friends of friends of Adam:

```cypher
MATCH (user:User { name: 'Adam' })-[r1:FRIEND]-()-[r2:FRIEND]-(friend_of_a_friend)
RETURN friend_of_a_friend.name AS fofName
```

```none
+---------+
| fofName |
+---------+
| "David" |
+---------+
1 row
```

In this query, Cypher makes sure to not return matches where the pattern relationships `r1` and `r2` point to the same graph relationship.

This is however not always desired. If the query should return the user, it is possible to spread the matching over multiple `MATCH` clauses, like so:

```cypher
MATCH (user:User { name: 'Adam' })-[r1:FRIEND]-(friend)
MATCH (friend)-[r2:FRIEND]-(friend_of_a_friend)
RETURN friend_of_a_friend.name AS fofName
```

```none
+---------+
| fofName |
+---------+
| "David" |
| "Adam"  |
+---------+
2 rows
```

Note that while the following query looks similar to the previous one, it is actually equivalent to the one before.

```cypher
MATCH (user:User { name: 'Adam' })-[r1:FRIEND]-(friend),(friend)-[r2:FRIEND]-(friend_of_a_friend)
RETURN friend_of_a_friend.name AS fofName
```

Here, the `MATCH` clause has a single pattern with two paths, while the previous query has two distinct patterns.

```none
+---------+
| fofName |
+---------+
| "David" |
+---------+
1 row
```
