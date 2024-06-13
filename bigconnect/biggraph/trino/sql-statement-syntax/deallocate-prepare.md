# DEALLOCATE PREPARE

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
DEALLOCATE PREPARE statement_name
```



### Description <a href="#description" id="description"></a>

Removes a statement with the name `statement_name` from the list of prepared statements in a session.

### Examples <a href="#examples" id="examples"></a>

Deallocate a statement with the name `my_query`:

```
DEALLOCATE PREPARE my_query;
```



### See also <a href="#see-also" id="see-also"></a>

PREPARE, EXECUTE, EXECUTE IMMEDIATE
