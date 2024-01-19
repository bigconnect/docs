# Amazon Redshift

To add a database connection, click on the **gear** icon in the top right, and navigate to **Admin settings** > **Databases** > **Add a database**.

## Settings

You can edit these settings at any time. Just remember to save your changes.

#### Display name

The display name for the database in the Answers interface.

#### Host

Your database’s IP address, or its domain name (e.g., esc.mydatabase.com).

#### Port

The database port. E.g., 3306.

#### Database name

The name of the database you want to connect to.

#### Schemas

Here you can specify which schemas you want to sync and scan. Options are:

* All
* Only these…
* All except…

For the **Only these** and **All except** options, you can input a comma-separated list of values to tell Answers which schemas you want to include (or exclude). For example:

```
foo,bar,baz     
```

You can use the `*` wildcard to match multiple schemas.

Let’s say you have three schemas: foo, bar, and baz.

* If you have **Only these…** set, and enter the string `b*`, you’ll sync with bar and baz.
* If you have **All except…** set, and enter the string `b*`, you’ll just sync foo.

Note that only the `*` wildcard is supported; you can’t use other special characters or regexes.

#### Username

The database username for the account that you want to use to connect to your database. You can set up multiple connections to the same database using different user accounts to connect to the same database, each with different sets of privileges.

#### Password

The password for the username that you use to connect to the database.

#### Use an SSH tunnel

See our guide to SSH tunneling.

#### Additional JDBC connection string options

You can append options to the connection string that Answers uses to connect to your database.

#### Re-run queries for simple explorations

Turn this option **OFF** if people want to click **Run** (the play button) before applying any Summarize or filter selections.

By default, Answers will execute a query as soon as you choose an grouping option from the **Summarize** menu or a filter condition from the drill-through menu. If your database is slow, you may want to disable re-running to avoid loading data on each click.

#### Choose when Answers syncs and scans

Turn this option **ON** to manage the queries that Answers uses to stay up to date with your database. For more information, see Syncing and scanning databases.

#### Database syncing

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

Turn this option **ON** to scan a sample of values every time Answers runs a sync.

A fingerprinting query examines the first 10,000 rows from each column and uses that data to guesstimate how many unique values each column has, what the minimum and maximum values are for numeric and timestamp columns, and so on. If you leave this option **OFF**, Answers will only fingerprint your columns once during setup.

#### Default result cache duration

How long to keep question results. By default, Answers will use the value you supply on the cache settings page, but if this database has other factors that influence the freshness of data, it could make sense to set a custom duration. You can also choose custom durations on individual questions or dashboards to help improve performance.

Options are:

* **Use instance default (TTL)**. TTL is time to live, meaning how long the cache remains valid before Answers should run the query again.
* **Custom**.

You can also set cache duration per questions. See Advanced caching controls.
