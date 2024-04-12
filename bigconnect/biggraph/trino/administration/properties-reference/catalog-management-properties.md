# Catalog management properties

The following properties are used to configure catalog management with further controls for dynamic catalog management.

### `catalog.management`# <a href="#catalog-management" id="catalog-management"></a>

* **Type:** string
* **Allowed values:** `static`, `dynamic`
* **Default value:** `static`

When set to `static`, Trino reads catalog property files and configures available catalogs only on server startup. When set to `dynamic`, catalog configuration can also be managed using CREATE CATALOG and DROP CATALOG. New worker nodes joining the cluster receive the current catalog configuration from the coordinator node.

Warning

This feature is experimental only. Because of the security implications the syntax might change and be backward incompatible.

Warning

Some connectors are known not to release all resources when dropping a catalog that uses such connector. This includes all connectors that can read data from HDFS, S3, GCS, or Azure, which are Hive connector, Iceberg connector, Delta Lake connector, and Hudi connector.

Warning

The complete `CREATE CATALOG` query is logged, and visible in the Web UI. This includes any sensitive properties, like passwords and other credentials. See Secrets.

### `catalog.prune.update-interval`# <a href="#catalog-prune-update-interval" id="catalog-prune-update-interval"></a>

* **Type:** duration
* **Default value:** `5s`
* **Minimum value:** `1s`

Requires catalog.management to be set to `dynamic`. Interval for pruning dropped catalogs. Dropping a catalog does not interrupt any running queries that use it, but makes it unavailable to any new queries.

### `catalog.store` <a href="#catalog-store" id="catalog-store"></a>

* **Type:** string
* **Allowed values:** `file`, `memory`
* **Default value:** `file`

Requires catalog.management to be set to `dynamic`. When set to `file`, creating and dropping catalogs using the SQL commands adds and removes catalog property files on the coordinator node. Trino server process requires write access in the catalog configuration directory. Existing catalog files are also read on the coordinator startup. When set to `memory`, catalog configuration is only managed in memory, and any existing files are ignored on startup.

### `catalog.config-dir`# <a href="#catalog-config-dir" id="catalog-config-dir"></a>

* **Type:** string
* **Default value:** `etc/catalog/`

Requires catalog.management to be set to `static` or catalog.store to be set to `file`. The directory with catalog property files.

### `catalog.disabled-catalogs`# <a href="#catalog-disabled-catalogs" id="catalog-disabled-catalogs"></a>

* **Type:** string

Requires catalog.management to be set to `static` or catalog.store to be set to `file`. Comma-separated list of catalogs to ignore on startup.

### `catalog.read-only`# <a href="#catalog-read-only" id="catalog-read-only"></a>

* **Type:** string
* **Default value:** `false`

Requires catalog.store to be set to `file`. If true, existing catalog property files cannot be removed with `DROP CATALOG`, and no new catalog files can be written with identical names with `CREATE CATALOG`. As a result, a coordinator restart resets the known catalogs to the existing files only.
