# Azure Data Lake Storage Gen1

The Azure Data Lake Storage Gen1 destination writes data to Microsoft Azure Data Lake Storage Gen1. You can use the Azure Data Lake Storage Gen1 destination in standalone and cluster batch pipelines. To write to Azure Data Lake Storage Gen2, use the Azure Data Lake Storage Gen2 destination.&#x20;

Before you use the destination, you must perform some prerequisite tasks.

When you configure the Azure Data Lake Storage Gen1 destination, you specify information to connect to Azure. You define a directory template and time basis to determine the output directories that the destination creates and the files where records are written.

You can define a file prefix and suffix, the data time zone, and properties that define when the destination closes a file. You can specify the amount of time that a record can be written to its associated directory and what happens to late records.

When desired, you can write records, use the defined Avro schema, and roll files based on record header attributes. For more information, see Record Header Attributes for Record-Based Writes.

You can use Gzip, Bzip2, Snappy, LZ4, and other compression formats to write output files.

The destination can generate events for an event stream. For more information about the event framework, see Dataflow Triggers Overview.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Complete the following prerequisites before you configure the Azure Data Lake Storage Gen1 destination:

1.  If necessary, create a new Azure Active Directory application for Data Pipelines.

    For information about creating a new application, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal).
2.  Ensure that the Azure Active Directory Data Pipelines application has the appropriate access control to perform the necessary tasks.

    The Data Pipelines application requires Write and Execute permissions to write data to Azure. If also reading from Azure, the application requires Read permission as well.

    For information about configuring Gen1 access control, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-security-overview).
3.  If necessary, create an Azure Data Lake Storage Gen1 storage account.

    For information about creating an account, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal#create-a-data-lake-storage-gen1-account).
4.  Create a folder in the storage account. The destination writes data to the folder.

    For information about creating a folder, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal#createfolder).
5. Retrieve information from Azure to configure the destination.

After you complete all of the prerequisite tasks, you can configure the Azure Data Lake Storage Gen1 destination.

#### Retrieve Authentication Information <a href="#ariaid-title3" id="ariaid-title3"></a>

Azure Data Lake Storage Gen1 destination connects to Azure using Azure Active Directory service principal authentication, also known as service-to-service authentication.

The destination requires the following Azure authentication information:

*   Application ID - Application ID for the Azure Active Directory Data Pipelines application. Also known as the client ID.

    For information on accessing the application ID from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key).
* Auth Token Endpoint - OAuth 2.0 token endpoint for the Azure Active Directory v1.0 application for Data Pipelines. For example: `https://login.microsoftonline.com/<uuid>/oauth2/token.`
*   Application Key - Authentication key or client secret for the Azure Active Directory application. Also known as the client secret.

    For information on accessing the application key from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key).

### Directory Templates <a href="#ariaid-title4" id="ariaid-title4"></a>

By default, the Azure Data Lake Storage Gen1 destination uses directory templates to create output directories. The destination writes records to the directories based on the configured time basis.

You can alternatively write records to directories based on the `targetDirectory` record header attribute. Using the `targetDirectory` attribute disables the ability to define directory templates.

When you define a directory template, you can use a mix of constants, field values, and datetime variables. You can use the `every` function to create new directories at regular intervals based on hours, minutes, or seconds, starting on the hour. You can also use the `record:valueOrDefault` function to create new directories from field values or a default in the directory template.

For example, the following directory template creates output directories for event data based on the state and timestamp of a record with hours as the smallest unit of measure, creating a new directory every hour:

```
 /outputfiles/${record:valueOrDefault("/State", "unknown")}/${YY()}-${MM()}-${DD()}-${hh()}
```

You can use the following elements in a directory template:ConstantsYou can use any constant, such as `output`.

Datetime Variables

You can use datetime variables, such as `${YYYY()}` or `${DD()}`. The destination creates directories as needed, based on the smallest datetime variable that you use. For example, if the smallest variable is hours, then the directories are created for every hour of the day that receives output records.

When you use datetime variables in an expression, use all of the datetime variables between one of the year variables and the smallest variable that you want to use. Do not skip a variable within the progression. For example, to create directories on a daily basis, use a year variable, a month variable, and then a day variable. You might use one of the following datetime variable progressions:

```
${YYYY()}-${MM()}-${DD()}
${YY()}_${MM()}_${DD()}
```

For details about datetime variables, see Datetime Variables.

`every` function

