# Databases

## Adding and managing databases <a href="#adding-and-managing-databases" id="adding-and-managing-databases"></a>

Adding a database connection

To add a database connection, click on the **gear** icon in the top right, and navigate to **Admin settings** > **Databases** > **Add a database**.

Fill out the fields for that database, and click **Save changes** at the bottom.

The connection settings differ database to database. For the list of connection settings available for your database, click on the link to your database below.

## Deleting databases

**Caution: Deleting a database is irreversible! All saved questions and dashboard cards based on the database will be deleted as well!**

Go to **Admin settings** > **Databases** > your database and click **Remove this database**.

## Restoring the Sample Database

If you’ve deleted the Answers Sample Database, go to **Admin settings** > **Databases** and click **Bring the Sample Database back**.

## Database users, roles, and privileges <a href="#database-users-roles-and-privileges" id="database-users-roles-and-privileges"></a>

We recommend creating a `answers` database user with the following database roles:

* `analytics` for read access to any schemas or tables used for analysis.
* Optional `answers_actions` for write access to tables used for Answers actions.
* Optional `answers_model_caching` for write access to the schema used for Answers model caching.

Bundling your privileges into roles based on use cases makes it easier to manage privileges in the future (especially in multi-tenant situations). For example, you could:

* Use the same `analytics` role for other BI tools in your data stack that need read-only access to the analytics tables in your database.
* Revoke the write access for `answers_model_caching` without affecting the write access for `answers_actions`.

### Minimum database privileges

n order to view and query your tables in Answers, you’ll have to give Answers’s database user:

* `CONNECT` to your database.
* `SELECT` privileges to any schemas or tables that you want to use in Answers.

To organize these privileges (and make maintenance easier down the line):

* Create a database role called `analytics`.
* Create a database user called `answers`.
* Add `answers` to the `analytics` role.
* Add privileges to the `analytics` role.

For example, if you’re using a Postgres database, you’d log in as an admin and run the SQL statements:

```sql
-- Create a role named "analytics".
CREATE ROLE analytics WITH LOGIN;

-- Add the CONNECT privilege to the role.
GRANT CONNECT ON DATABASE "your_database" TO analytics;

-- Create a database user named "answers".
CREATE USER answers WITH PASSWORD "your_password";

-- Give the role to the answers user.
GRANT analytics TO answers;

-- Add query privileges to the role (options 1-4):

-- Option 1: Uncomment the line below to let users with the analytics role query ALL DATA (In Postgres 14 or higher. See [Predefined Roles](https://www.postgresql.org/docs/current/predefined-roles.html#PREDEFINED-ROLES)).
-- GRANT pg_read_all_data TO analytics;

-- Option 2: Uncomment the line below to let users with the analytics role query anything in the DATABASE.
-- GRANT USAGE ON DATABASE "your_schema" TO analytics;
-- GRANT SELECT ON DATABASE "your_schema"  TO analytics;

-- Option 3: Uncomment the line below to let users with the analytics role query anything in a specific SCHEMA.
-- GRANT USAGE ON SCHEMA "your_schema" TO analytics;
-- GRANT SELECT ON ALL TABLES IN SCHEMA "your_schema" TO analytics;

-- Option 4: Uncomment the line below to let users with the analytics role query anything in a specific TABLE.
-- GRANT USAGE ON SCHEMA "your_schema" TO analytics;
-- GRANT SELECT ON "your_table" IN SCHEMA "your_schema" TO analytics;

```

Depending on how you use Answers, you can also additionally grant:

* `TEMPORARY` privileges to create temp tables.
* `EXECUTE` privileges to use stored procedures or user-defined functions.

Remember that when you grant privileges to a role, all users with that role will get those privileges.

### Grant all database privileges

If you don’t want to structure your database privileges yet:

* Create a `answers` database user.
* Give `answers` all privileges to the database.

This is a good option if you’re connecting to a local database for development or testing.

### Privileges to enable actions

Actions let Answers write back to specific tables in your database.

In addition to the minimum database privileges, you’ll need to grant write access to any tables used with actions:

* Create a new role called `answers_actions`.
* Give the role `INSERT`, `UPDATE`, and `DELETE` privileges to any tables used with Answers actions.
* Give the `answers_actions` role to the `answers` user.

