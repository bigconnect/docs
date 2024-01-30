# SAP HANA Query Consumer

The SAP HANA Query Consumer origin reads from an SAP HANA database using the specified SQL query.

The SQL query can read data from a single table or from a join of tables. The origin returns data as a map with column names and field values.

When you configure SAP HANA Query Consumer, you specify connection information and credentials that determine how the origin connects to the database. You configure the query mode, SQL query and related information to define the data returned by the database. You can call stored procedures from the SQL query.

You can enable SAP HANA split batch commands, which allow parallel execution of the query on partitioned tables. You can specify custom properties that your driver requires. And you can specify what the origin does when encountering an unsupported data type: convert the data to string or stop the pipeline.

By default, the origin generates JDBC record header attributes and JDBC field attributes that provide additional information about each record and field. You can configure the origin to generate SAP HANA record header attributes that provide details about the connection.

The origin can generate events for an event stream

Before you use the origin, you must install a JDBC driver.

### Installing the JDBC Driver <a href="#ariaid-title2" id="ariaid-title2"></a>

Before you use the SAP HANA Query Consumer origin, install the JDBC driver for the database. You cannot access the database until you install the required driver.

### Offset Column and Offset Value <a href="#ariaid-title3" id="ariaid-title3"></a>

The SAP HANA Query Consumer origin uses an offset column and initial offset value to determine where to start reading data within a table. Include both the offset column and the offset value in the WHERE clause of the SQL query.

The offset column must be a column in the table with unique non-null values, such as a primary key or indexed column. The initial offset value is a value within the offset column where you want the origin to start reading.

When the origin performs an incremental query, you must configure the offset column and offset value. For full queries, you can optionally configure them.

### Full and Incremental Mode <a href="#ariaid-title4" id="ariaid-title4"></a>

The SAP HANA Query Consumer origin can perform queries in two modes:

**Incremental mode**

When the SAP HANA Query Consumer origin performs an incremental query, it uses the initial offset as the offset value in the first SQL query. As the origin completes processing the results of the first query, it saves the last offset value that it processes. Then it waits the specified query interval before performing a subsequent query.

When the origin performs a subsequent query, it returns data based on the last-saved offset. You can reset the origin to use the initial offset value.

Use incremental mode for append-only tables or when you do not need to capture changes to older rows. By default, the origin uses incremental mode.

**Full mode**

When the SAP HANA Query Consumer origin performs a full query, it runs the specified SQL query. If you optionally configure the offset column and initial offset value, the origin uses the initial offset as the offset value in the SQL query each time it requests data.

When the origin completes processing the results of the full query, it waits the specified query interval, and then performs the same query again.Use full mode to capture all row updates. You might use a Record Deduplicator in the pipeline to minimize repeated rows. Not ideal for large tables.

Tip: If you want to process the results from a single full query and then stop the pipeline, you can enable the origin to generate events and use the Pipeline Finisher to stop the pipeline automatically.

### Recovery <a href="#ariaid-title5" id="ariaid-title5"></a>

The SAP HANA Query Consumer origin supports recovery after a deliberate or unexpected stop when it performs incremental queries. Recovery is not supported for full queries.

In incremental mode, the origin uses offset values in the offset column to determine where to continue processing after a deliberate or unexpected stop. To ensure seamless recovery in incremental mode, use a primary key or indexed column as the offset column. As the SAP HANA Query Consumer origin processes data, it tracks the offset value internally. When the pipeline stops, the origin notes where it stopped processing data. When you restart the pipeline, it continues from the last-saved offset.

When the origin performs full queries, the origin runs the full query again after you restart the pipeline.

### SQL Query <a href="#ariaid-title6" id="ariaid-title6"></a>

The SQL query defines the data returned from the database.

You define the query in the SQL Query property on the JDBC tab. Or, you can define the query in a runtime resource, and then use the `runtime:loadResource` function in the SQL Query property to load the query from the resource file at runtime. For example, you might enter the following expression for the property:

```
${runtime:loadResource("myquery.sql", false)}
```

The SQL query guidelines that you use depend on whether you configure the origin to perform an incremental or full query.Note: Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.

#### SQL Query for Incremental Mode <a href="#ariaid-title7" id="ariaid-title7"></a>

