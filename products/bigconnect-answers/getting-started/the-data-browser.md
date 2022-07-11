# The data browser

In this article, we’ll show how you can use the **Data Browser** and other reference tools to learn about your datasets, as well as how administrators can curate information in Answers about their data.

All the examples in this article use the Sample Database that’s automatically installed with your Answers instance.

_Users_ can browse data in these two areas of the application:

* Browse Data makes it easy to navigate by database or table name.
* The Data Reference area provides detailed information about every table.

_Admins_ also have access to these areas within the Admin section:

* The Databases page gives a high level view of your databases.
* The Data Model tab allows admins to edit table and column metadata.

### Browse Data <a href="#browse-data" id="browse-data"></a>

To display all databases connected to your Answers, click on **Browse Data** from the navigation sidebar. Click on a database to view its schemas and/or tables.

When you hover over tables, icons appear:

* The yellow **lightning icon** will create an X-ray.
* The gray **book icon** will take you to the you to the table’s Data Reference page.

To view the rows in the table, click on the table’s name.

![Fig. 1. Navigating through the browse data screen to easily access information.](https://www.metabase.com/learn/images/exploring-data-with-metabase-data-browser/browse-data.png)

### Lightning bolts create X-rays

X-rays are a way to autogenerate questions and explorations of your data. At MWARE, we’ve seen quite a few dashboards, so we have a good idea of the kinds of things that people often want to see, and we’ve designed X-rays to give people quick insights into their tables. Figure 2 is an X-ray that was automatically created for the `People` table.

![Fig. 2. Example of an X-ray based on the people table.](https://www.metabase.com/learn/images/exploring-data-with-metabase-data-browser/x-ray-examples.png)

To create an X-ray in **Browse Data**, hover over a table and click on the yellow lightning bolt (figure 3).

![Fig. 3. Hovering over a table in the Browse Data section, revealing a lightning icon and a book icon with the text 'X-ray this table'.](https://www.metabase.com/learn/images/exploring-data-with-metabase-data-browser/browse-data-xray.png)

### Data Reference pages

To visit the data reference section, click on the **book icon** on any of the pages in the Browse Data section. The data reference section is essentially a data dictionary.

Reference pages include database names and descriptions, as well as information about segments and metrics. Segments are filters that you can easily reference in the query builder. Metrics are an easy way to refer to a computed number (for example, revenue).

![Fig. 4. Landing page for the data reference section. Three tabs on the left that say Segments, Metrics, and Our Data.](https://www.metabase.com/learn/images/exploring-data-with-metabase-data-browser/data-ref-landing.png)

Let’s dive into a database. Once we click into our Sample Database, two tabs appear on the left side of the screen (figure 5).

The **Details tab** contains metadata about this database. As you can see in figure 5, the tab features three sections that you can use to provide information about this dataset to your users:

* General description
* Why this database is interesting
* Things to be aware of about this database

![Fig. 5. Data reference screen for the Sample Database.](https://www.metabase.com/learn/images/exploring-data-with-metabase-data-browser/data-reference.png)

Not only can you see the database's description, you can also see why the dataset might be useful or edit the page's information.

Admins have the option to click on the **Edit** button in the upper right hand corner to update this information (which they should).

The **Tables** tab displays the table names and descriptions. You can click on a table to view a **Details** tab, as well as view the **Fields in this table** tab. You can view a list of **Questions about this table** (provided you have permission to view those questions), and have the option to create an X-ray of the table.

The Details tab suggests useful questions to ask the table at the bottom of the page (figure 6).

![Fig. 6. The details tab of the Orders table.](https://www.metabase.com/learn/images/exploring-data-with-metabase-data-browser/data-ref-orders-description.png)

Admins can edit field names and types in the **Fields in this table** tab (figure 7).

![Fig. 7. The fields tab of the Orders table from an admin's perspective (with the Edit button in the upper right).](https://www.metabase.com/learn/images/exploring-data-with-metabase-data-browser/data-ref-orders-fields.png)

### Access Answers Admin

To access Answers Admin, go to the navigation sidebar, click on the **gears** icon at the bottom, and select **Admin settings**.

In the Admin panel you can add, update, and remove databases, as well as edit metadata about your data.

### The Databases page

The **Databases** page in the **Admin Panel** displays connection information about your databases:

* The database type
* How Answers is connected to your Answers instance
* Sync settings

Answers does a lightweight sync every hour to keep your in-app data current, but you can use this page to manually sync your database, manage sync frequency, and with some databases, determine which schemas to sync.

### Editing metadata in Data Model

Picking clear names and adding useful descriptions for context will help users improve their analysis. Answers can automatically try to create human-readable names of your tables and columns for you, but if we miss the mark, you can always disable the Friendly Table and Field Names feature.

To make changes to your metadata in Answers, visit the **Data Model** tab in the Admin panel. The Data Model tab displays options to edit metadata for the database, tables, and columns. For example, you can edit a column’s name, visibility, type, and description. You can also remap foreign keys to give human readable names to foreign key columns!

Some tips for making life easier for people:

* When column names are confusing, you can change their names or add a description.
* If you have address columns, you can hide them from your users to protect privacy.
* You can pick your preferred filter interface from three options (search box, list of values, or plain input box).

Perhaps the _most important_ piece of metadata you can change is the field type. As depicted in figure 10 below, there is a long list of field types to choose from. Selecting the correct type for a column can connect information across multiple tables, and give context to Answers so it can choose visualizations appropriate for your field types. For example, once you’ve accurately identified latitude and longitude columns in your table, you will be able to use map visualizations.\
