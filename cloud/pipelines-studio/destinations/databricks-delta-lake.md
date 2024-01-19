# Databricks Delta Lake

The Databricks Delta Lake destination writes data to one or more Delta Lake tables on Databricks.&#x20;

Use the Databricks Delta Lake destination for the following use cases:

**Bulk load new data into Delta Lake tables**

Build a pipeline that bulk loads new data into Delta Lake tables on Databricks. When processing new data, the destination uses the COPY command to load data into Delta Lake tables. For a detailed solution of how to design this pipeline, see Bulk Loading Data into a Delta Lake Table.

**Merge changed data into Delta Lake tables**

Build a pipeline that reads change data capture (CDC) data from a database and replicates the changes to Delta Lake tables on Databricks. When processing CDC data, the destination uses the MERGE command to load data into Delta Lake tables. For a detailed solution of how to design this pipeline, see Merging Changed Data into a Delta Lake Table.

The Databricks Delta Lake destination first stages the pipeline data in text files in Amazon S3 or Azure Data Lake Storage Gen2. Then, the destination sends the COPY or MERGE command to Databricks to process the staged files.

The Databricks Delta Lake destination uses a JDBC URL to connect to the Databricks cluster. When you configure the destination, you specify the JDBC URL and credentials to use to connect to the cluster. You also define the connection information that the destination uses to connect to the staging location in Amazon S3 or Azure Data Lake Storage Gen2.

You specify the tables in Delta Lake to write the data to. The destination writes data from record fields to table columns based on matching names. You can configure the destination to compensate for data drift by creating new columns in existing database tables when new fields appear in records or by creating new database tables.

You can configure the root field for the row, and any first-level fields that you want to exclude from the record. You can also configure the destination to replace missing fields or fields with invalid data types with user-defined default values and to replace newline characters in string fields with a specified character. You can specify the quoting mode, define quote and escape characters, and configure the destination to trim spaces.

The Databricks Delta Lake destination can use CRUD operations defined in the `sdc.operation.type` record header attribute to write data. For information about Data Collector change data processing and a list of CDC-enabled origins, see Processing Changed Data.
