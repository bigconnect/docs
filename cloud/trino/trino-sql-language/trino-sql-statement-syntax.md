# SQL Statement Syntax

This chapter describes the SQL syntax used in Trino.

### ALTER SCHEMA

#### **Synopsis**

```sql
ALTER SCHEMA name RENAME TO new_name
```

#### **Description**

Change the definition of an existing schema.

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

### ANALYZE

#### **Synopsis**

```sql
ANALYZE table_name [ WITH ( property_name = expression [, ...] ) ]
```

#### **Description**

Collects table and column statistics for a given table.

The optional `WITH` clause can be used to provide connector-specific properties. To list all available properties, run the following query:

`SELECT * FROM system.metadata.analyze_properties`

### CALL

#### **Synopsis**

```sql
CALL procedure_name ( [ name => ] expression [, ...] )
```

#### **Description**

Call a procedure.

Procedures can be provided by connectors to perform data manipulation or administrative tasks. For example, the [System Connector](https://prestosql.io/docs/current/connector/system.html) defines a procedure for killing a running query.

Some connectors, such as the [PostgreSQL Connector](https://prestosql.io/docs/current/connector/postgresql.html), are for systems that have their own stored procedures. These stored procedures are separate from the connector-defined procedures discussed here and thus are not directly callable via `CALL`.

### COMMENT

#### **Synopsis**

```sql
COMMENT ON TABLE name IS 'comments'
```

#### **Description**

Set the comment for a table. The comment can be removed by setting the comment to `NULL`.

### COMMIT

#### **Synopsis**

```sql
COMMIT [ WORK ]
```

#### **Description**

Commit the current transaction.

### CREATE ROLE

#### **Synopsis**

```sql
CREATE ROLE role_name
[ WITH ADMIN ( user | USER user | ROLE role | CURRENT_USER | CURRENT_ROLE ) ]
```

#### **Description**

`CREATE ROLE` creates the specified role in the current catalog.

The optional `WITH ADMIN` clause causes the role to be created with the specified user as a role admin. A role admin has permission to drop or grant a role. If the optional `WITH ADMIN` clause is not specified, the role is created with current user as admin.

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

### CREATE VIEW

#### **Synopsis**

```sql
CREATE [ OR REPLACE ] VIEW view_name
[ SECURITY { DEFINER | INVOKER } ]
AS query
```

#### **Description**

Create a new view of a `SELECT` query. The view is a logical table that can be referenced by future queries. Views do not contain any data. Instead, the query stored by the view is executed everytime the view is referenced by another query.

The optional `OR REPLACE` clause causes the view to be replaced if it already exists rather than raising an error.

#### Security

In the default `DEFINER` security mode, tables referenced in the view are accessed using the permissions of the view owner \(the _creator_ or _definer_ of the view\) rather than the user executing the query. This allows providing restricted access to the underlying tables, for which the user may not be allowed to access directly.

In the `INVOKER` security mode, tables referenced in the view are accessed using the permissions of the user executing the query \(the _invoker_ of the view\). A view created in this mode is simply a stored query.

Regardless of the security mode, the `current_user` function will always return the user executing the query and thus may be used within views to filter out rows or otherwise restrict access.

### DEALLOCATE PREPARE

#### **Synopsis**

```sql
DEALLOCATE PREPARE statement_name
```

#### **Description**

Removes a statement with the name `statement_name` from the list of prepared statements in a session.

### DELETE

#### **Synopsis**

```sql
DELETE FROM table_name [ WHERE condition ]
```

#### **Description**

Delete rows from a table. If the `WHERE` clause is specified, only the matching rows are deleted. Otherwise, all rows from the table are deleted.

#### Limitations

Some connectors have limited or no support for `DELETE`. See connector documentation for more details.

### DESCRIBE

#### **Synopsis**

```sql
DESCRIBE table_name
```

#### **Description**

`DESCRIBE` is an alias for `SHOW COLUMNS`.

### DESCRIBE INPUT

#### **Synopsis**

```sql
DESCRIBE INPUT statement_name
```

#### **Description**

Lists the input parameters of a prepared statement along with the position and type of each parameter. Parameter types that cannot be determined will appear as `unknown`.

### DESCRIBE OUTPUT

#### **Synopsis**

```sql
DESCRIBE OUTPUT statement_name
```

#### **Description**

List the output columns of a prepared statement, including the column name \(or alias\), catalog, schema, table, type, type size in bytes, and a boolean indicating if the column is aliased.

### DROP ROLE

#### **Synopsis**

```sql
DROP ROLE role_name
```

#### **Description**

`DROP ROLE` drops the specified role in the current catalog.

For `DROP ROLE` statement to succeed, the user executing it should possess admin privileges for the given role.

#### Limitations

Some connectors do not support role management. See connector documentation for more details.

### DROP SCHEMA

#### **Synopsis**

```sql
DROP SCHEMA [ IF EXISTS ] schema_name
```

#### **Description**

Drop an existing schema. The schema must be empty.

The optional `IF EXISTS` clause causes the error to be suppressed if the schema does not exist.

### DROP TABLE

#### **Synopsis**

```sql
DROP TABLE  [ IF EXISTS ] table_name
```

#### **Description**

Drops an existing table.

The optional `IF EXISTS` clause causes the error to be suppressed if the table does not exist.

### DROP VIEW

#### **Synopsis**

```sql
DROP VIEW [ IF EXISTS ] view_name
```

#### **Description**

Drop an existing view.

The optional `IF EXISTS` clause causes the error to be suppressed if the view does not exist.

### EXECUTE

#### **Synopsis**

```sql
EXECUTE statement_name [ USING parameter1 [ , parameter2, ... ] ]
```

#### **Description**

Executes a prepared statement with the name `statement_name`. Parameter values are defined in the `USING` clause.

### EXPLAIN

#### **Synopsis**

```sql
EXPLAIN [ ( option [, ...] ) ] statement

where option can be one of:

    FORMAT { TEXT | GRAPHVIZ | JSON }
    TYPE { LOGICAL | DISTRIBUTED | VALIDATE | IO }
```

#### **Description**

Show the logical or distributed execution plan of a statement, or validate the statement. Use `TYPE DISTRIBUTED` option to display fragmented plan. Each plan fragment is executed by a single or multiple Presto nodes. Fragments separation represent the data exchange between Presto nodes. Fragment type specifies how the fragment is executed by Presto nodes and how the data is distributed between fragments:

* `SINGLE`Fragment is executed on a single node.
* `HASH`Fragment is executed on a fixed number of nodes with the input data distributed using a hash function.
* `ROUND_ROBIN` Fragment is executed on a fixed number of nodes with the input data distributed in a round-robin fashion.
* `BROADCAST` Fragment is executed on a fixed number of nodes with the input data broadcasted to all nodes.
* `SOURCE`Fragment is executed on nodes where input splits are accessed.

### EXPLAIN ANALYZE

#### **Synopsis**

```sql
EXPLAIN ANALYZE [VERBOSE] statement
```

#### **Description**

Execute the statement and show the distributed execution plan of the statement along with the cost of each operation.

The `VERBOSE` option will give more detailed information and low-level statistics; understanding these may require knowledge of Presto internals and implementation details.

{% hint style="info" %}
The stats may not be entirely accurate, especially for queries that complete quickly.
{% endhint %}

### GRANT

#### **Synopsis**

```sql
GRANT ( privilege [, ...] | ( ALL PRIVILEGES ) )
ON [ TABLE ] table_name TO ( user | USER user | ROLE role )
[ WITH GRANT OPTION ]
```

#### **Description**

Grants the specified privileges to the specified grantee.

Specifying `ALL PRIVILEGES` grants `DELETE`, `INSERT` and `SELECT` privileges.

Specifying `ROLE PUBLIC` grants privileges to the `PUBLIC` role and hence to all users.

The optional `WITH GRANT OPTION` clause allows the grantee to grant these same privileges to others.

For `GRANT` statement to succeed, the user executing it should possess the specified privileges as well as the `GRANT OPTION` for those privileges.

### GRANT ROLES

#### **Synopsis**

```sql
GRANT role [, ...]
TO ( user | USER user | ROLE role) [, ...]
[ GRANTED BY ( user | USER user | ROLE role | CURRENT_USER | CURRENT_ROLE ) ]
[ WITH ADMIN OPTION ]
```

#### **Description**

Grants the specified role\(s\) to the specified principal\(s\) in the current catalog.

If the `WITH ADMIN OPTION` clause is specified, the role\(s\) are granted to the users with `GRANT` option.

For the `GRANT` statement for roles to succeed, the user executing it either should be the role admin or should possess the `GRANT` option for the given role.

The optional `GRANTED BY` clause causes the role\(s\) to be granted with the specified principal as a grantor. If the `GRANTED BY` clause is not specified, the roles are granted with the current user as a grantor.

### INSERT

#### **Synopsis**

```sql
INSERT INTO table_name [ ( column [, ... ] ) ] query
```

#### **Description**

Insert new rows into a table.

If the list of column names is specified, they must exactly match the list of columns produced by the query. Each column in the table not present in the column list will be filled with a `null` value. Otherwise, if the list of columns is not specified, the columns produced by the query must exactly match the columns in the table being inserted into.

### PREPARE

#### **Synopsis**

```sql
PREPARE statement_name FROM statement
```

#### **Description**

Prepares a statement for execution at a later time. Prepared statements are queries that are saved in a session with a given name. The statement can include parameters in place of literals to be replaced at execution time. Parameters are represented by question marks.

### RESET SESSION

#### **Synopsis**

```sql
RESET SESSION name
RESET SESSION catalog.name
```

#### **Description**

Reset a session property value to the default value.

### REVOKE

#### **Synopsis**

```sql
REVOKE [ GRANT OPTION FOR ]
( privilege [, ...] | ALL PRIVILEGES )
ON [ TABLE ] table_name FROM ( user | USER user | ROLE role )
```

#### **Description**

Revokes the specified privileges from the specified grantee.

Specifying `ALL PRIVILEGES` revokes DELETE, INSERT and SELECT privileges.

Specifying `ROLE PUBLIC` revokes privileges from the `PUBLIC` role. Users will retain privileges assigned to them directly or via other roles.

The optional `GRANT OPTION FOR` clause also revokes the privileges to grant the specified privileges.

For `REVOKE` statement to succeed, the user executing it should possess the specified privileges as well as the `GRANT OPTION` for those privileges.

#### Limitations

Some connectors have no support for `REVOKE`. See connector documentation for more details.

### REVOKE ROLES

#### **Synopsis**

```sql
REVOKE
[ ADMIN OPTION FOR ]
role [, ...]
FROM ( user | USER user | ROLE role) [, ...]
[ GRANTED BY ( user | USER user | ROLE role | CURRENT_USER | CURRENT_ROLE ) ]
```

#### **Description**

Revokes the specified role\(s\) from the specified principal\(s\) in the current catalog.

If the `ADMIN OPTION FOR` clause is specified, the `GRANT` permission is revoked instead of the role.

For the `REVOKE` statement for roles to succeed, the user executing it either should be the role admin or should possess the `GRANT` option for the given role.

The optional `GRANTED BY` clause causes the role\(s\) to be revoked with the specified principal as a revoker. If the `GRANTED BY` clause is not specified, the roles are revoked by the current user as a revoker.

#### Limitations

Some connectors do not support role management. See connector documentation for more details.

### ROLLBACK

#### **Synopsis**

```sql
ROLLBACK [ WORK ]
```

#### **Description**

Rollback the current transaction.

### SELECT

#### **Synopsis**

```sql
[ WITH with_query [, ...] ]
SELECT [ ALL | DISTINCT ] select_expression [, ...]
[ FROM from_item [, ...] ]
[ WHERE condition ]
[ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]
[ HAVING condition]
[ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]
[ ORDER BY expression [ ASC | DESC ] [, ...] ]
[ OFFSET count [ ROW | ROWS ] ]
[ LIMIT { count | ALL } | FETCH { FIRST | NEXT } [ count ] { ROW | ROWS } { ONLY | WITH TIES } ]
```

where `from_item` is one of

```sql
table_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
from_item join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]
```

and `join_type` is one of

```sql
[ INNER ] JOIN
LEFT [ OUTER ] JOIN
RIGHT [ OUTER ] JOIN
FULL [ OUTER ] JOIN
CROSS JOIN
```

and `grouping_element` is one of

```sql
()
expression
GROUPING SETS ( ( column [, ...] ) [, ...] )
CUBE ( column [, ...] )
ROLLUP ( column [, ...] )
```

#### **Description**

Retrieve rows from zero or more tables.

### SET ROLE

#### **Synopsis**

```sql
SET ROLE ( role | ALL | NONE )
```

#### **Description**

`SET ROLE` sets the enabled role for the current session in the current catalog.

`SET ROLE role` enables a single specified role for the current session. For the `SET ROLE role` statement to succeed, the user executing it should have a grant for the given role.

`SET ROLE ALL` enables all roles that the current user has been granted for the current session.

`SET ROLE NONE` disables all the roles granted to the current user for the current session.

#### Limitations

Some connectors do not support role management. See connector documentation for more details.

### SET SESSION

#### **Synopsis**

```sql
SET SESSION name = expression
SET SESSION catalog.name = expression
```

#### **Description**

Set a session property value.

### SHOW CATALOGS

#### **Synopsis**

```sql
SHOW CATALOGS [ LIKE pattern ]
```

#### **Description**

List the available catalogs. The `LIKE` clause can be used to restrict the list of catalog names.

### SHOW COLUMNS

#### **Synopsis**

```sql
SHOW COLUMNS FROM table
```

#### **Description**

List the columns in `table` along with their data type and other attributes.

### SHOW CREATE TABLE

#### **Synopsis**

```sql
SHOW CREATE TABLE table_name
```

#### **Description**

Show the SQL statement that creates the specified table.

### SHOW CREATE VIEW

#### **Synopsis**

```sql
SHOW CREATE VIEW view_name
```

#### **Description**

Show the SQL statement that creates the specified view.

### SHOW FUNCTIONS

#### **Synopsis**

```sql
SHOW FUNCTIONS [ LIKE pattern ]
```

#### **Description**

List all the functions available for use in queries. The `LIKE` clause can be used to restrict the list of functions.

### SHOW GRANTS

#### **Synopsis**

```sql
SHOW GRANTS [ ON [ TABLE ] table_name ]
```

#### **Description**

List the grants for the current user on the specified table in the current catalog.

If no table name is specified, the command lists the grants for the current user on all the tables in all schemas of the current catalog.

The command requires the current catalog to be set.

{% hint style="info" %}
Ensure that authentication has been enabled before running any of the authorization commands.
{% endhint %}

#### Limitations

Some connectors have no support for `SHOW GRANTS`. See connector documentation for more details.

### SHOW ROLE GRANTS

#### **Synopsis**

```sql
SHOW ROLE GRANTS [ FROM catalog ]
```

#### **Description**

List non-recursively the ```ROLE``s that have been granted to the session user in ``catalog```, or the current catalog if `catalog` is not specified.

### SHOW ROLES

#### **Synopsis**

```sql
SHOW [CURRENT] ROLES [ FROM catalog ]
```

#### **Description**

`SHOW ROLES` lists all the roles in `catalog` or in the current catalog if `catalog` is not specified.

`SHOW CURRENT ROLES` lists the enabled roles for the session in `catalog, or in the current catalog if catalog` is not specified.

### SHOW SCHEMAS

#### **Synopsis**

```sql
SHOW SCHEMAS [ FROM catalog ] [ LIKE pattern ]
```

#### **Description**

List the schemas in `catalog` or in the current catalog. The `LIKE` clause can be used to restrict the list of schema names.

### SHOW SESSION

#### **Synopsis**

```sql
SHOW SESSION [ LIKE pattern ]
```

#### **Description**

List the current session properties. The `LIKE` clause can be used to restrict the list of session properties.

### SHOW STATS

#### **Synopsis**

```sql
SHOW STATS FOR table
SHOW STATS FOR ( SELECT * FROM table [ WHERE condition ] )
```

#### **Description**

Returns approximated statistics for the named table or for the results of a \(limited\) query.

Statistics are returned for each column, along with a summary row.

| Column | Description |
| :--- | :--- |
| `column_name` | The name of the column \(`NULL` for the summary row\) |
| `data_size` | The total size in bytes of all of the values in the column |
| `distinct_values_count` | The number of distinct values in the column |
| `nulls_fractions` | The portion of the values in the column that are `NULL` |
| `row_count` | The number of rows \(only returned for the summary row\) |
| `low_value` | The lowest value found in this column \(only for some types\) |
| `high_value` | The highest value found in this column \(only for some types\) |

### SHOW TABLES

#### **Synopsis**

```sql
SHOW TABLES [ FROM schema ] [ LIKE pattern ]
```

#### **Description**

List the tables in `schema` or in the current schema. The `LIKE` clause can be used to restrict the list of table names.

### START TRANSACTION

#### **Synopsis**

```sql
START TRANSACTION [ mode [, ...] ]
```

where `mode` is one of

```sql
ISOLATION LEVEL { READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
READ { ONLY | WRITE }
```

#### **Description**

Start a new transaction for the current session.

### USE

#### **Synopsis**

```sql
USE catalog.schema
USE schema
```

#### **Description**

Update the session to use the specified catalog and schema. If a catalog is not specified, the schema is resolved relative to the current catalog.

### VALUES

#### **Synopsis**

```sql
VALUES row [, ...]
```

where `row` is a single expression or

```sql
( column_expression [, ...] )
```

#### **Description**

Defines a literal inline table.

`VALUES` can be used anywhere a query can be used \(e.g., the `FROM` clause of a `SELECT`, an `INSERT`, or even at the top level\). `VALUES` creates an anonymous table without column names, but the table and columns can be named using an `AS` clause with column aliases.

