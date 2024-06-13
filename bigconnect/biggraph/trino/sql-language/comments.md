# Comments

### Synopsis <a href="#synopsis" id="synopsis"></a>

Comments are part of a SQL statement or script that are ignored for processing. Comments begin with double dashes and extend to the end of the line. Block comments begin with `/*` and extend to the next occurrence of `*/`, possibly spanning over multiple lines.

### Examples <a href="#examples" id="examples"></a>

The following example displays a comment line, a comment after a valid statement, and a block comment:

```
-- This is a comment.
SELECT * FROM table; -- This comment is ignored.

/* This is a block comment
   that spans multiple lines
   until it is closed. */
```
