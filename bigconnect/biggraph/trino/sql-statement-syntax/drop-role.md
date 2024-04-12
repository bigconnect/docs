# DROP ROLE

### Synopsis[#](https://trino.io/docs/current/sql/drop-role.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DROP ROLE role_name
[ IN catalog ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/drop-role.html#description) <a href="#description" id="description"></a>

`DROP ROLE` drops the specified role.

For `DROP ROLE` statement to succeed, the user executing it should possess admin privileges for the given role.

The optional `IN catalog` clause drops the role in a catalog as opposed to a system role.

### Examples[#](https://trino.io/docs/current/sql/drop-role.html#examples) <a href="#examples" id="examples"></a>

Drop role `admin`

```
DROP ROLE admin;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/drop-role.html#limitations) <a href="#limitations" id="limitations"></a>

Some connectors do not support role management. See connector documentation for more details.

### See also[#](https://trino.io/docs/current/sql/drop-role.html#see-also) <a href="#see-also" id="see-also"></a>

[CREATE ROLE](https://trino.io/docs/current/sql/create-role.html), [SET ROLE](https://trino.io/docs/current/sql/set-role.html), [GRANT role](https://trino.io/docs/current/sql/grant-roles.html), [REVOKE role](https://trino.io/docs/current/sql/revoke-roles.html)
