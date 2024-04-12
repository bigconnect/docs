# CREATE TABLE

### Synopsis[#](https://trino.io/docs/current/sql/create-table.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
CREATE [ OR REPLACE ] TABLE [ IF NOT EXISTS ]
table_name (
  { column_name data_type [ NOT NULL ]
      [ COMMENT comment ]
      [ WITH ( property_name = expression [, ...] ) ]
  | LIKE existing_table_name
      [ { INCLUDING | EXCLUDING } PROPERTIES ]
  }
  [, ...]
)
[ COMMENT table_comment ]
[ WITH ( property_name = expression [, ...] ) ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/create-table.html#description) <a href="#description" id="description"></a>

Create a new, empty table with the specified columns. Use [CREATE TABLE AS](https://trino.io/docs/current/sql/create-table-as.html) to create a table with data.

The optional `OR REPLACE` clause causes an existing table with the specified name to be replaced with the new table definition. Support for table replacement varies across connectors. Refer to the connector documentation for details.

The optional `IF NOT EXISTS` clause causes the error to be suppressed if the table already exists.

`OR REPLACE` and `IF NOT EXISTS` cannot be used together.

The optional `WITH` clause can be used to set properties on the newly created table or on single columns. To list all available table properties, run the following query:

```
SELECT * FROM system.metadata.table_properties
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

To list all available column properties, run the following query:

```
SELECT * FROM system.metadata.column_properties
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The `LIKE` clause can be used to include all the column definitions from an existing table in the new table. Multiple `LIKE` clauses may be specified, which allows copying the columns from multiple tables.

If `INCLUDING PROPERTIES` is specified, all of the table properties are copied to the new table. If the `WITH` clause specifies the same property name as one of the copied properties, the value from the `WITH` clause will be used. The default behavior is `EXCLUDING PROPERTIES`. The `INCLUDING PROPERTIES` option maybe specified for at most one table.

### Examples[#](https://trino.io/docs/current/sql/create-table.html#examples) <a href="#examples" id="examples"></a>

Create a new table `orders`:

```
CREATE TABLE orders (
  orderkey bigint,
  orderstatus varchar,
  totalprice double,
  orderdate date
)
WITH (format = 'ORC')
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create the table `orders` if it does not already exist, adding a table comment and a column comment:

```
CREATE TABLE IF NOT EXISTS orders (
  orderkey bigint,
  orderstatus varchar,
  totalprice double COMMENT 'Price in cents.',
  orderdate date
)
COMMENT 'A table to keep track of orders.'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create the table `bigger_orders` using the columns from `orders` plus additional columns at the start and end:

```
CREATE TABLE bigger_orders (
  another_orderkey bigint,
  LIKE orders,
  another_orderdate date
)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/create-table.html#see-also) <a href="#see-also" id="see-also"></a>

[ALTER TABLE](https://trino.io/docs/current/sql/alter-table.html), [DROP TABLE](https://trino.io/docs/current/sql/drop-table.html), [CREATE TABLE AS](https://trino.io/docs/current/sql/create-table-as.html), [SHOW CREATE TABLE](https://trino.io/docs/current/sql/show-create-table.html)
