# COMMENT

### Synopsis[#](https://trino.io/docs/current/sql/comment.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
COMMENT ON ( TABLE | VIEW | COLUMN ) name IS 'comments'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/comment.html#description) <a href="#description" id="description"></a>

Set the comment for a object. The comment can be removed by setting the comment to `NULL`.

### Examples[#](https://trino.io/docs/current/sql/comment.html#examples) <a href="#examples" id="examples"></a>

Change the comment for the `users` table to be `master table`:

```
COMMENT ON TABLE users IS 'master table';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Change the comment for the `users` view to be `master view`:

```
COMMENT ON VIEW users IS 'master view';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Change the comment for the `users.name` column to be `full name`:

```
COMMENT ON COLUMN users.name IS 'full name';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/comment.html#see-also) <a href="#see-also" id="see-also"></a>

[Comments](https://trino.io/docs/current/language/comments.html)
