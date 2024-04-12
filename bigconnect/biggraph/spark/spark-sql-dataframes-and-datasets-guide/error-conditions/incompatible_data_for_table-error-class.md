# INCOMPATIBLE\_DATA\_FOR\_TABLE error class

SQLSTATE: none assigned

Cannot write incompatible data for table `<tableName>`:

This error class has the following derived error classes:

### AMBIGUOUS\_COLUMN\_NAME

Ambiguous column name in the input data `<colName>`.

### CANNOT\_FIND\_DATA

Cannot find data for the output column `<colName>`.

### CANNOT\_SAFELY\_CAST

Cannot safely cast `<colName>` `<srcType>` to `<targetType>`.

### EXTRA\_STRUCT\_FIELDS

Cannot write extra fields `<extraFields>` to the struct `<colName>`.

### NULLABLE\_ARRAY\_ELEMENTS

Cannot write nullable elements to array of non-nulls: `<colName>`.

### NULLABLE\_COLUMN

Cannot write nullable values to non-null column `<colName>`.

### NULLABLE\_MAP\_VALUES

Cannot write nullable values to map of non-nulls: `<colName>`.

### STRUCT\_MISSING\_FIELDS

Struct `<colName>` missing fields: `<missingFields>`.

### UNEXPECTED\_COLUMN\_NAME

Struct `<colName>` `<order>`-th field name does not match (may be out of order): expected `<expected>`, found `<found>`.
