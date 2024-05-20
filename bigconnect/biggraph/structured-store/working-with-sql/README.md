# Working with SQL

Ignite comes with ANSI-99 compliant, horizontally scalable and fault-tolerant distributed SQL database. The distribution is provided either by partitioning the data across cluster nodes or by full replication, depending on the use case.

As a SQL database, Ignite supports all DML commands including SELECT, UPDATE, INSERT, and DELETE queries and also implements a subset of DDL commands relevant for distributed systems.

You can interact with Ignite as you would with any other SQL enabled storage by connecting with JDBC or ODBC drivers from both external tools and applications. Java, .NET and C++ developers can leverage native SQL APIs.

Internally, SQL tables have the same data structure as key-value caches. It means that you can change partition distribution of your data and leverage affinity colocation techniques for better performance.

Ignite’s default SQL engine uses H2 Database to parse and optimize queries and generate execution plans, but also the Apache Calcite-based SQL engine can be enabled to execute queries.

### Distributed Queries

Queries against partitioned tables are executed in a distributed manner:

* The query is parsed and split into multiple “map” queries and a single “reduce” query.
* All the map queries are executed on all the nodes where required data resides.
* All the nodes provide result sets of local execution to the query initiator that, in turn, will merge provided result sets into the final results.

You can force a query to be processed locally, i.e. on the subset of data that is stored on the node where the query is executed.

### Local Queries

If a query is executed over a replicated table, it will be run against the local data.

Queries over partitioned tables are executed in a distributed manner. However, you can force local execution of a query over a partitioned table.

\
