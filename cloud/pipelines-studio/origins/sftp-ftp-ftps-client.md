# SFTP/FTP/FTPS Client

The SFTP/FTP/FTPS Client origin reads files from a server using the Secure File Transfer Protocol (SFTP), File Transfer Protocol (FTP), or FTP Secure (FTPS) protocol.&#x20;

When you configure the SFTP/FTP/FTPS Client origin, you specify the protocol to use and the URL where the files reside on the remote server. You can specify whether to process files in subdirectories, a file name pattern, and the first file to process. You can use glob patterns or regular expressions to define the file name pattern that you want to use.

If the server requires authentication, configure the credentials for the protocol you are using. For the SFTP protocol, the origin can require that the server be listed in a known hosts file. For the FTPS protocol, the origin can authenticate with the server using a client certificate and can authenticate the certificate from the FTPS server.

You can configure the origin to download files to an archive directory if the origin encounters errors while reading the files.

When the pipeline stops, the SFTP/FTP/FTPS Client origin notes where it stops reading. When the pipeline starts again, the origin continues processing from where it stopped by default. You can reset the origin to process all requested files.

The origin can generate events for an event stream.&#x20;

After processing a file, the origin can keep, archive, or delete the file.

### File Name Pattern and Mode <a href="#ariaid-title2" id="ariaid-title2"></a>

Use a file name pattern to define the files that the SFTP/FTP/FTPS Client origin processes. You can use either a glob pattern or a regular expression to define the file name pattern.

The SFTP/FTP/FTPS Client origin process files at the specified path based on the file name pattern mode and file name pattern. When processing subdirectories, the origin uses the same pattern to locate file names in the subdirectories. The origin does not use the pattern to locate subdirectories.

When you specify a glob pattern, you can use UNIX-style wildcards, such as \* or ?. For example, the pattern `??a` represents three-character file names which end with `a`. The pattern `*.txt` represents file names of one or more characters ending with `.txt`.

In a glob pattern, you cannot use a tilde (\~) or slash (/). You cannot use a period (.) at the beginning of the pattern. The origin treats a period as a literal in other spots in the pattern.

The origin processes files in order based on the specified read order.

For more information about glob syntax, see the [Oracle Java documentation](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob).&#x20;

Default is `*`, which processes all files.

### Read Order <a href="#ariaid-title3" id="ariaid-title3"></a>

The SFTP/FTP/FTPS Client origin reads files in ascending order based on the last-modified timestamp. When the origin reads from a secondary location - not the directory where the files are created and written - the last-modified timestamp should be when the file is moved to the directory to be processed.Tip: Avoid moving files using commands that preserve the existing timestamp, such as `cp -p`. Preserving the existing timestamp can be problematic in some cases, such as moving files across time zones.

When ordering based on timestamp, any files with the same timestamp are read in lexicographically ascending order based on the file names.

For example, when reading files with the `log*.json` file name pattern, the origin reads the following files in the following order:

| `File Name`     | `Last Modified Timestamp` |
| --------------- | ------------------------- |
| `log-1.json`    | `APR 24 2016 14:03:35`    |
| `log-0054.json` | `APR 24 2016 14:05:03`    |
| `log-0055.json` | `APR 24 2016 14:45:11`    |
| `log-2.json`    | `APR 24 2016 14:45:11`    |

### First File for Processing <a href="#ariaid-title4" id="ariaid-title4"></a>

Configure a first file for processing when you want the SFTP/FTP/FTPS Client origin to ignore one or more existing files in the directory.

When you define a first file to process, the origin starts processing with the specified file and continues processing files in the expected read order: files that match the file name pattern in ascending order based on the last-modified timestamp.

When you do not specify a first file, the origin processes the files in the directory that match the file name pattern, starting with the earliest file and continuing in ascending order.

For example, if you specify a first file with the last-modified timestamp of 6/01/2017 00:00:00, the origin starts processing with that file and ignores all older files in the directory.Note: When you restart a stopped pipeline, the origin ignores this property. It starts where it left off regardless of the first file name unless you reset the origin.

### Credentials <a href="#ariaid-title5" id="ariaid-title5"></a>

The SFTP/FTP/FTPS Client origin can use several methods to authenticate with the remote server. From the Credentials tab, configure the authentication required by the remote server.

Authentication options differ for each protocol:

