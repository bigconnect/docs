# DEALLOCATE PREPARE

### Synopsis[#](https://trino.io/docs/current/sql/deallocate-prepare.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
DEALLOCATE PREPARE statement_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/deallocate-prepare.html#description) <a href="#description" id="description"></a>

Removes a statement with the name `statement_name` from the list of prepared statements in a session.

### Examples[#](https://trino.io/docs/current/sql/deallocate-prepare.html#examples) <a href="#examples" id="examples"></a>

Deallocate a statement with the name `my_query`:

```
DEALLOCATE PREPARE my_query;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

PREPARE, EXECUTE, EXECUTE IMMEDIATE
