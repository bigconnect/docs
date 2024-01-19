# JDBC Producer

The JDBC Producer destination uses a JDBC connection to write data to a database table. You can also use the JDBC Producer destination to write change capture data from a Microsoft SQL Server change log.&#x20;

Data Pipelines provides database-specific destinations, such as SQL Server 2019 BDC Bulk Loader and MemSQL Fast Loader for MemSQL or MySQL databases.

When you configure JDBC Producer, you specify connection information, table name, and optionally define field mappings.

By default, JDBC Producer writes data to the table based on the matching field names. You can override the default field mappings by defining specific mappings. To determine which table rows to update or delete, the destination detects the list of primary key columns for the table, and then uses the fields mapped to those columns to match rows.

You can configure the stage to rollback an entire batch if an error occurs while writing part of the batch. You can also configure custom properties that your driver requires.

The JDBC Producer can use CRUD operations defined in the `sdc.operation.type` record header attribute to write data. You can define a default operation for records without the header attribute or value. You can also configure whether to use multi-row operations for inserts and deletes, and how to handle records with unsupported operations.

When processing data from a CDC-enabled origin, you can specify the origin change log to aid record processing.

To use a JDBC version older than 4.0, you can specify the driver class name and define a health check query.

You can use the JDBC Producer as part of the Drift Synchronization Solution for PostgreSQL.

### Database Vendors and Drivers <a href="#ariaid-title2" id="ariaid-title2"></a>

The JDBC Producer destination can write data to multiple database vendors.

The following table lists the supported and tested database versions for this stage. You can use the stage with other JDBC-compliant databases, but full support is not guaranteed.&#x20;

| Database Vendor      | Supported Versions                                                                                                                                                                                                                                    | Tested Versions                                                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| MySQL                | MySQL 5.7 and later                                                                                                                                                                                                                                   | <ul><li>MySQL 5.7 with the MySQL Connector/J 8.0.12 driver</li><li>MySQL 8.0 with the MySQL Connector/J 8.0.12 driver</li></ul>     |
| Oracle               | <ul><li>Oracle 11g, 12c, 18c, 19c, and later</li><li>Oracle Real Application Clusters (RAC) 12c, 18c, 19c, and later</li></ul><p>Also supported:</p><ul><li>Hosted systems, such as AWS RDS</li><li>Derived systems, such as Oracle Exadata</li></ul> | <ul><li>Oracle 11g, 19c with the Oracle 19.3.0 JDBC driver</li><li>Oracle RAC 12c, 19c with the Oracle 19.3.0 JDBC driver</li></ul> |
| PostgreSQL           | PostgreSQL 9.x and later                                                                                                                                                                                                                              | <ul><li>PostgreSQL 9.6.9</li><li>PostgreSQL 10.4</li><li>PostgreSQL 11.7</li><li>PostgreSQL 12.2</li><li>PostgreSQL 13.0</li></ul>  |
| Microsoft SQL Server | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                                                                                                                                             | <ul><li>SQL Server 2017</li><li>SQL Server 2019</li></ul>                                                                           |

### Installing the JDBC Driver <a href="#ariaid-title3" id="ariaid-title3"></a>

Before you use the JDBC Producer destination, install the JDBC driver for the database. You cannot access the database until you install the required driver.Note: When connecting to a PostgreSQL or Microsoft SQL Server database, you do not need to install a JDBC driver. Data Pipelines includes the JDBC driver required for those databases.

### Define the CRUD Operation <a href="#ariaid-title4" id="ariaid-title4"></a>

The JDBC Producer destination can insert, update, or delete data. The destination writes the records based on the CRUD operation defined in a CRUD operation header attribute or in operation-related stage properties.

You can define the CRUD operation in the following ways:

**CRUD operation header attribute**

You can define the CRUD operation in a CRUD operation record header attribute. The destination looks for the CRUD operation to use in the `sdc.operation.type` record header attribute.The attribute can contain one of the following numeric values:

