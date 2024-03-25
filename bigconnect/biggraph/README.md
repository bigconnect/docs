# BigConnect Core

BigConnect Core is a massively scalable Data Lake designed for holding and querying vast amounts of raw data. It can store objects and links with multi-valued properties and raw data, fast queries, aggregations, distributed data processing pipelines and super-strong security (row and property-level).

High-level features:

* Massively distributed Big Data store based on [Hadoop](https://hadoop.apache.org/docs/stable/)
* Distributed OLTP database for high-performance computing with in-memory speed built on [Apache Ignite](https://ignite.apache.org/)
* Fully containerized Docker architecture for fast horizontal and vertical scalability in Kubernetes or Docker Swarm clusters
* Ridiculously fast data ingestion (25k objects / sec. on average hardware)
* Dynamic data model (concepts, relations, properties, tables), multivalued properties and property metadata support
* Authorizations and visibilities for rows and columns
* Indexing for fast search: full-text, fuzzy, spatial, aggregations
* Powerful streaming engine for data enrichment, events and notifications
* Async distributed processing pipeline for applying custom logic on import/update/delete operationg
* Full [Cypher ](cypher/)query language support
* Extensible and modular
* SQL language support using [Trino ](trino/)to allow rich reporting on graph objects

{% content-ref url="biggraph-architecture-and-concepts.md" %}
[biggraph-architecture-and-concepts.md](biggraph-architecture-and-concepts.md)
{% endcontent-ref %}



