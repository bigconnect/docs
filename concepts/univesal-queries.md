# Univesal Queries

The Data Model can be queried in three ways: with SQL queries like any typical database, with Cypher queries like any typical graph structure or by using textual queries.

![Univesal Queries in BigConnect](../.gitbook/assets/image%20%2831%29.png)

### SQL Queries

BigConnect uses [Presto](https://prestosql.io/) as its underlying SQL query engine. Presto is an open source distributed SQL query engine for running interactive analytic queries against data sources of all sizes ranging from gigabytes to petabytes. Presto was designed and written from the ground up for interactive analytics and approaches the speed of commercial data warehouses while scaling to the size of organizations like Facebook.

Presto allows querying data where it lives, including Hive, Cassandra, relational databases or even proprietary data stores. A single Presto query can combine data from multiple sources, allowing for analytics across your entire organization.

Presto is targeted at analysts who expect response times ranging from sub-second to minutes. Presto breaks the false choice between having fast analytics using an expensive commercial solution or using a slow "free" solution that requires excessive hardware.

It fully supports and extends the SQL query language and provides a large collection of SQL functions and procedures for the most challenging tasks, from handling JSON data types to GeoSpatial, HyperLogLog, Quantiles and more.

Have a look at the [SQL Language Reference](../reference/sql-language.md) page for supported SQL clauses, functions and operators.

### OLAP

BigConnect uses Druid for running interactive OLAP queries. In fact, we are using a customized version of Druid with added support for joins, virtual columns, temporary data sources, columnar histograms, bit-slice indexing etc.

[Apache Druid](https://druid.apache.org/) is a real-time analytics database designed for fast slice-and-dice analytics \("OLAP" queries\) on large data sets. Druid is most often used as a database for powering use cases where real-time ingest, fast query performance, and high uptime are important. As such, Druid is commonly used for powering GUIs of analytical applications, or as a backend for highly concurrent APIs that need fast aggregations.

Druid was the optimal engine for BigConnect because its characteristics:

* It collects massive amounts of data in real time and indexes them into a queryable format, ensuring very fast data aggregations \(a few seconds at the slowest\) based on distributed processing
* Its OLAP time-series data format enables analysts to perform data exploration, filtering, and visualization as desired. Such free and flexible data exploration is essential for users to intuitively select the required data and determine correlations between different dimensions on it
* It has an extensible architecture

### Cypher Queries

BigConnect uses its own graph engine for the Unstructured data model. Unstructured data is mapped to Concepts, Relations and Properties. Concepts can be anything you can think of: persons, vehicles, bank transactions, phone calls, equipment, companies, locations, events, network packets, log files etc. Concepts are linked to each other using meaningful Relations like “works at”, “lives in”, “has friend”, “is brother of”, “sent from IP”, “source file” etc.

Concepts and relations can also have attributes. For a company these can be the company name, formation date, address etc. For a person these can be its first name, last name, birth date, phone number, email address etc. For a “works at” relation we can have the “start date” and “end date” attributes to denote when the person started to work at a company. For a “sent from IP” relation we can have the “timestamp” and the “user” etc.

The graph engine can be queried in depth using Cypher and textual queries.

Cypher is a declarative graph query language that allows for expressive and efficient querying and updating of the graph. It is designed to be suitable for both developers and operations professionals. Cypher is designed to be simple, yet powerful; highly complicated database queries can be easily expressed, enabling you to focus on your domain, instead of getting lost in database access.

Cypher is inspired by a number of different approaches and builds on established practices for expressive querying. Many of the keywords, such as WHERE and ORDER BY, are inspired by [SQL](http://en.wikipedia.org/wiki/SQL). Pattern matching borrows expression approaches from [SPARQL](http://en.wikipedia.org/wiki/SPARQL). Some of the list semantics are borrowed from languages such as Haskell and Python. Cypher’s constructs, based on English prose and neat iconography, make queries easy both to write, and to read.

Have a look at the [Cypher Language Reference](../reference/cypher-language.md) page for supported  clauses, functions and operators.

### Textual Queries

BigConnect has [Elasticsearch](https://github.com/elastic/elasticsearch) embedded in its core runtime engine for indexing and querying data. Elasticsearch provides real-time search and analytics for all types of data. Whether you have structured or unstructured text, numerical data, or geospatial data, Elasticsearch can efficiently store and index it in a way that supports fast searches. You can go far beyond simple data retrieval and aggregate information to discover trends and patterns in your data. And as your data and query volume grows, the distributed nature of Elasticsearch enables your deployment to grow seamlessly right along with it.

Elasticsearch supports running Full-Text queries, Term-level queries, Compound queries, Joining queries, GeoSpatial queries and other types of Specialized queries \(eg. More like this\).

Besides running queries, Elasticsearch can also perform very fast aggregations on query results. There are many different types of aggregations, each with its own purpose and output. To better understand these types, please refer to the official Elasticsearch[ documentation on aggregations](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations.html).

