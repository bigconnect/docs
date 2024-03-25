# Clauses

## **Reading clauses**

These comprise clauses that read data from the database.

The flow of data within a Cypher® query is an unordered sequence of maps with key-value pairs — a set of possible bindings between the variables in the query and values derived from the database. This set is refined and augmented by subsequent parts of the query.

| Clause         | Description                                                                                              |
| -------------- | -------------------------------------------------------------------------------------------------------- |
| MATCH          | Specify the patterns to search for in the database.                                                      |
| OPTIONAL MATCH | Specify the patterns to search for in the database while using `nulls` for missing parts of the pattern. |
| START          | Find starting points through legacy indexes.                                                             |

## **Projecting clauses**

These comprise clauses that define which expressions to return in the result set. The returned expressions may all be aliased using `AS`.

| Clause          | Description                                                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| RETURN …​ \[AS] | Defines what to include in the query result set.                                                                              |
| WITH …​ \[AS]   | Allows query parts to be chained together, piping the results from one to be used as starting points or criteria in the next. |
| UNWIND …​ \[AS] | Expands a list into a sequence of rows.                                                                                       |

## **Reading sub-clauses**

These comprise sub-clauses that must operate as part of reading clauses.

| Sub-clause                                | Description                                                                                                                                   |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| WHERE                                     | Adds constraints to the patterns in a `MATCH` or `OPTIONAL MATCH` clause or filters the results of a `WITH` clause.                           |
| ORDER BY \[ASC\[ENDING] \| DESC\[ENDING]] | A sub-clause following `RETURN` or `WITH`, specifying that the output should be sorted in either ascending (the default) or descending order. |
| SKIP                                      | Defines from which row to start including the rows in the output.                                                                             |
| LIMIT                                     | Constrains the number of rows in the output.                                                                                                  |

## **Reading hints**

These comprise clauses used to specify planner hints when tuning a query.&#x20;

| Hint             | Description                                                                                                                |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------- |
| USING INDEX      | Index hints are used to specify which index, if any, the planner should use as a starting point.                           |
| USING INDEX SEEK | Index seek hint instructs the planner to use an index seek for this clause.                                                |
| USING SCAN       | Scan hints are used to force the planner to do a label scan (followed by a filtering operation) instead of using an index. |
| USING JOIN       | Join hints are used to enforce a join operation at specified points.                                                       |

## **Writing clauses**

These comprise clauses that write the data to the database.

| Clause        | Description                                                                                                                  |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| CREATE        | Create nodes and relationships.                                                                                              |
| DELETE        | Delete nodes, relationships or paths. Any node to be deleted must also have all associated relationships explicitly deleted. |
| DETACH DELETE | Delete a node or set of nodes. All associated relationships will automatically be deleted.                                   |
| SET           | Update labels on nodes and properties on nodes and relationships.                                                            |
| REMOVE        | Remove properties and labels from nodes and relationships.                                                                   |
| FOREACH       | Update data within a list, whether components of a path, or the result of aggregation.                                       |

## **Reading/Writing clauses**

These comprise clauses that both read data from and write data to the database.

| Clause          | Description                                                                                                               |
| --------------- | ------------------------------------------------------------------------------------------------------------------------- |
| MERGE           | Ensures that a pattern exists in the graph. Either the pattern already exists, or it needs to be created.                 |
| --- ON CREATE   | Used in conjunction with `MERGE`, this write sub-clause specifies the actions to take if the pattern needs to be created. |
| --- ON MATCH    | Used in conjunction with `MERGE`, this write sub-clause specifies the actions to take if the pattern already exists.      |
| CALL \[…​YIELD] | Invoke a procedure deployed in the database and return any results.                                                       |
| CREATE UNIQUE   | A mixture of `MATCH` and `CREATE`, matching what it can, and creating what is missing.                                    |

## **Set operations**

| Clause    | Description                                                                                |
| --------- | ------------------------------------------------------------------------------------------ |
| UNION     | Combines the result of multiple queries into a single result set. Duplicates are removed.  |
| UNION ALL | Combines the result of multiple queries into a single result set. Duplicates are retained. |

## **Importing data**

| Clause                    | Description                                                                                                                         |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| LOAD CSV                  | Use when importing data from CSV files.                                                                                             |
| --- USING PERIODIC COMMIT | This query hint may be used to prevent an out-of-memory error from occurring when importing large amounts of data using `LOAD CSV`. |

###
