# SHOW SESSION

### Synopsis[#](https://trino.io/docs/current/sql/show-session.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW SESSION [ LIKE pattern ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/show-session.html#description) <a href="#description" id="description"></a>

List the current [session properties](https://trino.io/docs/current/sql/set-session.html#session-properties-definition).

[Specify a pattern](https://trino.io/docs/current/functions/comparison.html#like-operator) in the optional `LIKE` clause to filter the results to the desired subset. For example, the following query allows you to find session properties that begin with `query`:

```
SHOW SESSION LIKE 'query%'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/show-session.html#see-also) <a href="#see-also" id="see-also"></a>

[RESET SESSION](https://trino.io/docs/current/sql/reset-session.html), [SET SESSION](https://trino.io/docs/current/sql/set-session.html)

\
