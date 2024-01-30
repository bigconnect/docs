# File Tail

The File Tail origin reads lines of data as they are written to an active file after reading related archived files in the same directory. File Tail generates a record for each line of data.

When you configure File Tail, you define the full path to the active file, the naming convention for archived files, and the first archived file to process. These properties determine the set of files that are read and the order of processing.

For example, a File Tail is configured to tail a log called server.log, starting with server.log.1, expecting archived files named in reverse counter order. When the pipeline runs, it starts reading lines from the beginning of server.log.1, ignoring older files. When it reaches the end of server.log.1, it continues processing data in server.log. When it completes reading existing data in server.log, it tails incoming data for the file.

When you stop a pipeline, File Tail notes where it stops reading. When you restart the pipeline, File Tail continues processing from the line where it left off, even if it has been archived to a different file.

You can reset the origin for File Tail. When you reset the origin, the File Tail treats all properties as if you are starting the pipeline for the first time - processing all requested files before continuing to the active file.

File Tail generates record header attributes that enable you to use the origins of a record in pipeline processing.

The origin can generate events for an event stream. For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

Note: If you use logrotate with the copytruncate option, use the Directory origin to process the rolled or archived files. File Tail does not support copytruncate.

## File Processing and Archived File Names <a href="#ariaid-title2" id="ariaid-title2"></a>

File Tail processes the active file and archived files based on how the source server generates files. When you specify the naming convention for archived files, File Tail determines the file generation method and processes the data accordingly.

File Tail can process data based on the following common file generation and archive strategies:The server writes data to a single active file and archives files as neededThe server writes new data to a single file, such as weblog.log. Periodically, it rolls older data from the active file to an archive file, such as weblog.log1 or weblog.log.2015-10-15.File Tail intelligently tracks its relative progress through new and archived data and is not affected by file name changes.File Tail can process archived files that use the following naming conventions:

*   Active File with Reverse Counter - The server appends a number to the end of each archived file, such as data.1, data.2, and so on.

    File Tail processes files in reverse numeric order before progressing to the active file.
*   Active File with Date - The server appends a date to the end of the archived file, such as event.log.2015-11, event.log.2015-12. This method is generally used by Log4j servers. File Tail provides a set of available date patterns. If the date pattern of your log files is not available, you might use Alphabetical.

    File Tail processes files in reverse order based on the selected date pattern before progressing to the active file.

    If you configure the origin to use this naming convention, the origin uses the Active File with Reverse Counter convention.
*   Active File with Alphabetical - The server generates files in alphabetically increasing order, such as file.json.a, file.json.b, file.json.c. You might use this method to process Apache logs archived with the logrotate utility. You can also use Alphabetical to process files with appended dates that do not match the provided date patterns.

    File Tail processes files in alphabetical order before progressing to the active file.

The server writes a new active file as needed, archiving the previous file without renamingThe server writes to an active file, creating a new active file as necessary. When it creates a new active file, it archives the previous active file without renaming it. The active file changes, but file names do not change.The server generates files using a pattern that you define with a regular expression.Use Files Matching a Pattern as the archive file naming convention.File Tail processes the files in alphabetical order before progressing to the most recent file - the active file.For example, if you use log\_\[0-9]{4}-\[0-9]{2}-\[0-9]{2}\\\\.json as the file name pattern, File Tail might read log-2015-5-01.json, then log-2015-05-02.json, and so on - until it reaches the file for the current date.

## Multiple Paths and File Sets <a href="#ariaid-title3" id="ariaid-title3"></a>

File Tail can read sets of files in different directories. When File Tail processes different sets of files, it merges all data in the pipeline. If you need to separate the records later, you can use the tag record attribute.

To define a set of files to be processed, you specify the full path to an active file and related properties. You can define additional file sets by configuring a set of properties for each set of files.

You can also use wildcards to define glob patterns in the path definition to represent multiple paths and active files. When you define glob patterns, note the following restrictions:

*   The part of the path before wildcards must exist when the pipeline starts unless you enable the origin to read from late directories. Use the Allow Late Directories property to allow reading from late directories.For example, without late directories enabled, the /server/ part of the following directory must exist when you start the pipeline.

    ```
    /server/*/activity.log
    ```
*   While you can use multiple wildcards in a path, do not use multiple wildcards in a row.For example, this path is valid:

    ```
    /user/*/logs/*/log-*.json
    ```

    But the following path does not pass validation because it includes two adjacent wildcards:

    ```
    /user/*/logs/*/*/log-*.json
    ```

## First File for Processing <a href="#ariaid-title4" id="ariaid-title4"></a>

Configure a first file for processing when you want the File Tail origin to ignore one or more existing files in the directory.

When you define a first file to process, File Tail starts processing with the specified file and continues based on the selected naming convention for archived files. When you do not specify a first file, the origin processes all files in the directory based on the naming convention.