```sql
-- Create a role to bundle database privileges for Answers actions.
CREATE ROLE answers_actions WITH LOGIN;

-- Grant write privileges to the TABLE used with Answers actions.
GRANT INSERT, UPDATE, DELETE ON "your_table" IN SCHEMA "your_schema" TO answers_actions;

-- Grant role to the answers user.
GRANT answers_actions TO answers;
```

### Privileges to enable model caching

Model caching lets Answers save query results to a specific schema in your database. Answers’s database user will need the `CREATE` privilege to set up the dedicated schema for model caching, as well as write access (`INSERT`, `UPDATE`, `DELETE`) to that schema.

In addition to the minimum database privileges:

* Create a new role called `answers_model_caching`.
* Give the role `CREATE` access to the database.
* Give the role `INSERT`, `UPDATE`, and `DELETE` privileges to the schema used for model caching.
* Give the `answers_model_caching` role to the `answers` user.

```sql
-- Create a role to bundle database privileges for Answers model caching.
CREATE ROLE answers_model_caching WITH LOGIN;

-- If you don't want to give CREATE access to your database,
-- add the schema manually before enabling modeling caching.
GRANT CREATE ON "database" TO answers_model_caching;

-- Grant write privileges to the SCHEMA used for model caching.
GRANT USAGE ON "your_schema" TO answers_model_caching;
GRANT INSERT, UPDATE, DELETE ON "your_model's_table" IN SCHEMA "your_schema" TO answers_model_caching;

-- Grant role to the answers user.
GRANT answers_model_caching TO answers;
```

### Privileges to enable uploads

You can upload CSVs to supported databases. Answers’s database user should have write access (`INSERT`, `UPDATE`, `DELETE`) to the schema where you want to store the uploads.

You’ll first need to create a schema to store uploads (or use an existing schema) and tell Answers that you want to use that schema to store uploads.

In addition to the minimum database privileges:

* Create a new role called `answers_uploads`.
* Give the role `INSERT`, `UPDATE`, and `DELETE` privileges to the schema where you want to store uploads.
* Give the `answers_uploads` role to the `answers` user.

```sql
-- Create a role to bundle database privileges for uploads.
CREATE ROLE answers_uploads WITH LOGIN;

-- Grant write privileges to the SCHEMA used for uploads.
GRANT USAGE ON "your_schema" TO answers_uploads;
GRANT INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA "your_schema" TO answers_uploads;

-- Grant role to the answers user.
GRANT answers_uploads TO answers;

```

### Multi-tenant permissions

If you’re setting up multi-tenant permissions for customers who need SQL access, you can create one database connection per customer. That means each customer will connect to the database using their own database user.

Let’s say you have customers named Tangerine and Lemon:

* Create new database users `answers_tangerine` and `answers_lemon`.
* Create a `customer_facing_analytics` role with the `CONNECT` privilege.
* Create roles to bundle privileges specific to each customer’s use case. For example:
  * `tangerine_queries` to bundle read privileges for people to query and create stored procedures against the Tangerine schema.
  * `lemon_queries` to bundle read privileges for people to query tables in the Lemon schema.
  * `lemon_actions` to bundle the write privileges needed to create actions on a Lemonade table in the Lemon schema.
* Add each user to their respective roles.

```sql
-- Create one database user per customer.
CREATE USER answers_tangerine WITH PASSWORD "orange";
CREATE USER answers_lemon WITH PASSWORD "yellow";

-- Create a role to bundle privileges for all customers.
CREATE ROLE customer_facing_analytics;
GRANT CONNECT ON DATABASE "citrus" TO customer_facing_analytics;
GRANT customer_facing_analytics TO answers_tangerine, answers_lemon;

-- Create a role to bundle analytics read access for customer Tangerine.
CREATE ROLE tangerine_queries;
GRANT USAGE ON SCHEMA "tangerine" TO tangerine_queries;
GRANT SELECT, EXECUTE ON ALL TABLES IN SCHEMA "tangerine" TO tangerine_queries;
GRANT tangerine_queries TO answers_tangerine;

-- Create a role to bundle analytics read access for customer Lemon.
CREATE ROLE lemon_queries;
GRANT USAGE ON SCHEMA "lemon" TO lemon_queries;
GRANT SELECT ON ALL TABLES IN SCHEMA "lemon" TO lemon_queries;
GRANT lemon_queries TO answers_lemon;

-- Create a role to bundle privileges to Answers actions for customer Lemon.
CREATE ROLE lemon_actions;
GRANT INSERT, UPDATE, DELETE ON TABLE "lemonade" IN SCHEMA "lemon" TO lemon_actions;
GRANT lemon_actions TO answers_lemon;
```

