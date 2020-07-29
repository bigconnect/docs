# DataCharm

DataCharm is a distributed, in-memory SQL query engine designed to query large data sets distributed over one or more heterogeneous data sources.

DataCharm is based on Presto.

### What DataCharm is Not

Since DataCharm can be called a _database_, it makes sense to begin with a definition of what DataCharm is not.

Do not mistake the fact that DataCharm understands SQL with it providing the features of a standard database. DataCharm is not a general-purpose relational database. It is not a replacement for databases like MySQL, PostgreSQL or Oracle. DataCharm was not designed to handle Online Transaction Processing \(OLTP\). This is also true for many other databases designed and optimized for data warehousing or analytics.

### What DataCharm Is

DataCharm is a tool designed to efficiently query vast amounts of data using distributed queries. If you work with terabytes or petabytes of data, you are likely using tools that interact with Hadoop and HDFS. DataCharm was designed as an alternative to tools that query HDFS using pipelines of MapReduce jobs, such as Hive or Pig, but DataCharm is not limited to accessing HDFS. DataCharm can be and has been extended to operate over different kinds of data sources, including traditional relational databases and other data sources such as BigConnect's Big Graph or Kafka \(StreamNet\).

DataCharm was designed to handle data warehousing and analytics: data analysis, aggregating large amounts of data and producing reports. These workloads are often classified as Online Analytical Processing \(OLAP\).

DataCharm originates from Presto and has many improvements like query plan optimization, additional connectors, cluster management and more.

