# INSERT

### Synopsis[#](https://trino.io/docs/current/sql/insert.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
INSERT INTO table_name [ ( column [, ... ] ) ] query
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/insert.html#description) <a href="#description" id="description"></a>

Insert new rows into a table.

If the list of column names is specified, they must exactly match the list of columns produced by the query. Each column in the table not present in the column list will be filled with a `null` value. Otherwise, if the list of columns is not specified, the columns produced by the query must exactly match the columns in the table being inserted into.

### Examples[#](https://trino.io/docs/current/sql/insert.html#examples) <a href="#examples" id="examples"></a>

Load additional rows into the `orders` table from the `new_orders` table:

```
INSERT INTO orders
SELECT * FROM new_orders;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Insert a single row into the `cities` table:

```
INSERT INTO cities VALUES (1, 'San Francisco');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Insert multiple rows into the `cities` table:

```
INSERT INTO cities VALUES (2, 'San Jose'), (3, 'Oakland');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Insert a single row into the `nation` table with the specified column list:

```
INSERT INTO nation (nationkey, name, regionkey, comment)
VALUES (26, 'POLAND', 3, 'no comment');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Insert a row without specifying the `comment` column. That column will be `null`:

```
INSERT INTO nation (nationkey, name, regionkey)
VALUES (26, 'POLAND', 3);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/insert.html#see-also) <a href="#see-also" id="see-also"></a>

[VALUES](https://trino.io/docs/current/sql/values.html)

\
