# UPDATE

### Synopsis[#](https://trino.io/docs/current/sql/update.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
UPDATE table_name SET [ ( column = expression [, ... ] ) ] [ WHERE condition ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/update.html#description) <a href="#description" id="description"></a>

Update selected columns values in existing rows in a table.

The columns named in the `column = expression` assignments will be updated for all rows that match the `WHERE` condition. The values of all column update expressions for a matching row are evaluated before any column value is changed. When the type of the expression and the type of the column differ, the usual implicit CASTs, such as widening numeric fields, are applied to the `UPDATE` expression values.

### Examples[#](https://trino.io/docs/current/sql/update.html#examples) <a href="#examples" id="examples"></a>

Update the status of all purchases that haven’t been assigned a ship date:

```
UPDATE
  purchases
SET
  status = 'OVERDUE'
WHERE
  ship_date IS NULL;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Update the account manager and account assign date for all customers:

```
UPDATE
  customers
SET
  account_manager = 'John Henry',
  assign_date = now();
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Update the manager to be the name of the employee who matches the manager ID:

```
UPDATE
  new_hires
SET
  manager = (
    SELECT
      e.name
    FROM
      employees e
    WHERE
      e.employee_id = new_hires.manager_id
  );
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/update.html#limitations) <a href="#limitations" id="limitations"></a>

Some connectors have limited or no support for `UPDATE`. See connector documentation for more details.
