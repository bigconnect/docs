# Azure Data Lake Storage Gen1

The Azure Data Lake Storage Gen1 origin reads data from Microsoft Azure Data Lake Storage Gen1. The origin can create multiple threads to enable parallel processing in a multithreaded pipeline. To read data from Microsoft Azure Data Lake Storage Gen2, use the Azure Data Lake Storage Gen2 origin. For information about supported versions, see Supported Systems and Versions.

The files to be processed must all share a file name pattern and be fully written. Use the origin only in pipelines configured for standalone execution mode.

When you configure the Azure Data Lake Storage Gen1 origin, you define the directory to use, the read order, the file name pattern, the file name pattern mode, and the first file to process. You can use glob patterns or regular expressions to define the file name pattern that you want to use.

You can configure the origin to read from subdirectories when the origin reads files by last modified timestamp. To use multiple threads for processing, specify the number of threads to use.

You can also enable reading compressed files. After processing a file, the Azure Data Lake Storage Gen1 origin can keep, archive, or delete the file.

When the pipeline stops, the Azure Data Lake Storage Gen1 origin notes where it stops reading. When the pipeline starts again, the origin continues processing from where it stopped by default. You can reset the origin to process all requested files.

The origin generates record header attributes that enable you to use the origins of a record in pipeline processing.

The origin can generate events for an event stream. For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

## Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Complete the following prerequisites before you configure the Azure Data Lake Storage Gen1 origin:

1.  If necessary, create a new Azure Active Directory application for BigConnect Pipelines.

    For information about creating a new application, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal).
2.  Ensure that the Azure Active Directory BigConnect Pipelines application has the appropriate access control to perform the necessary tasks.

    The BigConnect Pipelines application requires Read and Execute permissions to read data in Azure. If also writing to Azure, the application requires Write permission as well.

    For information about configuring Gen1 access control, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-security-overview).
3. Retrieve information from Azure to configure the origin.

After you complete all of the prerequisite tasks, you can configure the Azure Data Lake Storage Gen1 origin.

### Retrieve Authentication Information <a href="#ariaid-title3" id="ariaid-title3"></a>

Azure Data Lake Storage Gen1 origin connects to Azure using Azure Active Directory service principal authentication, also known as service-to-service authentication.

The origin requires the following Azure authentication information:

*   Application ID - Application ID for the Azure Active Directory BigConnect Pipelines application. Also known as the client ID.

    For information on accessing the application ID from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key).
* Auth Token Endpoint - OAuth 2.0 token endpoint for the Azure Active Directory v1.0 application for BigConnect Pipelines. For example: `https://login.microsoftonline.com/<uuid>/oauth2/token.`
*   Application Key - Authentication key or client secret for the Azure Active Directory application. Also known as the client secret.

    For information on accessing the application key from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key).

## File Directory <a href="#ariaid-title4" id="ariaid-title4"></a>

To define the directory that the Azure Data Lake Storage Gen1 origin reads files from, enter an absolute directory. Use a glob pattern to include wildcards and define multiple directories to read files from.

For example, suppose you have the following folders:

```
/hr/employees/CA/SFO
/hr/employees/CA/SJC
/hr/employees/CA/LAX
/hr/employees/WA/SEA
```

When defining the directory, you can include wildcards to have the origin read files from different combinations of folders. The following table shows some possibilities:

| Folders to Read                                                                   | File Directory Defined |
| --------------------------------------------------------------------------------- | ---------------------- |
| <p>/hr/employees/CA/SFO</p><p>/hr/employees/CA/SJC</p><p>/hr/employees/CA/LAX</p> | /hr/employees/CA/\*    |
| <p>/hr/employees/CA/SFO</p><p>/hr/employees/CA/SJC</p>                            | /hr/employees/CA/S\*   |
| <p>/hr/employees/CA/SFO</p><p>/hr/employees/CA/SJC</p><p>/hr/employees/WA/SEA</p> | /hr/employees/\*/S\*   |

