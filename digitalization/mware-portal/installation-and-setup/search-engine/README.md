# Search Engine

A search engine is a critical component of your portal installation. Creating an Example Cluster can get you started with the installation, but this guide demonstrates setting up a **production** environment.

When you start the portal, a built-in Elasticsearch server (sidecar) starts simultaneously. This default search engine provides search capabilities as a convenience for testing, but it isn’t supported for use in production. Getting Started with Elasticsearch describes production-level Elasticsearch setup. Using the Sidecar or Embedded Elasticsearch describes the default Elasticsearch server (sidecar in 7.3 and embedded in 7.2) features and limitations.

NOTE

The portal’s [Solr](http://lucene.apache.org/solr) support will receive one more update (to support Solr 8) before being deprecated. Though it can still be used, Solr is not bundled with the portal and must be connected remotely, even for development and testing. To use Solr, see Installing Solr.

### Java Requirements <a href="#java-requirements" id="java-requirements"></a>

* Search engines require the environment variable `JAVA_HOME` or `ES_JAVA_HOME` ([Elasticsearch 8](https://www.elastic.co/guide/en/elasticsearch/reference/8.8/install-elasticsearch.html#jvm-version)). Set it on your search engine host.
*   If you’re using version 7.2, Elasticsearch and the portal must use the same Java version and distribution. Consult the [Elasticsearch compatibility matrix](https://www.elastic.co/support/matrix#matrix\_jvm) and the portal’s Search Engine Compatibility Matrix to learn more about supported JDK distributions and versions. You can specify this in Elasticsearch:

    ```properties
    [Elasticsearch Home]/bin/elasticsearch.in.sh`: `JAVA_HOME=/path/to/java`
    ```

The Java version and distribution requirement doesn’t apply to version 7.3+ because the Elasticsearch 7 connector communicates via HTTP; there’s no JVM level serialization. See [Elastic’s High-Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/7.x/java-rest-high.html) documentation for details.

The same requirement doesn’t apply to Solr either because the Solr connector also communicates via HTTP.

### Clustering the Search Engine <a href="#clustering-the-search-engine" id="clustering-the-search-engine"></a>

A production environment’s search engine should be clustered for load management and optimal performance. Both Elasticsearch and Solr can be configured on multiple nodes in the remote environment.

* To configure a remote Elasticsearch server or cluster, see Getting Started with Elasticsearch.
* To configure a remote Solr server or cluster, see Installing Solr.

\
