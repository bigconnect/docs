# SET ROLE

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SET ROLE ( role | ALL | NONE )
[ IN catalog ]
```



### Description <a href="#description" id="description"></a>

`SET ROLE` sets the enabled role for the current session.

`SET ROLE role` enables a single specified role for the current session. For the `SET ROLE role` statement to succeed, the user executing it should have a grant for the given role.

`SET ROLE ALL` enables all roles that the current user has been granted for the current session.

`SET ROLE NONE` disables all the roles granted to the current user for the current session.

The optional `IN catalog` clause sets the role in a catalog as opposed to a system role.

### Limitations <a href="#limitations" id="limitations"></a>

Some connectors do not support role management. See connector documentation for more details.

### See also <a href="#see-also" id="see-also"></a>

CREATE ROLE, DROP ROLE, GRANT role, REVOKE role

\
