# PREPARE

### Synopsis[#](https://trino.io/docs/current/sql/prepare.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
PREPARE statement_name FROM statement
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/prepare.html#description) <a href="#description" id="description"></a>

Prepares a statement for execution at a later time. Prepared statements are queries that are saved in a session with a given name. The statement can include parameters in place of literals to be replaced at execution time. Parameters are represented by question marks.

### Examples[#](https://trino.io/docs/current/sql/prepare.html#examples) <a href="#examples" id="examples"></a>

Prepare a select query:

```
PREPARE my_select1 FROM
SELECT * FROM nation;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Prepare a select query that includes parameters. The values to compare with `regionkey` and `nationkey` will be filled in with the [EXECUTE](https://trino.io/docs/current/sql/execute.html) statement:

```
PREPARE my_select2 FROM
SELECT name FROM nation WHERE regionkey = ? AND nationkey < ?;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Prepare an insert query:

```
PREPARE my_insert FROM
INSERT INTO cities VALUES (1, 'San Francisco');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/prepare.html#see-also) <a href="#see-also" id="see-also"></a>

[EXECUTE](https://trino.io/docs/current/sql/execute.html), [DEALLOCATE PREPARE](https://trino.io/docs/current/sql/deallocate-prepare.html), [EXECUTE IMMEDIATE](https://trino.io/docs/current/sql/execute-immediate.html), [DESCRIBE INPUT](https://trino.io/docs/current/sql/describe-input.html), [DESCRIBE OUTPUT](https://trino.io/docs/current/sql/describe-output.html)
