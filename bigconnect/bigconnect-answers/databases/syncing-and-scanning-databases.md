# Syncing and scanning databases

Answers runs different types of queries to stay up to date with your database.

* Syncs get updated schemas to display in the Data Browser.
* Scans take samples of column values to populate filter drop-down menus and suggest helpful visualizations. Answers does not store _complete_ tables from your database.
* Fingerprinting takes an additional sample of column values to help with smart behavior, such as auto-binning for bar charts.

### Initial sync, scan, and fingerprinting

When Answers first connects to your database, Answers performs a sync to determine the metadata of the columns in your tables and automatically assign each column a semantic type. Once the sync is successful, Answers runs scans of each table to look for URLs, JSON, encoded strings, etc. The fingerprinting queries run once the syncs are complete.

You can follow the progress of these queries from **Admin** > **Troubleshooting** > **Logs**.

Once the queries are done running, you can view and edit the synced metadata from **Admin settings** > **Table Metadata**. For more info, see editing metadata.

### Choose when Answers syncs and scans

If you want to change the default schedule for sync and scan queries:

1. Go to **Admin** > **Databases** > your database.
2. Expand **Show advanced options**.
3. Turn ON **Choose when syncs and scans happen**.

### Scheduling database syncs

If you’ve turned on Choose when syncs and scans happen, you’ll be able to set:

* The frequency of the sync: hourly (default) or daily.
* The time to run the sync, in the timezone of the server where your Answers app is running.

### Scheduling database scans

If you’ve turned ON Choose when syncs and scans happen, you’ll see the following scan options:

* **Regularly, on a schedule** allows you to run scan queries at a frequency that matches the rate of change to your database. The time is set in the timezone of the server where your Answers app is running. This is the best option for a small database, or tables with distinct values that get updated often.
* **Only when adding a new filter widget** is a great option if you want scan queries to run on demand. Turning this option **ON** means that Answers will only scan and cache the values of the field(s) that are used when a new filter is added to a dashboard or SQL question.
* **Never, I’ll do this manually if I need to** is an option for databases that are either prohibitively large, or which never really have new values added. Use the Re-scan field values now button to run a manual scan and bring your filter values up to date.

### Manually syncing tables and columns

1. Go to **Admin settings** > **Databases** > your database.
2. Click **Sync database schema now**.

### Manually scanning column values

To scan values from all the columns in a table:

1. Go to **Admin settings** > **Table Metadata** > your database.
2. Select the table that you want to bring up to date with your database.
3. Click the **gear icon** at the top of the page.
4. Click **Re-scan this table**.

To scan values from a specific column:

1. Go to **Admin settings** > **Table Metadata** > your database.
2. Select the table.
3. Find the column you want bring up to date with your database.
4. Click the **gear icon** in the panel for that column.
5. Click **Re-scan this field**.

### Periodically refingerprint tables

> Periodic refingerprinting will increase the load on your database.

By default, Answers only runs fingerprinting queries when you first connect your database.

Turn this setting ON if you want Answers to use larger samples of column values when making suggestions in the UI:

1. Go to **Admin** > **Databases** > your database.
2. Expand **Show advanced options**.
3. Turn ON **Periodically refingerprint tables**.

### Clearing cached values

To ask Answers to forget the data from previous scans and fingerprinting:

1. Go to **Admin settings** > **Table Metadata** > your database.
2. Select the table.
3. Optional: select the column.
4. Click the **gear icon**.
5. Click **Discard cached field values**.

### Disabling syncing and scanning for specific tables

To prevent Answers from running syncs and scans against a specific table, change the table visibility to **Hidden**:

1. Go to **Admin settings** > **Table Metadata** > your database.
2. Hover over the table name in the sidebar.
3. Click the **eye** icon.

{% hint style="info" %}
Hiding a table will also prevent it from showing up in the query builder and data reference. People can still query hidden tables from the SQL editor.
{% endhint %}

### Syncing and scanning using the API

Answers syncs and scans regularly, but if the database administrator has just changed the database schema, or if a lot of data is added automatically at specific times, you may want to write a script that uses the Answers API to force a sync or scan. Our API provides two ways to initiate a sync or scan of a database:

1. Using a session token: the `/api/database/:id/sync_schema` or `api/database/:id/rescan_values` endpoints. These endpoints do the same things as going to the database in the Admin Panel and choosing **Sync database schema now** or **Re-scan field values now** respectively. To use these endpoints, you have to authenticate with a user ID and pass a session token in the header of your request.
2. Using an API key: `/api/notify/db/:id`. We created this endpoint so that people could notify their Answers to sync after an ETL operation finishes. To use this endpoint, you must pass an API key by defining the `MB_API_KEY` environment variable.

### How database syncs work

An Answers **sync** is a query that gets a list of updated table and view names, column names, and column data types from your database:

```sql
SELECT
    TRUE
FROM 
    "your_schema"."your_table_or_view"
WHERE 
    1 <> 1
LIMIT 0
```

This query runs against your database during setup, and again every hour by default. This scanning query is fast with most relational databases, but can be slower with MongoDB. Syncing can’t be turned off completely, otherwise Answers wouldn’t work.

### How database scans work

An Answers **scan** is a query that caches the column _values_ for filter drop-downs by looking at the first 1,000 distinct records from each table, in ascending order:

```sql
SELECT
    "your_table_or_view"."column" AS "column"
FROM
    "your_schema"."your_table_or_view"
GROUP BY
    "your_table_or_view"."column"
ORDER BY
    "your_table_or_view"."column" ASC
LIMIT 1000
```

For each record, Answers only stores the first 100 kilobytes of text, so if you have data with 1,000 characters each (like addresses), and your column has more than 100 unique addresses, Answers will only cache the first 100 values from the scan query.

Cached column values are displayed in filter drop-down menus. If people type in the filter search box for values that aren’t in the first 1,000 distinct records or 100kB of text, Answers will run a query against your database to look for those values on the fly.

A scan is more intensive than a sync query, so it only runs once during setup, and again once a day by default. If you disable scans entirely, you’ll need to bring things up to date by running manual scans.

To reduce the number of tables and fields Answers needs to scan in order to stay current with your connected database, Answers will only scan values for fields that someone has queried in the last fourteen days.

### How database fingerprinting works

The fingerprinting query looks at the first 10,000 rows from a given table or view in your database:

```sql
SELECT
    *
FROM
    "your_schema"."your_table_or_view"
LIMIT 10000

```

The result of this query is used to provide better suggestions in the Answers UI (such as filter drop-downs and auto-binning). To avoid putting strain on your database, Answers only runs fingerprinting queries the first time you set up a database connection. To change this default, you can turn ON Periodically re-fingerprint tables.
