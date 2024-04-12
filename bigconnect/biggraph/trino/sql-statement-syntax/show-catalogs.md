# SHOW CATALOGS

### Synopsis[#](https://trino.io/docs/current/sql/show-catalogs.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW CATALOGS [ LIKE pattern ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/show-catalogs.html#description) <a href="#description" id="description"></a>

List the available catalogs.

[Specify a pattern](https://trino.io/docs/current/functions/comparison.html#like-operator) in the optional `LIKE` clause to filter the results to the desired subset. For example, the following query allows you to find catalogs that begin with `t`:

```
SHOW CATALOGS LIKE 't%'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)\
