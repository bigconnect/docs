# SQL Syntax

Spark SQL is Apache Spark’s module for working with structured data. The SQL Syntax section describes the SQL syntax in detail along with usage examples when applicable. This document provides a list of Data Definition and Data Manipulation Statements, as well as Data Retrieval and Auxiliary Statements.

#### DDL Statements

Data Definition Statements are used to create or modify the structure of database objects in a database. Spark SQL supports the following Data Definition Statements:

* ALTER DATABASE
* ALTER TABLE
* ALTER VIEW
* CREATE DATABASE
* CREATE FUNCTION
* CREATE TABLE
* CREATE VIEW
* DROP DATABASE
* DROP FUNCTION
* DROP TABLE
* DROP VIEW
* REPAIR TABLE
* TRUNCATE TABLE
* USE DATABASE

#### DML Statements

Data Manipulation Statements are used to add, change, or delete data. Spark SQL supports the following Data Manipulation Statements:

* INSERT TABLE
* INSERT OVERWRITE DIRECTORY
* LOAD

#### Data Retrieval Statements

Spark supports `SELECT` statement that is used to retrieve rows from one or more tables according to the specified clauses. The full syntax and brief description of supported clauses are explained in SELECT section. The SQL statements related to SELECT are also included in this section. Spark also provides the ability to generate logical and physical plan for a given query using EXPLAIN statement.

* SELECT Statement
  * Common Table Expression
  * CLUSTER BY Clause
  * DISTRIBUTE BY Clause
  * GROUP BY Clause
  * HAVING Clause
  * Hints
  * Inline Table
  * File
  * JOIN
  * LIKE Predicate
  * LIMIT Clause
  * OFFSET Clause
  * ORDER BY Clause
  * Set Operators
  * SORT BY Clause
  * TABLESAMPLE
  * Table-valued Function
  * WHERE Clause
  * Aggregate Function
  * Window Function
  * CASE Clause
  * PIVOT Clause
  * UNPIVOT Clause
  * LATERAL VIEW Clause
  * LATERAL SUBQUERY
  * TRANSFORM Clause
* EXPLAIN

#### Auxiliary Statements

* ADD FILE
* ADD JAR
* ANALYZE TABLE
* CACHE TABLE
* CLEAR CACHE
* DESCRIBE DATABASE
* DESCRIBE FUNCTION
* DESCRIBE QUERY
* DESCRIBE TABLE
* LIST FILE
* LIST JAR
* REFRESH
* REFRESH TABLE
* REFRESH FUNCTION
* RESET
* SET
* SHOW COLUMNS
* SHOW CREATE TABLE
* SHOW DATABASES
* SHOW FUNCTIONS
* SHOW PARTITIONS
* SHOW TABLE EXTENDED
* SHOW TABLES
* SHOW TBLPROPERTIES
* SHOW VIEWS
* UNCACHE TABLE
