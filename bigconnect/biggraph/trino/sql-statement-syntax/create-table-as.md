# CREATE TABLE AS

### Synopsis[#](https://trino.io/docs/current/sql/create-table-as.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
CREATE [ OR REPLACE ] TABLE [ IF NOT EXISTS ] table_name [ ( column_alias, ... ) ]
[ COMMENT table_comment ]
[ WITH ( property_name = expression [, ...] ) ]
AS query
[ WITH [ NO ] DATA ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/create-table-as.html#description) <a href="#description" id="description"></a>

Create a new table containing the result of a [SELECT](https://trino.io/docs/current/sql/select.html) query. Use [CREATE TABLE](https://trino.io/docs/current/sql/create-table.html) to create an empty table.

The optional `OR REPLACE` clause causes an existing table with the specified name to be replaced with the new table definition. Support for table replacement varies across connectors. Refer to the connector documentation for details.

The optional `IF NOT EXISTS` clause causes the error to be suppressed if the table already exists.

`OR REPLACE` and `IF NOT EXISTS` cannot be used together.

The optional `WITH` clause can be used to set properties on the newly created table. To list all available table properties, run the following query:

```
SELECT * FROM system.metadata.table_properties
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Examples[#](https://trino.io/docs/current/sql/create-table-as.html#examples) <a href="#examples" id="examples"></a>

Create a new table `orders_column_aliased` with the results of a query and the given column names:

```
CREATE TABLE orders_column_aliased (order_date, total_price)
AS
SELECT orderdate, totalprice
FROM orders
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create a new table `orders_by_date` that summarizes `orders`:

```
CREATE TABLE orders_by_date
COMMENT 'Summary of orders by date'
WITH (format = 'ORC')
AS
SELECT orderdate, sum(totalprice) AS price
FROM orders
GROUP BY orderdate
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create the table `orders_by_date` if it does not already exist:

```
CREATE TABLE IF NOT EXISTS orders_by_date AS
SELECT orderdate, sum(totalprice) AS price
FROM orders
GROUP BY orderdate
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create a new `empty_nation` table with the same schema as `nation` and no data:

```
CREATE TABLE empty_nation AS
SELECT *
FROM nation
WITH NO DATA
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/create-table-as.html#see-also) <a href="#see-also" id="see-also"></a>

[CREATE TABLE](https://trino.io/docs/current/sql/create-table.html), [SELECT](https://trino.io/docs/current/sql/select.html)
