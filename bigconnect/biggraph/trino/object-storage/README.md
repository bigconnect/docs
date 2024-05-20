# Object storage

Object storage systems are commonly used to create data lakes or data lake houses. These systems provide methods to store objects in a structured manner and means to access them, for example using an API over HTTP. The objects are files in various format including ORC, Parquet and others. Object storage systems are available as service from public cloud providers and others vendors, or can be self-hosted using commercial as well as open source offerings.

### Object storage connectors[#](https://trino.io/docs/current/object-storage.html#object-storage-connectors) <a href="#object-storage-connectors" id="object-storage-connectors"></a>

Trino accesses files directly on object storage and remote file system storage. The following connectors use this direct approach to read and write data files.

* Delta Lake connector
* Hive connector
* Hudi connector
* Iceberg connector

The connectors all support a variety of protocols and formats used on these object storage systems, and have separate requirements for metadata availability.

### Configuration[#](https://trino.io/docs/current/object-storage.html#configuration) <a href="#configuration" id="configuration"></a>

Use the following properties to control the support for different file systems in a catalog. Each catalog can only use one file system.

| Property                  | Description                                                                                                                  |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `fs.hadoop.enabled`       | Activate the legacy libraries and implementation based on the Hadoop ecosystem. Defaults to `true`.                          |
| `fs.native-azure.enabled` | Activate the native implementation for Azure Storage support, and deactivate all legacy support. Defaults to `false`.        |
| `fs.native-gcs.enabled`   | Activate the native implementation for Google Cloud Storage support, and deactivate all legacy support. Defaults to `false`. |
| `fs.native-s3.enabled`    | Activate the native implementation for S3 storage support, and deactivate all legacy support . Defaults to `false`.          |

### Native file system support[#](https://trino.io/docs/current/object-storage.html#native-file-system-support) <a href="#native-file-system-support" id="native-file-system-support"></a>

Trino includes optimized implementations to access the following systems, and compatible replacements:

* Azure Storage file system support
* Google Cloud Storage file system support
* S3 file system support

The native support is available in all four connectors, but must be activated for use.

### Legacy file system support[#](https://trino.io/docs/current/object-storage.html#legacy-file-system-support) <a href="#legacy-file-system-support" id="legacy-file-system-support"></a>

The default behavior uses legacy libraries that originate from the Hadoop ecosystem. All four connectors can use the related `hive.*` properties for access to the different object storage system. Documentation is available with the Hive connector and relevant dedicated pages:

* Hive connector
* Legacy Azure Storage support
* Legacy Google Cloud Storage support
* Legacy IBM COS support
* Legacy S3 support

### Other object storage support[#](https://trino.io/docs/current/object-storage.html#other-object-storage-support) <a href="#other-object-storage-support" id="other-object-storage-support"></a>

Trino also provides the following additional support and features for object storage:

* File system cache
* Metastores
* Object storage file formats
