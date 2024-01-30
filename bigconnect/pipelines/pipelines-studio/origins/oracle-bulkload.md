# Oracle Bulkload

The Oracle Bulkload origin reads all available data from multiple Oracle tables, then stops the pipeline. The origin can use multiple threads to enable the parallel processing of data.&#x20;

Use the Oracle Bulkload origin to quickly read database tables, such as when you want to migrate tables to another database or system. You can use the origin to read from static tables or non-static tables.

When you configure the Oracle Bulkload origin, you specify connection information and the tables to read. You can also configure advanced properties, such as the number of threads to use, the number of batches to include in each transaction request, the maximum batch size, and whether to consider the case of schemas and tables when executing queries.

By default, the origin generates field attributes that provide additional information about each field.

The origin can generate events for an event stream.

### Static and Non-Static Tables <a href="#ariaid-title8" id="ariaid-title8"></a>

You can use the Oracle Bulkload origin to read static tables, tables that do not change while the pipeline runs, or to read non-static tables, tables that change while the origin runs.

When using the origin to read from non-static tables that might change as the pipeline runs, configure the stage to use isolation levels. With isolation levels enabled, the origin uses the serializable isolation level and only reads changes committed when the pipeline starts. The origin does not capture changes made to the table while the pipeline runs. Oracle consistency checks that occur with this isolation level can significantly reduce throughput in an environment with many concurrent transactions.

After using the origin to migrate data from static tables, you can use a separate pipeline that includes the Oracle CDC Client origin to process CDC data from LogMiner redo logs or the JDBC Multitable Consumer origin to read data continuously from tables.

After using the origin to migrate data from non-static tables, processing CDC data does not capture changes made while migrating data, resulting in loss of data. Therefore, processing CDC data is not recommended after using the origin to migrate data from non-static tables.

### Batch Processing <a href="#ariaid-title9" id="ariaid-title9"></a>

Unlike most Data Pipelines origins, the Oracle Bulkload origin performs batch processing only. After processing all data, it stops the pipeline, rather than waiting for additional data as with streaming pipelines.

The Oracle Bulkload origin does not maintain an offset during processing. Each time that you run a pipeline that includes the Oracle Bulkload origin, the origin processes all available data in the specified tables, then stops the pipeline.

Tip: If the pipeline stops before processing is complete, you may want to clear the destination system of the processed records before starting the pipeline again.

### Schema and Table Names <a href="#ariaid-title10" id="ariaid-title10"></a>

When you configure the Oracle Bulkload origin, you specify the tables that you want to read. To specify the tables, you define the schema and a table name pattern.

You can use SQL wildcards to define a set of tables within a schema or across multiple schemas.

For example, say you want to process all tables in the `sales` schema that start with `SALES_`. You can use the following configuration to specify the tables to process:

* Schema: `sales`
* Table Name Pattern: `SALES_%`

You can configure the origin to consider the case of schema and table names when executing queries.

### Multithreaded Processing <a href="#ariaid-title11" id="ariaid-title11"></a>

The Oracle Bulkload origin performs parallel processing and enables the creation of a multithreaded pipeline.

When you start the pipeline, the Oracle Bulkload origin retrieves the list of tables defined in the table configuration. The origin then uses multiple concurrent threads for processing based on the Maximum Pool Size property on the Advanced tab.

As the pipeline runs, Oracle creates blocks of data in memory. The Oracle Bulkload origin creates a task from a block of data and passes it to an available pipeline runner. The pipeline runner creates batches from the task for processing based on the maximum batch size configured for the origin.

A pipeline runner is a sourceless pipeline instance - an instance of the pipeline that includes all of the processors, executors, and destinations in the pipeline and handles all pipeline processing after the origin. Each pipeline runner processes one batch at a time, just like a pipeline that runs on a single thread.

When tasks created from Oracle blocks are smaller than desired, like when they are smaller than the maximum batch size, you can configure the origin to merge small tasks. Use the Minimum Task Size property on the Advanced tab to specify the minimum number of records to include in a task. When set, smaller tasks are merged to enable more efficient processing.

Multithreaded pipelines preserve the order of records within each batch, just like a single-threaded pipeline. But since tasks are processed by different pipeline runners, the order that batches are written to destinations is not ensured.

### Field Attributes <a href="#ariaid-title12" id="ariaid-title12"></a>

The Oracle Bulkload origin generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

The following data type conversions do not include all information in the corresponding Data Pipelines type:

* The Oracle Number data type is converted to the Data Pipelines Decimal data type, which does not store scale and precision.
* The Oracle Timestamp data type is converted to the Data Pipelines Datetime data type, which does not store nanoseconds.

To preserve this information during data type conversion, the origin generates the following field attributes for these Data Pipelines data types:

| Data Pipelines Data Type | Generated Field Attribute | Description                                                   |
| ------------------------ | ------------------------- | ------------------------------------------------------------- |
| Decimal                  | precision                 | Provides the original precision for every number column.      |
| Decimal                  | scale                     | Provides the original scale for every number column.          |
| Datetime                 | nanoSeconds               | Provides the original nanoseconds for every timestamp column. |

You can use the `record:fieldAttribute` or `record:fieldAttributeOrDefault` functions to access the information in the attributes.&#x20;

### Event Generation <a href="#ariaid-title13" id="ariaid-title13"></a>

The Oracle Bulkload origin can generate events that you can use in an event stream.

Oracle Bulkload events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Record <a href="#ariaid-title14" id="ariaid-title14"></a>

Event records generated by the Oracle Bulkload origin have the following event-related record header attributes:

| Record Header Attribute       | Description                                                                                                                                           |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following type:</p><ul><li>table-finished - Generated when the origin completes processing all rows within a table.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                          |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                     |

The Oracle Bulkload origin can generate the following event record:

table-finishedThe Oracle Bulkload origin generates a table-finished event record when the origin completes processing all data within a table.The table-finished event record has the following additional fields:

| Event Record Field | Description                                                                  |
| ------------------ | ---------------------------------------------------------------------------- |
| schema             | Schema associated with the table that has no remaining data to be processed. |
| table              | Table that has no remaining data to be processed.                            |
| record-count       | Number of records that were successfully processed.                          |

### &#x20;<a href="#ariaid-title15" id="ariaid-title15"></a>

\
