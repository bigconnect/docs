# REVOKE role

### Synopsis[#](https://trino.io/docs/current/sql/revoke-roles.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
REVOKE
[ ADMIN OPTION FOR ]
role_name [, ...]
FROM ( user | USER user | ROLE role) [, ...]
[ GRANTED BY ( user | USER user | ROLE role | CURRENT_USER | CURRENT_ROLE ) ]
[ IN catalog ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/revoke-roles.html#description) <a href="#description" id="description"></a>

Revokes the specified role(s) from the specified principal(s).

If the `ADMIN OPTION FOR` clause is specified, the `GRANT` permission is revoked instead of the role.

For the `REVOKE` statement for roles to succeed, the user executing it either should be the role admin or should possess the `GRANT` option for the given role.

The optional `GRANTED BY` clause causes the role(s) to be revoked with the specified principal as a revoker. If the `GRANTED BY` clause is not specified, the roles are revoked by the current user as a revoker.

The optional `IN catalog` clause revokes the roles in a catalog as opposed to a system roles.

### Examples[#](https://trino.io/docs/current/sql/revoke-roles.html#examples) <a href="#examples" id="examples"></a>

Revoke role `bar` from user `foo`

```
REVOKE bar FROM USER foo;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Revoke admin option for roles `bar` and `foo` from user `baz` and role `qux`

```
REVOKE ADMIN OPTION FOR bar, foo FROM USER baz, ROLE qux;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/revoke-roles.html#limitations) <a href="#limitations" id="limitations"></a>

Some connectors do not support role management. See connector documentation for more details.

### See also# <a href="#see-also" id="see-also"></a>

CREATE ROLE, DROP ROLE, SET ROLE, GRANT role

\