For example, say the origin reads an active file with reverse counter archived files. To ignore all files older than a particular file, use that file as the first file to process.Note: When you restart a stopped pipeline, File Tail ignores this property. It starts where it left off regardless of the file name unless you reset the origin.

## Late Directories <a href="#ariaid-title5" id="ariaid-title5"></a>

You can configure File Tail to read files in late directories - directories that appear after the pipeline starts.

When reading from late directories, File Tail does not validate the directory path when you start the pipeline. If no valid directories exist when the pipeline starts, File Tail waits indefinitely for the appearance of a valid directory and a file to tail.

For example, say you want to tail the following files:

```
/logs/*/server/*/log.json
```

No valid directory or file exists when you start the pipeline, so File Tail waits until a valid directory and file appears before tailing the file.

This file path allows the origin to tail the files in the following late-arriving directories:

```
/logs/region1/server/8/log.json
/logs/region2/server/15/log.json
/logs/cloud/server/23/log.json
```

## Files Matching a Pattern - Pattern Constant <a href="#ariaid-title6" id="ariaid-title6"></a>

When you use the Files Matching a Pattern naming option, use the PATTERN constant as a placeholder for the file name pattern. Then you use the Pattern property to define the PATTERN constant.

For example, the following properties process data in files named weblog\_\<yyyy>-\<mm>-\<dd>.json and add a "webservers" tag attribute to each record:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/FileTail-Properties.png)

## Record Header Attributes <a href="#ariaid-title7" id="ariaid-title7"></a>

The File Tail origin creates record header attributes that include information about the originating file for the record.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes. For more information about working with record header attributes, see Working with Header Attributes.

The File Tail origin creates the following record header attributes:

* filename - Provides the name of the file where the record originated.
* file - Provides the file path and file name where the record originated.
* mtime - Provides the last-modified time for the file.
* offset - Provides the file offset in bytes. The file offset is the location in the file where the record originated.

The origin can also generate a user-defined tag attribute.

### Defining and Using a Tag <a href="#ariaid-title8" id="ariaid-title8"></a>

A tag is an optional record header attribute that you can define for sets of files. In the pipeline, you can use a function to return the value of the tag attribute.

Define a tag attribute so you can easily reference tagged records. Use the Tag property to define a tag for a set of files associated with a path. Note that when you use wildcards or glob patterns in a path to represent several sets of files, those files would all use the same tag.

In the pipeline, you can use the following expression to return the tag attribute:

```
record:attribute('tag')
```

For example, when you use two different paths to tail files from two servers, you can tag the records ServerA and ServerB. In the Stream Selector, you can use the following conditions to separate the data to two different branches of the pipeline:

```
${record:attribute('tag')='ServerA'}
```

```
${record:attribute('tag')='ServerB'}
```

## Multiple Line Processing <a href="#ariaid-title9" id="ariaid-title9"></a>

File Tail can process log and text data that includes multiple lines. You might use multiple line processing to include stack traces with log data, or to process MySQL multiline logs.

To process multiline data, you use a regular expression to define the pattern of the standard line. When File Tail encounters data that does not match the pattern, it appends the non-matching data to the previous line.

Evaluating data for the pattern can impact performance, so use multiple line processing only when required.

## File Tail Output <a href="#ariaid-title10" id="ariaid-title10"></a>

File Tail provides data and related metadata through separate output streams.

The origin passes information through the following output locations:DataPasses data read from the origin.MetadataImportant: This functionality will be deprecated in a future release. The record generated by the metadata output is the same as the File Tail event record. Best practice is to connect the metadata output to the Trash destination. Then, to use the event record, configure the origin to generate events. For more information, see Event Generation.Passes information generated about file processing. To retain this information for review, connect the metadata output location to any other destination or route the data as necessary. If you do not require this information, connect the metadata output location to the Trash destination.File processing metadata includes the following fields:

* fileName - Name of the file.
* inode - Index information for the file.
* time - Time that the event occurred.
* event - Processing event. START indicates that File Tail started processing the specified file. END indicates that File Tail completed processing the contents of the file.

Note: Inode most reliably identifies a file when a server renames archived files.

## Event Generation <a href="#ariaid-title11" id="ariaid-title11"></a>

The File Tail origin can generate events that you can use in an event stream. When you enable event generation, the origin generates event records each time the origin starts or completes reading a file.

File Tail events can be used in any logical way. For example:

*   With the Email executor to send a custom email after receiving an event.

    For an example, see Sending Email During Pipeline Processing.
*   With a destination to store event information.

    For an example, see Preserving an Audit Trail of Events.

For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

### Event Records <a href="#ariaid-title12" id="ariaid-title12"></a>

Event records generated by the File Tail origin have the following fields:

