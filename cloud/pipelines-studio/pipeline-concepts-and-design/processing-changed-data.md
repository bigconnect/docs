# Processing Changed Data

Certain stages enable you to easily process change capture data (CDC) or transactional data in a pipeline.

CDC-enabled origins can read change capture data. Some exclusively read change capture data, others can be configured to read it. When reading changed data, they determine the CRUD operation associated with the data and include CRUD operations - such as insert, update, upsert, or delete - in the sdc.operation.type record header attribute.

CRUD-enabled processors and destinations can use the CRUD operation type in the `sdc.operation.type` header attribute when writing records, enabling the external system to perform the appropriate operation.

Using a CDC-enabled origin and CRUD-enabled stages in a pipeline allows you to easily write changed data from one system into another. You can also use a CDC-enabled origin to write to non-CRUD destinations, and non-CDC origins to write to CRUD-enabled stages.&#x20;

## CRUD Operation Header Attribute

CDC-enabled origins read include the sdc.operation.type record header attribute in all records when reading changed data.

CRUD-enabled processors and destinations can use the CRUD operation type in the `sdc.operation.type` header attribute when writing records, enabling the external system to perform the appropriate operation.The sdc.operation.type record header attribute uses the following integers to represent CRUD operations:

* 1 for INSERT records
* 2 for DELETE records
* 3 for UPDATE records
* 4 for UPSERT records
* 5 for unsupported operations or codes
* 6 for UNDELETE records
* 7 for REPLACE records
* 8 for MERGE records

Note: Some origins use only a subset of the operations, based on the operations supported by the origin system. Similarly, destinations recognize only the subset of the operations that the destination systems support. See the origin and destination documentation for details about supported operations.

## CDC-Enabled Stages

CDC-enabled stages provide the CRUD operation type in the sdc.operation.type record header attribute. Some origins provide alternate and additional header attributes.

The following stages provide CRUD record header attributes:

| CDC-Enabled Stage                            | CRUD Record Header Attributes                                                                                                                                                                                                                                          |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| JDBC Query Consumer for Microsoft SQL Server | Includes the CRUD operation type in the sdc.operation.type record header attribute.                                                                                                                                                                                    |
| MySQL Binary Log                             | <p>Includes the CRUD operation type in the sdc.operation.type record header attribute.</p><p>Includes additional CDC information in record fields.</p>                                                                                                                 |
| Oracle CDC Client                            | <p>Includes the CRUD operation type in both of the following headers:</p><ul><li>sdc.operation.type</li><li>oracle.cdc.operation</li></ul><p>Includes additional CDC information in record header attributes with the oracle.cdc prefix, such as oracle.cdc.table.</p> |
| PostgreSQL CDC Client                        | <p>Includes the CRUD operation type in the record.</p><p>Includes additional CDC information in record header attributes with the postgres.cdc prefix, such as postgres.cdc.lsn.</p>                                                                                   |
| Salesforce                                   | Includes the CRUD operation type in the sdc.operation.type record header attribute.                                                                                                                                                                                    |
| SQL Parser                                   | <p>Includes the CRUD operation type in both of the following headers:</p><ul><li>sdc.operation.type</li><li>oracle.cdc.operation</li></ul>                                                                                                                             |
| SQL Server CDC Client                        | <p>Includes the CRUD operation type in the sdc.operation.type record header attribute.</p><p>Includes CDC information in header attributes named jdbc.&#x3C;CDC column name>.</p>                                                                                      |
| SQL Server Change Tracking                   | <p>Includes the CRUD operation type in the sdc.operation.type record header attribute.</p><p>Includes additional information from change tables in the jdbc.SYS_CHANGE header attributes.</p>                                                                          |

## Data Changes in SQL Server or Azure SQL Database <a href="#ariaid-title5" id="ariaid-title5"></a>

