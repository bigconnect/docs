# CREATE CATALOG

### Synopsis[#](https://trino.io/docs/current/sql/create-catalog.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
CREATE CATALOG
catalog_name
USING connector_name
[ WITH ( property_name = expression [, ...] ) ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/create-catalog.html#description) <a href="#description" id="description"></a>

Create a new catalog using the specified connector.

The optional `WITH` clause is used to set properties on the newly created catalog. Property names can be double quoted, which is required if they contain special characters, like `-`. Refer to the connectors documentation to learn about all available properties. All property values must be varchars (single quoted), including numbers and boolean values.

The query fails in the following circumstances:

* A required property is missing.
* An invalid property is set, for example there is a typo in the property name, or a property name from a different connector was used.
* The value of the property is invalid, for example a numeric value is out of range, or a string value doesn’t match the required pattern.
* The value references an environmental variable that is not set on the coordinator node.

Warning

The complete `CREATE CATALOG` query is logged, and visible in the [Web UI](https://trino.io/docs/current/admin/web-interface.html). This includes any sensitive properties, like passwords and other credentials. See [Secrets](https://trino.io/docs/current/security/secrets.html).

Note

This command requires the [catalog management type](https://trino.io/docs/current/admin/properties-catalog.html) to be set to `dynamic`.

### Examples[#](https://trino.io/docs/current/sql/create-catalog.html#examples) <a href="#examples" id="examples"></a>

Create a new catalog called `tpch` using the [TPCH connector](https://trino.io/docs/current/connector/tpch.html):

```
CREATE CATALOG tpch USING tpch;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create a new catalog called `brain` using the [Memory connector](https://trino.io/docs/current/connector/memory.html):

```
CREATE CATALOG brain USING memory
WITH ("memory.max-data-per-node" = '128MB');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Notice that the connector property contains dashes (`-`) and needs to quoted using a double quote (`"`). The value `128MB` is quoted using single quotes, because it is a string literal.

Create a new catalog called `example` using the PostgreSQL connector:

```
CREATE CATALOG example USING postgresql
WITH (
  "connection-url" = 'jdbc:pg:localhost:5432',
  "connection-user" = '${ENV:POSTGRES_USER}',
  "connection-password" = '${ENV:POSTGRES_PASSWORD}',
  "case-insensitive-name-matching" = 'true'
);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

This example assumes that the `POSTGRES_USER` and `POSTGRES_PASSWORD` environmental variables are set as [secrets](https://trino.io/docs/current/security/secrets.html) on all nodes of the cluster.

### See also# <a href="#see-also" id="see-also"></a>

* DROP CATALOG
* Catalog management properties
