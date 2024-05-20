# REVOKE privilege

### Synopsis[#](https://trino.io/docs/current/sql/revoke.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
REVOKE [ GRANT OPTION FOR ]
( privilege [, ...] | ALL PRIVILEGES )
ON ( table_name | TABLE table_name | SCHEMA schema_name )
FROM ( user | USER user | ROLE role )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/revoke.html#description) <a href="#description" id="description"></a>

Revokes the specified privileges from the specified grantee.

Specifying `ALL PRIVILEGES` revokes DELETE, INSERT and SELECT privileges.

Specifying `ROLE PUBLIC` revokes privileges from the `PUBLIC` role. Users will retain privileges assigned to them directly or via other roles.

If the optional `GRANT OPTION FOR` clause is specified, only the `GRANT OPTION` is removed. Otherwise, both the `GRANT` and `GRANT OPTION` are revoked.

For `REVOKE` statement to succeed, the user executing it should possess the specified privileges as well as the `GRANT OPTION` for those privileges.

Revoke on a table revokes the specified privilege on all columns of the table.

Revoke on a schema revokes the specified privilege on all columns of all tables of the schema.

### Examples[#](https://trino.io/docs/current/sql/revoke.html#examples) <a href="#examples" id="examples"></a>

Revoke `INSERT` and `SELECT` privileges on the table `orders` from user `alice`:

```
REVOKE INSERT, SELECT ON orders FROM alice;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Revoke `DELETE` privilege on the schema `finance` from user `bob`:

```
REVOKE DELETE ON SCHEMA finance FROM bob;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Revoke `SELECT` privilege on the table `nation` from everyone, additionally revoking the privilege to grant `SELECT` privilege:

```
REVOKE GRANT OPTION FOR SELECT ON nation FROM ROLE PUBLIC;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Revoke all privileges on the table `test` from user `alice`:

```
REVOKE ALL PRIVILEGES ON test FROM alice;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/revoke.html#limitations) <a href="#limitations" id="limitations"></a>

Some connectors have no support for `REVOKE`. See connector documentation for more details.

### See also# <a href="#see-also" id="see-also"></a>

DENY, GRANT privilege, SHOW GRANTS

\
