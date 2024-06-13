# DROP TABLE

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DROP TABLE  [ IF EXISTS ] table_name
```



### Description <a href="#description" id="description"></a>

Drops an existing table.

The optional `IF EXISTS` clause causes the error to be suppressed if the table does not exist. The error is not suppressed if a Trino view with the same name exists.

### Examples <a href="#examples" id="examples"></a>

Drop the table `orders_by_date`:

```
DROP TABLE orders_by_date
```



Drop the table `orders_by_date` if it exists:

```
DROP TABLE IF EXISTS orders_by_date
```



### See also <a href="#see-also" id="see-also"></a>

ALTER TABLE, CREATE TABLE
