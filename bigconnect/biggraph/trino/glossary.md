# Glossary

The glossary contains a list of key Trino terms and definitions.

Catalog

Catalogs define and name a configuration for connecting to a data source, allowing users to query the connected data. Each catalog’s configuration specifies a [connector](https://trino.io/docs/current/glossary.html#glossconnector) to define which data source the catalog connects to. For more information about catalogs, see [Catalog](https://trino.io/docs/current/overview/concepts.html#trino-concept-catalog).

Certificate

A public key [certificate](https://wikipedia.org/wiki/Public\_key\_certificate) issued by a [CA](https://trino.io/docs/current/glossary.html#glossca), sometimes abbreviated as cert, that verifies the ownership of a server’s private keys. Certificate format is specified in the [X.509](https://wikipedia.org/wiki/X.509) standard.

Certificate Authority (CA)

A trusted organization that signs and issues certificates. Its signatures can be used to verify the validity of [certificates](https://trino.io/docs/current/glossary.html#glosscert).

Cluster

A Trino cluster provides the resources to run queries against numerous data sources. Clusters define the number of nodes, the configuration for the JVM runtime, configured data sources, and others aspects. For more information, see [Cluster](https://trino.io/docs/current/overview/concepts.html#trino-concept-cluster).

Connector

Translates data from a data source into Trino schemas, tables, columns, rows, and data types. A [connector](https://trino.io/docs/current/connector.html) is specific to a data source, and is used in [catalog](https://trino.io/docs/current/glossary.html#glosscatalog) configurations to define what data source the catalog connects to. A connector is one of many types of [plugins](https://trino.io/docs/current/glossary.html#glossplugin)

Container

A lightweight virtual package of software that contains libraries, binaries, code, configuration files, and other dependencies needed to deploy an application. A running container does not include an operating system, instead using the operating system of the host machine. To learn more, read read about [containers](https://kubernetes.io/docs/concepts/containers/) in the Kubernetes documentation.

Data source

A system from which data is retrieved - for example, PostgreSQL or Iceberg on S3 data. In Trino, users query data sources with [catalogs](https://trino.io/docs/current/glossary.html#glosscatalog) that connect to each source. See [Data sources](https://trino.io/docs/current/overview/concepts.html#trino-concept-data-sources) for more information.

Data virtualization

[Data virtualization](https://wikipedia.org/wiki/Data\_virtualization) is a method of abstracting an interaction with multiple [heterogeneous data sources](https://trino.io/docs/current/glossary.html#glossdatasource), without needing to know the distributed nature of the data, its format, or any other technical details involved in presenting the data.

gzip

[gzip](https://wikipedia.org/wiki/Gzip) is a compression format and software that compresses and decompresses files. This format is used several ways in Trino, including deployment and compressing files in [object storage](https://trino.io/docs/current/glossary.html#glossobjectstorage). The most common extension for gzip-compressed files is `.gz`.

HDFS

[Hadoop Distributed Filesystem (HDFS)](https://wikipedia.org/wiki/Apache\_Hadoop#HDFS) is a scalable [open source](https://trino.io/docs/current/glossary.html#glossopensource) filesystem that was one of the earliest distributed big data systems created to store large amounts of data for the [Hadoop ecosystem](https://wikipedia.org/wiki/Apache\_Hadoop).

Java KeyStore (JKS)

The system of public key cryptography supported as one part of the Java security APIs. The legacy JKS system recognizes keys and [certificates](https://trino.io/docs/current/glossary.html#glosscert) stored in _keystore_ files, typically with the `.jks` extension, and by default relies on a system-level list of [CAs](https://trino.io/docs/current/glossary.html#glossca) in _truststore_ files installed as part of the current Java installation.

Key

A cryptographic key specified as a pair of public and private strings generally used in the context of [TLS](https://trino.io/docs/current/glossary.html#glosstls) to secure public network traffic.

Load Balancer (LB)

Software or a hardware device that sits on a network edge and accepts network connections on behalf of servers behind that wall, distributing traffic across network and server infrastructure to balance the load on networked services.

Object storage

[Object storage](https://en.wikipedia.org/wiki/Object\_storage) is a file storage mechanism. Examples of compatible object stores include the following:

* [Amazon S3](https://aws.amazon.com/s3)
* [Google Cloud Storage](https://cloud.google.com/storage)
* [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs)
* [MinIO](https://min.io/) and other S3-compatible stores
* [HDFS](https://trino.io/docs/current/glossary.html#glosshdfs)

Open-source

Typically refers to [open-source software](https://wikipedia.org/wiki/Open-source\_software). which is software that has the source code made available for others to see, use, and contribute to. Allowed usage varies depending on the license that the software is licensed under. Trino is licensed under the [Apache license](https://wikipedia.org/wiki/Apache\_License), and is therefore maintained by a community of contributors from all across the globe.

PEM file format

A format for storing and sending cryptographic keys and certificates. PEM format can contain both a key and its certificate, plus the chain of certificates from authorities back to the root [CA](https://trino.io/docs/current/glossary.html#glossca), or back to a CA vendor’s intermediate CA.

PKCS #12

A binary archive used to store keys and certificates or certificate chains that validate a key. [PKCS #12](https://wikipedia.org/wiki/PKCS\_12) files have `.p12` or `.pfx` extensions. This format is a less popular alternative to [PEM](https://trino.io/docs/current/glossary.html#glosspem).

Plugin

A bundle of code implementing the Trino [Service Provider Interface (SPI)](https://trino.io/docs/current/develop/spi-overview.html) that is used to add new [connectors](https://trino.io/docs/current/glossary.html#glossconnector), [data types](https://trino.io/docs/current/develop/types.html), [Functions and operators](https://trino.io/docs/current/functions.html), [access control implementations](https://trino.io/docs/current/develop/system-access-control.html), and other features of Trino.

Presto and PrestoSQL

The old name for Trino. To learn more about the name change to Trino, read [the history](https://wikipedia.org/wiki/Trino\_\(SQL\_query\_engine\)#History).

Query federation

A type of [data virtualization](https://trino.io/docs/current/glossary.html#glossdatavirtualization) that provides a common access point and data model across two or more heterogeneous data sources. A popular data model used by many query federation engines is translating different data sources to [SQL](https://trino.io/docs/current/glossary.html#glosssql) tables.

Secure Sockets Layer (SSL)

Now superseded by [TLS](https://trino.io/docs/current/glossary.html#glosstls), but still recognized as the term for what TLS does.

Structured Query Language (SQL)

The standard language used with relational databases. For more information, see [SQL](https://trino.io/docs/current/language.html).

Tarball

A common abbreviation for [TAR file](https://wikipedia.org/wiki/Tar\_\(computing\)), which is a common software distribution mechanism. This file format is a collection of multiple files distributed as a single file, commonly compressed using [gzip](https://trino.io/docs/current/glossary.html#glossgzip) compression.

Transport Layer Security (TLS)

[TLS](https://wikipedia.org/wiki/Transport\_Layer\_Security) is a security protocol designed to provide secure communications over a network. It is the successor to [SSL](https://trino.io/docs/current/glossary.html#glossssl), and used in many applications like HTTPS, email, and Trino. These security topics use the term TLS to refer to both TLS and SSL.
