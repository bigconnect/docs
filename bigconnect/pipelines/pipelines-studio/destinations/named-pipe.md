# Named Pipe

The Named Pipe destination writes data to a UNIX named pipe.

A named pipe (or FIFO) can be accessed by two separate processes on a machine - one process opens the pipe as the writer, and the other as a reader. The Named Pipe destination serves as the writer. Another process running on the same Data Pipelines machine can serve as the reader.

Use the destination to send data to an application that can read data from a named pipe. For example, the Greenplum `gpload` utility supports loading data from named pipes. You can configure the utility to read from the named pipe written to by the Named Pipe destination. The utility can then load that data into a Greenplum database.

Before you use the Named Pipe destination, you must create a named pipe on the machine where Data Pipelines is installed. You also must configure the named pipe reader - or reading application - that runs on the Data Pipelines machine to receive the data from the same named pipe.

When you configure the Named Pipe destination, you enter the full path to the local named pipe that you created. You also specify the data format that the destination uses to write data to the named pipe.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you can write to a named pipe, you must complete the following prerequisites:

* Create the named pipe on the local Data Pipelines machine.
* Configure the named pipe reader on the Data Pipelines local machine to read from the same named pipe.

#### Create the Named Pipe <a href="#ariaid-title3" id="ariaid-title3"></a>

Use the `mkfifo` command to create the named pipe on the same machine where Data Pipelines is installed.

For example, use the following command to create a local named pipe:

```
mkfifo /tmp/my_pipe
```

#### Configure the Named Pipe Reader <a href="#ariaid-title4" id="ariaid-title4"></a>

The named pipe reader - or reading application - must be installed on the Data Pipelines machine and must be configured to read from the same named pipe.

For example, configure the reader to read from the same named pipe that you created with the `mkfifo` command:

```
/tmp/my_pipe
```

As a best practice, we recommend starting the reader before starting the pipeline that contains the Named Pipe destination.

### Working with the Named Pipe Reader <a href="#ariaid-title5" id="ariaid-title5"></a>

The Named Pipe destination writes data to the named pipe, and the named pipe reader - or reading application - then reads the incoming data.

Consider the following ways that the Named Pipe destination and the named pipe reader interact:

*   If you start the pipeline before the named pipe reader is available to read, the pipeline remains in a STARTING state until the reader becomes available.

    As a best practice, we recommend starting the reader before starting the pipeline that contains the Named Pipe destination.
* When the named pipe reader becomes available, the pipeline transitions to a RUNNING state and the Named Pipe destination begins writing to the named pipe. If the destination writes faster than the reader can process the data, the named pipe might become full. In this case, the destination waits to write additional data until the named pipe can receive more data.
* If the pipeline stops while the named pipe reader is still available, the reader receives an IOException for the broken pipe.
* If the named pipe reader stops while the pipeline with the Named Pipe destination is running, Data Pipelines displays a stage exception about the broken pipe.

### Data Formats <a href="#ariaid-title6" id="ariaid-title6"></a>

The Named Pipe destination writes data to a named pipe based on the data format that you select. The destination supports data formats that produce a single line of output for each record. Data formats that produce a nested structure with multiple lines of output for each record - such as the XML data format - are not supported.

You can use the following data formats:

**Delimited**

The destination writes records as delimited data. When you use this data format, the root field must be list or list-map.

You can use the following delimited format types:

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

**Record**

The destination writes records in the SDC Record data format.

**Text**

The destination writes data from a single text field to the destination system. When you configure the stage, you select the field to use.You can configure the characters to use as record separators. By default, the destination uses a UNIX-style line ending (\n) to separate records.When a record does not contain the selected text field, the destination can report the missing field as an error or to ignore the missing field. By default, the destination reports an error.

When configured to ignore a missing text field, the destination can discard the record or write the record separator characters to create an empty line for the record. By default, the destination discards the record.

### &#x20;<a href="#ariaid-title7" id="ariaid-title7"></a>

\
