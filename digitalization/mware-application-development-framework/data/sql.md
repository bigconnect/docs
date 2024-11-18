# SQL

Connecting to and integrating SQL databases into Mware Application Development Framework

When you already have a database running, you can add external database connections through the Mware Application Development Framework interface with a few clicks.

Our core SQL datasource connections include:

* MS SQL Server
* MySQL / MariaDB
* Oracle
* PostgreSQL

### Adding the remote datasource

To add a new remote datasource choose the **Data** tab in Mware Application Development Framework, then click the `+` button in the left-hand panel.

<figure><img src="https://files.readme.io/2ef46dc-add_ds.png" alt="Add datasource"><figcaption><p>Add datasource</p></figcaption></figure>

You'll be prompted to choose the type of datasource you want. Next you will fill out the settings for your chosen SQL database. See each SQL datasource page for specific settings.

In each case, you will have the option to `Fetch tables`, `Create new table` or select another datasource.

<figure><img src="https://files.readme.io/326605b-Screenshot_2023-07-14_at_10.00.50.png" alt=""><figcaption></figcaption></figure>

> ### 📘Firewall considerations
>
> Your database server will need to be whitelisted in any firewalls protecting your database. If you are using the Cloud you should Whitelist the IP addresses in your firewall. If you are self-hosting you should whitelist the IP address of your Mware Application Development Frameworkserver on your database server.

> ### 📘
>
> If you use Docker to host your own Mware Application Development Framework installation and are connecting to a database on the same machine as your installation you should set the host to `host.docker.internal`, or `172.17.0.1` (if running on Linux).

\


### Fetching tables

By default when you add a new datasource, you will be presented a menu allowing you to choose which tables you want to fetch after the connection has been confirmed. However, in the dialog where you configure a new datasource, you can skip fetching tables.

Fetching tables can also be triggered at any stage after the source has been added. This is especially useful when you've added, removed, or updated a new table after you've added the connection to Mware Application Development Framework.

To fetch tables, head over to the added datasource in the sidebar, and find it under the `Tables` tab.

<figure><img src="https://files.readme.io/326605b-Screenshot_2023-07-14_at_10.00.50.png" alt=""><figcaption></figcaption></figure>

#### Fetching specific tables

It can be useful to fetch only the tables you need. Perhaps the database has a large number of tables and you don't want to pull them all in for your app.

To fetch a subset of your database tables, click `Fetch tables` and then toggle the tables you want to fetch.

<figure><img src="https://files.readme.io/f299355-Screenshot_2023-07-14_at_10.03.48.png" alt=""><figcaption></figcaption></figure>

### Defining relationships

To help Mware Application Development Framework understand your database you will need to create relationships between the different tables. This can be done from the configuration screen of your datasource. To get there, click your datasource in the sidebar and scroll down until you find the **Relationships** section.

<figure><img src="https://files.readme.io/0aec24a-define-relationship.png" alt=""><figcaption></figcaption></figure>

When you click the `Define relationship` button, you get a form in which you can define the relationship.

#### One-to-many

When you define a one-to-many relationship you will have to choose the _from_ table and its primary key column. After that, you need to choose the _to_ table and the foreign key column. For example, if you have a pets table, and an owner table, you first will have to select the owner primary index, and then select the pets "owner" foreign key column.

<figure><img src="https://files.readme.io/d5c457f-new_rel_one.png" alt=""><figcaption></figcaption></figure>

After setting this up, an `owner_id` column will be added to the `pets` table, as well as a `pets` column to be added to the `owner` table. These columns will only be added internally, meaning this column will not be added to the remote datasource. This is to allow Mware Application Development Framework to keep track of the relationships without interfering with the remote source. Visually, when you navigate to both tables, you'll see the relations displayed as blocks referring to the related row in the other table.

<figure><img src="https://files.readme.io/b21c771-pets.png" alt="Pets"><figcaption><p>Pets</p></figcaption></figure>

<figure><img src="https://files.readme.io/512fb75-owners.png" alt="Owners"><figcaption><p>Owners</p></figcaption></figure>

If you want to change the field being used in the relationship tags, you can do this by selecting the display column in each table.

#### Many-to-many

With a many-to-many relationship, a joining table will be needed. The steps will be similar to the above, however a "through" table needs to be selected.\
This "through" table will be the joining table.

> ### 📘Configuring the joining table
>
> To ensure many-to-many relationships function correctly in Mware Application Development Framework, it is important that your joining table is configured correctly. It must meet the following criteria:
>
> 1. Have a primary key that will _not be used_ as a foreign key
> 2. Have a _nullable_ foreign key for each table
>
> ![](https://files.readme.io/09b3818-joining\_table.png)

When saving rows, you will not need to save entries to the joining table - Mware Application Development Framework will automatically do this for you.

Many-to-many relationships could be useful if you had many managers across many departments, for example.

<figure><img src="https://files.readme.io/5cafd89-Screenshot_2023-12-14_at_17.11.49.png" alt="Defining a Many-to-Many relationship between Students and Classes"><figcaption><p>Defining a Many-to-Many relationship between Departments and Managers</p></figcaption></figure>

<figure><img src="https://files.readme.io/eff0e42-dept_manager.png" alt="Joining table"><figcaption><p>Joining table</p></figcaption></figure>

<figure><img src="https://files.readme.io/bc6371b-departments.png" alt="Classes table"><figcaption><p>Departments table</p></figcaption></figure>

<figure><img src="https://files.readme.io/510c57d-managers.png" alt="Students table"><figcaption><p>Managers table</p></figcaption></figure>

#### Editing relationships

After you have defined relationships, you will see them listed in the **Relationships** section of the datasource.

<figure><img src="https://files.readme.io/69c9f8b-Screenshot_2022-12-13_at_15.44.14.png" alt=""><figcaption></figcaption></figure>

To edit a relationship, simply click on an entry. You will then be able to change the keys and tables involved, or `Delete` the relationship. This will automatically remove any previous columns that were created for handling the relationships in both tables.

\


### Add query

As a complement to the Mware Application Development Framework integration of SQL tables into your app tables, you can also add Custom queries. These allow you to perform complex queries such as multiple _joins_, and _group by_.

To add queries for a given SQL Datasource or view existing queries, click on the datasource in the sources panel, and scroll to the bottom of the page.

<figure><img src="https://files.readme.io/4a307ac-Screenshot_2023-02-07_at_11.31.36.png" alt=""><figcaption></figcaption></figure>

Existing queries are also indicated by the _Search_ icon instead of the usual table icon.

<figure><img src="https://files.readme.io/6477596-Screenshot_2023-02-07_at_11.33.30.png" alt="A MySQL datasource with two queries"><figcaption><p>A MySQL datasource with two queries</p></figcaption></figure>

> ### 🚧Limitations
>
> Whilst custom queries offer a lot of flexibility in terms of the SQL query itself, you will lose some of the automatic functionality such as pagination and sorting.
>
> These can however still be added manually through the use of bindings. An example can be found in this discussions answer.

### Troubleshooting

If you are having issues with your SQL database in Mware Application Development Framework it is a good idea to raise this issue with the team. You can find out information about how to do this and provide the information that the team will need to fully debug your issue on the SQL troubleshooting page.
