# ALTER SCHEMA

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
ALTER SCHEMA name RENAME TO new_name
ALTER SCHEMA name SET AUTHORIZATION ( user | USER user | ROLE role )
```

### Description <a href="#description" id="description"></a>

Change the definition of an existing schema.

### Examples <a href="#examples" id="examples"></a>

Rename schema `web` to `traffic`:

```
ALTER SCHEMA web RENAME TO traffic
```



Change owner of schema `web` to user `alice`:

```
ALTER SCHEMA web SET AUTHORIZATION alice
```



Allow everyone to drop schema and create tables in schema `web`:

```
ALTER SCHEMA web SET AUTHORIZATION ROLE PUBLIC
```



### See Also <a href="#see-also" id="see-also"></a>

CREATE SCHEMA
