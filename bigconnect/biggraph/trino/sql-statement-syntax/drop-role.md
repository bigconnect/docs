# DROP ROLE

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DROP ROLE role_name
[ IN catalog ]
```



### Description <a href="#description" id="description"></a>

`DROP ROLE` drops the specified role.

For `DROP ROLE` statement to succeed, the user executing it should possess admin privileges for the given role.

The optional `IN catalog` clause drops the role in a catalog as opposed to a system role.

### Examples <a href="#examples" id="examples"></a>

Drop role `admin`

```
DROP ROLE admin;
```



### Limitations <a href="#limitations" id="limitations"></a>

Some connectors do not support role management. See connector documentation for more details.

### See also <a href="#see-also" id="see-also"></a>

CREATE ROLE, SET ROLE, GRANT role, REVOKE role
