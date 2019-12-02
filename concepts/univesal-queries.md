# Univesal Queries

The Data Model can be queried in three ways: with SQL queries like any typical database, with Cypher queries like any typical graph structure or by using textual queries.

![Univesal Queries in BigConnect](../.gitbook/assets/image%20%282%29.png)

### SQL Queries

BigConnect uses [Presto](https://prestosql.io/) as its underlying SQL query engine. Presto is an open source distributed SQL query engine for running interactive analytic queries against data sources of all sizes ranging from gigabytes to petabytes. Presto was designed and written from the ground up for interactive analytics and approaches the speed of commercial data warehouses while scaling to the size of organizations like Facebook.

Presto allows querying data where it lives, including Hive, Cassandra, relational databases or even proprietary data stores. A single Presto query can combine data from multiple sources, allowing for analytics across your entire organization.

Presto is targeted at analysts who expect response times ranging from sub-second to minutes. Presto breaks the false choice between having fast analytics using an expensive commercial solution or using a slow "free" solution that requires excessive hardware.

It fully supports and extends the SQL query language and provides a large collection of SQL functions and procedures for the most challenging tasks, from handling JSON data types to GeoSpatial, HyperLogLog, Quantiles and more.

Have a look at the [SQL Language Reference](../reference/sql-language.md) page for supported SQL clauses, functions and operators.

