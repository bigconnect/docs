# Universal Data Model

We live in a dynamic world where things change at an incredible rate and having an information system that can adapt easily to changes is an absolute requirement to stay ahead.

Data is everywhere, in various structures, formats and sizes. A Data Lake needs to ingest it, store it and understand it, but this is not trivial. How to store structured an unstructured data in a way that is easy to join, query and understand? This is where BDL excels by introducing a Universal Data Model.

![BigConnect Data Lake Universal Data Model](../.gitbook/assets/image%20%286%29.png)

The core foundation of BDL is the Universal Data Model. It represents the way you store, correlate and query all information using logical structures: Concepts, Relations, Properties, Databases, Schemas, Tables and Table Columns.

Unstructured data is mapped to Concepts, Relations, Properties.

Structured data is mapped to Databases, Schemas, Tables and Table Columns.

OLAP structures are created by mapping everything to Dimensions and Measures. BDL uses a custom, optimized implementation of Apache Druid.

Typically, the data model is used to provide meaning to the information stored in BDL, either at ingestion time or later, during the lifecycle of the data.

The Data Model is dynamic. It can be defined in the beginning or any time later, to adapt to any data structure, type or meaning.

