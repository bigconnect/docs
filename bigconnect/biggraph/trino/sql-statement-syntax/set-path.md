# SET PATH

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SET PATH path-element[, ...]
```



### Description <a href="#description" id="description"></a>

Define a collection of paths to functions or table functions in specific catalogs and schemas for the current session.

Each path-element uses a period-separated syntax to specify the catalog name and schema location `<catalog>.<schema>` of the function, or only the schema location `<schema>` in the current catalog. The current catalog is set with [USE](https://trino.io/docs/current/sql/use.html), or as part of a client tool connection. Catalog and schema must exist.

### Examples <a href="#examples" id="examples"></a>

The following example sets a path to access functions in the `system` schema of the `example` catalog:

```
SET PATH example.system;
```



The catalog uses the PostgreSQL connector, and you can therefore use the [query table function](https://trino.io/docs/current/connector/postgresql.html#postgresql-query-function) directly, without the full catalog and schema qualifiers:

```
SELECT
  *
FROM
  TABLE(
    query(
      query => 'SELECT
        *
      FROM
        tpch.nation'
    )
  );
```



### See also <a href="#see-also" id="see-also"></a>

* USE
* SQL environment properties

\
