# EXECUTE

### Synopsis[#](https://trino.io/docs/current/sql/execute.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
EXECUTE statement_name [ USING parameter1 [ , parameter2, ... ] ]
```

### Description[#](https://trino.io/docs/current/sql/execute.html#description) <a href="#description" id="description"></a>

Executes a prepared statement with the name `statement_name`. Parameter values are defined in the `USING` clause.

### Examples[#](https://trino.io/docs/current/sql/execute.html#examples) <a href="#examples" id="examples"></a>

Prepare and execute a query with no parameters:

```
PREPARE my_select1 FROM
SELECT name FROM nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
EXECUTE my_select1;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Prepare and execute a query with two parameters:

```
PREPARE my_select2 FROM
SELECT name FROM nation WHERE regionkey = ? and nationkey < ?;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
EXECUTE my_select2 USING 1, 3;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

This is equivalent to:

```
SELECT name FROM nation WHERE regionkey = 1 AND nationkey < 3;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

PREPARE, DEALLOCATE PREPARE, EXECUTE IMMEDIATE
