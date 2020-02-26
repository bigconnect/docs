# Cypher Language

Cypher is a declarative graph query language that allows for expressive and efficient querying and updating of the graph. It is designed to be suitable for both developers and operations professionals. Cypher is designed to be simple, yet powerful; highly complicated database queries can be easily expressed, enabling you to focus on your domain, instead of getting lost in database access.

Cypher is inspired by a number of different approaches and builds on established practices for expressive querying. Many of the keywords, such as `WHERE` and `ORDER BY`, are inspired by [SQL](http://en.wikipedia.org/wiki/SQL). Pattern matching borrows expression approaches from [SPARQL](http://en.wikipedia.org/wiki/SPARQL). Some of the list semantics are borrowed from languages such as Haskell and Python. Cypher’s constructs, based on English prose and neat iconography, make queries easy both to write, and to read.

**Structure**

Cypher borrows its structure from SQL — queries are built up using various clauses.

Clauses are chained together, and they feed intermediate result sets between each other. For example, the matching variables from one `MATCH` clause will be the context that the next clause exists in.

The query language is comprised of several distinct clauses. You can read more details about them later in the manual.

Here are a few clauses used to read from the graph:

* `MATCH`: The graph pattern to match. This is the most common way to get data from the graph.
* `WHERE`: Not a clause in its own right, but rather part of `MATCH`, `OPTIONAL MATCH` and `WITH`. Adds constraints to a pattern, or filters the intermediate result passing through `WITH`.
* `RETURN`: What to return.

Let’s see `MATCH` and `RETURN` in action.

Let’s create a simple example graph with the following query:

```sql
CREATE (john:Person {name: 'John'})
CREATE (joe:Person {name: 'Joe'})
CREATE (steve:Person {name: 'Steve'})
CREATE (sara:Person {name: 'Sara'})
CREATE (maria:Person {name: 'Maria'})
CREATE (john)-[:FRIEND]->(joe)-[:FRIEND]->(steve)
CREATE (john)-[:FRIEND]->(sara)-[:FRIEND]->(maria)
```

![](../../.gitbook/assets/image%20%28398%29.png)

For example, here is a query which finds a user called **'John'** and **'John’s'** friends \(though not his direct friends\) before returning both **'John'** and any friends-of-friends that are found.

```sql
MATCH (john {name: 'John'})-[:friend]->()-[:friend]->(fof)
RETURN john.name, fof.name
```

Resulting in:

```text
+----------------------+
| john.name | fof.name |
+----------------------+
+----------------------+
0 row
```

Next up we will add filtering to set more parts in motion:

We take a list of user names and find all nodes with names from this list, match their friends and return only those followed users who have a **'name'** property starting with **'S'**.

```sql
MATCH (user)-[:friend]->(follower)
WHERE user.name IN ['Joe', 'John', 'Sara', 'Maria', 'Steve'] AND follower.name =~ 'S.*'
RETURN user.name, follower.name
```

Resulting in:

```text
+---------------------------+
| user.name | follower.name |
+---------------------------+
+---------------------------+
0 row
```

And here are examples of clauses that are used to update the graph:

* `CREATE` \(and `DELETE`\): Create \(and delete\) nodes and relationships.
* `SET` \(and `REMOVE`\): Set values to properties and add labels on nodes using `SET` and use `REMOVE` to remove them.
* `MERGE`: Match existing or create new nodes and patterns. This is especially useful together with unique constraints.

