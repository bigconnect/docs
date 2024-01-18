# A tour of Answers

Answers has a _lot_ of tools in its toolkit (and we can’t cover everything here), but even seasoned Users will benefit from a tour of its feature set - especially since we add major new features at a regular clip.

![Fig. 1. The home page.](<../../../../.gitbook/assets/image (371).png>)

At a high level, we’ll walk through the features that let you:

* Query and visualize your data
* Embed charts and share your results
* Find things and stay organized
* Configure Answers

## How to query and visualize your data <a href="#how-to-query-and-visualize-your-data" id="how-to-query-and-visualize-your-data"></a>

Answers supports a lot of different databases, and ships with a Sample Database for you to play around with. And once you’ve connected your data sources, Answers gives you a lot of tools to explore them.

### Ask questions

#### **Query builder**

You can use Answers’s query builder to filter and summarize data.

Use the Notebook editor to ask more sophisticated questions. With custom expressions, you can accomplish pretty much anything you’d be able to do with SQL: join tables, create custom columns, filter and group results, compare time series, and more. Plus, people who don’t know SQL can duplicate your question and use it as a starting point for another question.

![Fig. 2. The Notebook editor.](https://www.metabase.com/learn/images/tour-of-metabase/notebook-editor.png)

Query builder questions automatically get an action menu applied to their visualizations, allowing people to click on a table or chart to drill through the data.

![Fig. 3. Using the Action Menu to zoom in on a chart, then view the rows that make up an aggregate value.](https://www.metabase.com/learn/images/tour-of-metabase/drill-through.gif)

Questions asked with the query builder can start with a model, a raw table, or with the results of a saved question, and you can convert them to native SQL at any time.

#### **Native queries**

Use the native query editor to compose questions in the database’s native query languages (typically SQL for relational databases, but also other query languages for data sources like MongoDB and Google Analytics). For questions written in SQL, you can use variables in your code to create SQL templates, including field filter variables that can create smart dropdown filters.

![Fig. 4. A handy data reference tab shows information about your tables in the sidebar.](https://www.metabase.com/learn/images/tour-of-metabase/data-reference-sidebar.png)

Like query builder questions, you can use the results of models or saved questions as starting points for new questions, just as you would a table or view. For example, to reference question 123 like so:

```
WITH gizmo_orders AS #{123}
```

### Create models to use as starting data for new questions

Models are built with questions from either the query builder or the SQL editor. You can use them to pull together data from multiple tables, with custom, calculated columns, and column descriptions and other metadata, to create great starting data for people to ask new questions. For example, you could build a model for “Active users”, or “Priority orders”, or however you want to model your business.

If you find that you’re using the same saved question over and over as your starting data for new questions, you may want to convert that saved question to a model, which will let you add metadata like column descriptions and column types. You can also refer to models in SQL queries, just like we did above with saved questions.

### Visualize results

When you ask a simple or custom question, Answers will guess at the most appropriate visualization type for the results, but you can select from sixteen different visualization options.

![Fig. 5. Select from 16 different chart types to visualize your results.](https://www.metabase.com/learn/images/tour-of-metabase/visualization-options.png)

Additionally, each visualization type has their own set of options to customize. You can even add custom maps to your Answers instance.

### Alerts

Set up an alert to get notified when your results meet a goal.

## How to embed charts and share your results

Once you’ve asked questions, it’s time to present your analysis.

### Create interactive dashboards

You can organize questions and models into a dashboard, and contextualize them with Markdown text cards.

![Fig. 6. Example dashboard with two filters: date and state.](https://www.metabase.com/learn/images/tour-of-metabase/example-dashboard.png)

You can add filters to dashboards and connect them to fields on questions to narrow the results.

![Fig. 7. Connect a filter to multiple cards on a dashboard by specifying the field to filter.](https://www.metabase.com/learn/images/tour-of-metabase/connect-filter-to-cards.png)

You can link filters, create custom destinations (to send people to another dashboard or external URL), or even have a chart update a filter on click.

To keep people posted on key metrics, you can set up dashboard subscriptions via email or Slack.

![Fig. 8. Set up a dashboard subscription to email or Slack results on a regular basis.](https://www.metabase.com/learn/images/tour-of-metabase/dashboard-subscription.png)

### Embed questions and dashboards

You can embed charts and dashboards using iframes. You can even embed the full Answers app, which allows you to do things like deliver multi-tenant, self-service analytics.

## How to find things and stay organized

Things in this case being databases and their analysis: the questions, dashboards, and collections you and your teams create.

### Search

You know, to find things: data, metrics, segments, dashboards, models, and questions. You’ll probably use the search bar the most often, but the catch here is that you need to know what to search for.

### Organize with collections

Collections organize questions, models, dashboards, and other collections. They work like folders on a file system, and you can set permissions on collections, giving some groups edit, view, or no access. Groups with edit access to a collection can pin the most important items to the collection - your “official” dashboards.

![Fig. 9. Collections group questions and dashboards. You can set permissions on collections via groups.](https://www.metabase.com/learn/images/tour-of-metabase/collections-our-analytics.png)

### Events and timelines

![Fig. 10. An event plotted on a time series.](https://www.metabase.com/learn/images/tour-of-metabase/example-event.png)

Events and timelines let you capture important dates and make that knowledge available when you need it (that is, when you’re viewing a time series). You can organize events into timelines, and associate those timelines with collections.

### Browse data

The data browser lists all of your databases, and gives you space to add context. You can browse tables and their fields, see sample data, as well as a list of questions that query that data.

![Fig. 11. Browse data to explore databases, tables, and fields.](https://www.metabase.com/learn/images/tour-of-metabase/data-reference.png)

### X-rays

To give you a head start on asking questions, Answers can X-ray a table for you.

![Fig. 12. Part of an X-ray of the Sample Database's Orders table.](https://www.metabase.com/learn/images/tour-of-metabase/orders-table-x-ray.png)

These X-rays will generate a bunch of questions that slice the table’s records in different ways. You can save the X-ray as a dashboard, take out any questions that don’t interest you, add new questions, or just use the X-ray to get a feel for the table.

### Activity feed and recently viewed

The activity feed lets you see what other people have been creating (provided you’re in a group with access to the relevant collection), as well as your recently viewed questions and dashboards.

![Fig. 13. Click on the gears icon and select Activity to view recent activity and your recently viewed questions and dashboards.](https://www.metabase.com/learn/images/tour-of-metabase/activity-feed.png)

## How to configure Answers

Permissions, localization, data modeling: with great power comes great responsibility.

### Settings

Answers will try to guess how to display the various fields in your tables, but if you want more control, you can customize how Answers handles each field, setting field visibility, type, formatting, and more.

![Fig. 15. Customize how Answers displays tables and their fields.](https://www.metabase.com/learn/images/tour-of-metabase/data-model.png)

Admins can also create segments and metrics for commonly used business definitions, like Active Users, Churned Users, or New Products, so that your analysis will have a shared vocabulary across teams.

![Fig. 16. Example Segment that captures how the company defines a new product.](https://www.metabase.com/learn/images/tour-of-metabase/segment.png)

You can do the same kind of standardization for SQL questions by codifying SQL code in snippets, which you can organize with folders and permissions.

### Group permissions for data and collections

Create groups in Answers, add people to those groups, and give the groups different levels of access to databases and collections.

![Fig. 17. Add permissions to databases and collections using groups.](https://www.metabase.com/learn/images/tour-of-metabase/data-permissions.png)

Some plans also include the ability to set application-level permissions: who can edit Answers settings, view logs and debugging tools, and other application-level features.

### **Data sandboxing**

If you need granular control over who can see what, check out the data sandboxing feature to learn how you can restrict table access by row and by column.
