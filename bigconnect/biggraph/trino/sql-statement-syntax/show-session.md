# SHOW SESSION

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SHOW SESSION [ LIKE pattern ]
```



### Description <a href="#description" id="description"></a>

List the current session properties.

Specify a pattern in the optional `LIKE` clause to filter the results to the desired subset. For example, the following query allows you to find session properties that begin with `query`:

```
SHOW SESSION LIKE 'query%'
```



### See also <a href="#see-also" id="see-also"></a>

RESET SESSION, SET SESSION

\
