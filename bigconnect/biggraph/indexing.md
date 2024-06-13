# Indexing

BigConnect Core needs a search index to run queries and aggregations against properties of vertices and edges. There are two implementations for the search index:

* [Elasticsearch5SearchIndex](https://github.com/bigconnect/bigconnect/blob/master/search/index/src/main/java/com/mware/ge/elasticsearch5/Elasticsearch5SearchIndex.java) - this is the "production-grade" implementation that uses Elasticsearch 7
* [DefaultSearchIndex](https://github.com/bigconnect/bigconnect/blob/master/core/src/main/java/com/mware/ge/search/DefaultSearchIndex.java) - built mostly for development purposes, it stores the data only in memory. Use this for development and keep in mind that restarting the JVM will lose all data.

The search index will index entities, relationships and properties which have been marked as searchable. See the [Data Model](https://docs.bigconnect.io/bigconnect/biggraph/data-model#concepts) page for more info about search properties. Upon indexing, these elements will be available for search.

BigConnect Core offers the following search capabilities:

* Full-Text searches
* Spatial searchs
* Wildcard Searches
* Fuzzy Searches
* Proximity Searches
* Range Searches
* Term Boosting
* Boolean Operators
* Term Grouping
* Field Grouping
* Casing and diacritics
* Sorting & Pagination
* Saved Searches

Besides searching, the index can also perform aggregations. BigConnect Core allows properties to be aggregated, mostly for providing search facets.

The following index aggregations are supported:

* Term
* Histogram
* GeoHash



