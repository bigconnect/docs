# Cloud Data

BigConnect uses a cached memory filesystem for working with cloud data, named Alluxio.

The benefits of Alluxio include:

* **Memory-Speed I/O**: Alluxio can be used as a distributed shared caching service so that compute applications talking to Alluxio can transparently cache frequently accessed data, especially from remote locations, to provide in-memory I/O throughput. In addition, Alluxio’s tiered storage that can utilize memory, SSD or disk makes elastically scale data-driven applications cost effective.
* **Simplified Cloud and Object Storage Adoption**: Cloud and object storage systems use different semantics that have performance implications compared to traditional file systems. Common file system operations such as directory listing and renaming often incur significant performance overhead. When accessing data in cloud storage, applications have no node-level locality or cross-application caching. Deploying Alluxio with cloud or object storage mitigates these problems because data will be retrieved from Alluxio instead of the underlying cloud or object storage.
* **Simplified Data Management**: Alluxio provides a single point of access to multiple data sources. In addition to connecting data sources of different types, Alluxio also enables users to simultaneously connect to different versions of the same storage system, such as multiple versions of HDFS, without complex system configuration and management.
* **Easy Application Deployment**: Alluxio manages communication between applications and file or object storages, translating data access requests from applications into underlying storage interfaces. Alluxio is Hadoop compatible. Existing data analytics applications, such as Spark and MapReduce programs, can run on top of Alluxio without any code change.

Alluxio brings three key areas of innovation together to provide a unique set of capabilities.

* **Global Namespace**: Alluxio serves as a single point of access to multiple independent storage systems regardless of physical location. This provides a unified view of all data sources and a standard interface for applications. See [Namespace Management](https://docs.alluxio.io/os/user/stable/en/advanced/Namespace-Management.html) for more details.
* **Intelligent Multi-tiering Caching**: Alluxio clusters act as a read and write cache for data in connected storage systems. Configurable policies automatically optimize data placement for performance and reliability across both memory and disk \(SSD/HDD\). Caching is transparent to the user and uses buffering to maintain consistency with persistent storage. See [Alluxio Storage Management](https://docs.alluxio.io/os/user/stable/en/advanced/Alluxio-Storage-Management.html) for more details.
* **Server-Side API Translation**: Alluxio supports industry common APIs, such as HDFS API, S3 API, FUSE API, REST API. It transparently converts from a standard client-side interface to any storage interface. Alluxio manages communication between applications and file or object storage, eliminating the need for complex system configuration and management. File data can look like object data and vice versa.

