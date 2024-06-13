# SQL statement support

The SQL statement support in Trino can be categorized into several topics. Many statements are part of the core engine and therefore available in all use cases. For example, you can always set session properties or inspect an explain plan and perform other actions with the globally available statements.

However, the details and architecture of the connected data sources can limit some SQL functionality. For example, if the data source does not support any write operations, then a DELETE statement cannot be executed against the data source.

Similarly, if the underlying system does not have any security concepts, SQL statements like CREATE ROLE cannot be supported by Trino and the connector.

The categories of these different topics are related to read operations, write operations, security operations and transactions.

Details of the support for specific statements is available with the documentation for each connector.

### Globally available statements <a href="#globally-available-statements" id="globally-available-statements"></a>

The following statements are implemented in the core engine and available with any connector:

* CALL
* DEALLOCATE PREPARE
* DESCRIBE INPUT
* DESCRIBE OUTPUT
* EXECUTE
* EXECUTE IMMEDIATE
* EXPLAIN
* EXPLAIN ANALYZE
* PREPARE
* RESET SESSION
* SET SESSION
* SET TIME ZONE
* SHOW FUNCTIONS
* SHOW SESSION
* USE
* VALUES

#### Catalog management <a href="#catalog-management" id="catalog-management"></a>

The following statements are used to manage dynamic catalogs:

* CREATE CATALOG
* DROP CATALOG

### Read operations <a href="#read-operations" id="read-operations"></a>

The following statements provide read access to data and meta data exposed by a connector accessing a data source. They are supported by all connectors:

* SELECT including MATCH\_RECOGNIZE
* DESCRIBE
* SHOW CATALOGS
* SHOW COLUMNS
* SHOW CREATE MATERIALIZED VIEW
* SHOW CREATE SCHEMA
* SHOW CREATE TABLE
* SHOW CREATE VIEW
* SHOW GRANTS
* SHOW ROLES
* SHOW SCHEMAS
* SHOW TABLES
* SHOW STATS

### Write operations <a href="#write-operations" id="write-operations"></a>

The following statements provide write access to data and meta data exposed by a connector accessing a data source. Availability varies widely from connector to connector:

#### Data management <a href="#data-management" id="data-management"></a>

* INSERT
* UPDATE
* DELETE
* TRUNCATE
* MERGE

#### Schema and table management# <a href="#schema-and-table-management" id="schema-and-table-management"></a>

* CREATE TABLE
* CREATE TABLE AS
* DROP TABLE
* ALTER TABLE
* CREATE SCHEMA
* DROP SCHEMA
* ALTER SCHEMA
* COMMENT

#### View management <a href="#view-management" id="view-management"></a>

* CREATE VIEW
* DROP VIEW
* ALTER VIEW

#### Materialized view management <a href="#materialized-view-management" id="materialized-view-management"></a>

* CREATE MATERIALIZED VIEW
* ALTER MATERIALIZED VIEW
* DROP MATERIALIZED VIEW
* REFRESH MATERIALIZED VIEW

#### Routine management <a href="#routine-management" id="routine-management"></a>

The following statements are used to manage catalog routines:

* CREATE FUNCTION
* DROP FUNCTION
* SHOW FUNCTIONS

### Security operations <a href="#security-operations" id="security-operations"></a>

The following statements provide security-related operations to security configuration, data, and meta data exposed by a connector accessing a data source. Most connectors do not support these operations:

Connector roles:

* CREATE ROLE
* DROP ROLE
* GRANT role
* REVOKE role
* SET ROLE
* SHOW ROLE GRANTS

Grants management:

* DENY
* GRANT privilege
* REVOKE privilege

### Transactions <a href="#transactions" id="transactions"></a>

The following statements manage transactions. Most connectors do not support transactions:

* START TRANSACTION
* COMMIT
* ROLLBACK
