# DROP VIEW

### Synopsis[#](https://trino.io/docs/current/sql/drop-view.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DROP VIEW [ IF EXISTS ] view_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/drop-view.html#description) <a href="#description" id="description"></a>

Drop an existing view.

The optional `IF EXISTS` clause causes the error to be suppressed if the view does not exist.

### Examples[#](https://trino.io/docs/current/sql/drop-view.html#examples) <a href="#examples" id="examples"></a>

Drop the view `orders_by_date`:

```
DROP VIEW orders_by_date
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop the view `orders_by_date` if it exists:

```
DROP VIEW IF EXISTS orders_by_date
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/drop-view.html#see-also) <a href="#see-also" id="see-also"></a>

[CREATE VIEW](https://trino.io/docs/current/sql/create-view.html)
