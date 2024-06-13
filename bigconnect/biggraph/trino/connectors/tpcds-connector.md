# TPCDS connector

The TPCDS connector provides a set of schemas to support the [TPC Benchmark™ DS (TPC-DS)](http://www.tpc.org/tpcds/). TPC-DS is a database benchmark used to measure the performance of complex decision support databases.

This connector can be used to test the capabilities and query syntax of Trino without configuring access to an external data source. When you query a TPCDS schema, the connector generates the data on the fly using a deterministic algorithm.

### Configuration <a href="#configuration" id="configuration"></a>

To configure the TPCDS connector, create a catalog properties file `etc/catalog/example.properties` with the following contents:

```
connector.name=tpcds
```

### TPCDS schemas <a href="#tpcds-schemas" id="tpcds-schemas"></a>

The TPCDS connector supplies several schemas:

```
SHOW SCHEMAS FROM example;
```

```
       Schema
--------------------
 information_schema
 sf1
 sf10
 sf100
 sf1000
 sf10000
 sf100000
 sf300
 sf3000
 sf30000
 tiny
(11 rows)
```

Ignore the standard schema `information_schema`, which exists in every catalog, and is not directly provided by the TPCDS connector.

Every TPCDS schema provides the same set of tables. Some tables are identical in all schemas. The _scale factor_ of the tables in a particular schema is determined from the schema name. For example, the schema `sf1` corresponds to scale factor `1` and the schema `sf300` corresponds to scale factor `300`. Every unit in the scale factor corresponds to a gigabyte of data. For example, for scale factor `300`, a total of `300` gigabytes are generated. The `tiny` schema is an alias for scale factor `0.01`, which is a very small data set useful for testing.

### Type mapping <a href="#type-mapping" id="type-mapping"></a>

Trino supports all data types used within the TPCDS schemas so no mapping is required.

### SQL support <a href="#sql-support" id="sql-support"></a>

The connector provides globally available and read operation statements to access data and metadata in the TPC-DS dataset.
