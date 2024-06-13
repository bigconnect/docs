# SHOW CREATE TABLE

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SHOW CREATE TABLE table_name
```

### Description <a href="#description" id="description"></a>

Show the SQL statement that creates the specified table.

### Examples <a href="#examples" id="examples"></a>

Show the SQL that can be run to create the `orders` table:

```
SHOW CREATE TABLE sf1.orders;
```

```
              Create Table
-----------------------------------------
 CREATE TABLE tpch.sf1.orders (
    orderkey bigint,
    orderstatus varchar,
    totalprice double,
    orderdate varchar
 )
 WITH (
    format = 'ORC',
    partitioned_by = ARRAY['orderdate']
 )
(1 row)
```

### See also <a href="#see-also" id="see-also"></a>

CREATE TABLE

\
