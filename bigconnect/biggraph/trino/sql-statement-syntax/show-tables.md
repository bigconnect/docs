# SHOW TABLES

### Synopsis[#](https://trino.io/docs/current/sql/show-tables.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW TABLES [ FROM schema ] [ LIKE pattern ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/show-tables.html#description) <a href="#description" id="description"></a>

List the tables in `schema` or in the current schema.

[Specify a pattern](https://trino.io/docs/current/functions/comparison.html#like-operator) in the optional `LIKE` clause to filter the results to the desired subset.. For example, the following query allows you to find tables that begin with `p`:

```
SHOW TABLES FROM tpch.tiny LIKE 'p%';
```
