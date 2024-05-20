# ALTER VIEW

### Synopsis[#](https://trino.io/docs/current/sql/alter-view.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
ALTER VIEW name RENAME TO new_name
ALTER VIEW name SET AUTHORIZATION ( user | USER user | ROLE role )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/alter-view.html#description) <a href="#description" id="description"></a>

Change the definition of an existing view.

### Examples[#](https://trino.io/docs/current/sql/alter-view.html#examples) <a href="#examples" id="examples"></a>

Rename view `people` to `users`:

```
ALTER VIEW people RENAME TO users
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Change owner of VIEW `people` to user `alice`:

```
ALTER VIEW people SET AUTHORIZATION alice
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/alter-view.html#see-also) <a href="#see-also" id="see-also"></a>

CREATE VIEW