| Field    | Description                                                                                                                                                                                                                |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fileName | Name of the file.                                                                                                                                                                                                          |
| inode    | Index information for the file.                                                                                                                                                                                            |
| time     | Time that the event occurred.                                                                                                                                                                                              |
| event    | <p>Event that generated the record. Uses one of the following values:</p><ul><li>START - File Tail started processing the specified file.</li><li>END - File Tail completed processing the contents of the file.</li></ul> |

## Data Formats <a href="#ariaid-title13" id="ariaid-title13"></a>

The File Tail origin processes lines of data. The origin handles data differently based on the type of data being processed.

The File Tail origin processes data formats as follows:

**JSON**

Generates a record for each JSON object. Each JSON object must be written to file as a single line.When an object exceeds the maximum line length defined for the origin, File Tail processes the object based on the error handling configured for the origin.

**Log**

Generates a record for every log line.When a line exceeds the maximum line length defined for the origin, File Tail truncates the line.You can include the processed log line as a field in the record. If the log line is truncated, and you request the log line in the record, File Tail includes the truncated line.You can define the log format or type to be read.

**Text**

Generates a record for each line in the file.When a line exceeds the maximum line length defined for the origin, File Tail truncates the line. The origin adds a boolean field named Truncated to indicate if the line was truncated.

## Configuring a File Tail Origin <a href="#ariaid-title14" id="ariaid-title14"></a>

Configure a File Tail origin to read data as it is written to a file.

1.  In the Properties panel, on the General tab, configure the following properties:

    | General Property | Description                                                                                                                                                                                                          |
    | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name             | Stage name.                                                                                                                                                                                                          |
    | Description      | Optional description.                                                                                                                                                                                                |
    | Produce Events   | Generates event records when events occur. Use for event handling.                                                                                                                                                   |
    | On Record Error  | <p>Error record handling for the stage:</p><ul><li>Discard - Discards the record.</li><li>Send to Error - Sends the record to the pipeline for error handling.</li><li>Stop Pipeline - Stops the pipeline.</li></ul> |
2.  On the File tab, configure the following properties:

    | File Property          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | File to Tail           | Directories and sets of files to process. Using simple or bulk edit mode, click the Add icon to enter additional sets of information.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | Path                   | <p>Full path to the active file to tail.</p><p>You can use wildcards to define glob patterns. Do not use multiple wildcards in a row. The path before the wildcard must exist unless you enable reading from late directories.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Naming                 | <p>Naming convention for archived files. Select the naming convention that describes how the server generates file names:</p><ul><li>Active File with Reverse Counter - File Tail processes files in reverse numeric order before progressing to the active file.</li><li><p>Active File with Date - File Tail processes files in reverse order based on the selected date pattern before progressing to the active file.</p><p>If you configure the origin to use this naming convention, the origin uses the Active File with Reverse Counter convention.</p></li><li>Active File with Alphabetical - File Tail processes files in alphabetical order before progressing to the active file. If you configure the origin to use this naming convention, the origin uses the Active File with Reverse Counter convention.</li></ul><ul><li>Files Matching a Pattern - File Tail processes the files in alphabetical order before progressing to the most recent file - the active file.</li></ul> |
    | Pattern                | <p>Regular expression that defines the file name pattern used to create files.</p><p>You can use UNIX-style wildcards, such as an asterisk or question mark.</p><p>Available only for the Files Matching a Pattern naming option.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | First File             | <p>Name of the first file to process when you start the pipeline. Use a name that uses the file name pattern. Leave empty to read all files in the directory with the specified naming convention.</p><p>When you enter a file name, File Tail reads the first file and then progresses to the active file.</p><p>When you restart a stopped pipeline, File Tail ignores this property. It starts where it left off regardless of the first file name unless you reset the origin.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | Tag                    | An optional record header attribute to help differentiate between different sets of records.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | Maximum Batch Size     | <p>Maximum number of lines to include in a batch. Honors values up to the Data Pipelines maximum batch size.</p><p>Default is 1000. The Data Pipelines default is 1000.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
    | Batch Wait Time (secs) | Number of seconds to wait before sending a batch.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Allow Late Directories | Allows reading files from directories that appear after the pipeline starts. When enabled, the origin does not validate the file path.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
3.  On the Post Processing tab, configure the following properties:

    | Post Processing Property | Description                                                                                                                                                                                     |
    | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | File Post Processing     | <p>The action taken after processing a file:</p><ul><li>None - Keeps the file in place.</li><li>Archive - Moves the file to the archive directory.</li><li>Delete - Deletes the file.</li></ul> |
    | Archive Directory        | Directory for archived files.                                                                                                                                                                   |
4.  On the Data Format tab, configure the following property:

    | Data Format Property | Description                                                                                                              |
    | -------------------- | ------------------------------------------------------------------------------------------------------------------------ |
    | Data Format          | <p>Format of data in the file. Use one of the following data formats:</p><ul><li>JSON</li><li>Log</li><li>Text</li></ul> |
