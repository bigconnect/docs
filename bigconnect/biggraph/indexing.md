# Data indexing

BigConnect Core needs a search index to run queries and aggregations against properties of vertices and edges. There are two implementations for the search index:

* [Elasticsearch5SearchIndex](https://github.com/bigconnect/bigconnect/blob/master/search/index/src/main/java/com/mware/ge/elasticsearch5/Elasticsearch5SearchIndex.java) - this is the "production-grade" implementation that uses Elasticsearch 7
* [DefaultSearchIndex](https://github.com/bigconnect/bigconnect/blob/master/core/src/main/java/com/mware/ge/search/DefaultSearchIndex.java) - built mostly for development purposes, it stores the data only in memory. Use this for development and keep in mind that restarting the JVM will lose all data.
