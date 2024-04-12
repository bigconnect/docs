# Identifiers

#### Description

An identifier is a string used to identify a database object such as a table, view, schema, column, etc. Spark SQL has regular identifiers and delimited identifiers, which are enclosed within backticks. Both regular identifiers and delimited identifiers are case-insensitive.

#### Syntax

**Regular Identifier**

```
{ letter | digit | '_' } [ , ... ]
```

**Note:** If `spark.sql.ansi.enabled` is set to true, ANSI SQL reserved keywords cannot be used as identifiers. For more details, please refer to [ANSI Compliance](https://spark.apache.org/docs/latest/sql-ref-ansi-compliance.html).

**Delimited Identifier**

```
`c [ ... ]`
```

#### Parameters

*   **letter**

    Any letter from A-Z or a-z.
*   **digit**

    Any numeral from 0 to 9.
*   **c**

    Any character from the character set. Use `` ` `` to escape special characters (e.g., `` ` ``).

#### Examples

```
-- This CREATE TABLE fails with ParseException because of the illegal identifier name a.b
CREATE TABLE test (a.b int);
Error in query:
[PARSE_SYNTAX_ERROR] Syntax error at or near '.': extra input '.'(line 1, pos 20)

== SQL ==
CREATE TABLE test (a.b int)
--------------------^^^

-- This CREATE TABLE works
CREATE TABLE test (`a.b` int);

-- This CREATE TABLE fails with ParseException because special character ` is not escaped
CREATE TABLE test1 (`a`b` int);
Error in query:
[PARSE_SYNTAX_ERROR] Syntax error at or near '`'(line 1, pos 24)

== SQL ==
CREATE TABLE test1 (`a`b` int)
------------------------^^^

-- This CREATE TABLE works
CREATE TABLE test (`a``b` int);
```
