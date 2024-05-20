# START TRANSACTION

### Synopsis[#](https://trino.io/docs/current/sql/start-transaction.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
START TRANSACTION [ mode [, ...] ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

where `mode` is one of

```
ISOLATION LEVEL { READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
READ { ONLY | WRITE }
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/start-transaction.html#description) <a href="#description" id="description"></a>

Start a new transaction for the current session.

### Examples[#](https://trino.io/docs/current/sql/start-transaction.html#examples) <a href="#examples" id="examples"></a>

```
START TRANSACTION;
START TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION READ WRITE;
START TRANSACTION ISOLATION LEVEL READ COMMITTED, READ ONLY;
START TRANSACTION READ WRITE, ISOLATION LEVEL SERIALIZABLE;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

COMMIT, ROLLBACK

\
