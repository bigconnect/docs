# SQL Server

To add a database connection, click on the **gear** icon in the top right, and navigate to **Admin settings** > **Databases** > **Add a database**.

Fill out the fields for that database, and click **Save changes** at the bottom.

## Settings

You can edit these settings at any time. Just remember to save your changes.

#### Display name

The display name for the database in the Answers interface.

#### Host

Your database’s IP address, or its domain name (e.g., esc.mydatabase.com).

#### Port

The database port. E.g., 1433. To use Dynamic Ports, leave this field empty.

#### Database name

The name of the database you’re connecting to.

#### Database instance name

If you’re running multiple databases on the same host, you can include the instance name here.

#### Username

The database username for the account that you want to use to connect to your database. You can set up multiple connections to the same database using different user accounts to connect to the same database, each with different sets of privileges.

#### Password

The password for the username that you use to connect to the database.

#### Use a secure connection (SSL)

Answers automatically tries to connect to databases with SSL first, then without if that doesn’t work. If it’s possible to connect to your database with an SSL connection, Answers will make that the default setting for your database. If you prefer to connect without this layer of security, you can always change this setting later, but we highly recommend keeping SSL turned on to keep your data secure.

#### ROWCOUNT Override

If left empty, Answers will apply a 2000 row limit to query results. You can set a different limit here, or set to 0 for “unconstrained”.

#### Use an SSH tunnel

See our guide to SSH tunneling.

#### Additional JDBC connection string options

You can append options to the connection string that Answers uses to connect to your database.

#### Re-run queries for simple explorations

Turn this option **OFF** if people want to click **Run** (the play button) before applying any Summarize or filter selections.

By default, Answers will execute a query as soon as you choose an grouping option from the **Summarize** menu or a filter condition from the drill-through menu. If your database is slow, you may want to disable re-running to avoid loading data on each click.

#### Choose when Answers syncs and scans

Turn this option **ON** to manage the queries that Answers uses to stay up to date with your database. For more information, see Syncing and scanning databases.

**Database syncing**

If you’ve selected **Choose when syncs and scans happen** > **ON**, you’ll be able to set:

