# RocksDB

This is the "small-grade" storage type designed for local, desktop use. It should have to problem to store more than 1 million vertices and edges but it will start to get laggy once you pass that boundary. It cannot be distributed due to the nature of [RocksDB](http://rocksdb.org/).

