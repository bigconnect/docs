# DROP SCHEMA

### Synopsis[#](https://trino.io/docs/current/sql/drop-schema.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DROP SCHEMA [ IF EXISTS ] schema_name [ CASCADE | RESTRICT ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/drop-schema.html#description) <a href="#description" id="description"></a>

Drop an existing schema. The schema must be empty.

The optional `IF EXISTS` clause causes the error to be suppressed if the schema does not exist.

### Examples[#](https://trino.io/docs/current/sql/drop-schema.html#examples) <a href="#examples" id="examples"></a>

Drop the schema `web`:

```
DROP SCHEMA web
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop the schema `sales` if it exists:

```
DROP SCHEMA IF EXISTS sales
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop the schema `archive`, along with everything it contains:

```
DROP SCHEMA archive CASCADE
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop the schema `archive`, only if there are no objects contained in the schema:

```
DROP SCHEMA archive RESTRICT
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

ALTER SCHEMA, CREATE SCHEMA