You can use the `every` function in a directory template to create directories at regular intervals based on hours, minutes, or seconds, beginning on the hour. The intervals should be a submultiple or integer factor of 60. For example, you can create directories every 15 minutes or 30 seconds.

Use the `every` function to replace the smallest datetime variable used in the template.For example, the following directory template creates directories every 5 minutes, starting on the hour:

```
/HDFS_output/${YYYY()}-${MM()}-${DD()}-${hh()}-${every(5,mm())}
```

For details about the `every` function, see Miscellaneous Functions.

`record:valueOrDefault` function

You can use the `record:valueOrDefault` function in a directory template to create directories with the value of a field or the specified default value if the field does not exist or if the field is null:

```
${record:valueOrDefault(<field path>, <default value>)}
```

For example, the following directory template creates a directory based on the `Product` field every day, and if the `Product` field is empty or null, uses `Misc` in the directory path:

```
/${record:valueOrDefault("/Product", "Misc")}/${YY()}-${MM()}-${DD()}
```

This template might create the following paths:

```
/Shirts/2015-07-31 
/Misc/2015-07-31
```

### Time Basis <a href="#ariaid-title5" id="ariaid-title5"></a>

When using directory templates, the time basis helps determine when directories are created. It also determines the directory that the destination uses when writing a record, and whether a record is late.

You can use the following times as the time basis:

**Processing Time**

When you use processing time as the time basis, the destination creates directories based on the processing time and the directory template, and writes records to the directories based on when they are processed.For example, say a directory template creates directories every minute and the time basis is the time of processing. Then, directories are created for every minute that the destination writes output records. And the output records are written to the directory for that minute of processing.To use the processing time as the time basis, use the following expression: `${time:now()}`. This is the default time basis.

**Record Time**

When you use the time associated with a record as the time basis, you specify a Date field in the record. The destination creates directories based on the datetimes associated with the records and writes the records to the appropriate directories.

For example, say a directory template creates directories every hour and the time basis is based on the record. Then, directories are created for every hour associated with output records and the destination writes the records to the related output directory.To use a time associated with the record, use an expression that calls a field and resolves to a datetime value, such as `${record:value("/Timestamp")}`.

### Late Records and Late Record Handling <a href="#ariaid-title6" id="ariaid-title6"></a>

When you use a record time as the time basis, you can define a time limit for records to be written to their associated output file. When the destination creates a new output file in a new directory, the previous output file is kept open for the specified late record time limit. When records that belong in that file arrive within the time limit, the destination writes the records to the open output file. When the late record time limit is reached, the output file is closed and any record that arrives past this limit is considered late.

Tip: The late records properties are not applicable if you use processing time as the time basis. If you use processing time, set the late record time limit to one second.

You can send late records to a late records file or to the stage for error handling. When you send records to a late records file, you define a late records directory template.

For example, you use a record time as the time basis, configure a one-hour late record time limit, configure late records to be sent to the stage for error handling, and use the default directory template value:

```
/tmp/out/${YYYY()}-${MM()}-${DD()}-${hh()} 
```

The first records that arrive have a datetime between the hours of 02:00 and 02:59, and so are written to an output file in the 02 directory. When records with a datetime between the hours of 03:00 and 03:59 arrive, the destination creates a new file in an 03 directory. The destination keeps the file in the 02 directory open for another hour.

If a record with a datetime between the hours of 02:00 and 02:59 arrives before the hour time limit, the destination writes the record to the open file in the 02 directory. After one hour, the destination closes the output file in the 02 directory. Any records with a datetime between the hours of 02:00 and 02:59 that arrive after the one-hour time limit are considered late. The late records are sent to the stage for error handling.

### Timeout to Close Idle Files <a href="#ariaid-title7" id="ariaid-title7"></a>

You can configure the maximum time that an open output file can remain idle. After no records are written to an output file for the specified amount of time, the destination closes the file.

You might want to configure an idle timeout when output files remain open and idle for too long, thus delaying another system from processing the files.

Output files might remain idle for too long for the following reasons:

* You configured the maximum number of records to be written to output files or the maximum size of output files, but records have stopped arriving. An output file that has not reached the maximum number of records or the maximum file size stays open until more records arrive.
*   You configured a date field in the record as the time basis and have configured a late record time limit, but records arrive in chronological order. When a new directory is created, the output file in the previous directory remains open for the configured late record time limit. However, no records are ever written to the open file in the previous directory.

    For example, when a record with a datetime of 03:00 arrives, the destination creates a new file in a new 03 directory. The previous file in the 02 directory is kept open for the late record time limit, which is an hour by default. However, when records arrive in chronological order, no records that belong in the 02 directory arrive after the 03 directory is created.

