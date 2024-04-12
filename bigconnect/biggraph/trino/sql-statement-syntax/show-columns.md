# SHOW COLUMNS

### Synopsis[#](https://trino.io/docs/current/sql/show-columns.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW COLUMNS FROM table [ LIKE pattern ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/show-columns.html#description) <a href="#description" id="description"></a>

List the columns in a `table` along with their data type and other attributes:

```
SHOW COLUMNS FROM nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
  Column   |     Type     | Extra | Comment
-----------+--------------+-------+---------
 nationkey | bigint       |       |
 name      | varchar(25)  |       |
 regionkey | bigint       |       |
 comment   | varchar(152) |       |
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

[Specify a pattern](https://trino.io/docs/current/functions/comparison.html#like-operator) in the optional `LIKE` clause to filter the results to the desired subset. For example, the following query allows you to find columns ending in `key`:

```
SHOW COLUMNS FROM nation LIKE '%key';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
  Column   |     Type     | Extra | Comment
-----------+--------------+-------+---------
 nationkey | bigint       |       |
 regionkey | bigint       |       |
```
