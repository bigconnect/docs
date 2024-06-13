# ALTER VIEW

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
ALTER VIEW name RENAME TO new_name
ALTER VIEW name SET AUTHORIZATION ( user | USER user | ROLE role )
```

### Description <a href="#description" id="description"></a>

Change the definition of an existing view.

### Examples <a href="#examples" id="examples"></a>

Rename view `people` to `users`:

```
ALTER VIEW people RENAME TO users
```



Change owner of VIEW `people` to user `alice`:

```
ALTER VIEW people SET AUTHORIZATION alice
```



### See also <a href="#see-also" id="see-also"></a>

CREATE VIEW
