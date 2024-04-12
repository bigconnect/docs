# UNSUPPORTED\_SUBQUERY\_EXPRESSION\_CATEGORY error class

SQLSTATE: 0A000

Unsupported subquery expression:

This error class has the following derived error classes:

### ACCESSING\_OUTER\_QUERY\_COLUMN\_IS\_NOT\_ALLOWED

Accessing outer query column is not allowed in this location`<treeNode>`.

### AGGREGATE\_FUNCTION\_MIXED\_OUTER\_LOCAL\_REFERENCES

Found an aggregate function in a correlated predicate that has both outer and local references, which is not supported: `<function>`.

### CORRELATED\_COLUMN\_IS\_NOT\_ALLOWED\_IN\_PREDICATE

Correlated column is not allowed in predicate: `<treeNode>`.

### CORRELATED\_COLUMN\_NOT\_FOUND

A correlated outer name reference within a subquery expression body was not found in the enclosing query: `<value>`.

### CORRELATED\_REFERENCE

Expressions referencing the outer query are not supported outside of WHERE/HAVING clauses: `<sqlExprs>`.

### LATERAL\_JOIN\_CONDITION\_NON\_DETERMINISTIC

Lateral join condition cannot be non-deterministic: `<condition>`.

### MUST\_AGGREGATE\_CORRELATED\_SCALAR\_SUBQUERY

Correlated scalar subqueries must be aggregated to return at most one row.

### NON\_CORRELATED\_COLUMNS\_IN\_GROUP\_BY

A GROUP BY clause in a scalar correlated subquery cannot contain non-correlated columns: `<value>`.

### NON\_DETERMINISTIC\_LATERAL\_SUBQUERIES

Non-deterministic lateral subqueries are not supported when joining with outer relations that produce more than one row`<treeNode>`.

### UNSUPPORTED\_CORRELATED\_REFERENCE\_DATA\_TYPE

Correlated column reference ‘`<expr>`’ cannot be `<dataType>` type.

### UNSUPPORTED\_CORRELATED\_SCALAR\_SUBQUERY

Correlated scalar subqueries can only be used in filters, aggregations, projections, and UPDATE/MERGE/DELETE commands`<treeNode>`.

### UNSUPPORTED\_IN\_EXISTS\_SUBQUERY

IN/EXISTS predicate subqueries can only be used in filters, joins, aggregations, window functions, projections, and UPDATE/MERGE/DELETE commands`<treeNode>`.
