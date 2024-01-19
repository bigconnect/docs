# Snowflake

The Snowflake destination writes data to one or more tables in a Snowflake database. You can use the Snowflake destination to write to any accessible Snowflake database, including those hosted on Amazon S3, Microsoft Azure, and private Snowflake installations.&#x20;

The Snowflake destination stages CSV files to either an internal Snowflake stage or an external stage in Amazon S3 or Microsoft Azure. Then, the destination sends a command to Snowflake to process the staged files.

You can use the Snowflake destination to write new data or change data capture (CDC) data to Snowflake. When processing new data, the destination can load data to Snowflake using the COPY command or Snowpipe. When processing CDC data, the destination uses the MERGE command.

The Snowflake destination writes data from record fields to table columns based on matching names. The destination can compensate for data drift by creating new columns and tables in Snowflake when new fields and table references appear in records.

When you configure the Snowflake destination, you specify the Snowflake region, account and connection information and the number of connections to use to write to Snowflake. You can also define additional Snowflake connection properties as needed.

You configure the Snowflake warehouse, database, schema, and the tables to use. You specify load method properties and staging details, and optionally define advanced properties for Amazon S3 or Microsoft Azure.

You can optionally enable data drift. When enabled, you can have the destination create new tables if you aren't using Snowpipe. You can also specify whether to create all new columns as Varchar instead of inferring the type, and whether to create Decimal columns for decimal data.

You can configure the root field for the row, and any first-level fields that you want to exclude from the record. You can also configure the destination to replace missing fields or fields containing invalid data types with the specified default values, and to replace newline characters in string fields with a specified character. You can specify the quoting mode, define quote and escape characters, and configure the destination to trim spaces.

When processing CDC data, you can specify the primary key columns for each table or have the destination query Snowflake for that information.

Before you use the Snowflake destination, you must install the Snowflake stage library and complete other prerequisite tasks.&#x20;

Note: we recommend configuring the Snowflake warehouse to auto-resume upon receiving new queries.

Tip: If you change the destination table schema manually rather than enabling [data drift handling](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Destinations/Snowflake.html#concept\_cn1\_msp\_2gb), restart the pipeline to allow the destination to discover schema changes.
