# Regular Expressions

### Regular Expressions Overview <a href="#ariaid-title2" id="ariaid-title2"></a>

A regular expression, also known as regex, describes a pattern for a string.

This appendix provides some details and tips specific to using regular expressions with the Data Pipelines. For a thorough description of how to define regular expressions, use a manual or online reference such as: [https://docs.oracle.com/javase/tutorial/essential/regex/index.html](https://docs.oracle.com/javase/tutorial/essential/regex/index.html).

For testing regular expressions, you might find the following website helpful: [https://regex101.com/](https://regex101.com/).

### Regular Expressions in the Pipeline <a href="#ariaid-title3" id="ariaid-title3"></a>

Though generally not required, you can use Java-based regular expressions at various locations within a pipeline to define, search for, or manipulate strings.

For example, in the Field Masker processor, you can use a fixed length, variable length, or custom static mask to mask data in a field. When that doesn't work for you, you can use a regular expression to define a specific custom mask. Similarly, you can use a regular expression to define the format of a log line if it does not use one of the listed formats.

The following table describes some examples of how you might use regular expressions in the pipeline:

| Location                                                        | Description                                                                                                                                                    |
| --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Directory origin                                                | Optionally use to define the pattern of the file name.                                                                                                         |
| File Tail origin                                                | <p>Use to define the ${PATTERN} constant when you use the Files Matching a Pattern naming option.</p><p>Use to define the structure of a log line or text.</p> |
| <p>Origins that process log data</p><p>Log Parser processor</p> | Optionally use to define the pattern of the log line.                                                                                                          |
| Field Masker processor                                          | Optionally use to define the field mask.                                                                                                                       |
| regexCapture function                                           | Use to define the groups and pattern of the string so you can specify the group to return.                                                                     |
| replaceAll function                                             | Use to define the string to replace.                                                                                                                           |

### Quick Reference <a href="#ariaid-title4" id="ariaid-title4"></a>

The following table includes some details you might find helpful when creating a regular expression:

| Character | Description                                                                                                                                                                    | Examples                                                                                                                                                                                                   |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[ ]      | Use brackets to define character classes.                                                                                                                                      | \[0-9]\[0-9]\[0-9] represents 3 digits ranging from 0 through 9, inclusive.                                                                                                                                |
| -         | Use the hyphen to define ranges.                                                                                                                                               | <p>[a-z] defines one lowercase letter from a to z.</p><p>[A-Z] defines one uppercase letter from A to Z.</p>                                                                                               |
| \|        | Indicates an alternate option to the character or group being defined.                                                                                                         | \[a-z \| A-Z] represents a single upper or lowercase letter.                                                                                                                                               |
| ( )       | Use parentheses to create groups, atomic groups, or lookarounds.                                                                                                               | (\[0-9]\[0-9]\[0-9])(-\|.)(\[0-9]\[0-9]\[0-9])(-\|.)(\[0-9]\[0-9]\[0-9]\[0-9]) represents phone numbers with area codes that are separated by dashes or periods as follows: 415-555-5555 and 415.555.5555. |
| < >       | <p>Use angle brackets to define named capture groups. Use the following syntax:</p><pre><code>(?&#x3C;groupName> ...) 
</code></pre><p>to set up a named field extraction.</p> |                                                                                                                                                                                                            |
| ^         | Use a carat to negate a character class.                                                                                                                                       | \[^A-G] defines a character that is not an uppercase letter from A to G.                                                                                                                                   |
| .         | A wildcard that represents any single character except newline or other special characters.                                                                                    |                                                                                                                                                                                                            |
| &&        | Use two ampersands to indicate the union of two ranges.                                                                                                                        | \[\w&&\[^1-9] represents all word characters except 1-9.                                                                                                                                                   |
| ?         | A quantifier that represents zero or one instance of the preceding character or group.                                                                                         | B-?7 represents B7 or B-7.                                                                                                                                                                                 |
| +         | A quantifier that represents one or more instances of the preceding character or group.                                                                                        | (\[0-9]\[0-9]\[0-9])-+(\[0-9]\[0-9]\[0-9]\[0-9]) represents phone numbers that area codes: 415-555-5555 and 555-5555.                                                                                      |
| \*        | A quantifier that represents zero or more instances of the preceding character or group.                                                                                       | (\[0-9]\[0-9]\[0-9]\[0-9]\[0-9])-(\[0-9]\[0-9]\[0-9]\[0-9])\* represents 5- and 9-digit zip codes.                                                                                                         |
| \\        | Use the backslash as an escape character.                                                                                                                                      |                                                                                                                                                                                                            |
| \\\\      | Represents a single backslash                                                                                                                                                  |                                                                                                                                                                                                            |
| \w        | Represents a word character - includes alphanumeric characters and the underscore.                                                                                             | \w\w\w-\w\w\w\w\w can represent an error code, such as SVR-30243.                                                                                                                                          |
| \W        | Represents a non-word character - includes everything except alphanumeric characters and the underscore.                                                                       |                                                                                                                                                                                                            |
| \d        | Represents a digit. Shorthand for 0-9.                                                                                                                                         | (\d\d\d\d\d)-(\d\d\d\d)\* represents a 9-digit zip code.                                                                                                                                                   |
| \D        | Represents a non-digit character.                                                                                                                                              | \D&&\S represents the alphabet in either case.                                                                                                                                                             |
| \s        | Represents a whitespace character - includes space, tab, line break and form feed.                                                                                             |                                                                                                                                                                                                            |
| \S        | Represents a non-space character - includes everything except the space, tab, line break, and form feed characters.                                                            |                                                                                                                                                                                                            |
|           | Tab character.                                                                                                                                                                 |                                                                                                                                                                                                            |
|           | Return character.                                                                                                                                                              |                                                                                                                                                                                                            |
|           | Line break or newline character.                                                                                                                                               |                                                                                                                                                                                                            |
| \f        | Form feed character.                                                                                                                                                           |                                                                                                                                                                                                            |

### Regex Examples <a href="#ariaid-title5" id="ariaid-title5"></a>

Masking credit card numbers, except for one groupYou can use the following regular expression in the Field Masker processor to mask all numbers in a credit or debit card except for the last 4 digits:

```
(.*)([0-9]{4})
```

This regex defines two groups. The first group uses `.*` to represent any number, with any number of numbers; the second group represents the last four digits. In the mask configuration, set the Groups To Show property to 2 to have the output data show the second group, resulting in the display of the last 4 digits of the credit card number.The following regular expressions perform the same task:

```
(\d*)(\d{4})
```

```
(\d*)(\d\d\d\d)
```
