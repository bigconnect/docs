# SQL statement support

The SQL statement support in Trino can be categorized into several topics. Many statements are part of the core engine and therefore available in all use cases. For example, you can always set session properties or inspect an explain plan and perform other actions with the [globally available statements](https://trino.io/docs/current/language/sql-support.html#sql-globally-available).

However, the details and architecture of the connected data sources can limit some SQL functionality. For example, if the data source does not support any write operations, then a [DELETE](https://trino.io/docs/current/sql/delete.html) statement cannot be executed against the data source.

Similarly, if the underlying system does not have any security concepts, SQL statements like [CREATE ROLE](https://trino.io/docs/current/sql/create-role.html) cannot be supported by Trino and the connector.

The categories of these different topics are related to [read operations](https://trino.io/docs/current/language/sql-support.html#sql-read-operations), [write operations](https://trino.io/docs/current/language/sql-support.html#sql-write-operations), [security operations](https://trino.io/docs/current/language/sql-support.html#sql-security-operations) and [transactions](https://trino.io/docs/current/language/sql-support.html#sql-transactions).

Details of the support for specific statements is available with the documentation for each connector.

### Globally available statements[#](https://trino.io/docs/current/language/sql-support.html#globally-available-statements) <a href="#globally-available-statements" id="globally-available-statements"></a>

The following statements are implemented in the core engine and available with any connector:

* [CALL](https://trino.io/docs/current/sql/call.html)
* [DEALLOCATE PREPARE](https://trino.io/docs/current/sql/deallocate-prepare.html)
* [DESCRIBE INPUT](https://trino.io/docs/current/sql/describe-input.html)
* [DESCRIBE OUTPUT](https://trino.io/docs/current/sql/describe-output.html)
* [EXECUTE](https://trino.io/docs/current/sql/execute.html)
* [EXECUTE IMMEDIATE](https://trino.io/docs/current/sql/execute-immediate.html)
* [EXPLAIN](https://trino.io/docs/current/sql/explain.html)
* [EXPLAIN ANALYZE](https://trino.io/docs/current/sql/explain-analyze.html)
* [PREPARE](https://trino.io/docs/current/sql/prepare.html)
* [RESET SESSION](https://trino.io/docs/current/sql/reset-session.html)
* [SET SESSION](https://trino.io/docs/current/sql/set-session.html)
* [SET TIME ZONE](https://trino.io/docs/current/sql/set-time-zone.html)
* [SHOW FUNCTIONS](https://trino.io/docs/current/sql/show-functions.html)
* [SHOW SESSION](https://trino.io/docs/current/sql/show-session.html)
* [USE](https://trino.io/docs/current/sql/use.html)
* [VALUES](https://trino.io/docs/current/sql/values.html)

#### Catalog management[#](https://trino.io/docs/current/language/sql-support.html#catalog-management) <a href="#catalog-management" id="catalog-management"></a>

The following statements are used to [manage dynamic catalogs](https://trino.io/docs/current/admin/properties-catalog.html):

* [CREATE CATALOG](https://trino.io/docs/current/sql/create-catalog.html)
* [DROP CATALOG](https://trino.io/docs/current/sql/drop-catalog.html)

### Read operations[#](https://trino.io/docs/current/language/sql-support.html#read-operations) <a href="#read-operations" id="read-operations"></a>

The following statements provide read access to data and meta data exposed by a connector accessing a data source. They are supported by all connectors:

* [SELECT](https://trino.io/docs/current/sql/select.html) including [MATCH\_RECOGNIZE](https://trino.io/docs/current/sql/match-recognize.html)
* [DESCRIBE](https://trino.io/docs/current/sql/describe.html)
* [SHOW CATALOGS](https://trino.io/docs/current/sql/show-catalogs.html)
* [SHOW COLUMNS](https://trino.io/docs/current/sql/show-columns.html)
* [SHOW CREATE MATERIALIZED VIEW](https://trino.io/docs/current/sql/show-create-materialized-view.html)
* [SHOW CREATE SCHEMA](https://trino.io/docs/current/sql/show-create-schema.html)
* [SHOW CREATE TABLE](https://trino.io/docs/current/sql/show-create-table.html)
* [SHOW CREATE VIEW](https://trino.io/docs/current/sql/show-create-view.html)
* [SHOW GRANTS](https://trino.io/docs/current/sql/show-grants.html)
* [SHOW ROLES](https://trino.io/docs/current/sql/show-roles.html)
* [SHOW SCHEMAS](https://trino.io/docs/current/sql/show-schemas.html)
* [SHOW TABLES](https://trino.io/docs/current/sql/show-tables.html)
* [SHOW STATS](https://trino.io/docs/current/sql/show-stats.html)

### Write operations[#](https://trino.io/docs/current/language/sql-support.html#write-operations) <a href="#write-operations" id="write-operations"></a>

The following statements provide write access to data and meta data exposed by a connector accessing a data source. Availability varies widely from connector to connector:

#### Data management[#](https://trino.io/docs/current/language/sql-support.html#data-management) <a href="#data-management" id="data-management"></a>

* [INSERT](https://trino.io/docs/current/sql/insert.html)
* [UPDATE](https://trino.io/docs/current/sql/update.html)
* [DELETE](https://trino.io/docs/current/sql/delete.html)
* [TRUNCATE](https://trino.io/docs/current/sql/truncate.html)
* [MERGE](https://trino.io/docs/current/sql/merge.html)

#### Schema and table management[#](https://trino.io/docs/current/language/sql-support.html#schema-and-table-management) <a href="#schema-and-table-management" id="schema-and-table-management"></a>

* [CREATE TABLE](https://trino.io/docs/current/sql/create-table.html)
* [CREATE TABLE AS](https://trino.io/docs/current/sql/create-table-as.html)
* [DROP TABLE](https://trino.io/docs/current/sql/drop-table.html)
* [ALTER TABLE](https://trino.io/docs/current/sql/alter-table.html)
* [CREATE SCHEMA](https://trino.io/docs/current/sql/create-schema.html)
* [DROP SCHEMA](https://trino.io/docs/current/sql/drop-schema.html)
* [ALTER SCHEMA](https://trino.io/docs/current/sql/alter-schema.html)
* [COMMENT](https://trino.io/docs/current/sql/comment.html)

#### View management[#](https://trino.io/docs/current/language/sql-support.html#view-management) <a href="#view-management" id="view-management"></a>

* [CREATE VIEW](https://trino.io/docs/current/sql/create-view.html)
* [DROP VIEW](https://trino.io/docs/current/sql/drop-view.html)
* [ALTER VIEW](https://trino.io/docs/current/sql/alter-view.html)

#### Materialized view management[#](https://trino.io/docs/current/language/sql-support.html#materialized-view-management) <a href="#materialized-view-management" id="materialized-view-management"></a>

* [CREATE MATERIALIZED VIEW](https://trino.io/docs/current/sql/create-materialized-view.html)
* [ALTER MATERIALIZED VIEW](https://trino.io/docs/current/sql/alter-materialized-view.html)
* [DROP MATERIALIZED VIEW](https://trino.io/docs/current/sql/drop-materialized-view.html)
* [REFRESH MATERIALIZED VIEW](https://trino.io/docs/current/sql/refresh-materialized-view.html)

#### Routine management[#](https://trino.io/docs/current/language/sql-support.html#routine-management) <a href="#routine-management" id="routine-management"></a>

The following statements are used to manage [catalog routines](https://trino.io/docs/current/routines/introduction.html#routine-catalog):

* [CREATE FUNCTION](https://trino.io/docs/current/sql/create-function.html)
* [DROP FUNCTION](https://trino.io/docs/current/sql/drop-function.html)
* [SHOW FUNCTIONS](https://trino.io/docs/current/sql/show-functions.html)

### Security operations[#](https://trino.io/docs/current/language/sql-support.html#security-operations) <a href="#security-operations" id="security-operations"></a>

The following statements provide security-related operations to security configuration, data, and meta data exposed by a connector accessing a data source. Most connectors do not support these operations:

Connector roles:

* [CREATE ROLE](https://trino.io/docs/current/sql/create-role.html)
* [DROP ROLE](https://trino.io/docs/current/sql/drop-role.html)
* [GRANT role](https://trino.io/docs/current/sql/grant-roles.html)
* [REVOKE role](https://trino.io/docs/current/sql/revoke-roles.html)
* [SET ROLE](https://trino.io/docs/current/sql/set-role.html)
* [SHOW ROLE GRANTS](https://trino.io/docs/current/sql/show-role-grants.html)

Grants management:

* [DENY](https://trino.io/docs/current/sql/deny.html)
* [GRANT privilege](https://trino.io/docs/current/sql/grant.html)
* [REVOKE privilege](https://trino.io/docs/current/sql/revoke.html)

### Transactions[#](https://trino.io/docs/current/language/sql-support.html#transactions) <a href="#transactions" id="transactions"></a>

The following statements manage transactions. Most connectors do not support transactions:

* [START TRANSACTION](https://trino.io/docs/current/sql/start-transaction.html)
* [COMMIT](https://trino.io/docs/current/sql/commit.html)
* [ROLLBACK](https://trino.io/docs/current/sql/rollback.html)
