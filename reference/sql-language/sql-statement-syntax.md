# SQL Statement Syntax

This chapter describes the SQL syntax used in Presto.

### ALTER SCHEMA

#### **Synopsis**

```sql
ALTER SCHEMA name RENAME TO new_name
```

#### **Description**

Change the definition of an existing schema.

#### **Examples**

Rename schema web to traffic:

```sql
ALTER SCHEMA web RENAME TO traffic
```

### ALTER TABLE

#### **Synopsis**

```sql
ALTER TABLE name RENAME TO new_name
ALTER TABLE name ADD COLUMN column_name data_type [ COMMENT comment ] [ WITH ( property_name = expression [, ...] ) ]
ALTER TABLE name DROP COLUMN column_name
ALTER TABLE name RENAME COLUMN column_name TO new_column_name
```

#### **Description**

Change the definition of an existing table.

#### Examples

Rename table users to people:

`ALTER TABLE users RENAME TO people;`

Add column zip to the users table:

`ALTER TABLE users ADD COLUMN zip varchar;`

Drop column zip from the users table:

`ALTER TABLE users DROP COLUMN zip;`

Rename column id to user\_id in the users table:

`ALTER TABLE users RENAME COLUMN id TO user_id;`

### ANALYZE

#### **Synopsis**

```sql
ANALYZE table_name [ WITH ( property_name = expression [, ...] ) ]
```

#### **Description**

Collects table and column statistics for a given table.

The optional `WITH` clause can be used to provide connector-specific properties. To list all available properties, run the following query:

`SELECT * FROM system.metadata.analyze_properties`

#### Examples

Analyze table web to collect table and column statistics:

`ANALYZE web;`

Analyze table stores in catalog hive and schema default:

`ANALYZE hive.default.stores;`

Analyze partitions '1992-01-01', '1992-01-02' from a Hive partitioned table sales:

`ANALYZE hive.default.sales WITH   
(partitions = ARRAY[ARRAY['1992-01-01'], ARRAY['1992-01-02']]);`

Analyze partitions with complex partition key \(state and city columns\) from a Hive partitioned table customers:

`ANALYZE hive.default.customers WITH   
(partitions = ARRAY[ARRAY['CA', 'San Francisco'], ARRAY['NY', 'NY']]);`

Analyze only columns department and product\_id for partitions '1992-01-01', '1992-01-02' from a Hive partitioned table sales:

`ANALYZE hive.default.sales WITH (`  
****    `partitions = ARRAY[ARRAY['1992-01-01'], ARRAY['1992-01-02']],  
    columns = ARRAY['department', 'product_id']);`

### CALL

#### **Synopsis**

```sql
CALL procedure_name ( [ name => ] expression [, ...] )
```

#### **Description**

Call a procedure.