5.  For JSON data, on the Data Format tab, configure the following properties:

    | JSON Property                                 | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | JSON Content                                  | <p>Type of JSON content. Use one of the following options:</p><ul><li>Array of Objects</li><li>Multiple Objects</li></ul>                                                                                                                                                                                                                                                                             |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Maximum Object Length (chars)                 | <p>Maximum number of characters in a JSON object.</p><p>Longer objects are diverted to the pipeline for error handling.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                                                                               |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                      |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                   |
6.  For log data, on the Data Format tab, configure the following properties:

    | Log Property                                  | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Log Format                                    | <p>Format of the log files. Use one of the following options:</p><ul><li>Common Log Format</li><li>Combined Log Format</li><li>Apache Error Log Format</li><li>Apache Access Log Custom Format</li><li>Regular Expression</li><li>Grok Pattern</li><li>Log4j</li><li>Common Event Format (CEF)</li><li>Log Event Extended Format (LEEF)</li></ul>                                                     |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Pattern for Multiline                         | Regular expression that defines the pattern for the standard log line.                                                                                                                                                                                                                                                                                                                                |
    | Max Line Length                               | <p>Maximum length of a log line. The origin truncates longer lines.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                                                                                                                                   |
    | Retain Original Line                          | <p>Determines how to treat the original log line. Select to include the original log line as a field in the resulting record.</p><p>By default, the original line is discarded.</p>                                                                                                                                                                                                                   |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                      |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                   |

    * When you select Apache Access Log Custom Format, use Apache log format strings to define the Custom Log Format.
    * When you select Regular Expression, enter the regular expression that describes the log format, and then map the fields that you want to include to each regular expression group.
    *   When you select Grok Pattern, you can use the Grok Pattern Definition field to define custom grok patterns. You can define a pattern on each line.

        In the Grok Pattern field, enter the pattern to use to parse the log. You can use a predefined grok patterns or create a custom grok pattern using patterns defined in Grok Pattern Definition.

        For more information about defining grok patterns and supported grok patterns, see Defining Grok Patterns.
    *   When you select Log4j, define the following properties:

        | Log4j Property        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                 |
        | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
        | On Parse Error        | <p>Determines how to handle information that cannot be parsed:</p><ul><li>Skip and Log Error - Skips reading the line and logs a stage error.</li><li>Skip, No Error - Skips reading the line and does not log an error.</li><li>Include as Stack Trace - Includes information that cannot be parsed as a stack trace to the previously-read log line. The information is added to the message field for the last valid log line.</li></ul> |
        | Use Custom Log Format | Allows you to define a custom log format.                                                                                                                                                                                                                                                                                                                                                                                                   |
        | Custom Log4J Format   | Use log4j variables to define a custom log format.                                                                                                                                                                                                                                                                                                                                                                                          |
7.  For text data, on the Data Format tab, configure the following properties:

    | Text Property                                 | Description                                                                                                                                                                                                                                                                                                                                                                                           |
    | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Compression Format                            | <p>The compression format of the files:</p><ul><li>None - Processes only uncompressed files.</li><li>Compressed File - Processes files compressed by the supported compression formats.</li><li>Archive - Processes files archived by the supported archive formats.</li><li>Compressed Archive - Processes files archived and compressed by the supported archive and compression formats.</li></ul> |
    | File Name Pattern within Compressed Directory | <p>For archive and compressed archive files, file name pattern that represents the files to process within the compressed directory. You can use UNIX-style wildcards, such as an asterisk or question mark. For example, *.json.</p><p>Default is *, which processes all files.</p>                                                                                                                  |
    | Pattern for Multiline                         | Regular expression that defines the pattern for the standard line of text.                                                                                                                                                                                                                                                                                                                            |
    | Max Line Length                               | <p>Maximum number of characters allowed for a line. Longer lines are truncated.</p><p>Adds a boolean field to the record to indicate if it was truncated. The field name is Truncated.</p><p>This property can be limited by the Data Pipelines parser buffer size. For more information, see Maximum Record Size.</p>                                                                                |
    | Use Custom Delimiter                          | Uses custom delimiters to define records instead of line breaks.                                                                                                                                                                                                                                                                                                                                      |
    | Custom Delimiter                              | One or more characters to use to define records.                                                                                                                                                                                                                                                                                                                                                      |
    | Include Custom Delimiter                      | Includes delimiter characters in the record.                                                                                                                                                                                                                                                                                                                                                          |
    | Charset                                       | Character encoding of the files to be processed.                                                                                                                                                                                                                                                                                                                                                      |
    | Ignore Control Characters                     | Removes all ASCII control characters except for the tab, line feed, and carriage return characters.                                                                                                                                                                                                                                                                                                   |
