# VALUES

### Synopsis[#](https://trino.io/docs/current/sql/values.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
VALUES row [, ...]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

where `row` is a single expression or

```
( column_expression [, ...] )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/values.html#description) <a href="#description" id="description"></a>

Defines a literal inline table.

`VALUES` can be used anywhere a query can be used (e.g., the `FROM` clause of a SELECT, an INSERT, or even at the top level). `VALUES` creates an anonymous table without column names, but the table and columns can be named using an `AS` clause with column aliases.

### Examples[#](https://trino.io/docs/current/sql/values.html#examples) <a href="#examples" id="examples"></a>

Return a table with one column and three rows:

```
VALUES 1, 2, 3
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Return a table with two columns and three rows:

```
VALUES
    (1, 'a'),
    (2, 'b'),
    (3, 'c')
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Return table with column `id` and `name`:

```
SELECT * FROM (
    VALUES
        (1, 'a'),
        (2, 'b'),
        (3, 'c')
) AS t (id, name)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create a new table with column `id` and `name`:

```
CREATE TABLE example AS
SELECT * FROM (
    VALUES
        (1, 'a'),
        (2, 'b'),
        (3, 'c')
) AS t (id, name)
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

INSERT, SELECT