SQL Server and Azure SQL Database offer multiple methods to track data changes. In Data Collector, the appropriate origin depends on the method that the database uses to track changes, as shown in the following table:

| CDC tables             | <p>SQL Server CDC Client</p><p>For more information about CDC tables, see the <a href="https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-data-capture-sql-server?view=sql-server-ver15">Microsoft documentation</a>.</p>              |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Change tracking tables | <p>SQL Server Change Tracking</p><p>For more information about change tracking tables, see the <a href="https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-tracking-sql-server?view=sql-server-ver15">Microsoft documentation</a>.</p> |
| Temporal tables        | <p>JDBC Multitable Consumer or JDBC Query Consumer</p><p>For more information about temporal tables, see the <a href="https://docs.microsoft.com/en-us/sql/relational-databases/tables/temporal-tables?view=sql-server-ver15">Microsoft documentation</a>.</p>           |

## CRUD-Enabled Stages

The following stages recognize CRUD operations stored in record header attributes and can perform writes based on those values. Some stages also provide CRUD-related properties.

| CRUD-Enabled Stage        | Supported Operations                                                                                                                          | Stage Processing                                                                                                                                                                                                                                                                                                                   |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| JDBC Tee processor        | <ul><li>INSERT</li><li>UPDATE</li><li>DELETE</li></ul>                                                                                        | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Default Operation and Unsupported Operation Handling properties in the stage</li></ul><p>Includes a Change Log property that enables processing records based on the CDC-enabled origin that you use. </p> |
| Couchbase destination     | <ul><li>INSERT</li><li>UPDATE</li><li>UPSERT</li><li>DELETE</li></ul>                                                                         | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Default Write Operation and Unsupported Operation Handling properties in the stage</li></ul>                                                                                                               |
| Elasticsearch destination | <ul><li>CREATE (INSERT)</li><li>UPDATE</li><li>INDEX (UPSERT)</li><li>DELETE</li><li>UPDATE with <code>doc_as_upsert</code> (MERGE)</li></ul> | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Default Operation and Unsupported Operation Handling properties in the stage</li></ul>                                                                                                                     |
| GPSS Producer destination | <ul><li>INSERT</li><li>UPDATE</li><li>MERGE</li></ul>                                                                                         | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Default Operation and Unsupported Operation Handling properties in the stage</li></ul>                                                                                                                     |
| JDBC Producer destination | <ul><li>INSERT</li><li>UPDATE</li><li>DELETE</li></ul>                                                                                        | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Default Operation and Unsupported Operation Handling properties in the stage</li></ul><p>Includes a Change Log property that enables processing records based on the CDC-enabled origin that you use. </p> |
| Kudu destination          | <ul><li>INSERT</li><li>UPDATE</li><li>UPSERT</li><li>DELETE</li></ul>                                                                         | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Default Operation and Unsupported Operation Handling properties in the stage</li></ul>                                                                                                                     |
| MapR DB JSON destination  | <ul><li>INSERT</li><li>UPDATE</li><li>DELETE</li></ul>                                                                                        | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Insert API and Set API properties in the stage</li></ul>                                                                                                                                                   |
| MongoDB destination       | <ul><li>INSERT</li><li>UPDATE</li><li>REPLACE</li><li>DELETE</li></ul>                                                                        | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Upsert property in the stage</li></ul>                                                                                                                                                                     |
| Redis destination         | <ul><li>INSERT</li><li>UPDATE</li><li>REPLACE</li><li>DELETE</li></ul>                                                                        | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li></ul>                                                                                                                                                                                                          |
| Salesforce destination    | <ul><li>INSERT</li><li>UPDATE</li><li>UPSERT</li><li>DELETE</li><li>UNDELETE</li></ul>                                                        | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li><li>Default Operation and Unsupported Operation Handling properties in the stage</li></ul>                                                                                                                     |
| Snowflake destination     | <ul><li>INSERT</li><li>UPDATE</li><li>DELETE</li></ul>                                                                                        | <p>Determines the operation to use based on:</p><ul><li><code>sdc.operation.type</code> record header attribute</li></ul>                                                                                                                                                                                                          |

