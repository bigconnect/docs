# JDBC Query

The JDBC Query executor connects through JDBC to a database and runs one or more user-defined SQL queries each time it receives an event record.&#x20;

Use the JDBC Query executor as part of an event stream in the pipeline. For example, you might use the JDBC Query executor to execute a stored procedure in the database when the pipeline stop event is generated.Important: This stage does not support connecting to non-RDBMS systems, including Hive, Impala, Kudu, or Snowflake. Support for untested systems is not guaranteed. For a list of tested systems, see "Database Vendors and Drivers". Use the Hive Query executor to run queries on Hive or Impala.

The JDBC Query executor can commit data to the database after each batch or can commit data to the database for each record. By default, the executor commits after each batch.

When you configure the JDBC Query executor, you specify JDBC connection properties and the queries to run. You can configure custom properties that your driver requires, advanced connection properties, and the commit type to use. You can also enable the executor to run insert and delete queries in parallel to improve throughput.

To use a JDBC version older than 4.0, you can specify the driver class name and define a health check query.

You can also configure the executor to generate events for another event stream.&#x20;

### Database Vendors and Drivers <a href="#ariaid-title2" id="ariaid-title2"></a>

The JDBC Query executor can run queries on database data from multiple database vendors.

The following table lists the supported and tested database versions for this stage. You can use the stage with other JDBC-compliant databases, but full support is not guaranteed.&#x20;

| Database Vendor      | Supported Versions                                                                                                                                                                                                                                    | Tested Versions                                                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| MySQL                | MySQL 5.7 and later                                                                                                                                                                                                                                   | <ul><li>MySQL 5.7 with the MySQL Connector/J 8.0.12 driver</li><li>MySQL 8.0 with the MySQL Connector/J 8.0.12 driver</li></ul>     |
| Oracle               | <ul><li>Oracle 11g, 12c, 18c, 19c, and later</li><li>Oracle Real Application Clusters (RAC) 12c, 18c, 19c, and later</li></ul><p>Also supported:</p><ul><li>Hosted systems, such as AWS RDS</li><li>Derived systems, such as Oracle Exadata</li></ul> | <ul><li>Oracle 11g, 19c with the Oracle 19.3.0 JDBC driver</li><li>Oracle RAC 12c, 19c with the Oracle 19.3.0 JDBC driver</li></ul> |
| PostgreSQL           | PostgreSQL 9.x and later                                                                                                                                                                                                                              | <ul><li>PostgreSQL 9.6.9</li><li>PostgreSQL 10.4</li><li>PostgreSQL 11.7</li><li>PostgreSQL 12.2</li><li>PostgreSQL 13.0</li></ul>  |
| Microsoft SQL Server | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                                                                                                                                             | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                           |

### Installing the JDBC Driver <a href="#ariaid-title3" id="ariaid-title3"></a>

Before you use the JDBC Query executor, install the JDBC driver for the database. You cannot access the database until you install the required driver.

Note: When connecting to a PostgreSQL or Microsoft SQL Server database, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for those databases.

### SQL Queries <a href="#ariaid-title4" id="ariaid-title4"></a>

You define one or more SQL queries that the JDBC Query executor runs on the database each time it receives an event record.

The JDBC Query executor waits for each query to complete before continuing with the next query for the same event record. It also waits for all queries to complete before starting the queries for the next event record. Depending on the speed of the pipeline and the complexity of the queries, the wait for query completion can slow pipeline performance.

When running multiple queries for an event record, the executor skips the subsequent queries if a query fails.

You can include a subset of the functions provided with the DP expression language in a query. When entering a query in the executor, press Ctrl + Space Bar to view the list of valid functions you can use.

### Event Generation <a href="#ariaid-title5" id="ariaid-title5"></a>

The JDBC Query executor can generate events that you can use in an event stream. When you enable event generation, the executor generates events for each successful or failed query.

JDBC Query events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title6" id="ariaid-title6"></a>

Event records generated by the JDBC Query executor have the following event-related record header attributes. Record header attributes are stored as String values.

| Record Header Attribute       | Description                                                                                                                                                                                  |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following event types:</p><ul><li>successful-query - Generated after a query successfully completes.</li><li>failed-query - Generated after a query fails.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                 |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                            |

The JDBC Query executor can generate the following types of event records:Successful query

The executor generates a successful-query event record after successfully completing a query.

Successful-query event records have the `sdc.event.type` record header attribute set to `sucessful-query` and include the following fields:

| Event Field Name | Description                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------ |
| query            | Query completed.                                                                                             |
| query-result     | Number of rows affected by query. Included if the Include Query Result Count in Events property is selected. |

Failed query

The executor generates a failed-query event record after failing to complete a query.

Failed-query event records have the `sdc.event.type` record header attribute set to `failed-query` and include the following field:

| Event Field Name | Description      |
| ---------------- | ---------------- |
| query            | Query attempted. |