Procedures can be provided by connectors to perform data manipulation or administrative tasks. For example, the [System Connector](https://prestosql.io/docs/current/connector/system.html) defines a procedure for killing a running query.

Some connectors, such as the [PostgreSQL Connector](https://prestosql.io/docs/current/connector/postgresql.html), are for systems that have their own stored procedures. These stored procedures are separate from the connector-defined procedures discussed here and thus are not directly callable via `CALL`.

#### Examples

Call a procedure using positional arguments:

`CALL test(123, 'apple');`

Call a procedure using named arguments:

`CALL test(name => 'apple', id => 123);`

Call a procedure using a fully qualified name:

`CALL catalog.schema.test();`

### COMMENT

#### **Synopsis**

```sql
COMMENT ON TABLE name IS 'comments'
```

#### **Description**

Set the comment for a table. The comment can be removed by setting the comment to `NULL`.

#### Examples

Change the comment for the users table to be master table:

`COMMENT ON TABLE users IS 'master table';`

### COMMIT

#### **Synopsis**

```sql
COMMIT [ WORK ]
```

#### **Description**

Commit the current transaction.

#### Examples

`COMMIT;  
COMMIT WORK;`

### CREATE ROLE

#### **Synopsis**

```sql
CREATE ROLE role_name
[ WITH ADMIN ( user | USER user | ROLE role | CURRENT_USER | CURRENT_ROLE ) ]
```

#### **Description**

`CREATE ROLE` creates the specified role in the current catalog.

The optional `WITH ADMIN` clause causes the role to be created with the specified user as a role admin. A role admin has permission to drop or grant a role. If the optional `WITH ADMIN` clause is not specified, the role is created with current user as admin.

#### Examples

Create role admin

`CREATE ROLE admin;`

Create role moderator with admin bob:

`CREATE ROLE moderator WITH ADMIN USER bob;`

#### Limitations

Some connectors do not support role management. See connector documentation for more details.

### CREATE SCHEMA

#### **Synopsis**

```sql
CREATE SCHEMA [ IF NOT EXISTS ] schema_name
[ WITH ( property_name = expression [, ...] ) ]
```

#### **Description**

Create a new, empty schema. A schema is a container that holds tables, views and other database objects.

The optional `IF NOT EXISTS` clause causes the error to be suppressed if the schema already exists.

The optional `WITH` clause can be used to set properties on the newly created schema. To list all available schema properties, run the following query:

`SELECT * FROM system.metadata.schema_properties`

#### Examples

Create a new schema web in the current catalog:

`CREATE SCHEMA web`

Create a new schema sales in the hive catalog:

`CREATE SCHEMA hive.sales`

Create the schema traffic if it does not already exist:

`CREATE SCHEMA IF NOT EXISTS traffic`

### CREATE TABLE

#### **Synopsis**

```sql
CREATE TABLE [ IF NOT EXISTS ]
table_name (
  { column_name data_type [ COMMENT comment ] [ WITH ( property_name = expression [, ...] ) ]
  | LIKE existing_table_name [ { INCLUDING | EXCLUDING } PROPERTIES ] }
  [, ...]
)
[ COMMENT table_comment ]
[ WITH ( property_name = expression [, ...] ) ]
```

#### **Description**

Create a new, empty table with the specified columns. Use `CREATE TABLE AS` to create a table with data.

The optional `IF NOT EXISTS` clause causes the error to be suppressed if the table already exists.

The optional `WITH` clause can be used to set properties on the newly created table or on single columns. To list all available table properties, run the following query:

`SELECT * FROM system.metadata.table_properties`

To list all available column properties, run the following query:

`SELECT * FROM system.metadata.column_properties`

The `LIKE` clause can be used to include all the column definitions from an existing table in the new table. Multiple `LIKE` clauses may be specified, which allows copying the columns from multiple tables.

If `INCLUDING PROPERTIES` is specified, all of the table properties are copied to the new table. If the `WITH` clause specifies the same property name as one of the copied properties, the value from the `WITH` clause will be used. The default behavior is `EXCLUDING PROPERTIES`. The `INCLUDING PROPERTIES` option maybe specified for at most one table.

#### Examples

Create a new table orders:

`CREATE TABLE orders (  
  orderkey bigint,  
  orderstatus varchar,  
  totalprice double,  
  orderdate date  
)  
WITH (format = 'ORC')`

Create the table `orders` if it does not already exist, adding a table comment and a column comment:

`CREATE TABLE IF NOT EXISTS orders (  
  orderkey bigint,  
  orderstatus varchar,  
  totalprice double COMMENT 'Price in cents.',  
  orderdate date  
)  
COMMENT 'A table to keep track of orders.'`

Create the table `bigger_orders` using the columns from `orders` plus additional columns at the start and end:

`CREATE TABLE  (  
  another_orderkey bigint,  
  LIKE orders,   
  another_orderdate date  
)`

### CREATE TABLE AS

#### **Synopsis**

```sql
CREATE TABLE [ IF NOT EXISTS ] table_name [ ( column_alias, ... ) ]
[ COMMENT table_comment ]
[ WITH ( property_name = expression [, ...] ) ]
AS query
[ WITH [ NO ] DATA ]
```

#### **Description**

Create a new table containing the result of a `SELECT` query. Use `CREATE TABLE` to create an empty table.

The optional `IF NOT EXISTS` clause causes the error to be suppressed if the table already exists.

The optional `WITH` clause can be used to set properties on the newly created table. To list all available table properties, run the following query:

`SELECT * FROM system.metadata.table_properties`

#### Examples

Create a new table orders\_column\_aliased with the results of a query and the given column names:

`CREATE TABLE orders_column_aliased (order_date, total_price)  
AS  
SELECT orderdate, totalprice  
FROM orders`

Create a new table orders\_by\_date that summarizes orders:

`CREATE TABLE orders_by_date  
COMMENT 'Summary of orders by date'  
WITH (format = 'ORC')  
AS  
SELECT orderdate, sum(totalprice) AS price  
FROM orders  
GROUP BY orderdate`

Create the table orders\_by\_date if it does not already exist:

`CREATE TABLE IF NOT EXISTS orders_by_date AS  
SELECT orderdate, sum(totalprice) AS price  
FROM orders  
GROUP BY orderdate`

Create a new empty\_nation table with the same schema as nation and no data:

`CREATE TABLE empty_nation AS  
SELECT *  
FROM nation  
WITH NO DATA`

### CREATE VIEW

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### DEALLOCATE PREPARE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### DELETE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### DESCRIBE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### DESCRIBE INPUT

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### DESCRIBE OUTPUT

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### DROP ROLE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

###  DROP SCHEMA

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### DROP TABLE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### DROP VIEW

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### EXECUTE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### EXPLAIN

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### EXPLAIN ANALYZE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### GRANT

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### GRANT ROLES

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### INSERT

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### PREPARE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### RESET SESSION

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### REVOKE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### REVOKE ROLES

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### ROLLBACK

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SELECT

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SET ROLE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SET SESSION

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW CATALOGS

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

###  SHOW COLUMNS

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW CREATE TABLE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW CREATE VIEW

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW FUNCTIONS

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW GRANTS

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW ROLE GRANTS

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW ROLES

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW SCHEMAS

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW SESSION

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW STATS

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### SHOW TABLES

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### START TRANSACTION

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### USE

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

### VALUES

#### **Synopsis**

```sql
XXXX
```

#### **Description**

XXXX

#### Examples

XXXX

