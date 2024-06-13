# PREPARE

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
PREPARE statement_name FROM statement
```



### Description <a href="#description" id="description"></a>

Prepares a statement for execution at a later time. Prepared statements are queries that are saved in a session with a given name. The statement can include parameters in place of literals to be replaced at execution time. Parameters are represented by question marks.

### Examples <a href="#examples" id="examples"></a>

Prepare a select query:

```
PREPARE my_select1 FROM
SELECT * FROM nation;
```



Prepare a select query that includes parameters. The values to compare with `regionkey` and `nationkey` will be filled in with the EXECUTE statement:

```
PREPARE my_select2 FROM
SELECT name FROM nation WHERE regionkey = ? AND nationkey < ?;
```



Prepare an insert query:

```
PREPARE my_insert FROM
INSERT INTO cities VALUES (1, 'San Francisco');
```



### See also <a href="#see-also" id="see-also"></a>

EXECUTE, DEALLOCATE PREPARE, EXECUTE IMMEDIATE, DESCRIBE INPUT, DESCRIBE OUTPUT