* 1 for INSERT
* 2 for DELETE
* 3 for UPDATE

If your pipeline includes a CRUD-enabled origin that processes changed data, the destination simply reads the operation type from the `sdc.operation.type` header attribute that the origin generates. If your pipeline uses a non-CDC origin, you can use the Expression Evaluator or a scripting processor to define the record header attribute.&#x20;

**Operation stage properties**

You define a default operation in the destination properties. The destination uses the default operation when the `sdc.operation.type` record header attribute is not set.You can also define how to handle records with unsupported operations defined in the `sdc.operation.type` header attribute. The destination can discard them, send them to error, or use the default operation.

### Update and Delete Operations <a href="#ariaid-title5" id="ariaid-title5"></a>

For update and delete operations, the JDBC Producer destination automatically detects the primary key of the table and uses that key in the WHERE clause that updates or deletes rows. The destination supports compound primary keys, keys that consist of more than one column.

For example, in the following database table, named `customer`, the `id` column is the primary key:

| id | first | middle | last  |
| -- | ----- | ------ | ----- |
| 1  | john  | f      | smith |
| 2  | john  | m      | doe   |
| 3  | mary  | jane   | smith |

Suppose the sdc.operation.type record header attribute for the following record is set to 2, to delete the record from the table:

```
{
 "id": 1,
 "first": "john",
 "middle": "m",
 "last": "doe"
}
```

Then, the destination matches the row with the same primary key and creates the following query:

```
DELETE FROM customer WHERE id = 1
```

Note that the destination matches the row based on the primary key and not the other fields in the record.

### Single and Multi-row Operations <a href="#ariaid-title6" id="ariaid-title6"></a>

JDBC Producer performs single-row operations by default. That is, it executes a SQL statement for each record. When supported by the destination database, you can configure JDBC Producer to perform multi-row operations. Depending on the sequence of the data, multi-row operations can improve pipeline performance.

When performing multi-row operations, JDBC Producer creates a single SQL statement for sequential insert rows and for sequential delete rows. JDBC Producer does not perform multi-row update operations. You can configure the Statement Parameter Limit property to limit the number of parameters in an insert operation - that is, you can limit the number of records included in an insert statement.

For example, say the pipeline generates three insert records, followed by two update records, and four delete records. If you enable multi-row operations and do not set a statement parameter limit, JDBC Producer generates a single insert SQL statement for the three insert records, two update statements - one for each of the update records, and a single delete statement for the four delete records. On the other hand, if you enable multi-row operations and set the statement parameter limit to two, JDBC Producer generates two insert SQL statements - one for two insert records and one for the third insert record, two update statements - one for each of the update records, and a single delete statement for the four delete records.

Important: Before enabling multi-row operations, verify that the database supports the SQL statements that JDBC Producer uses.

Error handling for multi-row operations depends on the database. If the database reports the individual record that causes an error in a multi-row statement, the stage sends that record to the error stream. If the database does not report which record causes an error, the stage sends all the records from the statement to the error stream.

For multi-row inserts, JDBC Producer uses the following SQL statement:

```
INSERT INTO <table name> (<col1>, <col2>, <col3>) 
     VALUES (<record1 field1>,<record1 field2>,<record1 field3>), 
     (<r2 f1>,<r2 f2>,<r2 f3>), (<r3 f1>,<r3 f2>,<r3 f3>),...;
```

For multi-row deletes, JDBC Producer uses the following SQL statement for tables with a single primary key:

```
DELETE FROM <table name> WHERE <primary key> IN (<key1>, <key2>, <key3>,...);
```

For multi-row deletes, JDBC Producer uses the following SQL statement for tables with multiple primary keys:

```
DELETE FROM <table name> WHERE (<pkey1>, <pkey2>, <pkey3>)
      IN ((<key1-1>, <key1-2>, <key1-3>),(<key2-1>, <key2-2>, <key2-2>),...);
```
