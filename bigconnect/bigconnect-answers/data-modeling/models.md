# Models

Models are a fundamental building block in Answers. Models curate data from another table or tables from the same database to anticipate the kinds of questions people will ask of the data. You can think of them as derived tables, or a special kind of saved question meant to be used as the starting point for new questions. You can base a model on a SQL or query builder question, which means you can include custom, calculated columns in your model.

Models:

* Let you update column descriptions and customize metadata to create great starting points for exploration.
* Show up higher in search results and get highlighted when other users start new questions to promote reuse.
* Live in collections to keep them separate from messy database schemas.
* Can surface individual records in search results.

## How to use models

You can use models to:

* Create, uh, models, with model here meaning an intuitive description of some concept in your business that you codify as a set of columns. An example model could be a “customer”, which is a table that pulls together customer information from multiple tables and adds computed columns, like adding a lifetime value (LTV) column. This model represents the measures and dimensions that you think are relevant to your understanding of your customers.
* Create summary tables that aggregate data from multiple tables.
* Clean up tables with unnecessary columns and rows filtered out.

The idea with models is to give other people a good “starting point table” that makes it easier to answer any questions they have about the subject being modeled.

## Create a model

First, search for models that already exist. If you can’t find one that meets your needs, you can create a model:

* from scratch, or
* from a saved question.

Models you create are automatically pinned to the current collection.

### Create a model from scratch

1. In the upper right, click **New +** > **Model**.
2. Choose either the query builder or a native query (if you want to use SQL). The advantage of using the query builder is that Answers will be able to fill out some of the metadata for you; if you use SQL, you’ll have to fill out that metadata manually.
3. Select your data.
4. Create and save your query.

### Create a model from a saved question

1. Ask a question using either the query builder or the SQL editor, or select an existing saved question that you want to convert to a model.
2. Save the question.
3. Click on the **…** > **Turn this into a model**.

<figure><img src="https://www.metabase.com/docs/latest/data-modeling/images/turn-into-a-model.png" alt=""><figcaption></figcaption></figure>

## Model detail page

To view a model’s detail page:

* From a collection: click on the **book** icon next to a model.
* From a model: click on the **info** button in the upper right, then click **Model details**.

<figure><img src="https://www.metabase.com/docs/latest/data-modeling/images/model-detail.png" alt=""><figcaption></figcaption></figure>

Here you’ll see several tabs:

* **Used by**: lists the items based on the model.
* **Schema**: lists the fields in the model.
* **Actions**: lists the actions in the model, and allows you to create new actions.

The model detail page also shows some basic info about the model:

* Description
* Contact (who wrote the model)
* Backing table(s)

To start a new question based on the model, click **Explore**.

To edit the model’s underlying query, click **Edit definition**.

You can also edit the model’s metadata.

## Add metadata to columns in a model

Metadata is the secret sauce of models. When you write a SQL query, Answers can display the results, but it can’t “know” what kind of data it’s returning (like it can with questions built using the query builder). What this means in practice is that people won’t be able to drill-through the results, because Answers doesn’t understand what the results are. With models, however, you can tell Answers what kind of data is in each returned column so that Answers can still do its drill-through magic. Metadata will also make filtering nicer by showing the correct filter widget, and it will help Answers to pick the right visualization for the results.

If you only set one kind of metadata, set the **Column type** to let Answers know what kind of data it’s working with.

**Display name**

What people will see as the column’s name.

**Description**

A place to write helpful context for the column.

**Database column this maps to**

For models based on SQL queries, you can tell Answers if the column has the same type as an existing database column.

**Column type**

You can set the column type. The default is “No special type”. Be sure to set the Column type so people can interact with the results.

**This column should appear in…**

You can specify whether a column should appear in the table view, or just in a detail view (when you click on the entity/primary key for the row).

* Table and detail views
* Detail views only

**Display as**

* Text
* Link (it’s a URL people should be able to click on)

**Surface individual records in search by matching against this column**

For string fields in records with entity keys, Answers will give you the option make the values in that field show up when people search your Answers. Essentially, Answers will index these values and make them available to Answers’s search engine. This option is handy when people often want to jump straight to an individual record in your model.

For example, if you have a model with accounts, you could turn on this option for a column listing the account’s name or email so that people can quickly search for specific accounts in the model from anywhere in your Answers. When people click on a record in the search results, Answers will jump straight to the model and the object detail for that record.

