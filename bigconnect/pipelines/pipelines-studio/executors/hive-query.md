# Hive Query

The Hive Query executor connects to Hive or Impala and performs one or more user-defined Hive or Impala queries each time it receives an event record.&#x20;

Use the Hive Query executor as part of an event stream to perform event-driven queries in Hive or Impala. You can use the executor in any logical way, such as running Hive or Impala queries after the Hive Metadata destination updates the Hive metastore, or after the Hadoop FS or MapR FS destination closes files.

For example, you can use the Hive Query executor to perform the Invalidate Metadata query for Impala as part of the Drift Synchronization Solution for Hive or to configure table properties for newly-created tables.

When using the Hive Query executor with Impala, you can use the default driver included with Data Pipelines, or you can install an Impala JDBC driver.

Note: The Hive Query executor waits for each query to complete before continuing with the next query for the same event record. It also waits for all queries to complete before starting the queries for the next event record. Depending on the speed of the pipeline and the complexity of the queries, the wait for query completion can slow pipeline performance.

When you configure the Hive Query executor, you configure JDBC connection information to Hive, and optionally add additional HDFS configuration properties to use. You specify the queries that you want to run and indicate whether to run the remaining queries after a query fails.

You can also configure the executor to generate events for another event stream.&#x20;

For a solution that describes how to use the Hive Query executor, see Automating Impala Metadata Updates for Drift Synchronization for Hive.

### Related Event Generating Stages <a href="#ariaid-title2" id="ariaid-title2"></a>

Use the Hive Query executor in an event stream. The Hive Query executor is designed to run a set of Hive or Impala queries after receiving an event record. You can use the Hive Query executor with any event-generating stage where the logic suits your needs.

When implementing the Drift Synchronization Solution for Hive with Impala, use the executor to run the Invalidate Metadata query after the Hive Metastore destination changes table structures and after the Hadoop FS destination writes files to Hive.

### Installing the Impala Driver <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use the Apache Hive JDBC driver included with Data Pipelines to perform Impala queries. However, some distributions recommend using a native Impala JDBC driver.

To use the included Apache Hive JDBC driver included with Data Pipelines, you do not need to perform any additional steps.

To use an Impala JDBC driver, perform the following steps:

1. Download the native Impala JDBC driver for the Hive distribution that you use.
2.  Install the driver as an external library for the stage library used by the Hive Query executor.

    For example, say the executor is configured to use the CDH 5.12.0 stage library. If you use the Package Manager to install the driver, then in the Install External Libraries dialog box, you select the CDH 5.12.0 stage library, then browse to select the Impala driver libraries to install

### Hive and Impala Queries <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use the Hive Query executor to execute a set of Hive or Impala queries each time the executor receives an event record.

The Hive Query executor waits for each query to complete before continuing with the next query for the same event record. It also waits for all queries to complete before starting the queries for the next event record. Depending on the speed of the pipeline and the complexity of the queries, the wait for query completion can slow pipeline performance.

When possible, avoid using Hive Query executor to run long-running queries. Also, when running multiple queries for an event record, you can configure the executor to skip the remaining queries if a query fails. By default, the executor continues to run the rest of the queries.

You can use the fields and attributes in an event record in queries. For example, for event records generated when Hive Metastore creates or updates a table, you can use the table name in the event record to perform additional tasks.

For a list of field names and descriptions in an event record, see the "Event Records" documentation for the event generating stage.

### Impala Queries for the Drift Synchronization Solution for Hive <a href="#ariaid-title5" id="ariaid-title5"></a>

The Drift Synchronization Solution for Hive enables a pipeline to automatically create and update Hive tables and to write files to the tables.

When implementing the Drift Synchronization Solution for Hive with Impala, you can use the Hive Query executor to submit an invalidate metadata query each time you need to update the Impala metadata cache.

Connect a Hive Query executor to the event stream from the Hive Metastore destination and the Hadoop FS destination. You can use the same executor for both, or use a separate executor for each.Processing event records from the Hive Metastore destinationThe Hive Metastore destination generates an event record each time it changes a table and places the table name in the "table" record header attribute. Use the following query to update the Impala metadata cache:

```
invalidate metadata ${record:attribute('/table')}
```

When the Hive Query executor receives the event record, it runs the Invalidate Metadata query on the table specified in the event record.Processing event records from the Hadoop FS destinationThe Hadoop FS destination generates an event record each time it closes a file. It places the file path in a "filepath" field in the event record. If you use a separate Hive Query executor for each destination, use the following query to update the Impala cache:

```
invalidate metadata `${file:pathElement(record:value('/filepath'), -3)}`.
`${file:pathElement(record:value('/filepath'), -2)}`      
```

This expression uses the third-to-last section of the path as the database name, and the second-to-last section of the path as the table name.If you want to use the same Hive Query executor to process records from Hive Query and Hadoop FS, add an Expression Evaluator in the Hadoop FS event stream to perform this processing.&#x20;

### Event Generation <a href="#ariaid-title6" id="ariaid-title6"></a>

The Hive Query executor can generate events that you can use in an event stream.

The executor submits queries to Hive each time it receives an event record. When you enable event generation, the executor generates events each time it determines if the submitted query has completed.

Hive Query events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title7" id="ariaid-title7"></a>

Event records generated by the Hive Query executor have the following event-related record header attributes. Record header attributes are stored as String values:

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                           |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following event types:</p><ul><li>successful-query - Generated when the executor determines that Hive successfully ran the submitted query.</li><li>failed-query - Generated when the executor determines that Hive failed to run the query.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                          |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                     |

The Hive Query executor can generate the following types of event records:Successful query

The executor generates a successful query event record when Hive successfully completes the query.

A successful query event record has the sdc.event.type record header attribute set to successful-query and includes the following field:

| Event Field Name | Description                           |
| ---------------- | ------------------------------------- |
| query            | The query that Hive ran successfully. |

Failed query

The executor generates a failed query event record when Hive fails to run the query.

A failed query event record has the sdc.event.type record header attribute set to failed-query and can include the following fields:

| Event Field Name   | Description                                                                                                                                                                                |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| query              | The query that Hive failed to run.                                                                                                                                                         |
| unexecuted-queries | <p>Any additional queries that did not execute.</p><p>The event record includes this field only when you configure the executor to skip running subsequent queries when a query fails.</p> |
