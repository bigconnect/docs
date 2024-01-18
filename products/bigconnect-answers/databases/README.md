# Databases

### Adding and managing databases <a href="#adding-and-managing-databases" id="adding-and-managing-databases"></a>

Adding a database connection

To add a database connection, click on the **gear** icon in the top right, and navigate to **Admin settings** > **Databases** > **Add a database**.

Fill out the fields for that database, and click **Save changes** at the bottom.

The connection settings differ database to database. For the list of connection settings available for your database, click on the link to your database below.

### Connecting to supported databases

The databases listed below have official drivers maintained by the MWARE team.&#x20;

* [Amazon Athena](https://www.metabase.com/docs/latest/databases/connections/athena)
* [BigQuery](https://www.metabase.com/docs/latest/databases/connections/bigquery) (Google Cloud Platform)
* [Druid](https://www.metabase.com/docs/latest/databases/connections/druid)
* [MongoDB (version 4.2 or higher)](https://www.metabase.com/docs/latest/databases/connections/mongodb)
* [MySQL (version 5.7 or higher, as well as MariaDB version 10.2 or higher)](https://www.metabase.com/docs/latest/databases/connections/mysql)
* [Oracle](https://www.metabase.com/docs/latest/databases/connections/oracle)
* [PostgreSQL](https://www.metabase.com/docs/latest/databases/connections/postgresql)
* [Presto](https://www.metabase.com/docs/latest/databases/connections/presto)
* [Redshift (Amazon Web Services)](https://www.metabase.com/docs/latest/databases/connections/redshift)
* [Snowflake](https://www.metabase.com/docs/latest/databases/connections/snowflake)
* [SparkSQL](https://www.metabase.com/docs/latest/databases/connections/sparksql)
* [SQL Server](https://www.metabase.com/docs/latest/databases/connections/sql-server)
* [SQLite](https://www.metabase.com/docs/latest/databases/connections/sqlite)
* [Vertica](https://www.metabase.com/docs/latest/databases/connections/vertica)

If you don’t see your database listed here, see [partner and community drivers](https://www.metabase.com/docs/latest/developers-guide/partner-and-community-drivers#partner-drivers).

As of version 46.6.4, Metabase [no longer supports H2 connections](https://www.metabase.com/blog/security-incident-summary). But Metabase still ships with an H2 database to include an embedded application database, as well as to provide some sample data out of the box.

[Connecting to databases hosted by a cloud provider](https://www.metabase.com/docs/latest/databases/connecting#connecting-to-databases-hosted-by-a-cloud-provider)

For provider-specific connection details, like connecting to a PostgreSQL data warehouse on RDS:

* [AWS’s Relational Database Service (RDS)](https://www.metabase.com/docs/latest/databases/connections/aws-rds)

[Granting database privileges](https://www.metabase.com/docs/latest/databases/connecting#granting-database-privileges)

For Metabase to connect, query, or write to your database, you must give Metabase a database user account with the correct database privileges. See [Database roles, users, and privileges](https://www.metabase.com/docs/latest/databases/users-roles-privileges).

[Syncing and scanning databases](https://www.metabase.com/docs/latest/databases/connecting#syncing-and-scanning-databases)

See [Syncing and scanning](https://www.metabase.com/docs/latest/databases/sync-scan).

[Deleting databases](https://www.metabase.com/docs/latest/databases/connecting#deleting-databases)

**Caution: Deleting a database is irreversible! All saved questions and dashboard cards based on the database will be deleted as well!**

Go to **Admin settings** > **Databases** > your database and click **Remove this database**.

[Restoring the Sample Database](https://www.metabase.com/docs/latest/databases/connecting#restoring-the-sample-database)

If you’ve deleted the Metabase [Sample Database](https://www.metabase.com/glossary/sample\_database), go to **Admin settings** > **Databases** and click **Bring the Sample Database back**.
