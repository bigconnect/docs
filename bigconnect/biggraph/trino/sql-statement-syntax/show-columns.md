# SHOW COLUMNS

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SHOW COLUMNS FROM table [ LIKE pattern ]
```

### Description[#](https://trino.io/docs/current/sql/show-columns.html#description) <a href="#description" id="description"></a>

List the columns in a `table` along with their data type and other attributes:

```
SHOW COLUMNS FROM nation;
```

```
  Column   |     Type     | Extra | Comment
-----------+--------------+-------+---------
 nationkey | bigint       |       |
 name      | varchar(25)  |       |
 regionkey | bigint       |       |
 comment   | varchar(152) |       |
```

Specify a pattern in the optional `LIKE` clause to filter the results to the desired subset. For example, the following query allows you to find columns ending in `key`:

```
SHOW COLUMNS FROM nation LIKE '%key';
```

```
  Column   |     Type     | Extra | Comment
-----------+--------------+-------+---------
 nationkey | bigint       |       |
 regionkey | bigint       |       |
```
