# DROP MATERIALIZED VIEW

### Synopsis[#](https://trino.io/docs/current/sql/drop-materialized-view.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DROP MATERIALIZED VIEW [ IF EXISTS ] view_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/drop-materialized-view.html#description) <a href="#description" id="description"></a>

Drop an existing materialized view `view_name`.

The optional `IF EXISTS` clause causes the error to be suppressed if the materialized view does not exist.

### Examples[#](https://trino.io/docs/current/sql/drop-materialized-view.html#examples) <a href="#examples" id="examples"></a>

Drop the materialized view `orders_by_date`:

```
DROP MATERIALIZED VIEW orders_by_date;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop the materialized view `orders_by_date` if it exists:

```
DROP MATERIALIZED VIEW IF EXISTS orders_by_date;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

* CREATE MATERIALIZED VIEW
* SHOW CREATE MATERIALIZED VIEW
* REFRESH MATERIALIZED VIEW

\
