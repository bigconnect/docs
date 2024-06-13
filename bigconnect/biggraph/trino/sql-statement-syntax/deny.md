# DENY

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DENY ( privilege [, ...] | ( ALL PRIVILEGES ) )
ON ( table_name | TABLE table_name | SCHEMA schema_name)
TO ( user | USER user | ROLE role )
```



### Description <a href="#description" id="description"></a>

Denies the specified privileges to the specified grantee.

Deny on a table rejects the specified privilege on all current and future columns of the table.

Deny on a schema rejects the specified privilege on all current and future columns of all current and future tables of the schema.

### Examples <a href="#examples" id="examples"></a>

Deny `INSERT` and `SELECT` privileges on the table `orders` to user `alice`:

```
DENY INSERT, SELECT ON orders TO alice;
```



Deny `DELETE` privilege on the schema `finance` to user `bob`:

```
DENY DELETE ON SCHEMA finance TO bob;
```



Deny `SELECT` privilege on the table `orders` to everyone:

```
DENY SELECT ON orders TO ROLE PUBLIC;
```



### Limitations <a href="#limitations" id="limitations"></a>

The system access controls as well as the connectors provided by default in Trino have no support for `DENY`.

### See also <a href="#see-also" id="see-also"></a>

GRANT privilege, REVOKE privilege, SHOW GRANTS

\