When you define the SQL query for incremental mode, the SAP HANA Query Consumer origin requires a WHERE and ORDER BY clause in the query.

Use the following guidelines when you define the WHERE and ORDER BY clauses in the query:

In the WHERE clause, include the offset column and the offset valueThe origin uses an offset column and value to determine the data that is returned. Include both in the WHERE clause of the query.Use the OFFSET constant to represent the offset valueIn the WHERE clause, use ${OFFSET} to represent the offset value.

For example, when you start a pipeline, the following query returns all data from the table where the data in the offset column is greater than the initial offset value:

```
SELECT * FROM <tablename> WHERE <offset column> > ${OFFSET}
```

Tip: When the offset values are strings, enclose ${OFFSET} in single quotation marks.In the ORDER BY clause, include the offset column as the first columnTo avoid returning duplicate data, use the offset column as the first column in the ORDER BY clause.

Note: Using column that is not a primary key or indexed column in the ORDER BY clause can slow performance.

For example, the following query for incremental mode returns data from an Invoice table where the ID column is the offset column. The query returns all data where the ID is greater than the offset and orders the data by the ID:

```
 SELECT * FROM invoice WHERE id > ${OFFSET} ORDER BY id
```

#### SQL Query for Full Mode <a href="#ariaid-title8" id="ariaid-title8"></a>

You can define any type of SQL query for full mode.

For example, you can run the following query to return all data from an Invoice table:

```
SELECT * FROM invoice
```

When you define the SQL query for full mode, you can optionally include the WHERE and ORDER BY clauses using the same guidelines as for incremental mode. However, using these clauses to read from large tables can cause performance issues.

### JDBC Attributes <a href="#ariaid-title9" id="ariaid-title9"></a>

The SAP HANA Query Consumer origin generates record header attributes and field attributes that provide additional information about each record and field.

The origin receives these details from the JDBC driver.

#### JDBC Header Attributes <a href="#ariaid-title10" id="ariaid-title10"></a>

By default, the SAP HANA Query Consumer origin generates JDBC record header attributes that provide additional information about each record, such as the original data type of a field or the source tables for the record. The origin receives these details from the JDBC driver.

You can use the `record:attribute` or `record:attributeOrDefault` functions to access the information in the attributes.&#x20;

JDBC header attributes include a user-defined prefix to differentiate the JDBC header attributes from other record header attributes. By default, the prefix is `jdbc`.

You can change the prefix that the origin uses and you can configure the origin not to create JDBC header attributes using the Create JDBC Header Attributes and JDBC Header Prefix properties on the Advanced tab.

The origin can provide the following JDBC header attributes:

