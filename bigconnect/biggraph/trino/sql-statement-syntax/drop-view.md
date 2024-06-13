# DROP VIEW

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DROP VIEW [ IF EXISTS ] view_name
```



### Description <a href="#description" id="description"></a>

Drop an existing view.

The optional `IF EXISTS` clause causes the error to be suppressed if the view does not exist.

### Examples <a href="#examples" id="examples"></a>

Drop the view `orders_by_date`:

```
DROP VIEW orders_by_date
```



Drop the view `orders_by_date` if it exists:

```
DROP VIEW IF EXISTS orders_by_date
```



### See also <a href="#see-also" id="see-also"></a>

CREATE VIEW
