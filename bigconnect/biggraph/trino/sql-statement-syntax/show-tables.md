# SHOW TABLES

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SHOW TABLES [ FROM schema ] [ LIKE pattern ]
```



### Description <a href="#description" id="description"></a>

List the tables in `schema` or in the current schema.

Specify a pattern in the optional `LIKE` clause to filter the results to the desired subset.. For example, the following query allows you to find tables that begin with `p`:

```
SHOW TABLES FROM tpch.tiny LIKE 'p%';
```