* For all protocols, select an authentication method to log in to the remote server. Choose the method based on the protocol and remote server requirements:
  * None - The stage does not authenticate with the server.
  * Password - The stage authenticates with the server using a user name and password. You must specify the user name and password.
  * Private key - The stage authenticates using a private key. Use only with the SFTP protocol. You must specify the private key, either in a local file or in plain text.
* For the SFTP protocol, the stage can require that the server be listed in a known hosts file. You must specify the path to the known hosts file that contains the host keys for the approved SFTP servers.
* For the FTPS protocol, the stage can use certificates to authenticate with the server. You must specify the keystore file and password. You can also configure the stage to authenticate the server by specifying a truststore provider.&#x20;

### Record Header Attributes <a href="#ariaid-title6" id="ariaid-title6"></a>

The SFTP/FTP/FTPS Client origin creates record header attributes that include information about the originating file for the record. When the origin processes Avro data, it includes the Avro schema in an avroSchema record header attribute.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes.&#x20;

The SFTP/FTP/FTPS Client origin creates the following record header attributes:

* avroSchema - When processing Avro data, provides the Avro schema.
* filename - Provides the name of the file where the record originated.
* file - Provides the file path and file name where the record originated.
* mtime - Provides the last-modified time for the file.
* remoteUri - Provides the resource URL used by the stage.

### Event Generation <a href="#ariaid-title7" id="ariaid-title7"></a>

The SFTP/FTP/FTPS Client origin can generate events that you can use in an event stream. When you enable event generation, the origin generates event records each time the origin starts or completes reading a file. It can also generate events when it completes processing all available data and the configured batch wait time has elapsed.

SFTP/FTP/FTPS Client origin events can be used in any logical way. For example:

* With the SFTP/FTP/FTPS Client executor to move a file after processing it.
*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin continues processing from the last-saved offset unless you reset the origin.
* With the Email executor to send a custom email after receiving an event.

#### Event Records <a href="#ariaid-title8" id="ariaid-title8"></a>

Event records generated by the SFTP/FTP/FTPS Client origin have the following event-related record header attributes. Record header attributes are stored as String values:

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>new-file - Generated when the origin starts processing a new file.</li><li>finished-file - Generated when the origin completes processing a file.</li><li>no-more-data - Generated after the origin completes processing all available files and the number of seconds configured for Batch Wait Time has elapsed.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                                                                                            |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                                                                                       |

The SFTP/FTP/FTPS Client origin can generate the following types of event records:

new-fileThe SFTP/FTP/FTPS Client origin generates a new-file event record when it starts processing a new file.New-file event records have the `sdc.event.type` record header attribute set to `new-file` and include the following field:

| Event Record Field | Description                                                               |
| ------------------ | ------------------------------------------------------------------------- |
| filepath           | Path and name of the file that the origin started or finished processing. |

finished-fileThe SFTP/FTP/FTPS Client origin generates a finished-file event record when it finishes processing a file.Finished-file event records have the `sdc.event.type` record header attribute set to `finished-file` and include the following fields:

| Event Record Field | Description                                                               |
| ------------------ | ------------------------------------------------------------------------- |
| filepath           | Path and name of the file that the origin started or finished processing. |
| record-count       | Number of records successfully generated from the file.                   |
| error-count        | Number of error records generated from the file.                          |

no-more-dataThe SFTP/FTP/FTPS Client origin generates a no-more-data event record when the origin completes processing all available records and the number of seconds configured for Batch Wait Time elapses without any new files appearing to be processed.No-more-data event records generated by the SFTP/FTP/FTPS Client origin have the `sdc.event.type` record header attribute set to `no-more-data` and include the following fields:

| Event Record Field | Description                                                                                                                      |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| record-count       | Number of records successfully generated since the pipeline started or since the last no-more-data event was created.            |
| error-count        | Number of error records generated since the pipeline started or since the last no-more-data event was created.                   |
| file-count         | Number of files the origin attempted to process. Can include files that were unable to be processed or were not fully processed. |

### Post Processing <a href="#ariaid-title9" id="ariaid-title9"></a>

After processing files in data formats other than whole file, the SFTP/FTP/FTPS Client origin can keep, archive, or delete the files.

When archiving the files, the origin moves the files to the \<archive>/\<source> directory, where:

* \<archive> is the archive directory specified on the Post Processing tab. You can specify an absolute path to the archive directory or a path relative to the home directory of the user that the origin logs in as.
* \<source> is included when processing subdirectories. The origin creates a source directory that matches each subdirectory processed.

