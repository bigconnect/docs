# Developer Guide

BigGraph is mainly a big library providing services to handle most of the aspects of data storage, search, data model, collaboration, cypher queries, data worker runners, long running process runners and much more.

Secondly, BigGraph can also run by itself, featuring a Bolt Server to execute Cypher queries, the [CypherLab](cypher/cypherlab/) and runners for Data Worker plugins, Long Running Process plugins and External Worker plugins. See the [Architecture and Concepts](biggraph-architecture-and-concepts.md#the-graph-engine) page for details regarding what these mean.

{% hint style="info" %}
BigGraph is NOT a transactional system. It's main purpose is to ingest, store and search through large amounts of data in a fast way. It does not have the notion of a transaction, commit or rollback.
{% endhint %}

## Direct API

You can use the Direct API with RocksDB and Elasticsearch by including the following Maven dependencies (available in Maven Central).&#x20;

```markup
<dependency>
    <groupId>io.bigconnect</groupId>
    <artifactId>core</artifactId>
    <version>4.2.0</version>
</dependency>
<dependency>
    <groupId>io.bigconnect</groupId>
    <artifactId>storage-rocksdb</artifactId>
    <version>4.2.0</version>
</dependency>
<dependency>
    <groupId>io.bigconnect</groupId>
    <artifactId>search-index</artifactId>
    <version>4.2.0</version>
</dependency>
```

The following example creates [Graph](https://github.com/bigconnect/bigconnect/blob/master/core/src/main/java/com/mware/ge/Graph.java) instance and performs some basic operations.

```java
import com.mware.ge.*;
import com.mware.core.model.schema.SchemaConstants;
import static com.mware.ge.rocksdb.RocksDBGraphConfiguration.*;
import static com.mware.ge.elasticsearch5.DefaultIndexSelectionStrategy.*;
import static com.mware.ge.values.storable.Values.*;

/* Create a Graph instance (connect to RocksDB and Elasticsearch) */
Map configMap = new HashMap();
configMap.put(STREAMING_PROPERTY_VALUE_DATA_FOLDER, "/tmp/biggraph/spv");
configMap.put(DATA_PATH, dataPath.toString(), "/tmp/biggraph/data/");
configMap.put(WAL_PATH, walPath.toString(), "/tmp/biggraph/data/wal");
configMap.put(SEARCH_INDEX_PROP_PREFIX, Elasticsearch5SearchIndex.class.getName());
configMap.put(SEARCH_INDEX_PROP_PREFIX + "." + CONFIG_INDEX_NAME, "biggraph");
configMap.put(SEARCH_INDEX_PROP_PREFIX + "." + CONFIG_INDEX_NAME, "biggraph");
configMap.put(SEARCH_INDEX_PROP_PREFIX + "." + CLUSTER_NAME, "biggraph");
configMap.put(SEARCH_INDEX_PROP_PREFIX + "." + ES_LOCATIONS, "localhost");
Graph graph = RocksDBGraph.create(configMap);

/* Create a vertex with a property */
Authorizations authorizations = graph.createAuthorizations();
Vertex v1 = graph.prepareVertex(Visibility.EMPTY, SchemaConstants.CONCEPT_TYPE_THING)
     .setProperty("prop1", stringValue("value1"), null, Visibility.EMPTY)
     .save(authorizations);
Vertex v2 = graph.prepareVertex(Visibility.EMPTY, SchemaConstants.CONCEPT_TYPE_THING)
     .setProperty("prop1", stringValue("value2"), null, Visibility.EMPTY)
     .save(authorizations);     
graph.flush();

/* Create an edge between the two vertices and set a property on the edge */
Edge edge = graph.prepareEdge(v1, v2, "v1Tov2", Visibility.EMPTY)
                .setProperty("prop2", stringValue("value3"), Visibility.EMPTY)
                .save(authorizations);
getGraph().flush();

/* search for a vertex by on of its properties */
Query query = graph.query(AUTHORIZATIONS_A).has("prop1", stringValue("value1"));
try (QueryResultsIterable<Vertex> results = query.vertices()) {
   long totalHits = results.getTotalHits();
   results.forEach(v -> System.out.print(v.getId()));
} catch (IOException ex) {
  ex.printStackTrace();
}

                                
/* delete an element */
graph.deleteElement(v1, authorizations);

```

## Cypher

[Cypher](https://en.wikipedia.org/wiki/Cypher\_\(query\_language\)) queries are processed by the Bolt server using a fast binary protocol. Clients connect to it using a specific port (10242 by default) and execute queries, in a similar way to a JDBC driver. Driver libraries are available for Java, PHP and NodeJS.

Below is a simple example on how to connect to a Bolt server and execute a simple Cypher query.

```java
Config config = Config.builder()
                .withoutEncryption()
                .withTrustStrategy(Config.TrustStrategy.trustAllCertificates())
                .withLogging(slf4j())
                .build();

Driver driver = BigConnect.driver(URI.create("bolt://localhost:10242"), AuthTokens.basic( "admin", "admin"), config);
Session session = driver.session();
StatementResult r = session.run("match (d:document) return d.title");
while (r.hasNext()) {
  System.out.println(r.next().asMap());
}

session.close();
driver.close();
```

As you can see, it's quite similar with running SQL queries through a JDBC driver.

{% hint style="warning" %}
Careful planning of Cypher queries is essential, because they can seriosly damage the performance of your big graph. Avoid queries that make full scans over the entire database, that update the entire database or which need to process a large number of records.

BigGraph is a big data system and does not benefit from optimizations like local page caches, fast disk reads etc. It's a highly distributed system and scanning over huge datasets will require a significant amount of time.

The recommended way to use Cypher is to start from a single node (eg. match by id) and expand the graph starting from that node.&#x20;
{% endhint %}

## SQL

SQL queries are run against the datastore using [Trino](broken-reference). They are usually faster than Cypher queries for specific tasks because Trino pushes down WHERE filters to Elasticsearch shards. The more Trino workers and Elasticsearch shards and replicas you have, the faster your queries will be.

Normally all SQL statements supported by Trino should work.&#x20;

{% hint style="warning" %}
Inserts and deletes are not designed for large batches. If you plan to insert/delete data through SQL, you will find it to be slow. Please consider the Direct API for batch operations.
{% endhint %}