## Processing the Record

Change logs can provide record data in different formats. The JDBC Tee processor and JDBC Producer can decode most change log formats to generate record data based on the origin change log. When using other CRUD-enabled destinations, you might need to add additional processing to the pipeline to alter the format of the record.

For example, Microsoft SQL CDC records created by the JDBC Query Consumer contains CDC fields in the record, in addition to record data. You might use a Field Remover to drop any unnecessary fields from the record.

In contrast, the MySQL Server binary logs read by the My SQL Binary Log origin provides new or updated data in a New Data map field and changed or deleted data in a Changed Data map field. You might want to use the Field Flattener processor to flatten the map field with the data that you need, and a Field Remover to remove any unnecessary fields.

For details on the format of generated records, see the documentation for the CDC-enabled origin.

## Use Cases

You can use CDC-enabled origins and CRUD-enabled destinations in pipelines together or individually. Here are some typical use cases:

### CDC-enabled origin with CRUD-enabled destinations

You can use a CDC-enabled origin and a CRUD-enabled destination to easily process changed records and write them to a destination system.

For example, say you want to write CDC data from Microsoft SQL Server to Kudu. To do this, you use the CDC-enabled JDBC Query Consumer origin to read data from a Microsoft SQL Server change capture table. The origin places the CRUD operation type in the sdc.operation.type header attribute, in this case: 1 for INSERT, 2 for DELETE, 3 for UPDATE.

You configure the pipeline to write to the CRUD-enabled Kudu destination. In the Kudu destination, you can specify a default operation for any record with no value set in the sdc.operation.type attribute, and you can configure error handling for invalid values. You set the default to INSERT and you configure the destination to use this default for invalid values. In the sdc.operation.type attribute, the Kudu destination supports 1 for INSERT, 2 for DELETE, 3 for UPDATE, and 4 for UPSERT.

When you run the pipeline, the JDBC Query Consumer origin determines the CRUD operation type for each record and writes it to the sdc.operation.type record header attribute. And the Kudu destination uses the operation in the sdc.operation.type attribute to inform the Kudu destination system how to process each record. Any record with an undeclared value in the sdc.operation.type attribute, such as a record created by the pipeline, is treated like an INSERT record. And any record with an invalid value uses the same default behavior.

### CDC-enabled origin to non-CRUD destinations

If you need to write changed data to a destination system without a CRUD-enabled destination, you can use an Expression Evaluator or scripting processor to move the CRUD operation information from the sdc.operation.type header attribute to a field, so the information is retained in the record.

For example, say you want to read from Oracle LogMiner redo logs and write the records to Hive tables with all of the CDC information in record fields. To do this, you'd use the Oracle CDC Client origin to read the redo logs, then add an Expression Evaluator to pull the CRUD information from the sdc.operation.type header attribute into the record. Oracle CDC Client writes additional CDC information such as the table name and scn into oracle.cdc header attributes, so you can use expressions to pull that information into the record as well. Then you can use the Hadoop FS destination to write the enhanced records to Hive.

### Non-CDC origin to CRUD destinations

When reading data from a non-CDC origin, you can use the Expression Evaluator or scripting processors to define the sdc.operation.type header attribute.

For example, say you want to read from a transactional database table and keep a dimension table in sync with the changes. You'd use the JDBC Query Consumer to read the source table and a JDBC Lookup processor to check the dimension table for the primary key value of each record. Then, based on the output of the lookup processor, you know if there was a matching record in the table or not. Using an Expression Evaluator, you set the sdc.operation.type record header attribute - 3 to update the records that had a matching record, and 1 to insert new records.

When you pass the records to the JDBC Producer destination, the destination uses the operation in the sdc.operation.type header attribute to determine how to write the records to the dimension table.
