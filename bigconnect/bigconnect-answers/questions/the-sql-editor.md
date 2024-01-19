# The SQL editor

If you ever need to ask questions that can’t be expressed using the query builder, you can use SQL instead.

## Starting a new SQL query

Select **+ New** in the main nav bar. If you have the permissions to use the SQL editor, you’ll see an option to start a new **SQL query** next to a little console icon.

After clicking **SQL query**, you’ll see an editor where you can write and run queries in SQL (or your database’s native querying language).

<figure><img src="https://www.metabase.com/docs/latest/questions/images/SQLInterface.png" alt=""><figcaption></figcaption></figure>

To try it out, make sure you’ve selected the Sample Database, then paste in this short SQL query:

```
SELECT
    sum(subtotal),
    created_at
FROM orders
GROUP BY created_at;
```

Don’t worry if you don’t understand this just yet. Click the blue **Run query** button to execute your query.

You’ll notice that the table that comes back is the same as if you had used the query builder to ask for the sum of `Subtotal` in the `Orders` table, grouped by the `Created At` date.

## Running query selections

You can run your SQL query by pressing **ctrl + enter** on Windows and Linux, or **⌘ + return** on a Mac. You can also run only part of a query by highlighting the part you’d like to run before clicking the run button or using the run shortcut key.

Questions asked using SQL can be saved, downloaded, converted to models, and added to dashboards just like questions asked using the query builder.

You can also refer to models and saved questions in your SQL queries.

## Using SQL filters

If you or someone else wrote a SQL query that includes variables, that question might have filter widgets at the top of the screen above the editor. Filter widgets let you modify the SQL query before it’s run, changing the results you might get.

<figure><img src="https://www.metabase.com/docs/latest/questions/images/SQL-filter-widget.png" alt=""><figcaption></figcaption></figure>

Writing SQL queries that use variables or parameters can be very powerful, but it’s also a bit more advanced, so that topic has its own page if you’d like to learn more.

## SQL snippets

You can use SQL snippets to save, reuse, and share SQL code across multiple questions that are composed using the SQL editor.

## How Answers executes SQL queries

When you run a query from the SQL editor, Answers sends the query to your database exactly as it is written. Any results or errors displayed in Answers are the same as the results or errors that you would get if you ran the query directly against your database. If the SQL syntax of your query doesn’t match the SQL dialect used by your database, your database won’t be able to run the query.

## Question version history

For questions, dashboards, and models, Answers keeps a version history for the previous fifteen versions of that item.
