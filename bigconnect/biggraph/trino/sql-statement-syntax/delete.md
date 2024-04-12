# DELETE

### Synopsis[#](https://trino.io/docs/current/sql/delete.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DELETE FROM table_name [ WHERE condition ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/delete.html#description) <a href="#description" id="description"></a>

Delete rows from a table. If the `WHERE` clause is specified, only the matching rows are deleted. Otherwise, all rows from the table are deleted.

### Examples[#](https://trino.io/docs/current/sql/delete.html#examples) <a href="#examples" id="examples"></a>

Delete all line items shipped by air:

```
DELETE FROM lineitem WHERE shipmode = 'AIR';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Delete all line items for low priority orders:

```
DELETE FROM lineitem
WHERE orderkey IN (SELECT orderkey FROM orders WHERE priority = 'LOW');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Delete all orders:

```
DELETE FROM orders;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/delete.html#limitations) <a href="#limitations" id="limitations"></a>

Some connectors have limited or no support for `DELETE`. See connector documentation for more details.
