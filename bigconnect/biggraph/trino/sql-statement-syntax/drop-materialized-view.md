# DROP MATERIALIZED VIEW

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DROP MATERIALIZED VIEW [ IF EXISTS ] view_name
```



### Description <a href="#description" id="description"></a>

Drop an existing materialized view `view_name`.

The optional `IF EXISTS` clause causes the error to be suppressed if the materialized view does not exist.

### Examples <a href="#examples" id="examples"></a>

Drop the materialized view `orders_by_date`:

```
DROP MATERIALIZED VIEW orders_by_date;
```



Drop the materialized view `orders_by_date` if it exists:

```
DROP MATERIALIZED VIEW IF EXISTS orders_by_date;
```



### See also <a href="#see-also" id="see-also"></a>

* CREATE MATERIALIZED VIEW
* SHOW CREATE MATERIALIZED VIEW
* REFRESH MATERIALIZED VIEW

\