For example, suppose you have files in the /home/data/orders directory on a remote host. You configure the origin to read files from the /home/data directory and its subdirectories. You also configure the origin to archive processed files to the /home/archive directory. After processing the files, the origin moves the files to the /home/archive/orders directory.

Note that your choice to specify the archive directory relative to the user's home directory is independent of your choice to specify the original location of the files relative to the user's home directory.

### Data Formats <a href="#ariaid-title10" id="ariaid-title10"></a>

The SFTP/FTP/FTPS Client origin processes data differently based on the data format. SFTP/FTP/FTPS Client processes the following types of data:

Avro

Generates a record for every Avro record. Includes a `precision` and `scale` field attribute for each Decimal field.

The stage includes the Avro schema in an `avroSchema` record header attribute. You can use one of the following methods to specify the location of the Avro schema definition:

* Message/Data Includes Schema - Use the schema in the file.
* In Pipeline Configuration - Use the schema that you provide in the stage configuration properties.
* Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the stage to look up the schema in Confluent Schema Registry by the schema ID or subject specified in the stage configuration.

Using a schema in the stage configuration or retrieving a schema from Confluent Schema Registry overrides any schema that might be included in the file and can improve performance.The stage reads files compressed by Avro-supported compression codecs without requiring additional configuration. To enable the stage to read files compressed by other codecs, use the compression format property in the stage.

Delimited

Generates a record for each delimited line.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record.You can also configure the stage to replace a string constant with null values and to ignore control characters.

Excel

Generates a record for every row in the file. Can process `.xls` or `.xlsx` files.

You can specify whether files include a header row and whether to ignore the header row. A header row must be the first row of a file. Vertical header columns are not recognized.

The origin cannot process Excel files with large numbers of rows. You can save such files as CSV files in Excel, and then use the origin to process with the delimited data format.

JSON

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array.When an object exceeds the maximum object length defined for the origin, the origin cannot continue processing data in the file. Records already processed from the file are passed to the pipeline. The behavior of the origin is then based on the error handling configured for the stage:

* Discard - The origin continues processing with the next file, leaving the partially-processed file in the directory.
* To Error - The origin continues processing with the next file. If a post-processing error directory is configured for the stage, the origin moves the partially-processed file to the error directory. Otherwise, it leaves the file in the directory.
* Stop Pipeline - The origin stops the pipeline.

Log

Generates a record for every log line.When a line exceeds the user-defined maximum line length, the origin truncates longer lines.You can include the processed log line as a field in the record. If the log line is truncated, and you request the log line in the record, the origin includes the truncated line.You can define the log format or type to be read.

Protobuf

Generates a record for every protobuf message.Protobuf messages must match the specified message type and be described in the descriptor file.When the data for a record exceeds 1 MB, the origin cannot continue processing data in the file. The origin handles the file based on file error handling properties and continues reading the next file.

Record

Generates a record for every record. Use to process records generated by a pipeline using the Record data format.For error records, the origin provides the original record as read from the origin in the original pipeline, as well as error information that you can use to correct the record.When processing error records, the origin expects the error file names and contents as generated by the original pipeline.

Text

Generates a record for each line of text or for each section of text based on a custom delimiter.When a line or section exceeds the maximum line length defined for the origin, the origin truncates it. The origin adds a boolean field named Truncated to indicate if the line was truncated.

Whole File

Streams whole files from the origin system to the destination system. You can specify a transfer rate or use all available resources to perform the transfer.

The origin generates two fields: one for a file reference and one for file information.&#x20;

XML

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.

Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

Note: Field attributes and record header attributes are written to destination systems automatically only when you use the SDC RPC data format in destinations.

When a record exceeds the user-defined maximum record length, the origin cannot continue processing data in the file. Records already processed from the file are passed to the pipeline. The behavior of the origin is then based on the error handling configured for the stage:

* Discard - The origin continues processing with the next file, leaving the partially-processed file in the directory.
* To Error - The origin continues processing with the next file. If a post-processing error directory is configured for the stage, the origin moves the partially-processed file to the error directory. Otherwise, it leaves the file in the directory.
* Stop Pipeline - The origin stops the pipeline.

Use the XML data format to process valid XML documents.&#x20;

**Tip**: If you want to process invalid XML documents, you can try using the text data format with custom delimiters.&#x20;
