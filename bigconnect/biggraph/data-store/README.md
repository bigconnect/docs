# Data storage

In BigConnect Core, the data is stored in a Graph by using Vertices, Edges, Properties and ExtendedData. There are three implementation for the Graph store:

* **AccumuloGraph** - this is the "production-grade" storage type. It's currently being used by large customers to store billions of vertices and edges. It uses [Apache Accumulo](https://accumulo.apache.org/) as the underlying NoSQL data store and is designed to run in clusters of hundreds of nodes.
* **RocksDBGraph** - this is the "small-grade" storage type designed for local, desktop use. It should have to problem to store more than 1 million vertices and edges but it will start to get laggy once you pass that boundary. It cannot be distributed due to the nature of [RocksDB](http://rocksdb.org/).
* **InMemoryGraph** - built mostly for development purposes, it stores the data only in memory. Use this for development and keep in mind that restarting the JVM will lose all data.

