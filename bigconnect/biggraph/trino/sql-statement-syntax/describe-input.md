# DESCRIBE INPUT

### Synopsis[#](https://trino.io/docs/current/sql/describe-input.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DESCRIBE INPUT statement_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/describe-input.html#description) <a href="#description" id="description"></a>

Lists the input parameters of a prepared statement along with the position and type of each parameter. Parameter types that cannot be determined will appear as `unknown`.

### Examples[#](https://trino.io/docs/current/sql/describe-input.html#examples) <a href="#examples" id="examples"></a>

Prepare and describe a query with three parameters:

```
PREPARE my_select1 FROM
SELECT ? FROM nation WHERE regionkey = ? AND name < ?;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
DESCRIBE INPUT my_select1;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 Position | Type
--------------------
        0 | unknown
        1 | bigint
        2 | varchar
(3 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Prepare and describe a query with no parameters:

```
PREPARE my_select2 FROM
SELECT * FROM nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
DESCRIBE INPUT my_select2;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 Position | Type
-----------------
(0 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

PREPARE
