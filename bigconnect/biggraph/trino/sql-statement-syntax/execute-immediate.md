# EXECUTE IMMEDIATE

### Synopsis[#](https://trino.io/docs/current/sql/execute-immediate.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
EXECUTE IMMEDIATE `statement` [ USING parameter1 [ , parameter2, ... ] ]
```

### Description[#](https://trino.io/docs/current/sql/execute-immediate.html#description) <a href="#description" id="description"></a>

Executes a statement without the need to prepare or deallocate the statement. Parameter values are defined in the `USING` clause.

### Examples[#](https://trino.io/docs/current/sql/execute-immediate.html#examples) <a href="#examples" id="examples"></a>

Execute a query with no parameters:

```
EXECUTE IMMEDIATE
'SELECT name FROM nation';
```



Execute a query with two parameters:

```
EXECUTE IMMEDIATE
'SELECT name FROM nation WHERE regionkey = ? and nationkey < ?'
USING 1, 3;
```



This is equivalent to:

```
PREPARE statement_name FROM SELECT name FROM nation WHERE regionkey = ? and nationkey < ?
EXECUTE statement_name USING 1, 3
DEALLOCATE PREPARE statement_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

EXECUTE, PREPARE, DEALLOCATE PREPARE
