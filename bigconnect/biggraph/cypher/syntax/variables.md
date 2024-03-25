# Variables

When you reference parts of a pattern or a query, you do so by naming them. The names you give the different parts are called variables.

In this example:

```cypher
MATCH (n)-->(b)
RETURN b
```

The variables are `n` and `b`.

{% hint style="info" %}
Variables are only visible in the same query part

Variables are not carried over to subsequent queries. If multiple query parts are chained together using `WITH`, variables have to be listed in the `WITH` clause to be carried over to the next part.
{% endhint %}