There are some limitations to this indexing:

* The indexed field must be a text/string type.
* The record containing the field must have an integer entity key.
* To keep your search speedy, Answers will only index 5000 unique values from that field, so this option isn’t the best choice to turn on for tables with a ton of records.

## Edit a model’s query

You can edit a model’s query by clicking on the down arrow next to the model’s name and clicking on **Edit query definition**. When you’re doing editing, be sure to save your changes. Unlike questions, which prompt you to save as a new question, any changes here will overwrite the existing model. If you want to create a new model from an existing model, select **Duplicate this model** from the model sidebar (the icon of two overlapping squares).

## Start a question from a model

See asking questions.

## Refer to a model in the SQL query editor <a href="#refer-to-a-model-in-the-sql-query-editor" id="refer-to-a-model-in-the-sql-query-editor"></a>

You can refer to a model in a SQL query just like you can refer to a saved question:

```sql
SELECT * FROM {{#1-customer-model}}     
```

Or as a common table expression (CTE):

```sql
WITH model AS {{#3807-invoice-model}}
SELECT *
FROM model;    
```

Simply typing `{{#}}` will allow you to search for models (for example, you could type in `{{#customer}}` to search models, questions, and tables with the word “customer” in the title.

You can also use the data reference sidebar to browse the models available. To open the data reference sidebar, click on the **book** icon.

## Model version history

For questions, dashboards, and models, Answers keeps a version history for the previous fifteen versions of that item. You can view changes, revert to previous versions, and archive outdated items.

## Verifying a model

Just like with a question, admins can verify models. Verifying a model will give it a check mark to let others know an admin vetted the model. If anyone makes any changes to the model, the check mark will disappear. An admin will have to verify the question again to restore the check mark.

## Model caching

_Currently available for PostgreSQL, MySQL, and Redshift_.

Answers can cache the results of your models so that the models load faster. Answers caches models by creating tables in a bespoke schema in your data warehouse, and saves the results of the queries that underlie your models in those tables. When people ask questions based on your models, Answers will substitute those cached results in place of running the model’s query.

To set up model caching:

1. Enable model caching.
2. Create a schema to store cached models.

> Model caching doesn’t work with data sandboxing.

### Enable model caching

Go to **Admin settings** > **Settings** > **Caching** > **Models** to turn the feature on.

<figure><img src="https://www.metabase.com/docs/latest/data-modeling/images/model-caching-custom.png" alt=""><figcaption></figcaption></figure>

You can set models to refresh based on one of the default frequencies, or select the **Custom** option to use cron syntax to specify your own caching update frequency. We recommend scheduling the cache to refresh on a frequency that makes sense with how often your source tables update with new data. The cron scheduler uses the Report Timezone if selected and otherwise uses the System Timezone.

If someone changes the query definition of a model, any question based on that model will skip the cache until the next cache refresh.

### Create a schema to store cached models

Go to **Admin settings** > **Databases** > your database > **Turn model caching on**.

<figure><img src="https://www.metabase.com/docs/latest/data-modeling/images/cache-model-schema.png" alt=""><figcaption></figcaption></figure>

If the credentials you’ve given Answers to connect to your database are permissive, Answers should do all the work for you: Answers will check if the schema exists, or otherwise attempt to create it.

If the connection’s credentials lack the necessary permissions to create the schema in your database, you’ll need to create the schema in the database yourself:

1.  Click on the **info icon** to get the schema name.

    > In the above image, the schema name is “answers\_cache\_134ba\_7”, but your schema name will differ.
2. Create the schema in your database—make sure you use the exact schema name from step 1.
3. Ensure that the credentials Answers uses can manage and write to that schema.

### Refreshing a model’s cached results

To refresh a model’s cached results, go to the model and click on the **i** info icon. In the info sidebar that opens, you’ll see a note about when Answers last refreshed the model’s cache, and an icon to refresh the cache.

### View model caching logs

You can view the logs for model caching by clicking on the **gear** icon in the upper right and selecting **Admin settings** > **Tools** > **Model caching logs**.&#x20;

### Caching individual models

On some paid plans, you can also toggle caching on or off for individual models. When viewing a model, click on the **…** in the upper right and select **Turn model caching on/off**.
