# DESCRIBE OUTPUT

### Synopsis[#](https://trino.io/docs/current/sql/describe-output.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DESCRIBE OUTPUT statement_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/describe-output.html#description) <a href="#description" id="description"></a>

List the output columns of a prepared statement, including the column name (or alias), catalog, schema, table, type, type size in bytes, and a boolean indicating if the column is aliased.

### Examples[#](https://trino.io/docs/current/sql/describe-output.html#examples) <a href="#examples" id="examples"></a>

Prepare and describe a query with four output columns:

```
PREPARE my_select1 FROM
SELECT * FROM nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
DESCRIBE OUTPUT my_select1;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 Column Name | Catalog | Schema | Table  |  Type   | Type Size | Aliased
-------------+---------+--------+--------+---------+-----------+---------
 nationkey   | tpch    | sf1    | nation | bigint  |         8 | false
 name        | tpch    | sf1    | nation | varchar |         0 | false
 regionkey   | tpch    | sf1    | nation | bigint  |         8 | false
 comment     | tpch    | sf1    | nation | varchar |         0 | false
(4 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Prepare and describe a query whose output columns are expressions:

```
PREPARE my_select2 FROM
SELECT count(*) as my_count, 1+2 FROM nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
DESCRIBE OUTPUT my_select2;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 Column Name | Catalog | Schema | Table |  Type  | Type Size | Aliased
-------------+---------+--------+-------+--------+-----------+---------
 my_count    |         |        |       | bigint |         8 | true
 _col1       |         |        |       | bigint |         8 | false
(2 rows)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Prepare and describe a row count query:

```
PREPARE my_create FROM
CREATE TABLE foo AS SELECT * FROM nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
DESCRIBE OUTPUT my_create;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

```
 Column Name | Catalog | Schema | Table |  Type  | Type Size | Aliased
-------------+---------+--------+-------+--------+-----------+---------
 rows        |         |        |       | bigint |         8 | false
(1 row)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/describe-output.html#see-also) <a href="#see-also" id="see-also"></a>

PREPARE
