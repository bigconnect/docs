# CREATE ROLE

### Synopsis[#](https://trino.io/docs/current/sql/create-role.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
CREATE ROLE role_name
[ WITH ADMIN ( user | USER user | ROLE role | CURRENT_USER | CURRENT_ROLE ) ]
[ IN catalog ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/create-role.html#description) <a href="#description" id="description"></a>

`CREATE ROLE` creates the specified role.

The optional `WITH ADMIN` clause causes the role to be created with the specified user as a role admin. A role admin has permission to drop or grant a role. If the optional `WITH ADMIN` clause is not specified, the role is created with current user as admin.

The optional `IN catalog` clause creates the role in a catalog as opposed to a system role.

### Examples[#](https://trino.io/docs/current/sql/create-role.html#examples) <a href="#examples" id="examples"></a>

Create role `admin`

```
CREATE ROLE admin;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create role `moderator` with admin `bob`:

```
CREATE ROLE moderator WITH ADMIN USER bob;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/create-role.html#limitations) <a href="#limitations" id="limitations"></a>

Some connectors do not support role management. See connector documentation for more details.

### See also# <a href="#see-also" id="see-also"></a>

DROP ROLE, SET ROLE, GRANT role, REVOKE role
