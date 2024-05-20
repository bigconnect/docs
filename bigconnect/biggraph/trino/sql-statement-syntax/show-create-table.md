# SHOW CREATE TABLE

### Synopsis[#](https://trino.io/docs/current/sql/show-create-table.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW CREATE TABLE table_name
```

### Description[#](https://trino.io/docs/current/sql/show-create-table.html#description) <a href="#description" id="description"></a>

Show the SQL statement that creates the specified table.

### Examples[#](https://trino.io/docs/current/sql/show-create-table.html#examples) <a href="#examples" id="examples"></a>

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

### See also[#](https://trino.io/docs/current/sql/show-create-table.html#see-also) <a href="#see-also" id="see-also"></a>

CREATE TABLE

\
