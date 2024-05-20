# SHOW SESSION

### Synopsis[#](https://trino.io/docs/current/sql/show-session.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SHOW SESSION [ LIKE pattern ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description# <a href="#description" id="description"></a>

List the current session properties.

Specify a pattern in the optional `LIKE` clause to filter the results to the desired subset. For example, the following query allows you to find session properties that begin with `query`:

```
SHOW SESSION LIKE 'query%'
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

RESET SESSION, SET SESSION

\
