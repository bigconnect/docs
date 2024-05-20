# DROP CATALOG

### Synopsis[#](https://trino.io/docs/current/sql/drop-catalog.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DROP CATALOG catalog_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/drop-catalog.html#description) <a href="#description" id="description"></a>

Drops an existing catalog. Dropping a catalog does not interrupt any running queries that use it, but makes it unavailable to any new queries.

Warning

Some connectors are known not to release all resources when dropping a catalog that uses such connector. This includes all connectors that can read data from HDFS, S3, GCS, or Azure, which are Hive connector, Iceberg connector, Delta Lake connector, and Hudi connector.

Note

This command requires the catalog management type to be set to `dynamic`.

### Examples[#](https://trino.io/docs/current/sql/drop-catalog.html#examples) <a href="#examples" id="examples"></a>

Drop the catalog `example`:

```
DROP CATALOG example;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

* CREATE CATALOG
* Catalog management properties
