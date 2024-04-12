# SHOW GRANTS

### Synopsis[#](https://trino.io/docs/current/sql/show-grants.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW GRANTS [ ON [ TABLE ] table_name ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/show-grants.html#description) <a href="#description" id="description"></a>

List the grants for the current user on the specified table in the current catalog.

If no table name is specified, the command lists the grants for the current user on all the tables in all schemas of the current catalog.

The command requires the current catalog to be set.

Note

Ensure that authentication has been enabled before running any of the authorization commands.

### Examples[#](https://trino.io/docs/current/sql/show-grants.html#examples) <a href="#examples" id="examples"></a>

List the grants for the current user on table `orders`:

```
SHOW GRANTS ON TABLE orders;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

List the grants for the current user on all the tables in all schemas of the current catalog:

```
SHOW GRANTS;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/show-grants.html#limitations) <a href="#limitations" id="limitations"></a>

Some connectors have no support for `SHOW GRANTS`. See connector documentation for more details.

### See also[#](https://trino.io/docs/current/sql/show-grants.html#see-also) <a href="#see-also" id="see-also"></a>

[GRANT privilege](https://trino.io/docs/current/sql/grant.html), [REVOKE privilege](https://trino.io/docs/current/sql/revoke.html)

\