In either situation, configure an idle timeout so that other systems can process the files sooner, instead of waiting for the configured maximum records, maximum file size, or late records conditions to occur.

### Recovery <a href="#ariaid-title8" id="ariaid-title8"></a>

The Azure Data Lake Storage Gen1 destination supports recovery after an unexpected stop of the pipeline by renaming temporary files when the pipeline restarts.

The destination names temporary open output files using the following format:

```
_tmp_<prefix>_<runnerId>
```

Where `<prefix>` is the file prefix defined for the destination and `<runnerId>` is the ID of the pipeline runner performing the pipeline processing. For example, when the destination prefix is defined as `sdc` and the destination runs from a single-threaded pipeline, the temporary file is named like so: `_tmp_sdc_0`.When the destination closes the file, either after it is fully written, after the idle timeout expires, or when you deliberately stop the pipeline, it renames the file to remove the `_tmp_` string and to replace the pipeline runner ID with a random unique identifier like so:

```
<prefix>_e7ce67c5-013d-47a7-9496-8c882ddb28a0
```

However, when the pipeline stops unexpectedly, the temporary files remain. When the pipeline restarts, the destination scans all subdirectories of the defined directory template to rename any temporary files that match the defined prefix for the destination. After the destination renames the temporary files, it continues writing to new output files.

Note: The destination renames all temporary files that match the defined prefix in all subdirectories of the defined directory template, even if those files were not written by that pipeline. So if you happen to have another file whose name begins with the same pattern - `_tmp_<prefix>` - the destination renames that file also.The destination might not rename all temporary files in the following situations:The directory template includes an expression with the record:value or record:valueOrDefault function.If the record:value or record:valueOrDefault function evaluates to an empty string or to a subdirectory, the destination cannot determine those locations when the pipeline restarts. As a result, the destination cannot rename any temporary files written to those locations.For example, let’s assume that the directory template is defined as follows:

```
/tmp/out/${YY()}-${MM()}-${DD()}/${sdc:hostname()}/${record:value('/a')}/${record:value('/b')}
```

If the expression `${record:value('/b')}` evaluates to an empty string or to a subdirectory such as `/folder1/folder2`, then the destination cannot determine those locations when the pipeline restarts.The directory is defined in the targetDirectory record header attribute.When the directory is defined in the targetDirectory record header attribute, the destination cannot determine where to look for temporary files when the pipeline restarts. As a result, it cannot rename the temporary files.

In either of these situations, you must manually rename the temporary files.

File recovery can slow down the pipeline as it restarts. If needed, you can configure the destination to skip file recovery.

### Event Generation <a href="#ariaid-title9" id="ariaid-title9"></a>

The Azure Data Lake Storage Gen1 destination can generate events that you can use in an event stream. When you enable event generation, the Azure Data Lake Storage Gen1 destination generates event records each time the destination completes writing to an output file or completes streaming a whole file.

The events can be used in any logical way. For example:

* With an ADLS Gen1 File Metadata executor to change file metadata, create an empty file, or remove a file or directory after closing a file.
* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title10" id="ariaid-title10"></a>

The Azure Data Lake Storage Gen1 destination event records include the following event-related record header attributes. Record header attributes are stored as String values.

| Record Header Attribute       | Description                                                                                                                                                                                                                    |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>file-closed - Generated when the destination closes a file.</li><li>wholeFileProcessed - Generated when the destination completes streaming a whole file.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                   |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                              |

The destination can generate the following types of event records:File closureThe destination generates a file closure event record when it closes an output file.File closure event records have the `sdc.event.type` record header attribute set to `file-closed` and include the following fields:

| Field    | Description                       |
| -------- | --------------------------------- |
| filepath | Absolute path to the closed file. |
| filename | File name of the closed file.     |
| length   | Size of the closed file in bytes. |

Whole file processedThe destination generates an event record when it completes streaming a whole file. Whole file event records have the `sdc.event.type` record header attribute set to `wholeFileProcessed` and have the following fields:

