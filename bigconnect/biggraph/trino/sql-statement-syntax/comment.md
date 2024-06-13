# COMMENT

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
COMMENT ON ( TABLE | VIEW | COLUMN ) name IS 'comments'
```



### Description <a href="#description" id="description"></a>

Set the comment for a object. The comment can be removed by setting the comment to `NULL`.

### Examples <a href="#examples" id="examples"></a>

Change the comment for the `users` table to be `master table`:

```
COMMENT ON TABLE users IS 'master table';
```



Change the comment for the `users` view to be `master view`:

```
COMMENT ON VIEW users IS 'master view';
```



Change the comment for the `users.name` column to be `full name`:

```
COMMENT ON COLUMN users.name IS 'full name';
```



### See also <a href="#see-also" id="see-also"></a>

Comments