We recommend bundling privileges into roles based on use cases per customer. That way, you can reuse common privileges across customers while still being able to grant or revoke granular privileges per customer. For example:

* If customer Tangerine needs to query the Tangerine schema from another analytics tool, you can use the `tangerine_queries` role when setting up that tool.
* If customer Lemon decides that they don’t want to use Answers actions anymore (but they still want to ask questions), you can simply revoke or drop the `lemon_actions` role.

## Syncing and scanning databases <a href="#syncing-and-scanning-databases" id="syncing-and-scanning-databases"></a>

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

## Encrypting your database connection <a href="#encrypting-your-database-connection" id="encrypting-your-database-connection"></a>

Answers stores connection information for the various databases you add in the Answers application database. To prevent bad actors from being able to access these details if they were to gain access to the application DB, Answers can automatically encrypt them with AES256 + SHA512 when they are saved, and decrypt them on-the-fly whenever they are needed.

### Creating an encryption key

1.  Generate a secret key that is at least 16 characters (longer is even better!). We recommend using a secure random key generator, such as `openssl`.

    > Take care not to lose this key because you can’t decrypt connection details without it. If you lose (or change) the key, you’ll have to reset all of the connection details that have been encrypted with it in the Admin Panel.
2. Set your secret key as the environment variable `MB_ENCRYPTION_SECRET_KEY`.

**Example commands for creating and adding a key**

1.  You can use `openssl` to generate a cryptographically-secure, randomly-generated 32-character key.

    ```
    openssl rand -base64 32
    ```
2.  Copy the key to your clipboard. It should look something like this:

    ```
    IYqrSi5QDthvFWe4/WdAxhnra5DZC3RKx3ZSrOJDKsM=
    ```
3.  Set the key as an environment variable and start Answers as usual.

    ```
    MB_ENCRYPTION_SECRET_KEY="IYqrSi5QDthvFWe4/WdAxhnra5DZC3RKx3ZSrOJDKsM=" java -jar answers.jar
    ```

Once you set the `MB_ENCRYPTION_SECRET_KEY` value, Answers will securely encrypt and store the connection details for each new database that you add.

Note:

Some versions of Linux interpret single-quotes (`'`) and double-quotes (`"`) differently for environment variable values. If you upgrade to a newer version of Linux, and the key originally used single-quotes, you might need to wrap the key in double-quotes, so that the single-quotes are interpreted literally. For example, `MB_ENCRYPTION_SECRET_KEY='IYq...sM='` would become `MB_ENCRYPTION_SECRET_KEY="'IYq...sM='"`

### Encrypting an existing connection

If you added databases before setting the MB\_ENCRYPTION\_SECRET\_KEY value, you can encrypt the connection details by going to each one of those databases in Admin settings > Databases and clicking on the Save button. Existing databases with unencrypted details will continue to work normally.

### Rotating an encryption key

1. We recommend that you backup your data before doing a key rotation.
2. Stop running your Answers app.
3. Run the CLI command `rotate-encryption-key`.
   * Set the current encryption key as `MB_ENCRYPTION_SECRET_KEY`.
   * Set the new encryption key as a parameter.

**Example command for rotating a key**

```
MB_ENCRYPTION_SECRET_KEY=your-current-key java -jar answers.jar rotate-encryption-key new-key
```

### Disabling an encryption key

To disable an encryption key, follow the steps to rotate an encryption key, but use an empty string (`""`) as the new key.

**Example command for disabling a key**

```
MB_ENCRYPTION_SECRET_KEY="your-current-key" java -jar answers.jar rotate-encryption-key ""
```

