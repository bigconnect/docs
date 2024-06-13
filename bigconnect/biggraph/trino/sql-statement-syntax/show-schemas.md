# SHOW SCHEMAS

### Synopsis <a href="#synopsis" id="synopsis"></a>

```
SHOW SCHEMAS [ FROM catalog ] [ LIKE pattern ]
```



### Description <a href="#description" id="description"></a>

List the schemas in `catalog` or in the current catalog.

Specify a pattern in the optional `LIKE` clause to filter the results to the desired subset. For example, the following query allows you to find schemas that have `3` as the third character:

```
SHOW SCHEMAS FROM tpch LIKE '__3%'
```