For more information about glob patterns, see the [Oracle Java documentation](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob).

## File Name Pattern and Mode <a href="#ariaid-title5" id="ariaid-title5"></a>

Use a file name pattern to define the files that the Azure Data Lake Storage Gen1 origin processes. You can use either a glob pattern or a regular expression to define the file name pattern.

The Azure Data Lake Storage Gen1 origin processes files based on the file name pattern mode, file name pattern, and specified directory. For example, if you specify a `/logs/weblog/` directory, glob mode, and `*.json` as the file name pattern, the origin processes all files with the `json` extension in the `/logs/weblog/` directory.

The origin processes files in order based on the specified read order.

For more information about glob syntax, see the [Oracle Java documentation](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob). For more information about regular expressions, see Regular Expressions Overview.

## Read Order <a href="#ariaid-title6" id="ariaid-title6"></a>

The Azure Data Lake Storage Gen1 origin reads files in ascending order based on the timestamp or file name:

Last Modified TimestampThe Azure Data Lake Storage Gen1 origin can read files in ascending order based on the last modified timestamp associated with the file. When the origin reads from a secondary location - not the directory where the files are created and written - the last-modified timestamp should be when the file is moved to the directory to be processed.Tip: Avoid moving files using commands that preserve the existing timestamp, such as `cp -p`. Preserving the existing timestamp can be problematic in some cases, such as moving files across time zones.When ordering based on timestamp, any files with the same timestamp are read in lexicographically ascending order based on the file names.For example, when reading files with the `log*.json` file name pattern, the origin reads the following files in the following order:

| `File Name`    | `Last Modified`        |
| -------------- | ---------------------- |
| `log-1.json`   | `APR 24 2016 14:03:35` |
| `log-903.json` | `APR 24 2016 14:05:03` |
| `log-2.json`   | `APR 24 2016 14:45:11` |
| `log-3.json`   | `APR 24 2016 14:45:11` |

Notice, the log-2.json and log-3.json files have identical timestamps, and so are processed in lexicographically ascending order based on their file names.Lexicographically Ascending File NamesThe Azure Data Lake Storage Gen1 origin can read files in lexicographically ascending order based on file names. Note that lexicographically ascending order reads the numbers 1 through 11 as follows:

```
1, 10, 11, 2, 3, 4... 9
```

For example, when reading files with the `web*.log` file name pattern, the Azure Data Lake Storage Gen1 origin reads the following files in the following order:

```
web-1.log
web-10.log
web-11.log
web-2.log
web-3.log
web-4.log
web-5.log
web-6.log
web-7.log
web-8.log
web-9.log
```

To read these files in logical and lexicographically ascending order, you might add leading zeros to the file naming convention as follows:

```
web-0001.log
web-0002.log
web-0003.log
...
web-0009.log
web-0010.log
web-0011.log
```

## Multithreaded Processing <a href="#ariaid-title7" id="ariaid-title7"></a>

The Azure Data Lake Storage Gen1 origin uses multiple concurrent threads to process data based on the Number of Threads property.

