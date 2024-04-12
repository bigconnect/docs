# DROP TABLE

### Synopsis[#](https://trino.io/docs/current/sql/drop-table.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DROP TABLE  [ IF EXISTS ] table_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/drop-table.html#description) <a href="#description" id="description"></a>

Drops an existing table.

The optional `IF EXISTS` clause causes the error to be suppressed if the table does not exist. The error is not suppressed if a Trino view with the same name exists.

### Examples[#](https://trino.io/docs/current/sql/drop-table.html#examples) <a href="#examples" id="examples"></a>

Drop the table `orders_by_date`:

```
DROP TABLE orders_by_date
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop the table `orders_by_date` if it exists:

```
DROP TABLE IF EXISTS orders_by_date
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/drop-table.html#see-also) <a href="#see-also" id="see-also"></a>

[ALTER TABLE](https://trino.io/docs/current/sql/alter-table.html), [CREATE TABLE](https://trino.io/docs/current/sql/create-table.html)
