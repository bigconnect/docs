# BigConnect Base

BigGraph is a massively scalable graph store designed for holding and querying vast amounts of raw data. It can store objects and links with multi-valued properties and raw data, fast queries, aggregations, distributed data processing pipelines and super-strong security (row and property-level).

High-level features:

* Massively distributed Big Data store based on Hadoop, Accumulo, Zookeeper
* Ridiculously fast data ingestion (25k objects / sec. on average hardware)
* Dynamic data model (concepts, relations, properties, tables), multivalued properties and property metadata support
* Authorizations and visibilities for rows and columns
* Indexing for fast search: full-text, fuzzy, spatial, aggregations
* Async distributed processing pipeline for applying custom logic on import/update/delete operationg
* Full Cypher query language support
* Extensible and modular
* SQL language support using [BigConnect Trino](trino/) to allow rich reporting on graph objects
* Integrated with [BigConnect Data Pipelines](../pipelines/pipelines-studio/) for data ingestion and manipulation

{% content-ref url="biggraph-architecture-and-concepts.md" %}
[biggraph-architecture-and-concepts.md](biggraph-architecture-and-concepts.md)
{% endcontent-ref %}



