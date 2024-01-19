# SQL Server 2019 BDC Bulk Loader

The SQL Server 2019 BDC Bulk Loader destination writes data to Microsoft SQL Server 2019 Big Data Cluster (BDC) using a bulk insert.&#x20;

The destination writes data from record fields to table columns based on matching names. You can configure the destination to automatically handle data drift in SQL Server tables. The destination can create new SQL Server or external tables when new table references appear in records, and the destination can create new SQL Server columns when new field references appear in records. The destination cannot write data from record fields that store data in the Map, List, List\_map, or File\_ref data types.

When you configure the destination, specify connection information, including the database, schema, and table to write to, and the number of connections used. You can specify a row field that lists the fields to include in each row written to the table or you can have the destination write all rows except specified fields. You can also configure the destination to replace missing fields or fields containing invalid data types with specified default values, and to replace newline characters in string fields with a specified character.

### External Tables <a href="#ariaid-title7" id="ariaid-title7"></a>

In SQL Server 2019 BDC, you can define external tables that access data virtualized in SQL Server. For more information, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/relational-databases/polybase/data-virtualization?view=sqlallproducts-allversions).

To configure the SQL Server 2019 BDC Bulk Loader destination to write to an existing external table, set the appropriate properties on the JDBC tab:

* Set the Database property to the SQL Server database where SQL Server 2019 BDC virtualizes the external table.
* Set the Table property to include the name of the external table.

Note: When you configure the destination to write to an existing external table, the destination cannot automatically handle changes to columns.

To configure the SQL Server 2019 BDC Bulk Loader destination to write to a new external table when handling changes to tables, see Changes to Tables

### Enabling Data Drift Handling <a href="#ariaid-title8" id="ariaid-title8"></a>

You can configure the SQL Server 2019 BDC Bulk Loader destination to automatically handle changes to columns or tables, also known as data drift.

For example, if a record suddenly includes a new `Address2` field, a destination configured to handle column changes creates a new `Address2` column in the target table. Similarly, if the destination writes data to tables based on the region name in the `Region` field and a new `SW-3` region shows up in a record, a destination configured to handle table changes creates a new `SW-3` table and writes the record to the new table.

You can use the data drift functionality to create all necessary tables in an empty database schema.

#### Changes to Columns

To configure the destination to automatically create new columns when data drift occurs, select the Data Drift Enabled property on the JDBC tab. The SQL Server 2019 BDC Bulk Loader destination creates new columns in SQL Server tables when new fields appear in records.Note: The destination cannot create new columns in existing external tables. Therefore, the destination cannot automatically handle changes to columns in external tables.

By default, the destination creates new columns based on the data in the new fields, such as creating a Double column for decimal data. You can, however, configure the destination to create all new columns as Varchar.

#### Changes to Tables <a href="#ariaid-title10" id="ariaid-title10"></a>

In destinations configured to create new columns when data drift occurs, you can also configure the destination to create new tables as needed. Select the Table Auto Create property. By default, the destination creates these new tables as SQL Server tables.

To have the destination create new tables as external tables, select the External Tables property, and then configure the external tables on the External Table tab. The destination requires the external data source and external file format, which you create in SQL Server 2019 BDC. For more information, see the [Microsoft documentation](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-external-table-transact-sql?view=sql-server-ver15).

For example, suppose you created an external data source named `mydatasource` in SQL Server 2019 BDC to write to Hadoop. You also created an external file format named `myfileformat` to write data in text-delimited format. You want to configure the destination to process employee data. When data drift requires new tables, you want the destination to write them to external tables in the /webdata/employee.tbl file in Hadoop. On the JDBC tab, you select the Table Auto Create property and the External Tables property. You configure the External Table tab as follows:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/SQLServerBDCBulk-DataDriftExternalTable.png)

As the pipeline runs, the data that the destination must write includes a new table named `ClickStream`, which contains the fields `url`, `event_date`, and `user_IP`. The destination automatically creates the following SQL statement to create the external table:

```
CREATE EXTERNAL TABLE ClickStream (
    url varchar(50),
    event_date date,
    user_IP varchar(50)
)
WITH (
        LOCATION='/webdata/employee.tbl',
        DATA_SOURCE = mydatasource,
        FILE_FORMAT = myfileformat
    )
;
```

#### Generated Data Types <a href="#ariaid-title11" id="ariaid-title11"></a>

When creating new tables or creating new columns in existing tables, the SQL Server 2019 BDC Bulk Loader destination generates the new column names based on the field names and the column data types based on the type of data in the field.

You can configure the destination to create all new columns as Varchar. However, by default, the SQL Server 2019 BDC Bulk Loader destination creates columns as follows:

| Record Field Data Type | SQL Server 2019 BDC Column Data Type |
| ---------------------- | ------------------------------------ |
| Boolean                | Bit                                  |
| Byte                   | Tinyint                              |
| Byte\_array            | Binary                               |
| Char                   | Char                                 |
| Date                   | Date                                 |
| Datetime               | Datetime                             |
| Decimal                | Decimal                              |
| Float                  | Real                                 |
| Integer                | Int                                  |
| Long                   | Bigint                               |
| Short                  | Smallint                             |
| String                 | Varchar                              |
| Time                   | Time                                 |
| Zoned\_datetime        | Datetimeoffset                       |

Note: The destination cannot write fields that contain data in the Map, List\_map, List, or File\_ref data types. The stage generates an error record when encountering these data types.

### Row Generation <a href="#ariaid-title12" id="ariaid-title12"></a>

By default, the SQL Server 2019 BDC Bulk Loader destination includes all fields from a record in the row written to the SQL Server 2019 BDC table. You can configure the destination to include only specified fields.

In the Row Field property, you specify a map or list-map field in the record that contains all the data that the destination writes from the record. By default, the property is set to the root field, `/`, and the destination writes all the fields from the record. If you specify a map or list-map field, the destination writes only the data from the fields in the specified map or list-map field and excludes all other record data. Edit the Row Field property when the data that you want to write to SQL Server 2019 BDC exists in a single map or list-map field within the record.

If you do not want to include all fields and do not have a map or list-map field that contains the fields you want included, you can configure the destination to ignore specific fields. Edit the Fields to Ignore property to list the fields that you do not want written.

By default, the destination treats records with missing fields or with invalid data types in fields as error records. You can configure the destination to replace missing fields and data of invalid types with default values. You can also specify the default values to use for each data type. You can also configure the destination to replace newline characters in string fields with a replacement character.
