# START TRANSACTION

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
START TRANSACTION [ mode [, ...] ]
```



where `mode` is one of

```
ISOLATION LEVEL { READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
READ { ONLY | WRITE }
```



### Description <a href="#description" id="description"></a>

Start a new transaction for the current session.

### Examples <a href="#examples" id="examples"></a>

```
START TRANSACTION;
START TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION READ WRITE;
START TRANSACTION ISOLATION LEVEL READ COMMITTED, READ ONLY;
START TRANSACTION READ WRITE, ISOLATION LEVEL SERIALIZABLE;
```



### See also <a href="#see-also" id="see-also"></a>

COMMIT, ROLLBACK

\
