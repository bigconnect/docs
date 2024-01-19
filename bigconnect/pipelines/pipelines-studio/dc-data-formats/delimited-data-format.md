# Delimited Data Format

Data Collector can read and write delimited data.

## Reading Delimited Data

Origins that read delimited data generate a record for each delimited line in a file, object, or message. Processors that process delimited data generate records as described in the processor overview.You can read delimited data in the following formats:

* Default CSV - File that includes comma-separated values. Ignores empty lines in the file.
* RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.
* MS Excel CSV - Microsoft Excel comma-separated file.
* MySQL CSV - MySQL comma-separated file.
* Tab-Separated Values - File that includes tab-separated values.
* PostgreSQL CSV - PostgreSQL comma-separated file.
* PostgreSQL Text - PostgreSQL text file.
* Custom - File that uses user-defined delimiter, escape, and quote characters.
* Multi Character Delimited - File that uses multiple user-defined characters to delimit fields and lines, and single user-defined escape and quote characters.

You can use a list or list-map root field type for delimited data, and optionally include field names from a header line, when available.

When using a header line, you can enable handling records with additional columns. The additional columns are named using a custom prefix and integers in sequential increasing order, such as `_extra_1`, `_extra_2`. When you disallow additional columns, records that include additional columns are sent to error.

You can also replace a string constant with null values.

When a record exceeds the maximum record length defined for the stage, message-based origins and processors process the object based on the error handling configured for the stage.

When a record exceeds the maximum length, file-based origins cannot continue reading the file. Records already read from the file are passed to the pipeline. The behavior of the origin is then based on the error handling configured for the stage.

## Writing Delimited Data

When processing delimited data, file- or object-based destinations write each record as a delimited row in a file or object. Message-based destinations write each record as a message. Processors write delimited data as specified in the processor overview.

The destination writes records as delimited data. When you use this data format, the root field must be list or list-map.You can write delimited data as the following formats:

* Default CSV - File that includes comma-separated values. Ignores empty lines in the file.
* RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.
* MS Excel CSV - Microsoft Excel comma-separated file.
* MySQL CSV - MySQL comma-separated file.
* Tab-Separated Values - File that includes tab-separated values.
* PostgreSQL CSV - PostgreSQL comma-separated file.
* PostgreSQL Text - PostgreSQL text file.
* Custom - File that uses user-defined delimiter, escape, and quote characters.
