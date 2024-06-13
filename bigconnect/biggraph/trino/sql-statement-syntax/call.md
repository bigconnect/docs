# CALL

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
CALL procedure_name ( [ name => ] expression [, ...] )
```



### Description <a href="#description" id="description"></a>

Call a procedure.

Procedures can be provided by connectors to perform data manipulation or administrative tasks. For example, the System connector defines a procedure for killing a running query.

Some connectors, such as the PostgreSQL connector, are for systems that have their own stored procedures. These stored procedures are separate from the connector-defined procedures discussed here and thus are not directly callable via `CALL`.

See connector documentation for details on available procedures.

### Examples <a href="#examples" id="examples"></a>

Call a procedure using positional arguments:

```
CALL test(123, 'apple');
```



Call a procedure using named arguments:

```
CALL test(name => 'apple', id => 123);
```



Call a procedure using a fully qualified name:

```
CALL catalog.schema.test();
```
