# INVALID\_FORMAT error class

SQLSTATE: 42601

The format is invalid: `<format>`.

This error class has the following derived error classes:

### CONT\_THOUSANDS\_SEPS

Thousands separators (, or G) must have digits in between them in the number format.

### CUR\_MUST\_BEFORE\_DEC

Currency characters must appear before any decimal point in the number format.

### CUR\_MUST\_BEFORE\_DIGIT

Currency characters must appear before digits in the number format.

### EMPTY

The number format string cannot be empty.

### ESC\_AT\_THE\_END

The escape character is not allowed to end with.

### ESC\_IN\_THE\_MIDDLE

The escape character is not allowed to precede `<char>`.

### MISMATCH\_INPUT

The input `<inputType>` `<input>` does not match the format.

### THOUSANDS\_SEPS\_MUST\_BEFORE\_DEC

Thousands separators (, or G) may not appear after the decimal point in the number format.

### UNEXPECTED\_TOKEN

Found the unexpected `<token>` in the format string; the structure of the format string must match: `[MI|S]` `[$]` `[0|9|G|,]*` `[.|D]` `[0|9]*` `[$]` `[PR|MI|S]`.

### WRONG\_NUM\_DIGIT

The format string requires at least one number digit.

### WRONG\_NUM\_TOKEN

At most one `<token>` is allowed in the number format.
