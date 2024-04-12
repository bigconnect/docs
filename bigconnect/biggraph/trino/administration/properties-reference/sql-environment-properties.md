# SQL environment properties

SQL environment properties allow you to globally configure parameters relevant to all SQL queries and the context they are processed in.

### `sql.forced-session-time-zone`[#](https://trino.io/docs/current/admin/properties-sql-environment.html#sql-forced-session-time-zone) <a href="#sql-forced-session-time-zone" id="sql-forced-session-time-zone"></a>

* **Type:** string

Force the time zone for any query processing to the configured value, and therefore override the time zone of the client. The time zone must be specified as a string such as `UTC` or [other valid values](https://trino.io/docs/current/language/types.html#timestamp-p-with-time-zone-data-type).

### `sql.default-catalog`# <a href="#sql-default-catalog" id="sql-default-catalog"></a>

* **Type:** string

Set the default catalog for all clients. Any default catalog configuration provided by a client overrides this default.

### `sql.default-schema`# <a href="#sql-default-schema" id="sql-default-schema"></a>

* **Type:** string

Set the default schema for all clients. Must be set to a schema name that is valid for the default catalog. Any default schema configuration provided by a client overrides this default.

### `sql.default-function-catalog`# <a href="#sql-default-function-catalog" id="sql-default-function-catalog"></a>

* **Type:** string

Set the default catalog for SQL routine storage for all clients. The connector used in the catalog must support Routine management. Any usage of a fully qualified name for a routine overrides this default.

The default catalog and schema for SQL routine storage must be configured together, and the resulting entry must be set as part of the path. For example, the following example section for Config properties uses the `functions` schema in the `brain` catalog for routine storage, and adds it as the only entry on the path:

```
sql.default-function-catalog=brain
sql.default-function-schema=default
sql.path=brain.default
```

### `sql.default-function-schema`[#](https://trino.io/docs/current/admin/properties-sql-environment.html#sql-default-function-schema) <a href="#sql-default-function-schema" id="sql-default-function-schema"></a>

* **Type:** string

Set the default schema for SQL routine storage for all clients. Must be set to a schema name that is valid for the default function catalog. Any usage of a fully qualified name for a routine overrides this default.

### `sql.path`# <a href="#sql-path" id="sql-path"></a>

* **Type:** string

Define the default collection of paths to functions or table functions in specific catalogs and schemas. Paths are specified as `catalog_name.schema_name`. Multiple paths must be separated by commas. Find more details about the path in SET PATH.
