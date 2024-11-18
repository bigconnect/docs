# SQL troubleshooting

Within this section we will discuss how to troubleshoot issues with your SQL database - this will likely be with the help of a member of the team. If you are experiencing an issue one of the best ways to resolve it is to contact our support.

### Minimum SQL versions

You must be using a minimum version of:

1. SQL Server - SQL Server 2016 and onwards
2. MySQL - MySQL 8.0 and onwards
3. MariaDB - MariaDB 10.6 and onwards
4. Oracle - Oracle Database 19c and onwards
5. Postgres - Postgres 9.4 and onwards

This is primarily for aggregate functions and Common Table Expressions (CTE) which are required for it to function.

Other dialect "compatible" databases may work, but we cannot guarantee support for these in future versions. If you need guaranteed support, please reach out to us.

### Log SQL queries

> ### 📘SQL query logging
>
> This feature is designed for investigating issues accessing data in your SQL database, we recommend only enabling this when needed and disabling in production environments.

It is possible to log the queries that the system is sending to your SQL database, to investigate issues or performance problems you're encountering when accessing a database. This can only be enabled in self host environments. To enable this set the environment variable `SQL_LOGGING_ENABLE` to `1` on your app services (docker-compose or Kubernetes) or if using the single image simply supply this environment variable to the container. Once you have restarted the server/container you will see SQL queries being logged in a format like:

`[SQL] [PG] query="select * from table limit $1" values="100"`

### SQL Schema export

> ### 📘Schema export is designed for debugging
>
> This feature is designed to help the team resolve issues with your schema - we want to provide the best experience possible for every user out of the box and providing your schema helps to debug issues with typing, relations and other unique scenarios.

In this article we will cover how to export the schema of your database - this can be done manually with tools like `pg_dump` for Postgres, but we have provided a tool  to export what is most helpful when debugging issues. We would also like to highlight that this export is _only_ the structure and schema of your tables and does not contain any data; however if you have any sensitive table or column names please discuss this with the team.

It is worth noting while the export is a good representation of your data, it is not designed to be used to replicate your data within a production environment, we have designed this to extract the information needed for debugging, not a perfect mirror of the database.

### Exporting your schema

This feature is currently available exclusively for self-hosted instances. It is unlikely that this capability will be added to cloud instances.

To start exporting your schema you will need to click into your database that you are experiencing an issue with. To start, click into the datasource from the "Data" section of the app builder.

![](https://files.readme.io/4919ff4-image.png)

Within a datasource you will be able to look at its settings, tables, relationships and queries. Within these options you can select the "Settings" tab of your datasource - this will only be available SQL based datasources.

![](https://files.readme.io/4514cb7-image.png)

From here clicking the download button will trigger a schema export from your database. This will export all of the tables that can be accessed, not necessarily just the ones that are imported.

From here, you will have a file named like `PostgresSQL-dump.sql` - you can open this file see the schema of your tables depicted.
