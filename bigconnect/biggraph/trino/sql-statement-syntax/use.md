# USE

### Synopsis[#](https://trino.io/docs/current/sql/use.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
USE catalog.schema
USE schema
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/use.html#description) <a href="#description" id="description"></a>

Update the session to use the specified catalog and schema. If a catalog is not specified, the schema is resolved relative to the current catalog.

### Examples[#](https://trino.io/docs/current/sql/use.html#examples) <a href="#examples" id="examples"></a>

```
USE hive.finance;
USE information_schema;
```
