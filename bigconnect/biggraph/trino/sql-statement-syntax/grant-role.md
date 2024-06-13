# GRANT role

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
GRANT role_name [, ...]
TO ( user | USER user_mame | ROLE role_name) [, ...]
[ GRANTED BY ( user | USER user | ROLE role | CURRENT_USER | CURRENT_ROLE ) ]
[ WITH ADMIN OPTION ]
[ IN catalog ]
```



### Description <a href="#description" id="description"></a>

Grants the specified role(s) to the specified principal(s).

If the `WITH ADMIN OPTION` clause is specified, the role(s) are granted to the users with `GRANT` option.

For the `GRANT` statement for roles to succeed, the user executing it either should be the role admin or should possess the `GRANT` option for the given role.

The optional `GRANTED BY` clause causes the role(s) to be granted with the specified principal as a grantor. If the `GRANTED BY` clause is not specified, the roles are granted with the current user as a grantor.

The optional `IN catalog` clause grants the roles in a catalog as opposed to a system roles.

### Examples <a href="#examples" id="examples"></a>

Grant role `bar` to user `foo`

```
GRANT bar TO USER foo;
```



Grant roles `bar` and `foo` to user `baz` and role `qux` with admin option

```
GRANT bar, foo TO USER baz, ROLE qux WITH ADMIN OPTION;
```



### Limitations <a href="#limitations" id="limitations"></a>

Some connectors do not support role management. See connector documentation for more details.

### See also <a href="#see-also" id="see-also"></a>

CREATE ROLE, DROP ROLE, SET ROLE, REVOKE role

\