| JDBC Header Attribute                   | Description                                                                                                                                                                                                                                                                                                                                   |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \<JDBC prefix>.tables                   | <p>Provides a comma-separated list of source tables for the fields in the record.Note: Not all JDBC drivers provide this information.</p><p>Oracle uses all caps for schema, table, and column names by default. Names can be lower- or mixed-case only if the schema, table, or column was created with quotation marks around the name.</p> |
| \<JDBC prefix>.\<column name>.jdbcType  | Provides the numeric value of the original SQL data type for each field in the record. See the [Java documentation](https://docs.oracle.com/javase/8/docs/api/constant-values.html#java.sql.Types.ARRAY) for a list of the data types that correspond to numeric values.                                                                      |
| \<JDBC prefix>.\<column name>.precision | Provides the original precision for all numeric and decimal fields.                                                                                                                                                                                                                                                                           |
| \<JDBC prefix>.\<column name>.scale     | Provides the original scale for all numeric and decimal fields.                                                                                                                                                                                                                                                                               |

#### JDBC Field Attributes <a href="#ariaid-title11" id="ariaid-title11"></a>

The SAP HANA Query Consumer origin generates field attributes for columns converted to the Decimal or Datetime data types in Data Pipelines. The attributes provide additional information about each field.

The following data type conversions do not include all information in the corresponding Data Pipelines data type:

* Database Decimal and Numeric data types are converted to the Data Pipelines Decimal data type, which does not store scale and precision.
* The database Timestamp data type is converted to the Data Pipelines Datetime data type, which does not store nanoseconds.

To preserve this information during data type conversion, the origin generates the following field attributes for these Data Pipelines data types:

| Data Pipelines Data Type | Generated Field Attribute | Description                                                          |
| ------------------------ | ------------------------- | -------------------------------------------------------------------- |
| Decimal                  | precision                 | Provides the original precision for every decimal or numeric column. |
| Decimal                  | scale                     | Provides the original scale for every decimal or numeric column.     |
| Datetime                 | nanoSeconds               | Provides the original nanoseconds for every timestamp column.        |

You can use the `record:fieldAttribute` or `record:fieldAttributeOrDefault` functions to access the information in the attributes.&#x20;

### SAP HANA Header Attributes <a href="#ariaid-title12" id="ariaid-title12"></a>

The SAP HANA Query Consumer origin can include SAP HANA connection information , such as the driver version or application name, in record header attributes. The origin receives these details from the JDBC driver.

The attributes are named `SapHANA.<attribute>`.

For example, the driver might include the following record header attributes:

```
SapHANA.APPLICATIONUSER:<user name>
SapHANA.DRIVERVERSION:2.4.76-7ca985c0cc5ea9fa063ab376dab1bf7b859dd9cc
SapHANA.APPLICATION:com.streamsets.pipeline.BootstrapMain
```

Use the Include SAP HANA Connection Details on the SAP HANA tab to enable generating the SAP HANA header attributes.

### Event Generation <a href="#ariaid-title13" id="ariaid-title13"></a>

The SAP HANA Query Consumer origin can generate events that you can use in an event stream. When you enable event generation, the origin generates an event when it completes processing the data returned by the specified query. The origin also generates an event when a query completes successfully and when it fails to complete.

Events generated by the origin can be used in any logical way. For example:

*   With the Pipeline Finisher executor to stop the pipeline and transition the pipeline to a Finished state when the origin completes processing available data.

    When you restart a pipeline stopped by the Pipeline Finisher executor, the origin processes data based on how you configured the origin. For example, if you configure the origin to run in incremental mode, the origin saves the offset when the executor stops the pipeline. When it restarts, the origin continues processing from the last-saved offset. In contrast, if you configure the origin to run in full mode, when you restart the pipeline, the origin uses the initial offset, if specified.
* With the Email executor to send a custom email after receiving an event.
* With a destination to store information about completed queries.

#### Event Record <a href="#ariaid-title14" id="ariaid-title14"></a>

Event records generated by the SAP HANA Query Consumer origin have the following event-related record header attributes:

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                                               |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses one of the following types:</p><ul><li>no-more-data - Generated when the origin completes processing all data returned by a query.</li><li>jdbc-query-success - Generated when the origin successfully completes a query.</li><li>jdbc-query-failure - Generated when the origin fails to complete a query.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                                              |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                                         |

The origin can generate the following types of event records:No-more-dataThe origin generates a no-more-data event record when it completes processing all data returned by a query.No-more-data event records generated by the origin have the `sdc.event.type` set to `no-more-data` and include the following field:

| Event Record Field | Description                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------- |
| record-count       | Number of records successfully generated since the pipeline started or since the last no-more-data event was created. |

Query successThe origin generates a query success event record when it completes processing the data returned from a query.The query success event records have the `sdc.event.type` record header attribute set to `jdbc-query-success` and include the following fields:

| Field         | Description                         |
| ------------- | ----------------------------------- |
| query         | Query that completed successfully.  |
| timestamp     | Timestamp when the query completed. |
| row-count     | Number of processed rows.           |
| source-offset | Offset after the query completed.   |

Query failureThe origin generates a query failure event record when it fails to complete processing the data returned from a query.The query failure event records have the `sdc.event.type` record header attribute set to jdbc-query-failure and include the following fields:

| Field         | Description                                           |
| ------------- | ----------------------------------------------------- |
| query         | Query that failed to complete.                        |
| timestamp     | Timestamp when the query failed to complete.          |
| row-count     | Number of records from the query that were processed. |
| source-offset | Origin offset after query failure.                    |
| error         | First error message.                                  |

### &#x20;<a href="#ariaid-title15" id="ariaid-title15"></a>

\
