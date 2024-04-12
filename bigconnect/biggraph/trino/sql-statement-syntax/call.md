# CALL

### Synopsis[#](https://trino.io/docs/current/sql/call.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
CALL procedure_name ( [ name => ] expression [, ...] )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/call.html#description) <a href="#description" id="description"></a>

Call a procedure.

Procedures can be provided by connectors to perform data manipulation or administrative tasks. For example, the [System connector](https://trino.io/docs/current/connector/system.html) defines a procedure for killing a running query.

Some connectors, such as the [PostgreSQL connector](https://trino.io/docs/current/connector/postgresql.html), are for systems that have their own stored procedures. These stored procedures are separate from the connector-defined procedures discussed here and thus are not directly callable via `CALL`.

See connector documentation for details on available procedures.

### Examples[#](https://trino.io/docs/current/sql/call.html#examples) <a href="#examples" id="examples"></a>

Call a procedure using positional arguments:

```
CALL test(123, 'apple');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Call a procedure using named arguments:

```
CALL test(name => 'apple', id => 123);
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Call a procedure using a fully qualified name:

```
CALL catalog.schema.test();
```
