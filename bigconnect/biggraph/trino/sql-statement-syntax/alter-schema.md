# ALTER SCHEMA

### Synopsis[#](https://trino.io/docs/current/sql/alter-schema.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
ALTER SCHEMA name RENAME TO new_name
ALTER SCHEMA name SET AUTHORIZATION ( user | USER user | ROLE role )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/alter-schema.html#description) <a href="#description" id="description"></a>

Change the definition of an existing schema.

### Examples[#](https://trino.io/docs/current/sql/alter-schema.html#examples) <a href="#examples" id="examples"></a>

Rename schema `web` to `traffic`:

```
ALTER SCHEMA web RENAME TO traffic
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Change owner of schema `web` to user `alice`:

```
ALTER SCHEMA web SET AUTHORIZATION alice
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Allow everyone to drop schema and create tables in schema `web`:

```
ALTER SCHEMA web SET AUTHORIZATION ROLE PUBLIC
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See Also[#](https://trino.io/docs/current/sql/alter-schema.html#see-also) <a href="#see-also" id="see-also"></a>

[CREATE SCHEMA](https://trino.io/docs/current/sql/create-schema.html)