Each thread reads data from a single file, and each file can have a maximum of one thread read from it at a time. The file read order is based on the configuration for the [Read Order property](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Origins/ADLS-G1.html#concept\_ogz\_q3h\_ldb).

As the pipeline runs, each thread connects to the origin system, creates a batch of data, and passes the batch to an available pipeline runner. A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin.

Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since batches are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

For example, say you configure the origin to read files from a directory using five threads and the Last Modified Timestamp read order. When you start the pipeline, the origin creates five threads, and BigConnect Pipelines creates a matching number of pipeline runners.

The Azure Data Lake Storage Gen1 origin assigns a thread to each of the five oldest files in the directory. Each thread processes its assigned file, passing batches of data to the origin. Upon receiving data, the origin passes a batch to each of the pipeline runners for processing.

After each thread completes processing a file, it continues to the next file based on the last-modified timestamp, until all files are processed.

For more information about multithreaded pipelines, see Multithreaded Pipeline Overview.

## Reading from Subdirectories <a href="#ariaid-title8" id="ariaid-title8"></a>

When using the Last Modified Timestamp read order, the Azure Data Lake Storage Gen1 origin can read files in subdirectories of the specified file directory.

When you configure the origin to read from subdirectories, it reads files from all subdirectories. It reads files in ascending order based on timestamp, regardless of the location of the file within the directory.

For example, say you configure the Azure Data Lake Storage Gen1 origin to read from the /logs/ file directory, select the Last Modified Timestamp read order, and enable reading from subdirectories. The Azure Data Lake Storage Gen1 origin reads the following files in the following order based on timestamp, even though the files are written to different subdirectories.

| `File Name`     | `Directory`   | `Last Modified Timestamp` |
| --------------- | ------------- | ------------------------- |
| `log-1.json`    | `/logs/west/` | `APR 24 2016 14:03:35`    |
| `log-0054.json` | `/logs/east/` | `APR 24 2016 14:05:03`    |
| `log-0055.json` | `/logs/west/` | `APR 24 2016 14:45:11`    |
| `log-2.json`    | `/logs/`      | `APR 24 2016 14:45:11`    |

### Post-Processing Subdirectories <a href="#ariaid-title9" id="ariaid-title9"></a>

When the Azure Data Lake Storage Gen1 origin reads from subdirectories, it uses the subdirectory structure when archiving files during post-processing.

You can archive files when the origin completes processing a file or when it cannot fully process a file.

For example, say you configure the origin to archive processed files to a "processed" archive directory. After successfully reading the files in the example above, it writes them to the following directories:

| `File Name`     | `Archive Directory`     |
| --------------- | ----------------------- |
| `log-1.json`    | `/processed/logs/west/` |
| `log-0054.json` | `/processed/logs/east/` |
| `log-0055.json` | `/processed/logs/west/` |
| `log-2.json`    | `/processed/logs/`      |

## First File for Processing <a href="#ariaid-title10" id="ariaid-title10"></a>

Configure a first file for processing when you want the Azure Data Lake Storage Gen1 origin to ignore one or more existing files in the directory.

When you define a first file to process, the Azure Data Lake Storage Gen1 origin starts processing with the specified file and continues based on the read order and file name pattern. When you do not specify a first file, the origin processes all files in the directory that match the file name pattern.

For example, say the Azure Data Lake Storage Gen1 origin reads files based on last-modified timestamp. To ignore all files older than a particular file, use that file name as the first file to process.

Similarly, say you have the origin reading files based on lexicographically ascending file names, and the file directory includes the following files: web\_001.log, web\_002.log, web\_003.log.

If you configure web\_002.log as the first file, the origin reads web\_002.log and continues to web\_003.log. It skips web\_001.log.

## Record Header Attributes <a href="#ariaid-title11" id="ariaid-title11"></a>

The Azure Data Lake Storage Gen1 origin creates record header attributes that include information about the originating file for the record.

When the origin processes Avro data, it includes the Avro schema in an avroSchema record header attribute.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes. For more information about working with record header attributes, see Working with Header Attributes.

The Azure Data Lake Storage Gen1 origin creates the following record header attributes:

* avroSchema - When processing Avro data, provides the Avro schema.
* baseDir - Base directory containing the file where the record originated.
* filename - Provides the name of the file where the record originated.
* file - Provides the file path and file name where the record originated.
* mtime - Provides the last-modified time for the file.
* offset - Provides the file offset in bytes. The file offset is the location in the file where the record originated.

## Event Generation <a href="#ariaid-title12" id="ariaid-title12"></a>

The Azure Data Lake Storage Gen1 origin can generate events that you can use in an event stream. When you enable event generation, the origin generates event records each time the origin starts or completes reading a file. It can also generate events when it completes processing all available data and the configured batch wait time has elapsed.

Events generated by the Azure Data Lake Storage Gen1 origin can be used in any logical way. For example:

*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin continues processing from the last-saved offset unless you reset the origin.
* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

### Event Records <a href="#ariaid-title13" id="ariaid-title13"></a>

Event records generated by the Azure Data Lake Storage Gen1 origin have the following event-related record header attributes. Record header attributes are stored as String values:

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>new-file - Generated when the origin starts processing a new file.</li><li>finished-file - Generated when the origin completes processing a file.</li><li>no-more-data - Generated after the origin completes processing all available files and the number of seconds configured for Batch Wait Time has elapsed.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                                                                                            |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                                                                                       |

The Azure Data Lake Storage Gen1 origin can generate the following types of event records:

new-fileThe Azure Data Lake Storage Gen1 origin generates a new-file event record when it starts processing a new file.New-file event records have the `sdc.event.type` record header attribute set to `new-file` and include the following field:

| Event Record Field | Description                                                               |
| ------------------ | ------------------------------------------------------------------------- |
| filepath           | Path and name of the file that the origin started or finished processing. |

finished-fileThe Azure Data Lake Storage Gen1 origin generates a finished-file event record when it finishes processing a file.Finished-file event records have the `sdc.event.type` record header attribute set to `finished-file` and include the following fields:

| Event Record Field | Description                                                               |
| ------------------ | ------------------------------------------------------------------------- |
| filepath           | Path and name of the file that the origin started or finished processing. |
| record-count       | Number of records successfully generated from the file.                   |
| error-count        | Number of error records generated from the file.                          |

no-more-dataThe Azure Data Lake Storage Gen1 origin generates a no-more-data event record when the origin completes processing all available records and the number of seconds configured for Batch Wait Time elapses without any new files appearing to be processed.No-more-data event records have the `sdc.event.type` record header attribute set to `no-more-data` and include the following fields:

| Event Record Field | Description                                                                                                                      |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| record-count       | Number of records successfully generated since the pipeline started or since the last no-more-data event was created.            |
| error-count        | Number of error records generated since the pipeline started or since the last no-more-data event was created.                   |
| file-count         | Number of files the origin attempted to process. Can include files that were unable to be processed or were not fully processed. |

## Buffer Limit and Error Handling <a href="#ariaid-title14" id="ariaid-title14"></a>

The Azure Data Lake Storage Gen1 origin passes each record to a buffer. The size of the buffer determines the maximum size of the record that can be processed. Decrease the buffer limit when memory on the BigConnect Pipelines machine is limited. Increase the buffer limit to process larger records when memory is available.

When a record is larger than the specified limit, the Azure Data Lake Storage Gen1 origin processes the source file based on the stage error handling:DiscardThe origin discards the record and all remaining records in the file, and then continues processing the next file.Send to ErrorWith a buffer limit error, the origin cannot send the record to the pipeline for error handling because it is unable to fully process the record.

Instead, the origin creates a message stating that a buffer overrun error occurred. The message includes the file and offset where the buffer overrun error occurred. The information displays in the pipeline history and displays as an alert when you monitor the pipeline.

If an error directory is configured for the stage, the origin moves the file to the error directory and continues processing the next file.

Stop PipelineThe origin stops the pipeline and creates a message stating that a buffer overrun error occurred. The message includes the file and offset where the buffer overrun error occurred. The information displays as an alert and in the pipeline history.Note: You can also check the BigConnect Pipelines log file for error details.

## Data Formats <a href="#ariaid-title15" id="ariaid-title15"></a>

The Azure Data Lake Storage Gen1 origin processes data differently based on the data format. The origin processes the following types of data:

**Avro**

Generates a record for every Avro record. The origin includes the Avro schema in the `avroSchema` record header attribute. It also includes a `precision` and `scale` field attribute for each Decimal field.The origin expects each file to contain the Avro schema and uses the schema to process the Avro data.The origin reads files compressed by Avro-supported compression codecs without requiring additional configuration.

**Delimited**

Generates a record for each delimited line.The CSV parser that you choose determines the delimiter properties that you configure and how the stage handles parsing errors. You can specify if the data includes a header line and whether to use it. You can define the number of lines to skip before reading, the character set of the data, and the root field type to use for the generated record.You can also configure the stage to replace a string constant with null values and to ignore control characters.For more information about reading delimited data, see Reading Delimited Data.

**Excel**

Generates a record for every row in the file. Can process `.xls` or `.xlsx` files.

You can specify whether files include a header row and whether to ignore the header row. A header row must be the first row of a file. Vertical header columns are not recognized.

The origin cannot process Excel files with large numbers of rows. You can save such files as CSV files in Excel, and then use the origin to process with the delimited data format.

**JSON**

Generates a record for each JSON object. You can process JSON files that include multiple JSON objects or a single JSON array. When an object exceeds the maximum object length defined for the origin, the origin cannot continue processing data in the file. Records already processed from the file are passed to the pipeline. The behavior of the origin is then based on the error handling configured for the stage:

* Discard - The origin continues processing with the next file, leaving the partially-processed file in the directory.
* To Error - The origin continues processing with the next file. If a post-processing error directory is configured for the stage, the origin moves the partially-processed file to the error directory. Otherwise, it leaves the file in the directory.
* Stop Pipeline - The origin stops the pipeline.

**Log**

Generates a record for every log line.When a line exceeds the user-defined maximum line length, the origin truncates longer lines.You can include the processed log line as a field in the record. If the log line is truncated, and you request the log line in the record, the origin includes the truncated line.You can define the log format or type to be read.

**Protobuf**

Generates a record for every protobuf message.Protobuf messages must match the specified message type and be described in the descriptor file.When the data for a record exceeds 1 MB, the origin cannot continue processing data in the file. The origin handles the file based on file error handling properties and continues reading the next file.For information about generating the descriptor file, see Protobuf Data Format Prerequisites.

**Pipelines Record**

Generates a record for every record. Use to process records generated by a BigConnect pipeline using the Pipelines Record data format.For error records, the origin provides the original record as read from the origin in the original pipeline, as well as error information that you can use to correct the record.When processing error records, the origin expects the error file names and contents as generated by the original pipeline.TextGenerates a record for each line of text or for each section of text based on a custom delimiter.When a line or section exceeds the maximum line length defined for the origin, the origin truncates it. The origin adds a boolean field named Truncated to indicate if the line was truncated.For more information about processing text with a custom delimiter, see Text Data Format with Custom Delimiters.

**Whole File**

Streams whole files from the origin system to the destination system. You can specify a transfer rate or use all available resources to perform the transfer.The origin generates two fields: one for a file reference and one for file information.&#x20;

**XML**

Generates records based on a user-defined delimiter element. Use an XML element directly under the root element or define a simplified XPath expression. If you do not define a delimiter element, the origin treats the XML file as a single record.Generated records include XML attributes and namespace declarations as fields in the record by default. You can configure the stage to include them in the record as field attributes.You can include XPath information for each parsed XML element and XML attribute in field attributes. This also places each namespace in an xmlns record header attribute.

**Note**: Field attributes and record header attributes are written to destination systems automatically only when you use the Pipelines RPC data format in destinations. For more information about working with field attributes and record header attributes, and how to include them in records, see Field Attributes and Record Header Attributes.

When a record exceeds the user-defined maximum record length, the origin cannot continue processing data in the file. Records already processed from the file are passed to the pipeline. The behavior of the origin is then based on the error handling configured for the stage:

* Discard - The origin continues processing with the next file, leaving the partially-processed file in the directory.
* To Error - The origin continues processing with the next file. If a post-processing error directory is configured for the stage, the origin moves the partially-processed file to the error directory. Otherwise, it leaves the file in the directory.
* Stop Pipeline - The origin stops the pipeline.

Use the XML data format to process valid XML documents. For more information about XML processing, see Reading and Processing XML Data.

**Tip**: If you want to process invalid XML documents, you can try using the text data format with custom delimiters.

\