| Field             | Description                                                                                                                                                                                                                                        |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sourceFileInfo    | <p>A map of attributes about the original whole file that was processed. The attributes include:</p><ul><li>size - Size of the whole file in bytes.</li></ul><p>Additional attributes depend on the information provided by the origin system.</p> |
| targetFileInfo    | <p>A map of attributes about the whole file written to the destination. The attributes include:</p><ul><li>path - An absolute path to the processed whole file.</li></ul>                                                                          |
| checksum          | <p>Checksum generated for the written file.</p><p>Included only when you configure the destination to include checksums in the event record.</p>                                                                                                   |
| checksumAlgorithm | <p>Algorithm used to generate the checksum.</p><p>Included only when you configure the destination to include checksums in the event record.</p>                                                                                                   |

### Resolving Out of Memory Errors <a href="#ariaid-title11" id="ariaid-title11"></a>

The Azure Data Lake Storage Gen1 destination can encounter out of memory errors when the available memory is not sufficient for writing output files. Typically, this occurs when writing large files, but can occur with smaller files when sufficient memory is not available. This is due to a known issue with Hadoop.

If the destination encounters out of memory errors, you can resolve the issue by setting the Azure configuration property, `fs.azure.write.request.size`, as follows:

1. On the Delta Lake tab of the destination, add `fs.azure.write.request.size` to the Additional Configuration property.
2.  Determine the maximum amount of memory that you want the destination to use, then divide that by a number based on the cores on the Data Pipelines machine, as follows:

    ```
    <max destination memory> / (12 * <Data Pipelines cores>)
    ```

    If the destination is in a multithreaded pipeline, then you must also divide by the number of threads used by the pipeline, as follows:

    ```
    (<max destination memory> / <number of threads>) / (12 * <Data Pipelines cores>) 
    ```
3. Set the `fs.azure.write.request.size` property to the results of your calculation.

For more information about the Hadoop issue, HADOOP-16854, see the [Hadoop JIRA project](https://issues.apache.org/jira/browse/HADOOP-16854).

### Data Formats <a href="#ariaid-title12" id="ariaid-title12"></a>

The Azure Data Lake Storage Gen1 destination writes data to Azure Data Lake Storage Gen1 based on the data format that you select. You can use the following data formats:

**Avro**

The destination writes records based on the Avro schema. You can use one of the following methods to specify the location of the Avro schema definition:

* In Pipeline Configuration - Use the schema that you provide in the stage configuration.
* In Record Header - Use the schema included in the avroSchema record header attribute.
*   Confluent Schema Registry - Retrieve the schema from Confluent Schema Registry. Confluent Schema Registry is a distributed storage layer for Avro schemas. You can configure the destination to look up the schema in Confluent Schema Registry by the schema ID or subject.

    If using the Avro schema in the stage or in the record header attribute, you can optionally configure the destination to register the Avro schema with Confluent Schema Registry.

The destination includes the schema definition in each file.You can compress data with an Avro-supported compression codec. When using Avro compression, avoid using other compression properties in the destination.

**Binary**

The stage writes binary data to a single field in the record.

**Delimited**

The destination writes records as delimited data. When you use this data format, the root field must be list or list-map.You can use the following delimited format types:

* Default CSV - File that includes comma-separated values. Ignores empty lines in the file.
* RFC4180 CSV - Comma-separated file that strictly follows RFC4180 guidelines.
* MS Excel CSV - Microsoft Excel comma-separated file.
* MySQL CSV - MySQL comma-separated file.
* Tab-Separated Values - File that includes tab-separated values.
* PostgreSQL CSV - PostgreSQL comma-separated file.
* PostgreSQL Text - PostgreSQL text file.
* Custom - File that uses user-defined delimiter, escape, and quote characters.
* Multi Character Delimited - File that uses multiple user-defined characters to delimit fields and lines, and single user-defined escape and quote characters.

**JSON**

The destination writes records as JSON data. You can use one of the following formats:

* Array - Each file includes a single array. In the array, each element is a JSON representation of each record.
* Multiple objects - Each file includes multiple JSON objects. Each object is a JSON representation of a record.

**Protobuf**

Writes a batch of messages in each file.Uses the user-defined message type and the definition of the message type in the descriptor file to generate the messages in the file.For information about generating the descriptor file, see [Protobuf Data Format Prerequisites](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Data\_Formats/Protobuf-Prerequisites.html).

**Record**

The destination writes records in the Record data format.

**Text**

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.

When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

**Whole File**

Streams whole files to the destination system. The destination writes the data to the file and location defined in the stage. If a file of the same name already exists, you can configure the destination to overwrite the existing file or send the current file to error.

By default, written files use the default access permissions for the destination system. You can specify an expression that defines access permissions.For more information about the whole file data format, see Whole File Data Format.
