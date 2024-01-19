# Data in Motion

Data passes through the pipeline in batches. This is how it works:

The origin creates a batch as it reads data from the origin system or as data arrives from the origin system, noting the offset. The offset is the location where the origin stops reading.

The origin sends the batch when the batch is full or when the batch wait time limit elapses. The batch moves through the pipeline from processor to processor until it reaches pipeline destinations.

Destinations write the batch to destination systems, and Data Pipelines commits the offset internally. Based on the pipeline delivery guarantee, Data Pipelines either commits the offset as soon as it writes to any destination system or after receiving confirmation of the write from all destination systems. After the offset commit, the origin stage creates a new batch.

Note that this describes general pipeline behavior. Behavior can differ based on the specific pipeline configuration. For example, for the Kafka Consumer, the offset is stored in Kafka or ZooKeeper. And for origin systems that do not store data, such as Omniture and HTTP Client, offsets are not stored because they aren't relevant.

### Single and Multithreaded Pipelines

The information above describes a standard single-threaded pipeline - the origin creates a batch and passes it through the pipeline, creating a new batch only after processing the previous batch.

Some origins can generate multiple threads to enable parallel processing in multithreaded pipelines. In a multithreaded pipeline, you configure the origin to create the number of threads or amount of concurrency that you want to use. And Data Pipelines creates a number of pipeline runners based on the pipeline Max Runners property to perform pipeline processing. Each thread connects to the origin system, creates a batch of data, and passes the batch to an available pipeline runner.

Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread. When the flow of data slows, the pipeline runners wait idly until they are needed, generating an empty batch at regular intervals. You can configure the Runner Idle Time pipeline property to specify the interval or to opt out of empty batch generation.

All general references to pipelines in this guide describe single-threaded pipelines, but this information generally applies to multithreaded pipelines. For more details specific to multithreaded pipelines, see Multithreaded Pipeline Overview.

### Delivery Guarantee

When you configure a pipeline, you define how you want data to be treated: Do you want to prevent the loss of data or the duplication of data?‌

The Delivery Guarantee pipeline property offers the following choices:‌

#### At least once

Ensures that the pipeline processes all data.‌

If a failure causes Data Pipelines to stop while processing a batch of data, when it restarts, it reprocesses the batch. This option ensures that no data is lost.‌

With this option, Data Pipelines commits the offset after receiving write confirmation from destination systems. If a failure occurs after Data Pipelines passes data to destination systems but before receiving confirmation and committing the offset, up to one batch data might be duplicated in destination systems.‌

#### At most once

Ensures that data is not processed more than once.‌

If a failure causes Data Pipelines to stop while processing a batch of data, when it starts up, it begins processing with the next batch of data. This option avoids the duplication of data in destinations due to reprocessing.‌

With this option, Data Pipelines commits the offset after a write without waiting for confirmation from destination systems. If a failure occurs after Data Pipelines passes data to destinations and commits the offset, up to one batch of data might not get written to the destination systems.‌

### Data Types

Within a pipeline, Data Pipelines uses the following data types, converting data as necessary when reading from or writing to external systems:

| Data Type       | Description                                                                                                                                                            |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| BOOLEAN         | True or False. Corresponds to the Boolean Java data type.                                                                                                              |
| BYTE            | 8-bit signed whole number. Ranges from -128 to 127. Corresponds to the Byte Java data type.                                                                            |
| BYTE\_ARRAY     | Array of byte values. Corresponds to the Byte\[] Java data type.                                                                                                       |
| CHAR            | Stores a single Unicode character. Corresponds to the Char Java data type.                                                                                             |
| DATE            | Time object that includes the day, month, and four-digit year. Does not include time zone.                                                                             |
| DATETIME        | Time object that includes the day, month, four-digit year, hours, minutes, and seconds. Precise to the millisecond. Does not include time zone.                        |
| DECIMAL         | Arbitrary-precision signed decimal numbers. Corresponds to the BigDecimal Java class.                                                                                  |
| DOUBLE          | 64-bit double-precision IEEE 754 floating point. Corresponds to the Double Java data type.                                                                             |
| FILE\_REF       | Internal type for whole file format.                                                                                                                                   |
| FLOAT           | 32-bit single-precision IEEE 754 floating point. Corresponds to the Float Java data type.                                                                              |
| INTEGER         | 32-bit signed whole number. Corresponds to the Int Java data type.                                                                                                     |
| LIST            | Nested type. Contains an enumerated list of values. Values can be any type, and each value in a list can be a different type.                                          |
| LIST\_MAP       | Nested type. Contains a list of key-value pairs kept in a specific order. Keys are always strings. Values can be any type, and each value can be a different type.     |
| LONG            | 64-bit signed whole number. Ranges from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807. Corresponds to the Long Java data type.                               |
| MAP             | Nested type. Contains a list of key-value pairs. Keys are always strings. Values can be any type, and each value can be a different type.                              |
| SHORT           | 16-bit signed whole number. Ranges from -32,768 to 32767. Corresponds to the Short Java data type.                                                                     |
| STRING          | Text. Corresponds to the String Java class.                                                                                                                            |
| TIME            | Time object that includes the hours, minutes, and seconds. Precise to the millisecond. Does not include the time zone.                                                 |
| ZONED\_DATETIME | Time object that includes day, month, four-digit year, hours, minutes, seconds, and time zone. Precise to the nanosecond. Corresponds to the ZonedDateTime Java class. |
