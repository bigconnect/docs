# BigGraph Developer Guide

BigGraph is mainly a big library providing services to handle most of the aspects of data storage, search, data model, collaboration, cypher queries, data worker runners, long running process runners and much more.

Secondly, BigGraph can also run by itself, featuring a Bolt Server to execute Cypher queries, the [CypherLab](cypherlab/) and runners for Data Worker plugins, Long Running Process plugins and External Worker plugins. See the [Architecture and Concepts](biggraph-architecture-and-concepts.md#the-graph-engine) page for details regarding what these mean.

{% hint style="info" %}
BigGraph is NOT a transactional system. It's main purpose is to ingest, store and search through large amounts of data in a fast way. It does not have the notion of a transaction, commit or rollback.
{% endhint %}

## Direct API



## Cypher

[Cypher](https://en.wikipedia.org/wiki/Cypher_%28query_language%29) queries are processed by the Bolt server using a fast binary protocol. Clients connect to it using a specific port \(10242 by default\) and execute queries, in a similar way to a JDBC driver. Driver libraries are available for Java, PHP and NodeJS.

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

The recommended way to use Cypher is to start from a single node \(eg. match by id\) and expand the graph starting from that node. 
{% endhint %}

## SQL

SQL queries are run against the datastore using [Trino](../trino/). They are usually faster than Cypher queries for specific tasks because Trino pushes down WHERE filters to Elasticsearch shards. The more Trino workers and Elasticsearch shards and replicas you have, the faster your queries will be.

Normally all SQL statements supported by Trino should work. 

{% hint style="warning" %}
Inserts and deletes are not designed for large batches. If you plan to insert/delete data through SQL, you will find it to be slow. Please consider the Direct API for batch operations.
{% endhint %}