* The frequency of the [sync](https://www.metabase.com/docs/latest/databases/sync-scan#how-database-syncs-work): hourly (default) or daily.
* The time to run the sync, in the timezone of the server where your Answers app is running.

#### Scanning for filter values

Answers can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database.

If you’ve selected **Choose when syncs and scans happen** > **ON**, you’ll see the following options under **Scanning for filter values**:

* **Regularly, on a schedule** allows you to run scan queries at a frequency that matches the rate of change to your database. The time is set in the timezone of the server where your Answers app is running. This is the best option for a small database, or tables with distinct values that get updated often.
* **Only when adding a new filter widget** is a great option if you want scan queries to run on demand. Turning this option **ON** means that Answers will only scan and cache the values of the field(s) that are used when a new filter is added to a dashboard or SQL question.
* **Never, I’ll do this manually if I need to** is an option for databases that are either prohibitively large, or which never really have new values added. Use the Re-scan field values now button to run a manual scan and bring your filter values up to date.

#### Periodically refingerprint tables

> Periodic refingerprinting will increase the load on your database.

Turn this option **ON** to scan a sample of values every time Answers runs a [sync](https://www.metabase.com/docs/latest/databases/sync-scan#how-database-syncs-work).

A fingerprinting query examines the first 10,000 rows from each column and uses that data to guesstimate how many unique values each column has, what the minimum and maximum values are for numeric and timestamp columns, and so on. If you leave this option **OFF**, Answers will only fingerprint your columns once during setup.

#### Default result cache duration

How long to keep question results. By default, Answers will use the value you supply on the cache settings page, but if this database has other factors that influence the freshness of data, it could make sense to set a custom duration. You can also choose custom Answers on individual questions or dashboards to help improve performance.

Options are:

* **Use instance default (TTL)**. TTL is time to live, meaning how long the cache remains valid before Answers should run the query again.
* **Custom**.

You can also set cache duration per questions. See Advanced caching controls.

### Querying SQL Server Analysis Services (SSAS) data

BigConnect Answers can query multidimensional and tabular models hosted in SQL Server Analysis Services (SSAS) through your SQL Server connection. SQL Server reaches the SSAS model through a **linked server**, runs an MDX or DAX query against it with `OPENQUERY`, and exposes the result as a regular view. Answers then reads that view like any other table: you can build questions, dashboards, filters, alerts and subscriptions on top of cube data.

You don't need a separate driver. Everything below happens on the SQL Server side, using the SQL Server connection you set up above.

{% hint style="info" %}
This works with SSAS **Multidimensional** (cubes, queried with MDX) and SSAS **Tabular** (models, queried with DAX or MDX), and with every SQL Server edition that supports linked servers.
{% endhint %}

#### Prerequisites

* The **Analysis Services OLE DB provider (MSOLAP)** is installed on the machine that runs SQL Server. It ships with the Analysis Services client libraries.
* In SQL Server Management Studio, under **Server Objects** > **Linked Servers** > **Providers** > **MSOLAP** > **Properties**, the **Allow inprocess** option is turned on.
* The account SQL Server uses to connect to SSAS has at least read access to the SSAS database (a role with **Read** permission on the cube or model). SSAS only accepts Windows authentication; the simplest setup is to grant the role to the SQL Server service account.
* The SQL Server login that Answers uses has `SELECT` permission on the views you create in the next step

#### Step 1: Create a linked server to SSAS

Run the following on the SQL Server instance that Answers connects to. Replace the data source and catalog with your SSAS server and database.

```sql
EXEC master.dbo.sp_addlinkedserver
    @server     = N'SSAS',            -- name you will use in OPENQUERY
    @srvproduct = N'',
    @provider   = N'MSOLAP',
    @datasrc    = N'ssas-server\INSTANCE',
    @catalog    = N'CustomsAnalytics';  -- SSAS database

-- Connect to SSAS without impersonating the caller
EXEC master.dbo.sp_addlinkedsrvlogin
    @rmtsrvname = N'SSAS',
    @useself    = N'FALSE',
    @locallogin = NULL;
```

Test the connection:

```sql
SELECT * FROM OPENQUERY(SSAS, 'SELECT [Measures].DefaultMember ON 0 FROM [Excise]');
```

#### Step 2: Expose cube data as views

Create a view for each result set you want to analyze in Answers. Choose the grain carefully: the MDX or DAX query runs inside SSAS, so aggregate there rather than pulling the lowest level of detail.

**Multidimensional cube (MDX)**

```sql
CREATE VIEW analytics.v_excise_by_office_month AS
SELECT
    CAST("[Office].[Office Name].[Office Name].[MEMBER_CAPTION]" AS nvarchar(200)) AS office,
    CAST("[Date].[Calendar].[Month].[MEMBER_CAPTION]"           AS nvarchar(50))  AS month,
    CAST("[Measures].[Authorizations Issued]"                   AS int)           AS authorizations_issued,
    CAST("[Measures].[Guarantee Amount]"                        AS decimal(18,2)) AS guarantee_amount
FROM OPENQUERY(SSAS, '
    SELECT
        { [Measures].[Authorizations Issued], [Measures].[Guarantee Amount] } ON COLUMNS,
        NON EMPTY [Office].[Office Name].[Office Name].Members
                * [Date].[Calendar].[Month].Members ON ROWS
    FROM [Excise]');
```

**Tabular model (DAX)**

```sql
CREATE VIEW analytics.v_eori_requests_by_status AS
SELECT
    CAST("Request[Status]"          AS nvarchar(50)) AS status,
    CAST("Date[Year]"               AS int)          AS year,
    CAST("[Requests]"               AS int)          AS requests
FROM OPENQUERY(SSAS, '
    EVALUATE
    SUMMARIZECOLUMNS(
        Request[Status],
        ''Date''[Year],
        "Requests", [Request Count]
    )');
```

Always alias and `CAST` the columns. SSAS returns column names such as `[Measures].[Guarantee Amount]` and generic data types; explicit names and types let Answers recognize numbers, dates and categories correctly.

#### Step 3: Use the views in Answers

1. In **Admin settings** > **Databases**, open the SQL Server connection and click **Sync database schema now**.
2. The views appear under the schema you created them in (in the example, `analytics`). Add descriptions and semantic types in **Admin settings** > **Table metadata**, like for any table.
3. Build questions with the query builder or in the native SQL editor, add them to dashboards, and use filters, drill-through, alerts and subscriptions as usual.

You can also query SSAS directly from the native SQL editor:

```sql
SELECT *
FROM OPENQUERY(SSAS, 'EVALUATE TOPN(100, ''Operator'')');
```

#### Permissions and security <a href="#user-content-permissions-and-security" id="user-content-permissions-and-security"></a>

* SSAS sees every query as coming from the account configured on the linked server, not from the individual Answers user. Control who sees which cube data with Answers data and collection permissions on the views.
* Any SQL Server login mapped to the linked server can run arbitrary MDX or DAX through `OPENQUERY`. To keep users on the curated views, don't grant **Native query editing** on this database in Answers, and map only the Answers login with `sp_addlinkedsrvlogin @locallogin = N'<answers_login>'` instead of `NULL`

#### Limitations <a href="#user-content-limitations" id="user-content-limitations"></a>

* Results come back as flat rows and columns. Cube hierarchies, member properties and formatting defined in SSAS are not carried over; recreate hierarchies with Answers drill-through and segments.
* `OPENQUERY` takes a literal query string, so Answers filters on a view are applied by SQL Server after SSAS returns the result. Keep each view at the grain your dashboards need, or create parameterized stored procedures for very large cubes.
* Writeback to SSAS is not supported through this path.
* The SSAS server must be reachable from the SQL Server host, not from the Answers server.
