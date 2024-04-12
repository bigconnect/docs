# INVALID\_SCHEMA error class

SQLSTATE: 42K07

The input schema `<inputSchema>` is not a valid schema string.

This error class has the following derived error classes:

### NON\_STRING\_LITERAL

The input expression must be string literal and not null.

### NON\_STRUCT\_TYPE

The input expression should be evaluated to struct type, but got `<dataType>`.

### PARSE\_ERROR

Cannot parse the schema: `<reason>`
