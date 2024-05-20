# GRANT privilege

### Synopsis[#](https://trino.io/docs/current/sql/grant.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
GRANT ( privilege [, ...] | ( ALL PRIVILEGES ) )
ON ( table_name | TABLE table_name | SCHEMA schema_name)
TO ( user | USER user | ROLE role )
[ WITH GRANT OPTION ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/grant.html#description) <a href="#description" id="description"></a>

Grants the specified privileges to the specified grantee.

Specifying `ALL PRIVILEGES` grants DELETE, INSERT, UPDATE and SELECT privileges.

Specifying `ROLE PUBLIC` grants privileges to the `PUBLIC` role and hence to all users.

The optional `WITH GRANT OPTION` clause allows the grantee to grant these same privileges to others.

For `GRANT` statement to succeed, the user executing it should possess the specified privileges as well as the `GRANT OPTION` for those privileges.

Grant on a table grants the specified privilege on all current and future columns of the table.

Grant on a schema grants the specified privilege on all current and future columns of all current and future tables of the schema.

### Examples[#](https://trino.io/docs/current/sql/grant.html#examples) <a href="#examples" id="examples"></a>

Grant `INSERT` and `SELECT` privileges on the table `orders` to user `alice`:

```
GRANT INSERT, SELECT ON orders TO alice;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Grant `DELETE` privilege on the schema `finance` to user `bob`:

```
GRANT DELETE ON SCHEMA finance TO bob;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Grant `SELECT` privilege on the table `nation` to user `alice`, additionally allowing `alice` to grant `SELECT` privilege to others:

```
GRANT SELECT ON nation TO alice WITH GRANT OPTION;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Grant `SELECT` privilege on the table `orders` to everyone:

```
GRANT SELECT ON orders TO ROLE PUBLIC;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/grant.html#limitations) <a href="#limitations" id="limitations"></a>

Some connectors have no support for `GRANT`. See connector documentation for more details.

### See also# <a href="#see-also" id="see-also"></a>

DENY, REVOKE privilege, SHOW GRANTS
