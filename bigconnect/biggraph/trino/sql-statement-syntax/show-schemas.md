# SHOW SCHEMAS

### Synopsis[#](https://trino.io/docs/current/sql/show-schemas.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW SCHEMAS [ FROM catalog ] [ LIKE pattern ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/show-schemas.html#description) <a href="#description" id="description"></a>

List the schemas in `catalog` or in the current catalog.

[Specify a pattern](https://trino.io/docs/current/functions/comparison.html#like-operator) in the optional `LIKE` clause to filter the results to the desired subset. For example, the following query allows you to find schemas that have `3` as the third character:

```
SHOW SCHEMAS FROM tpch LIKE '__3%'
```
