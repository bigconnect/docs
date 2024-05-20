# ALTER MATERIALIZED VIEW

### Synopsis[#](https://trino.io/docs/current/sql/alter-materialized-view.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
ALTER MATERIALIZED VIEW [ IF EXISTS ] name RENAME TO new_name
ALTER MATERIALIZED VIEW name SET PROPERTIES property_name = expression [, ...]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/alter-materialized-view.html#description) <a href="#description" id="description"></a>

Change the name of an existing materialized view.

The optional `IF EXISTS` clause causes the error to be suppressed if the materialized view does not exist. The error is not suppressed if the materialized view does not exist, but a table or view with the given name exists.

#### SET PROPERTIES[#](https://trino.io/docs/current/sql/alter-materialized-view.html#set-properties) <a href="#set-properties" id="set-properties"></a>

The `ALTER MATERIALIZED VIEW SET PROPERTIES` statement followed by some number of `property_name` and `expression` pairs applies the specified properties and values to a materialized view. Ommitting an already-set property from this statement leaves that property unchanged in the materialized view.

A property in a `SET PROPERTIES` statement can be set to `DEFAULT`, which reverts its value back to the default in that materialized view.

Support for `ALTER MATERIALIZED VIEW SET PROPERTIES` varies between connectors. Refer to the connector documentation for more details.

### Examples[#](https://trino.io/docs/current/sql/alter-materialized-view.html#examples) <a href="#examples" id="examples"></a>

Rename materialized view `people` to `users` in the current schema:

```
ALTER MATERIALIZED VIEW people RENAME TO users;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Rename materialized view `people` to `users`, if materialized view `people` exists in the current catalog and schema:

```
ALTER MATERIALIZED VIEW IF EXISTS people RENAME TO users;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Set view properties (`x = y`) in materialized view `people`:

```
ALTER MATERIALIZED VIEW people SET PROPERTIES x = 'y';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Set multiple view properties (`foo = 123` and `foo bar = 456`) in materialized view `people`:

```
ALTER MATERIALIZED VIEW people SET PROPERTIES foo = 123, "foo bar" = 456;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Set view property `x` to its default value in materialized view `people`:

```
ALTER MATERIALIZED VIEW people SET PROPERTIES x = DEFAULT;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

* CREATE MATERIALIZED VIEW
* REFRESH MATERIALIZED VIEW
* DROP MATERIALIZED VIEW

\
